---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488979"
---
# <a name="delegates"></a>Delegados

Os representantes permitem cenários que os outros idiomas, como C++, Pascal e Modula – tenha resolvido com ponteiros de função. Ao contrário dos ponteiros de função do C++, entretanto, delegados são totalmente orientada a objeto e, ao contrário dos ponteiros de C++ para funções de membro, os delegados encapsulam uma instância do objeto e um método.

Uma declaração de delegado define uma classe que é derivada da classe `System.Delegate`. Uma instância de delegado encapsula uma lista de invocação, que é uma lista de um ou mais métodos, cada um deles é conhecida como uma entidade que pode ser chamada. Por exemplo, métodos de uma entidade que pode ser chamada consiste em uma instância e um método naquela instância. Para métodos estáticos, uma entidade que pode ser chamada consiste em apenas um método. Chamar uma instância delegate com um conjunto apropriado de argumentos faz com que cada uma das entidades de que pode ser chamado do delegado a ser invocado com o determinado conjunto de argumentos.

Uma propriedade interessante e útil de uma instância de delegado é que ele não sabe ou importa as classes dos métodos que ele encapsula; tudo o que importa é que esses métodos seja compatível ([declarações de delegado](delegates.md#delegate-declarations)) com o tipo do delegado. Isso torna delegados perfeitamente adequados para invocação "anônima".

## <a name="delegate-declarations"></a>Declarações de delegado

Um *delegate_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara um novo tipo de delegado.

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração delegate.

O `new` modificador é permitido somente em representantes declarados dentro de outro tipo, nesse caso, ele especifica que um delegado desse tipo oculta um membro herdado com o mesmo nome, conforme descrito em [o novo modificador](classes.md#the-new-modifier).

O `public`, `protected`, `internal`, e `private` modificadores controlam a acessibilidade do tipo de delegado. Dependendo do contexto no qual ocorre a declaração de delegado, alguns desses modificadores não podem ter permissão ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).

Nome do tipo do delegado *identificador*.

Opcional *formal_parameter_list* Especifica os parâmetros do delegado, e *return_type* indica o tipo de retorno do delegado.

Opcional *variant_type_parameter_list* ([listas de parâmetros de tipo de variante](interfaces.md#variant-type-parameter-lists)) especifica os parâmetros de tipo para o próprio delegado.

O tipo de retorno de um tipo delegado deve ser `void`, ou segura de saída ([safety variação](interfaces.md#variance-safety)).

Todos os tipos de parâmetro formal de um tipo de delegado devem ser de entrada-safe. Além disso, qualquer `out` ou `ref` tipos de parâmetro também devem ser segura de saída. Observe que mesmo `out` parâmetros devem ser de entrada-safe, devido a uma limitação da plataforma subjacente da execução.

Tipos de delegados em c# são o nome equivalente, não é estruturalmente equivalente. Especificamente, os dois tipos diferentes de delegado que têm o mesmo parâmetro de lista e tipo são considerados tipos diferentes de delegado de retorno. No entanto, as instâncias dos dois tipos de delegado diferentes, porém estruturalmente equivalente podem comparados como iguais ([delegar operadores de igualdade](expressions.md#delegate-equality-operators)).

Por exemplo:

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

Os métodos `A.M1` e `B.M1 `são compatíveis com ambos os tipos de delegado `D1` e `D2` , já que eles têm a mesma lista de tipo e o parâmetro de retorno; no entanto, esses tipos de delegado são dois tipos diferentes, para que não sejam intercambiáveis. Os métodos `B.M2`, `B.M3`, e `B.M4` são incompatíveis com os tipos de delegado `D1` e `D2`, já que eles têm tipos diferentes de retorno ou listas de parâmetros.

Como outras declarações de tipo genérico, os argumentos de tipo devem ser fornecidos para criar um tipo de delegado construído. Os tipos de parâmetro e o tipo de retorno de um tipo de delegado construído são criados pela substituição, para cada parâmetro de tipo na declaração de delegado, o argumento de tipo correspondente do tipo de delegado construído. O tipo de retorno resultante e tipos de parâmetro são usados na determinação de quais métodos são compatíveis com um tipo de delegado construído. Por exemplo:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

O método `X.F` é compatível com o tipo de delegado `Predicate<int>` e o método `X.G` é compatível com o tipo de delegado `Predicate<string>` .

A única maneira de declarar um tipo de delegado é por meio de um *delegate_declaration*. Um tipo de delegado é um tipo de classe que é derivado de `System.Delegate`. Tipos de delegado são implicitamente `sealed`, portanto, não é possível derivar de qualquer tipo de um tipo de delegado. Também não é permitido para derivar um tipo de classe não delegado de `System.Delegate`. Observe que `System.Delegate` é não o próprio tipo de um delegado; é um tipo de classe da qual todos os tipos de delegado são derivados.

C# fornece sintaxe especial para o delegado de instanciação e invocação. Com exceção de instanciação, qualquer operação que pode ser aplicada a uma classe ou instância de classe também pode ser aplicada a uma classe de delegado ou a instância, respectivamente. Em particular, é possível acessar membros do `System.Delegate` tipo por meio da sintaxe de acesso de membro normal.

O conjunto de métodos encapsulados por uma instância de delegado é chamado de uma lista de invocação. Quando uma instância de delegado é criada ([compatibilidade de delegado](delegates.md#delegate-compatibility)) de um único método, ele encapsula esse método, e sua lista de invocação contém apenas uma entrada. No entanto, quando duas instâncias de delegado não nulos são combinadas, suas listas de invocação são concatenadas – na ordem operando em seguida, o operando à direita, da esquerda para formar uma nova lista de invocação, que contém duas ou mais entradas.

Delegados são combinados usando o binário `+` ([operador de adição](expressions.md#addition-operator)) e `+=` operadores ([atribuição composta](expressions.md#compound-assignment)). Um delegado pode ser removido de uma combinação de delegados, usando o binário `-` ([operador de subtração](expressions.md#subtraction-operator)) e `-=` operadores ([atribuição composta](expressions.md#compound-assignment)). Delegados podem ser comparados quanto à igualdade ([delegar operadores de igualdade](expressions.md#delegate-equality-operators)).

O exemplo a seguir mostra a instanciação de um número de delegados e lista de sua invocação correspondente:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

Quando `cd1` e `cd2` são instanciados, cada um deles encapsular um método. Quando `cd3` é instanciado, ele tem uma lista de invocação dos dois métodos, `M1` e `M2`, nessa ordem. `cd4`da lista de invocação contém `M1`, `M2`, e `M1`, nessa ordem. Por fim, `cd5`da lista de invocação contém `M1`, `M2`, `M1`, `M1`, e `M2`, nessa ordem. Para obter mais exemplos de combinar delegados (bem como remover), consulte [invocação de delegado](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Compatibilidade de delegado

Um método ou delegate `M` está ***compatível*** com um tipo delegado `D` se todos os itens a seguir forem verdadeiras:

*  `D` e `M` têm o mesmo número de parâmetros e cada parâmetro na `D` tem o mesmo `ref` ou `out` modificadores de parâmetro correspondente no `M`.
*  Para cada parâmetro de valor (um parâmetro sem nenhum `ref` ou `out` modificador), uma conversão de identidade ([conversão de identidade](conversions.md#identity-conversion)) ou conversão de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe do tipo de parâmetro na `D` para o tipo de parâmetro correspondente no `M`.
*  Para cada `ref` ou `out` parâmetro, o parâmetro de tipo na `D` é o mesmo que o tipo de parâmetro no `M`.
*  Existe a uma identidade ou conversão de referência implícita do tipo de retorno de `M` para o tipo de retorno de `D`.

## <a name="delegate-instantiation"></a>Instanciação de delegado

Uma instância de um delegado é criada por um *delegate_creation_expression* ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)) ou uma conversão para um tipo de delegado. A instância do representante recém-criado refere-se, em seguida, como:

*  O método estático referenciado na *delegate_creation_expression*, ou
*  O objeto de destino (que não pode ser `null`) e um método referenciado na instância do *delegate_creation_expression*, ou
*  Outro delegado.

Por exemplo:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

Quando instanciado, instâncias de delegado sempre referem-se para o mesmo objeto de destino e o método. Lembre-se de que, quando dois delegados são combinados, ou um é removido da outra, um novo resultar de delegado com sua própria lista de invocação; as listas de invocação dos delegados combinados ou removidos permanecem inalteradas.

## <a name="delegate-invocation"></a>Invocação de delegado

C# fornece a sintaxe especial para invocar um delegado. Quando uma instância de não-nulo delegado cuja lista de invocação contém uma entrada é invocada, ele invoca o um método com os mesmos argumentos que foi fornecido e retorna o mesmo valor que a chamada ao método. (Consulte [delegar invocações](expressions.md#delegate-invocations) para obter informações detalhadas sobre invocação de delegado.) Se ocorrer uma exceção durante a invocação de um delegado desse tipo, e essa exceção não é capturada dentro do método que foi invocado, continua a pesquisa por uma cláusula catch de exceção no método que chamou o delegado como se esse método tivesse sido chamada diretamente a método ao qual delegar o que é conhecido.

Invocação de uma instância de delegado cuja lista de invocação contém várias entradas prossegue, invocando cada um dos métodos na lista de invocação, de forma síncrona, em ordem. Cada método chamado recebe o mesmo conjunto de argumentos, conforme fornecido para a instância do delegado. Se tal uma invocação de delegado inclui parâmetros de referência ([fazer referência a parâmetros](classes.md#reference-parameters)), cada invocação de método ocorram com uma referência à mesma variável; as alterações a essa variável em um método na lista de invocação será visível para os métodos ainda mais para baixo na lista de invocação. Se a invocação de delegado inclui parâmetros de saída ou um valor de retorno, seu valor final virá do último delegado na lista de invocação.

Se uma exceção ocorre durante o processamento da invocação de um delegado desse tipo, e essa exceção não é capturada dentro do método que foi invocado, a pesquisa por uma cláusula catch de exceção continua no método que chamou o delegado, e todos os métodos mais abaixo a lista de invocação não são invocados.

A tentativa de invocar uma instância de delegado, cujo valor é nula resulta em uma exceção do tipo `System.NullReferenceException`.

O exemplo a seguir mostra como criar uma instância, combinar, remover e invocar delegados:

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

Conforme mostrado na instrução `cd3 += cd1;`, um delegado pode estar presente em uma lista de invocação várias vezes. Nesse caso, ele é simplesmente chamado uma vez por ocorrência. Uma lista de invocação como esse, quando esse delegado é removido, a última ocorrência na lista de invocação é aquele realmente removido.

Imediatamente antes da execução da instrução final, `cd3 -= cd1;`, o delegado `cd3` refere-se a uma lista de invocação vazio. A tentativa de remover um delegado de uma lista vazia (ou para remover um delegado inexistente em uma lista de não vazio) não é um erro.

A saída produzida é:

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
