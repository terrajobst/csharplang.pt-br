---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703961"
---
# <a name="enums"></a>Enums

Um ***tipo de enumeração*** é um tipo de valor distinto ([tipos de valor](types.md#value-types)) que declara um conjunto de constantes nomeadas.

O exemplo

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

declara um tipo de enumeração chamado `Color` com membros `Red`, `Green`e `Blue`.

## <a name="enum-declarations"></a>Enumerar declarações

Uma declaração enum declara um novo tipo enum. Uma declaração enum começa com a palavra-chave `enum`e define o nome, a acessibilidade, o tipo subjacente e os membros da enumeração.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Cada tipo de enumeração tem um tipo integral correspondente chamado de ***tipo subjacente*** do tipo de enumeração. Esse tipo subjacente deve ser capaz de representar todos os valores de enumerador definidos na enumeração. Uma declaração de enumeração pode declarar explicitamente um tipo subjacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`. Observe que `char` não pode ser usado como um tipo subjacente. Uma declaração de enumeração que não declara explicitamente um tipo subjacente tem um tipo subjacente de `int`.

O exemplo

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

declara um enum com um tipo subjacente de `long`. Um desenvolvedor pode optar por usar um tipo subjacente de `long`, como no exemplo, para habilitar o uso de valores que estão no intervalo de `long`, mas não no intervalo de `int`, ou para preservar essa opção para o futuro.

## <a name="enum-modifiers"></a>Modificadores de enumeração

Um *enum_declaration* pode, opcionalmente, incluir uma sequência de modificadores de enumeração:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de enumeração.

Os modificadores de uma declaração enum têm o mesmo significado que os de uma declaração de classe ([modificadores de classe](classes.md#class-modifiers)). Observe, no entanto, que os modificadores `abstract` e `sealed` não são permitidos em uma declaração enum. Enums não podem ser abstract e não permitem derivação.

## <a name="enum-members"></a>Membros de enumeração

O corpo de uma declaração de tipo enum define zero ou mais membros enum, que são as constantes nomeadas do tipo enum. Dois membros enum não podem ter o mesmo nome.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Cada membro enum tem um valor constante associado. O tipo desse valor é o tipo subjacente para a enumeração que o contém. O valor constante para cada membro de enumeração deve estar no intervalo do tipo subjacente para a enumeração. O exemplo

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

resulta em um erro de tempo de compilação porque os valores constantes `-1`, `-2`e `-3` não estão no intervalo do tipo integral subjacente `uint`.

Vários membros enum podem compartilhar o mesmo valor associado. O exemplo

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

mostra uma enumeração na qual dois membros de enum--`Blue` e `Max`--têm o mesmo valor associado.

O valor associado de um membro enum é atribuído implicitamente ou explicitamente. Se a declaração do membro enum tiver um inicializador *constant_expression* , o valor dessa expressão constante, implicitamente convertido no tipo subjacente da enumeração, será o valor associado do membro enum. Se a declaração do membro enum não tiver nenhum inicializador, seu valor associado será definido implicitamente, da seguinte maneira:

*  Se o membro enum for o primeiro membro enum declarado no tipo enum, seu valor associado será zero.
*  Caso contrário, o valor associado do membro de enumeração será obtido aumentando o valor associado do membro de enumeração textualmente anterior por um. Esse valor maior deve estar dentro do intervalo de valores que podem ser representados pelo tipo subjacente. caso contrário, ocorrerá um erro de tempo de compilação.

O exemplo

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

imprime os nomes de membro de enumeração e seus valores associados. A saída é:

```console
Red = 0
Green = 10
Blue = 11
```

pelos seguintes motivos:

*  o membro de enumeração `Red` é atribuído automaticamente ao valor zero (já que ele não tem nenhum inicializador e é o primeiro membro de enumeração);
*  o membro de enumeração `Green` recebe explicitamente o valor `10`;
*  e o membro de enumeração `Blue` é atribuído automaticamente ao valor um maior que o membro que o precede de uma vez.

O valor associado de um membro enum pode não ser, direta ou indiretamente, usar o valor de seu próprio membro enum associado. Além dessa restrição de circularidade, os inicializadores de membro de enumeração podem se referir livremente a outros inicializadores de membro de enumeração, independentemente da sua posição textual. Dentro de um inicializador de membro de enumeração, os valores de outros membros de enum são sempre tratados como tendo o tipo de seu tipo subjacente, de modo que as conversões não são necessárias ao fazer referência a outros membros de enumeração.

O exemplo

```csharp
enum Circular
{
    A = B,
    B
}
```

resulta em um erro de tempo de compilação porque as declarações de `A` e `B` são circulares. `A` depende `B` explicitamente e `B` depende de `A` implicitamente.

Os membros de enumeração são nomeados e têm o escopo definido de forma exatamente análoga aos campos dentro das classes. O escopo de um membro de enumeração é o corpo de seu tipo de enumeração que o contém. Dentro desse escopo, os membros de enumeração podem ser referenciados por seu nome simples. De todos os outros códigos, o nome de um membro de enumeração deve ser qualificado com o nome de seu tipo de enumeração. Os membros de enumeração não têm nenhuma acessibilidade declarada--um membro de enumeração é acessível se seu tipo de enumeração que o contém está acessível.

## <a name="the-systemenum-type"></a>O tipo System. Enum

O tipo `System.Enum` é a classe base abstrata de todos os tipos enum (isso é distinto e diferente do tipo subjacente do tipo enum) e os membros herdados de `System.Enum` estão disponíveis em qualquer tipo de enumeração. Uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) existe de qualquer tipo enum para `System.Enum`, e uma conversão unboxing ([conversões unboxing](types.md#unboxing-conversions)) existe de `System.Enum` para qualquer tipo enum.

Observe que `System.Enum` não é um *enum_type*. Em vez disso, é uma *class_type* da qual todos os *enum_type*são derivados. O tipo `System.Enum` herda do tipo `System.ValueType` ([o tipo System. ValueType](types.md#the-systemvaluetype-type)), que, por sua vez, herda do tipo `object`. Em tempo de execução, um valor do tipo `System.Enum` pode ser `null` ou uma referência a um valor em caixa de qualquer tipo de enumeração.

## <a name="enum-values-and-operations"></a>Valores de enumeração e operações

Cada tipo de enumeração define um tipo distinto; uma conversão de enumeração explícita ([conversões de enumeração explícitas](conversions.md#explicit-enumeration-conversions)) é necessária para converter entre um tipo de enumeração e um tipo integral ou entre dois tipos de enumeração. O conjunto de valores que um tipo de enumeração pode assumir não é limitado por seus membros enum. Em particular, qualquer valor do tipo subjacente de uma enumeração pode ser convertido para o tipo de enumeração e é um valor válido distinto desse tipo de enumeração.

Os membros de enumeração têm o tipo de tipo de enumeração contendo (exceto em outros inicializadores de membro de enumeração: consulte [membros de enumeração](enums.md#enum-members)). O valor de um membro enum declarado no tipo enum `E` com o valor associado `v` é `(E)v`.

Os operadores a seguir podem ser usados em valores de tipos de enumeração: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operadores de comparação de enumeração](expressions.md#enumeration-comparison-operators)), binary `+` (operador de[adição](expressions.md#addition-operator)), binary `-` ([operador de subtração](expressions.md#subtraction-operator)), `^`, `&`, `|` ([operadores lógicos de enumeração](expressions.md#enumeration-logical-operators)), `~` (operador de[complemento](expressions.md#bitwise-complement-operator)) `++` `--`[Incremento de sufixo e diminuição de operadores](expressions.md#postfix-increment-and-decrement-operators) e [incrementos de prefixo e de decréscimo](expressions.md#prefix-increment-and-decrement-operators)). 

Cada tipo de enumeração deriva automaticamente da classe `System.Enum` (que, por sua vez, deriva de `System.ValueType` e `object`). Assim, os métodos herdados e as propriedades dessa classe podem ser usados em valores de um tipo enum.
