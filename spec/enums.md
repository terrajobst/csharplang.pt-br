---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229497"
---
# <a name="enums"></a>Enums

Uma ***tipo de enumeração*** é um tipo de valor distinto ([tipos de valor](types.md#value-types)) que declara um conjunto de constantes nomeadas.

O exemplo

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

declara um tipo de enumeração denominado `Color` com os membros `Red`, `Green`, e `Blue`.

## <a name="enum-declarations"></a>Declarações de enum

Uma declaração de enumeração declara um novo tipo de enum. Uma declaração de enumeração começa com a palavra-chave `enum`e define o nome, acessibilidade, tipo subjacente e os membros da enumeração.

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

Cada tipo de enumeração tem um tipo integral correspondente chamado de ***tipo subjacente*** o tipo de enumeração. Esse tipo subjacente deve ser capaz de representar todos os valores de enumerador definidos na enumeração. Uma declaração de enumeração pode declarar explicitamente um tipo subjacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`. Observe que `char` não pode ser usado como um tipo subjacente. Uma declaração de enumeração que não declara explicitamente um tipo subjacente tem um tipo subjacente de `int`.

O exemplo

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

declara uma enumeração com um tipo subjacente de `long`. Um desenvolvedor pode optar por usar um tipo subjacente de `long`, conforme mostrado no exemplo, para habilitar o uso de valores que estão no intervalo de `long` , mas não está no intervalo de `int`, ou para preservar essa opção para o futuro.

## <a name="enum-modifiers"></a>Modificadores de enum

Uma *enum_declaration* opcionalmente pode incluir uma sequência de modificadores de enum:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de enumeração.

Os modificadores de uma declaração de enumeração têm o mesmo significado que aquelas de uma declaração de classe ([modificadores de classe](classes.md#class-modifiers)). No entanto, observe que o `abstract` e `sealed` modificadores não são permitidos em uma declaração de enumeração. Enums não pode ser abstrato e não é possível fazer a derivação.

## <a name="enum-members"></a>Membros enum

O corpo de uma declaração de tipo de enumeração define zero ou mais membros de enumeração, que são constantes nomeadas do tipo enum. Não há dois membros de enum podem ter o mesmo nome.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Cada membro de enumeração tem um valor de constante associado. O tipo desse valor é o tipo subjacente para o enum recipiente. O valor da constante para cada membro de enumeração deve ser no intervalo do tipo subjacente para o enum. O exemplo

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

resulta em um erro de tempo de compilação porque os valores de constantes `-1`, `-2`, e `-3` não estão no intervalo do tipo integral subjacente `uint`.

Vários membros de enum podem compartilhar o mesmo valor associado. O exemplo

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

mostra uma enumeração na quais dois membros de enum – `Blue` e `Max` – tem o mesmo valor associado.

O valor associado de um membro de enumeração é atribuído implícita ou explicitamente. Se a declaração de membro de enumeração tem um *constant_expression* inicializador, o valor dessa expressão constante, implicitamente convertido no tipo subjacente da enumeração, é o valor associado do membro de enumeração. Se a declaração de membro de enumeração não tem nenhum inicializador, seu valor associado é definido implicitamente, da seguinte maneira:

*  Se o membro de enumeração é o primeiro membro de enumeração declarado no tipo enum, seu valor associado é zero.
*  Caso contrário, o valor associado do membro de enumeração é obtido, aumentando o valor associado do membro enum textualmente precedente por um. Esse valor deve estar dentro do intervalo de valores que podem ser representados pelo tipo subjacente, caso contrário, ocorrerá um erro de tempo de compilação.

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

Imprime os nomes de membro de enumeração e seus valores associados. A saída é:

```
Red = 0
Green = 10
Blue = 11
```

pelos seguintes motivos:

*  o membro de enumeração `Red` recebe automaticamente o valor zero (porque ele não tem nenhum inicializador e é o primeiro membro enum);
*  o membro de enumeração `Green` explicitamente recebe o valor `10`;
*  e o membro de enumeração `Blue` recebe automaticamente o valor maior do que o membro textualmente precedente.

O valor associado de um membro de enumeração não pode, direta ou indiretamente, use o valor de seu próprio membro enum associado. Além dessa restrição circularidade, inicializadores de membro de enumeração livremente podem se referir a outros inicializadores de membro de enumeração, independentemente de sua posição textual. Dentro de um inicializador de membro de enumeração, valores de outros membros de enumeração sempre são tratados como se tivessem o tipo do seu tipo subjacente, para que as conversões não são necessárias para se referir a outros membros de enum.

O exemplo

```csharp
enum Circular
{
    A = B,
    B
}
```

resulta em um erro de tempo de compilação porque as declarações das `A` e `B` são circulares. `A` depende `B` explicitamente, e `B` depende `A` implicitamente.

Membros de enumeração são chamados e no escopo de uma maneira exatamente análoga às campos dentro de classes. O escopo de um membro de enumeração é o corpo do seu tipo recipiente do enum. Dentro do escopo, os membros de enumeração podem ser referenciados por seus nomes simples. De outro código, o nome de um membro de enumeração deve ser qualificado com o nome do seu tipo de enum. Membros de Enum não têm qualquer acessibilidade declarada, um membro de enumeração é acessível se seu tipo de enum recipiente estiver acessível.

## <a name="the-systemenum-type"></a>O tipo System. Enum

O tipo `System.Enum` é a classe base abstrata de todos os tipos de enum (Isso é diferente do tipo subjacente do tipo enum e distintos), e os membros herdados do `System.Enum` estão disponíveis em qualquer tipo de enumeração. Uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) existe a partir de qualquer tipo de enumeração `System.Enum`e uma conversão unboxing ([conversões de conversão Unboxing](types.md#unboxing-conversions)) existe na `System.Enum` para qualquer tipo de enumeração.

Observe que `System.Enum` não é em si um *enum_type*. Em vez disso, ele é um *class_type* da qual todas as *enum_type*s são derivados. O tipo `System.Enum` herda do tipo `System.ValueType` ([ValueType o tipo](types.md#the-systemvaluetype-type)), que, por sua vez, herda do tipo `object`. Em tempo de execução, um valor do tipo `System.Enum` pode ser `null` ou uma referência a um valor demarcado de qualquer tipo de enumeração.

## <a name="enum-values-and-operations"></a>Operações e valores de enumeração

Cada tipo de enum define um tipo distinto; uma conversão explícita de enumeração ([conversões explícitas de enumeração](conversions.md#explicit-enumeration-conversions)) é necessária para converter entre um tipo de enumeração e um tipo integral ou entre dois tipos de enum. O conjunto de valores que pode assumir um tipo de enumeração não é limitado por seus membros de enum. Em particular, qualquer valor do tipo subjacente de um enum pode ser convertido para o tipo de enumeração e é um valor válido diferente daquele tipo enum.

Membros de enumeração têm o tipo do tipo de enumeração de contenção (exceto dentro de outros inicializadores de membro de enumeração: consulte [membros de Enum](enums.md#enum-members)). O valor de um membro de enumeração declarado no tipo de enumeração `E` com o valor associado `v` é `(E)v`.

Os operadores a seguir podem ser usados em valores de tipos enum: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([operadores de comparação de enumeração](expressions.md#enumeration-comparison-operators)) , binário `+`  ([operador de adição](expressions.md#addition-operator)), binário `-`  ([operador de subtração](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Operadores lógicos de enumeração](expressions.md#enumeration-logical-operators)), `~`  ([operador de complemento bit a bit](expressions.md#bitwise-complement-operator)), `++` e `--`  ([Incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators) e [incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)).

Cada tipo de enumeração automaticamente deriva da classe `System.Enum` (que, por sua vez, deriva `System.ValueType` e `object`). Assim, os métodos herdados e as propriedades dessa classe podem ser usadas nos valores de um tipo de enumeração.
