---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704094"
---
# <a name="delegates"></a>Delegados

Delegados habilitam cenários que outras linguagens — C++como, Pascal e modula--foram abordadas com ponteiros de função. No C++ entanto, diferentemente dos ponteiros de função, os delegados C++ são totalmente orientados a objeto e, ao contrário de ponteiros para funções de membro, os delegados encapsulam uma instância de objeto e um método.

Uma declaração delegate define uma classe que é derivada da classe `System.Delegate`. Uma instância de delegado encapsula uma lista de invocação, que é uma lista de um ou mais métodos, cada um dos quais é conhecido como uma entidade que possa ser chamada. Para métodos de instância, uma entidade que possa ser chamada consiste em uma instância e um método nessa instância. Para métodos estáticos, uma entidade que possa ser chamada consiste apenas em um método. Invocar uma instância de delegado com um conjunto apropriado de argumentos faz com que cada uma das entidades chamáveis do delegado seja invocada com o conjunto de argumentos fornecido.

Uma propriedade interessante e útil de uma instância de delegado é que ela não sabe ou se preocupa com as classes dos métodos que encapsula; Tudo o que importa é que esses métodos sejam compatíveis ([delegar declarações](delegates.md#delegate-declarations)) com o tipo do delegado. Isso torna os delegados perfeitamente adequados para invocação "anônima".

## <a name="delegate-declarations"></a>Delegar declarações

Um *delegate_declaration* é um *type_declaration* ([declarações de tipo](namespaces.md#type-declarations)) que declara um novo tipo de delegado.

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

É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de delegado.

O modificador `new` só é permitido em delegados declarados dentro de outro tipo; nesse caso, ele especifica que esse delegado oculta um membro herdado com o mesmo nome, conforme descrito no [novo modificador](classes.md#the-new-modifier).

Os modificadores `public`, `protected`, `internal` e `private` controlam a acessibilidade do tipo delegado. Dependendo do contexto no qual a declaração delegate ocorre, alguns desses modificadores podem não ser permitidos ([acessibilidade declarada](basic-concepts.md#declared-accessibility)).

O nome do tipo do delegado é *identificador*.

O *formal_parameter_list* opcional especifica os parâmetros do delegado e *return_type* indica o tipo de retorno do delegado.

O *variant_type_parameter_list* opcional ([listas de parâmetros de tipo de variante](interfaces.md#variant-type-parameter-lists)) especifica os parâmetros de tipo para o próprio delegado.

O tipo de retorno de um tipo delegado deve ser `void` ou de saída (segurança de[variação](interfaces.md#variance-safety)).

Todos os tipos de parâmetro formais de um tipo delegado devem ser de entrada segura. Além disso, qualquer tipo de parâmetro `out` ou `ref` também deve ser de saída segura. Observe que até mesmo os parâmetros `out` devem ser seguros de entrada, devido a uma limitação da plataforma de execução subjacente.

Tipos delegados em C# são equivalentes a nome, não é estruturalmente equivalente. Especificamente, dois tipos delegados diferentes que têm as mesmas listas de parâmetros e tipos de retorno são considerados tipos delegados diferentes. No entanto, as instâncias de dois tipos delegados distintos, mas estruturalmente equivalentes podem ser comparadas como iguais ([operadores de igualdade de delegado](expressions.md#delegate-equality-operators)).

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

Os métodos `A.M1` e `B.M1` são compatíveis com os tipos delegados `D1` e `D2`, pois eles têm o mesmo tipo de retorno e lista de parâmetros; no entanto, esses tipos delegados são dois tipos diferentes, portanto, não são intercambiáveis. Os métodos `B.M2`, `B.M3` e `B.M4` são incompatíveis com os tipos de delegado `D1` e `D2`, pois têm diferentes tipos de retorno ou listas de parâmetros.

Assim como outras declarações de tipo genérico, argumentos de tipo devem ser fornecidos para criar um tipo de delegado construído. Os tipos de parâmetro e o tipo de retorno de um tipo de delegado construído são criados pela substituição, para cada parâmetro de tipo na declaração de delegado, o argumento de tipo correspondente do tipo de delegado construído. O tipo de retorno resultante e os tipos de parâmetro são usados para determinar quais métodos são compatíveis com um tipo de delegado construído. Por exemplo:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

O método `X.F` é compatível com o tipo delegado `Predicate<int>` e o método `X.G` é compatível com o tipo delegado `Predicate<string>`.

A única maneira de declarar um tipo delegate é por meio de um *delegate_declaration*. Um tipo delegado é um tipo de classe que é derivado de `System.Delegate`. Os tipos delegados são implicitamente `sealed`, portanto não é permitido derivar qualquer tipo de um tipo delegado. Também não é permitido derivar um tipo de classe não-delegado de `System.Delegate`. Observe que `System.Delegate` não é, em si, um tipo delegado; é um tipo de classe do qual todos os tipos delegados são derivados.

C#fornece uma sintaxe especial para a instanciação e a invocação de delegado. Exceto para instanciação, qualquer operação que possa ser aplicada a uma classe ou instância de classe também pode ser aplicada a uma classe ou instância de delegado, respectivamente. Em particular, é possível acessar membros do tipo `System.Delegate` por meio da sintaxe de acesso de membro usual.

O conjunto de métodos encapsulado por uma instância de delegado é chamado de lista de invocação. Quando uma instância de delegado é criada ([compatibilidade de delegado](delegates.md#delegate-compatibility)) de um único método, ela encapsula esse método e sua lista de invocação contém apenas uma entrada. No entanto, quando duas instâncias delegadas não nulas são combinadas, suas listas de invocação são concatenadas – no operando de ordem esquerda e, em seguida, operando à direita – para formar uma nova lista de invocação, que contém duas ou mais entradas.

Os delegados são combinados usando o binário `+` ([operador de adição](expressions.md#addition-operator)) e os operadores de `+=` ([atribuição composta](expressions.md#compound-assignment)). Um delegado pode ser removido de uma combinação de delegados, usando o binário `-` ([operador de subtração](expressions.md#subtraction-operator)) e os operadores de `-=` ([atribuição composta](expressions.md#compound-assignment)). Os delegados podem ser comparados para igualdade ([operadores de igualdade de delegado](expressions.md#delegate-equality-operators)).

O exemplo a seguir mostra a instanciação de vários delegados e suas listas de invocação correspondentes:

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

Quando `cd1` e `cd2` são instanciados, cada um encapsula um método. Quando `cd3` é instanciado, ele tem uma lista de invocação de dois métodos, `M1` e `M2`, nessa ordem. a lista de invocação do `cd4` contém `M1`, `M2` e `M1`, nessa ordem. Finalmente, a lista de invocação do `cd5` contém `M1`, `M2`, `M1`, `M1` e `M2`, nessa ordem. Para obter mais exemplos de como combinar (bem como remover) delegados, consulte [delegar invocação](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Delegar compatibilidade

Um método ou delegado `M` é ***compatível*** com um tipo delegado `D` se todas as seguintes opções forem verdadeiras:

*  `D` e `M` têm o mesmo número de parâmetros, e cada parâmetro em `D` tem os mesmos modificadores `ref` ou `out` que o parâmetro correspondente em `M`.
*  Para cada parâmetro de valor (um parâmetro sem `ref` ou `out` modificador), existe uma conversão de identidade ([conversão de identidade](conversions.md#identity-conversion)) ou conversão de referência implícita ([conversões de referência implícitas](conversions.md#implicit-reference-conversions)) do tipo de parâmetro em `D` para o tipo de parâmetro correspondente no `M`.
*  Para cada parâmetro `ref` ou `out`, o tipo de parâmetro em `D` é o mesmo que o tipo de parâmetro em `M`.
*  Existe uma conversão de identidade ou de referência implícita do tipo de retorno de `M` para o tipo de retorno de `D`.

## <a name="delegate-instantiation"></a>Delegar instanciação

Uma instância de um delegado é criada por um *delegate_creation_expression* ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)) ou uma conversão para um tipo de delegado. A instância delegada recém-criada então se refere a:

*  O método estático referenciado no *delegate_creation_expression*ou
*  O objeto de destino (que não pode ser `null`) e o método de instância referenciado no *delegate_creation_expression*ou
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

Depois de instanciadas, as instâncias delegadas sempre se referem ao mesmo objeto e método de destino. Lembre-se de que, quando dois delegados são combinados, ou um é removido de outro, um novo delegado resulta em sua própria lista de invocação; as listas de invocação dos delegados combinados ou removidos permanecem inalteradas.

## <a name="delegate-invocation"></a>Invocação de delegado

C#fornece uma sintaxe especial para invocar um delegado. Quando uma instância de delegado não nula cuja lista de invocação contém uma entrada é invocada, ela invoca um método com os mesmos argumentos que foi fornecido e retorna o mesmo valor que o método referenciado. (Confira as [invocações de representante](expressions.md#delegate-invocations) para obter informações detalhadas sobre a invocação de delegado.) Se ocorrer uma exceção durante a invocação de tal delegado e essa exceção não for detectada dentro do método que foi invocado, a pesquisa de uma cláusula catch de exceção continuará no método que chamou o delegado, como se esse método tivesse chamado diretamente de método ao qual o delegado é referenciado.

A invocação de uma instância delegada cuja lista de invocação contém várias entradas continua invocando cada um dos métodos na lista de invocação, de forma síncrona, na ordem. Cada método chamado é passado como o mesmo conjunto de argumentos, como foi dado à instância delegada. Se essa invocação de delegado incluir parâmetros de referência ([parâmetros de referência](classes.md#reference-parameters)), cada invocação de método ocorrerá com uma referência à mesma variável; as alterações nessa variável por um método na lista de invocação ficarão visíveis para os métodos mais adiante na lista de invocação. Se a invocação de delegado incluir parâmetros de saída ou um valor de retorno, o valor final será proveniente da invocação do último delegado na lista.

Se ocorrer uma exceção durante o processamento da invocação de tal delegado, e essa exceção não for detectada dentro do método que foi invocado, a pesquisa de uma cláusula catch de exceção continuará no método que chamou o delegado e todos os métodos abaixo a lista de invocação não é invocada.

A tentativa de invocar uma instância delegada cujo valor é nulo resulta em uma exceção do tipo `System.NullReferenceException`.

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

Conforme mostrado na instrução `cd3 += cd1;`, um delegado pode estar presente em uma lista de invocação várias vezes. Nesse caso, ele é simplesmente invocado uma vez por ocorrência. Em uma lista de invocação como essa, quando esse delegado é removido, a última ocorrência na lista de invocação é a que foi realmente removida.

Imediatamente antes da execução da instrução final, `cd3 -= cd1;`, o delegado `cd3` se refere a uma lista de invocação vazia. A tentativa de remover um delegado de uma lista vazia (ou de remover um delegado não existente de uma lista não vazia) não é um erro.

A saída produzida é:

```console
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
