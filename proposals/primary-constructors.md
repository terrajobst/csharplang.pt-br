---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484968"
---
# <a name="primary-constructors"></a>Construtores primários

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

As classes podem ter uma lista de parâmetros e, quando elas fazem, sua especificação de classe base pode ter uma lista de argumentos.
Os parâmetros do construtor primário estão no escopo durante a declaração de classe e, se forem capturados por um membro de função ou função anônima, eles serão armazenados como campos particulares na classe.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

É comum ter muitos códigos clichês no código de inicialização do programa. No caso geral, um dado `x` de dados é mencionado muitas vezes:

- Como um campo privado `_x`
- Como um parâmetro `x` a um construtor
- Em uma atribuição `_x = x;` do campo do parâmetro no Construtor
- Como uma propriedade `X`
- No setter de propriedade `x = value;`
- No `return x;` de getter de propriedade

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

Para propriedades que não exigem validação ou computação, o tédio pode ser reduzido usando propriedades automáticas, gerando assim a necessidade de declarar um campo de apoio explícito para a propriedade. Mas se sua propriedade exigir qualquer tipo de lógica além do que uma propriedade automática fornece, a melhor é a que você faz.

Em vez disso, os construtores primários reduzem a sobrecarga colocando argumentos de construtor diretamente no escopo em toda a classe, novamente dispensandondo a necessidade de declarar explicitamente um campo de apoio. Portanto, o exemplo acima se tornaria:

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

Neste exemplo, o Construtor principal reduz o número de entidades nomeadas para `x` de três para duas, dispensando o `_x` campo de backup. Ele remove duas das três declarações de membro (mantendo apenas a declaração de propriedade em si) e reduz o número total de menção de `x`/`_x`/`X` de oito a cinco.


## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

As classes podem ter uma lista de parâmetros:

``` c#
public class C(int i, string s)
{
    ...
}
```

A lista de parâmetros faz com que um construtor seja declarado implicitamente para a classe, com a mesma acessibilidade que a própria classe.

``` c#
new C(5, "Hello");
```

Os parâmetros do construtor primário estão no escopo ao longo do corpo da classe. Se eles forem capturados por um membro de função ou função anônima, eles serão armazenados como campos particulares na classe. Se eles forem usados apenas durante a inicialização, eles não serão armazenados no objeto.

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

Se uma classe com um construtor primário tiver uma especificação de classe base, ela poderá ter uma lista de argumentos. Isso serve como a lista de argumentos para um inicializador de `base(...)` do construtor declarado implicitamente. Se nenhuma lista de argumentos for fornecida, será assumida uma prevazia.

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
A classe também pode ter construtores definidos explicitamente, mas todos eles precisam usar um inicializador de `this(...)`. Isso garante que o construtor primário é sempre chamado quando uma nova instância é construída.

Todos os inicializadores no corpo da classe se tornarão atribuições no Construtor gerado. Isso significa que, ao contrário de outras classes, inicializadores serão executados *depois* que o construtor base tiver sido invocado, não antes. Além disso, a classe gerada conterá atribuições para inicializar quaisquer campos privados que foram gerados para armazenar parâmetros de Construtor primários que foram capturados por membros. Esses membros são reescritos para usar o campo particular em vez do parâmetro de maneira semelhante a fechamentos para expressões lambda. Os campos primários gerados são inicializados primeiro e, em seguida, as atribuições geradas pelo inicializador são executadas na ordem de aparência na classe.

Para o exemplo acima, o efeito da declaração de classe é como se fosse reescrito da seguinte maneira:

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

A captura tem restrições semelhantes à captura de variáveis locais por expressões lambda. Por exemplo, parâmetros de `ref` e `out` são permitidos em construtores primários, mas não podem ser capturados para meus corpos de membros.


## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Em ordem aproximada de significância.

