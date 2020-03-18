---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "79484996"
---

# <a name="records-v2"></a>Registros v2

No passado, pensamos sobre os registros como um recurso para habilitar o trabalho com dados.

"Trabalhar com dados" é um grande grupo com várias facetas, portanto, pode ser interessante examinar cada um deles isoladamente. Vamos começar examinando um exemplo de registros hoje e algumas das suas desvantagens.

Por exemplo, um registro simples seria definido hoje da seguinte maneira

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

e o uso seria lido

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

Há vantagens significativas neste código:

1. A definição tem a versão resiliente, as propriedades podem ser facilmente adicionadas ou movidas
2. As propriedades podem ser definidas em qualquer ordem, e os nomes na inicialização sempre correspondem aos acessadores
3. Propriedades com valores padrão podem simplesmente ser ignoradas

A primeira falha é que as propriedades agora devem ser mutáveis.

# <a name="mutability"></a>Imutabilidade

O que gostaríamos é para C# fornecer uma maneira de definir um membro de `readonly` em inicializadores de objeto.
Como alguns tipos podem não ter sido projetados com essa inicialização em mente, também gostaríamos de ser aceitos.

A solução proposta é um novo modificador, `initonly`, que pode ser aplicado a propriedades e campos:

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

O CodeGen para isso é surpreendentemente simples: apenas definimos o campo ReadOnly.
Especificamente, as propriedades reduzidas teriam a seguinte aparência:

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

O CLR considera a definição de campos somente leitura como não verificável, mas não é seguro. Para dar suporte a um verificador mais avançado, a seguinte regra é proposta: um campo somente leitura pode ser modificado somente dentro de `initonly` métodos ou em um novo objeto que está na pilha CLR e não foi publicado por meio de uma chamada de método ou loja.

Isso deve resolver muitos dos problemas com a imutabilidade no exemplo de `UserInfo`, embora não precise de estratégias de emissão complicadas ou frágeis. No entanto, a imutabilidade apresenta um novo problema: criar facilmente um objeto com alterações.

# <a name="with-ing"></a>Com-ing

Ao programar com imutabilidade, fazer alterações em um objeto é feito construindo uma cópia com alterações em vez de fazer as alterações diretamente no objeto. Infelizmente, não há uma maneira conveniente de fazer isso no C#, mesmo com registros de estilo atual. Anteriormente, foi proposto que algum tipo de método "with" gerado automaticamente seja fornecido para registros que implementam essa funcionalidade. Se tivermos um mecanismo desse tipo, é importante que ele funcione com membros `initonly`. Para conseguir isso, é proposto que adicionamos uma expressão `with`, análoga a um inicializador de objeto. O uso de exemplo seria o seguinte:

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

O objeto `newUserName` resultante seria uma cópia de `userInfo`, com `Username` definido como "angocke".
O CodeGen na expressão de `with` também seria semelhante ao inicializador de objeto: um novo objeto é construído e, em seguida, o `Username` setter de `initonly` seria chamado no corpo do método.

É claro que a diferença aqui é que o novo objeto que está sendo construído não é uma nova criação de objeto simples, é uma duplicata do objeto original. Para fornecer essa funcionalidade, exigimos que o objeto forneça um "com Construtor" que forneça um objeto duplicado. Um construtor de `With` de exemplo teria A seguinte aparência:

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

Notavelmente, a expressão `with` definirá Membros `initonly`, assim como o inicializador de objeto, portanto, para dar suporte à verificação, devemos garantir que o objeto não possa ter sido publicado antes que os membros `initonly` sejam definidos. Para impor isso, o atributo `WithConstructor` (ou sintaxe equivalente) irá impor uma nova regra para o método: todas as instruções de retorno devem conter diretamente uma expressão de criação de objeto, possivelmente com um inicializador de objeto.

Se o construtor de `With` exigir validação, o usuário poderá introduzir um construtor para fazer essa validação, por exemplo,

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

A última parte da complexidade associada ao `With` é a herança. Se o registro for extensível, será necessário fornecer um novo `With` para a subclasse. Isso pode ser feito da seguinte maneira:

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