* A proposta usa a sintaxe que também foi proposta para registros posicionais. Se quisermos ambos os recursos, será necessária alguma acomodação. Por exemplo, um modificador de `data` em registros foi proposto.
* O tamanho de alocação de objetos construídos é menos óbvio, pois o compilador determina se deve alocar um campo para um parâmetro de construtor primário com base no texto completo da classe. Esse risco é semelhante à captura implícita de variáveis por expressões lambda.
* Uma tentação comum (ou padrão acidental) pode ser capturar o parâmetro "mesmo" em vários níveis de herança, já que ele passa a cadeia de construtor, em vez de explicitamente alocado a ele um campo protegido na classe base, levando a alocações duplicadas para os mesmos dados em objetos. Isso é muito semelhante ao risco atual de substituir Propriedades automáticas por propriedades automáticas. 
* Conforme proposto acima, não há lugar para lógica adicional que normalmente pode ser expressa em corpos de construtor. A extensão "principais corpos do Construtor" abaixo aborda isso.
* Como proposto, a semântica da ordem de execução é sutilmente diferente de com construtores comuns. Isso pode ser remediado, mas com o custo de algumas das propostas de extensão (notadamente "corpos do construtor primário").
* A proposta só funciona quando um único Construtor pode ser designado como primário.
* Não há como ter uma acessibilidade separada da classe e o Construtor principal. Por exemplo, em situações em que todos os construtores públicos são delegados para um construtor "Build-it-all" privado que seria necessário. Se necessário, a sintaxe poderá ser proposta posteriormente.


## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Os registros posicionais completos podem ser uma alternativa ou podem coexistir com construtores primários, dependendo das especificidades. Eles permitiriam *mais* abreviação em um número *menor* de cenários. Ambos são potencialmente úteis, mas ter ambos pode ser um exagero, a menos que eles possam ser perfeitamente integrados entre si.


## <a name="possible-extensions"></a>Extensões possíveis
[extensions]: #possible-extensions

Essas são variações ou adições à proposta principal que podem ser consideradas em conjunto, ou em um estágio posterior, se considerado útil.

### <a name="primary-constructor-bodies"></a>Corpos do construtor primário

Os construtores em si geralmente contêm lógica de validação de parâmetro ou outro código de inicialização não trivial que não pode ser expresso como inicializadores.

Construtores primários poderiam ser estendidos para permitir que blocos de instrução apareçam diretamente no corpo da classe. Essas instruções seriam inseridas no Construtor gerado no ponto em que aparecem entre as atribuições de inicialização e, portanto, seriam executadas intercaladas com inicializadores. Por exemplo:

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a>Campos de inicializador e funções de inicializador

Em uma classe com um construtor principal, podemos considerar declarações de campo e de método sem que os modificadores de acessibilidade sejam mais como variáveis locais e funções locais:

* Assim como os parâmetros de Construtor primários, os "campos de inicializador" só seriam capturados em um campo privado real se fossem usados em membros de função.
* As "funções do inicializador" só seriam consideradas para capturar os parâmetros do construtor primário e os campos de inicializador se eles eram usados em outros membros da função. Se não forem capturadas, elas poderão ser geradas de maneira mais ideal, como as funções locais.
* Assim como os parâmetros de Construtor primários, eles não estarão disponíveis por meio de acesso de membro, mas apenas como um nome simples.

Isso pode ser usado para variáveis temporárias e funções auxiliares que só são relevantes para a inicialização:

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

Isso pode ser muito sutil, especialmente porque a ausência de modificadores de acessibilidade em outro lugar significa apenas `private`. 

### <a name="initializer-statements"></a>Instruções do inicializador

Uma combinação de radicais das extensões acima, seria simplesmente permitir instruções diretamente no corpo da classe. Essas instruções são exatamente como os corpos de Construtor intercalados propostos acima, exceto que não precisam estar entre `{ }`. Para que isso seja suficientemente útil, as variáveis "locais" e as funções auxiliares também precisariam ser expressas no nível superior da classe, da maneira explorada na extensão "campos de inicializador e funções de inicializador" acima.


### <a name="member-access"></a>Acesso de membros

A proposta principal trata os parâmetros do construtor primário como parâmetros que só podem ser chamados de nomes simples. Uma alternativa é permitir que eles sejam referenciados como se fossem campos particulares, ou seja, com acesso de membro, *mesmo* que eles não sejam gerados como campos. Isso permitiria que eles fossem referenciados como `this.x` quando sombreados por variáveis locais e acessados de uma instância diferente como `other.x`.

Se aplicado à extensão "campos de inicializador e funções de inicializador", isso também reduziria o grau para o qual eles eram diferentes dos membros privados comuns. A única diferença seria que o compilador é livre para Elide-los do objeto, se usado apenas durante a inicialização.