Observe uma parte adicional da complexidade aqui: para substituir o construtor de `With` pelo tipo derivado, o idioma também precisará oferecer suporte a tipos de retorno covariantes em substituições.
Já existe uma proposta separada para esse recurso [aqui](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).

**Desvantagens**

- Fazer todas as instruções de retorno em `WithConstructor`s contém novas expressões de objeto é restritivo.
  Isso pode ser possivelmente mitigado pela análise de fluxo que garante que o novo objeto não escape o método
- O suporte à variação em substituições por meio de truques do compilador exigirá métodos stub, o que aumentará de forma quadrática com a profundidade da herança. A necessidade de um método stub é devido a um requisito de tempo de execução que substituisse as assinaturas sejam exatamente iguais. Se o requisito de tempo de execução fosse diminuído, os métodos de stub não seriam necessários.
- O uso de construtores encadeados do formulário `Type(Type original)` reserva efetivamente esse construtor para o uso do padrão. Como os construtores têm semântica exclusiva e não podem ser renomeados, isso pode ser limitado.


## <a name="wrapping-it-all-up-records"></a>Encapsulando tudo: registros

Os recursos acima permitem um estilo de programação que era muito difícil antes. Mas, mesmo com os novos recursos, pode ser bastante detalhado e propenso a erros para anotar tudo por conta própria. Há também alguns itens, como Equals e GetHashCode, que já podem ser escritos hoje, é apenas uma questão de trabalhos.
Além disso, uma falha significativa na implementação de igualdade sobre esses novos primitivos é que a igualdade estrutural é algo que deve ser alterado com o tipo de dados à medida que novos dados são adicionados, mas ao tratá-lo manualmente, é provável que essas coisas possam ficar fora de sincronia.

Portanto, é proposto que C# ofereça suporte à nova sintaxe para registros, não para fornecer novos recursos, mas para definir padrões e gerar código projetado para uso em registros. A sintaxe de exemplo ficaria parecida com

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

O código gerado para essa classe considera todos os campos públicos e as propriedades automáticas como membros estruturais do registro. Os membros do registro podem ser personalizados usando um novo atributo `RecordMember(bool)` que poderia ser usado para incluir ou excluir Membros. Os membros do registro seriam `initonly` por padrão e a igualdade seria gerada automaticamente para a classe com base nos membros do registro. A qualquer momento, o comportamento desses membros poderia ser personalizado simplesmente declarando-os na origem. A implementação escrita pelo usuário substituiria a implementação padrão em todos os padrões de uso.

Observe que a igualdade na aparência da herança é complexa, mas parece ter sido resolvida adequadamente na proposta de [outros registros](records.md).

## <a name="primary-constructors"></a>Construtores primários

A proposta de registro anterior também incluiu uma nova sintaxe para uma lista de parâmetros no próprio tipo, por exemplo,

```C#
class Point(int X, int Y);
```

No novo design, a lista de parâmetros seria um recurso ortogonal C# , que poderia ser perfeitamente integrado com registros. Se um construtor principal for incluído em um registro, ele teria novos padrões, assim como campos públicos e propriedades automáticas: os parâmetros no construtor primário seriam usados para gerar Propriedades de membro de registro público com o mesmo nome. Além disso, o construtor primário agora poderia ser usado para gerar um desconstrutor automaticamente.

Por exemplo, o seguinte registro com um construtor primário

```C#
data class Point(int X, int Y);
```

seria equivalente a

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

e a geração final do acima seria

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

Observe que fizemos uma outra informação em consideração para uma classe de dados com um construtor principal: em vez de definir os campos primários dentro do construtor protegido gerado, delegamos para o Construtor principal. Se a classe Point tivesse outro membro de registro não primário, por exemplo,

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

em seguida, isso alteraria o construtor protegido gerado da seguinte maneira:

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

Notavelmente, isso não responde ao que fazer sobre a herança de registros com construtores primários. Por exemplo,

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

Em vez de resolver de maneira arbitrária, uma abordagem mais explícita pode exigir que uma lista de parâmetros seja fornecida com a lista de base, por exemplo,

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

A lista de parâmetros na lista de base seria aplicada a uma chamada de `base` no construtor primário gerado:

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

Quanto ao que um construtor principal poderia significar fora de um registro, que ainda está aberto para uma proposta adicional.
