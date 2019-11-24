---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703961"
---
# <a name="enums"></a><span data-ttu-id="6433a-101">Enums</span><span class="sxs-lookup"><span data-stu-id="6433a-101">Enums</span></span>

<span data-ttu-id="6433a-102">Um ***tipo de enumeração*** é um tipo de valor distinto ([tipos de valor](types.md#value-types)) que declara um conjunto de constantes nomeadas.</span><span class="sxs-lookup"><span data-stu-id="6433a-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="6433a-103">O exemplo</span><span class="sxs-lookup"><span data-stu-id="6433a-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="6433a-104">declara um tipo de enumeração chamado `Color` com membros `Red`, `Green`e `Blue`.</span><span class="sxs-lookup"><span data-stu-id="6433a-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="6433a-105">Enumerar declarações</span><span class="sxs-lookup"><span data-stu-id="6433a-105">Enum declarations</span></span>

<span data-ttu-id="6433a-106">Uma declaração enum declara um novo tipo enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="6433a-107">Uma declaração enum começa com a palavra-chave `enum`e define o nome, a acessibilidade, o tipo subjacente e os membros da enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="6433a-108">Cada tipo de enumeração tem um tipo integral correspondente chamado de ***tipo subjacente*** do tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="6433a-109">Esse tipo subjacente deve ser capaz de representar todos os valores de enumerador definidos na enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="6433a-110">Uma declaração de enumeração pode declarar explicitamente um tipo subjacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`.</span><span class="sxs-lookup"><span data-stu-id="6433a-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="6433a-111">Observe que `char` não pode ser usado como um tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="6433a-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="6433a-112">Uma declaração de enumeração que não declara explicitamente um tipo subjacente tem um tipo subjacente de `int`.</span><span class="sxs-lookup"><span data-stu-id="6433a-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="6433a-113">O exemplo</span><span class="sxs-lookup"><span data-stu-id="6433a-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="6433a-114">declara um enum com um tipo subjacente de `long`.</span><span class="sxs-lookup"><span data-stu-id="6433a-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="6433a-115">Um desenvolvedor pode optar por usar um tipo subjacente de `long`, como no exemplo, para habilitar o uso de valores que estão no intervalo de `long`, mas não no intervalo de `int`, ou para preservar essa opção para o futuro.</span><span class="sxs-lookup"><span data-stu-id="6433a-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="6433a-116">Modificadores de enumeração</span><span class="sxs-lookup"><span data-stu-id="6433a-116">Enum modifiers</span></span>

<span data-ttu-id="6433a-117">Um *enum_declaration* pode, opcionalmente, incluir uma sequência de modificadores de enumeração:</span><span class="sxs-lookup"><span data-stu-id="6433a-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="6433a-118">É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="6433a-119">Os modificadores de uma declaração enum têm o mesmo significado que os de uma declaração de classe ([modificadores de classe](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="6433a-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="6433a-120">Observe, no entanto, que os modificadores `abstract` e `sealed` não são permitidos em uma declaração enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="6433a-121">Enums não podem ser abstract e não permitem derivação.</span><span class="sxs-lookup"><span data-stu-id="6433a-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="6433a-122">Membros de enumeração</span><span class="sxs-lookup"><span data-stu-id="6433a-122">Enum members</span></span>

<span data-ttu-id="6433a-123">O corpo de uma declaração de tipo enum define zero ou mais membros enum, que são as constantes nomeadas do tipo enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="6433a-124">Dois membros enum não podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="6433a-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="6433a-125">Cada membro enum tem um valor constante associado.</span><span class="sxs-lookup"><span data-stu-id="6433a-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="6433a-126">O tipo desse valor é o tipo subjacente para a enumeração que o contém.</span><span class="sxs-lookup"><span data-stu-id="6433a-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="6433a-127">O valor constante para cada membro de enumeração deve estar no intervalo do tipo subjacente para a enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="6433a-128">O exemplo</span><span class="sxs-lookup"><span data-stu-id="6433a-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="6433a-129">resulta em um erro de tempo de compilação porque os valores constantes `-1`, `-2`e `-3` não estão no intervalo do tipo integral subjacente `uint`.</span><span class="sxs-lookup"><span data-stu-id="6433a-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="6433a-130">Vários membros enum podem compartilhar o mesmo valor associado.</span><span class="sxs-lookup"><span data-stu-id="6433a-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="6433a-131">O exemplo</span><span class="sxs-lookup"><span data-stu-id="6433a-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="6433a-132">mostra uma enumeração na qual dois membros de enum--`Blue` e `Max`--têm o mesmo valor associado.</span><span class="sxs-lookup"><span data-stu-id="6433a-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="6433a-133">O valor associado de um membro enum é atribuído implicitamente ou explicitamente.</span><span class="sxs-lookup"><span data-stu-id="6433a-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="6433a-134">Se a declaração do membro enum tiver um inicializador *constant_expression* , o valor dessa expressão constante, implicitamente convertido no tipo subjacente da enumeração, será o valor associado do membro enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="6433a-135">Se a declaração do membro enum não tiver nenhum inicializador, seu valor associado será definido implicitamente, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6433a-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="6433a-136">Se o membro enum for o primeiro membro enum declarado no tipo enum, seu valor associado será zero.</span><span class="sxs-lookup"><span data-stu-id="6433a-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="6433a-137">Caso contrário, o valor associado do membro de enumeração será obtido aumentando o valor associado do membro de enumeração textualmente anterior por um.</span><span class="sxs-lookup"><span data-stu-id="6433a-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="6433a-138">Esse valor maior deve estar dentro do intervalo de valores que podem ser representados pelo tipo subjacente. caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="6433a-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="6433a-139">O exemplo</span><span class="sxs-lookup"><span data-stu-id="6433a-139">The example</span></span>

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

<span data-ttu-id="6433a-140">imprime os nomes de membro de enumeração e seus valores associados.</span><span class="sxs-lookup"><span data-stu-id="6433a-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="6433a-141">A saída é:</span><span class="sxs-lookup"><span data-stu-id="6433a-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="6433a-142">pelos seguintes motivos:</span><span class="sxs-lookup"><span data-stu-id="6433a-142">for the following reasons:</span></span>

*  <span data-ttu-id="6433a-143">o membro de enumeração `Red` é atribuído automaticamente ao valor zero (já que ele não tem nenhum inicializador e é o primeiro membro de enumeração);</span><span class="sxs-lookup"><span data-stu-id="6433a-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="6433a-144">o membro de enumeração `Green` recebe explicitamente o valor `10`;</span><span class="sxs-lookup"><span data-stu-id="6433a-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="6433a-145">e o membro de enumeração `Blue` é atribuído automaticamente ao valor um maior que o membro que o precede de uma vez.</span><span class="sxs-lookup"><span data-stu-id="6433a-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="6433a-146">O valor associado de um membro enum pode não ser, direta ou indiretamente, usar o valor de seu próprio membro enum associado.</span><span class="sxs-lookup"><span data-stu-id="6433a-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="6433a-147">Além dessa restrição de circularidade, os inicializadores de membro de enumeração podem se referir livremente a outros inicializadores de membro de enumeração, independentemente da sua posição textual.</span><span class="sxs-lookup"><span data-stu-id="6433a-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="6433a-148">Dentro de um inicializador de membro de enumeração, os valores de outros membros de enum são sempre tratados como tendo o tipo de seu tipo subjacente, de modo que as conversões não são necessárias ao fazer referência a outros membros de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="6433a-149">O exemplo</span><span class="sxs-lookup"><span data-stu-id="6433a-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="6433a-150">resulta em um erro de tempo de compilação porque as declarações de `A` e `B` são circulares.</span><span class="sxs-lookup"><span data-stu-id="6433a-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="6433a-151">`A` depende `B` explicitamente e `B` depende de `A` implicitamente.</span><span class="sxs-lookup"><span data-stu-id="6433a-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="6433a-152">Os membros de enumeração são nomeados e têm o escopo definido de forma exatamente análoga aos campos dentro das classes.</span><span class="sxs-lookup"><span data-stu-id="6433a-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="6433a-153">O escopo de um membro de enumeração é o corpo de seu tipo de enumeração que o contém.</span><span class="sxs-lookup"><span data-stu-id="6433a-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="6433a-154">Dentro desse escopo, os membros de enumeração podem ser referenciados por seu nome simples.</span><span class="sxs-lookup"><span data-stu-id="6433a-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="6433a-155">De todos os outros códigos, o nome de um membro de enumeração deve ser qualificado com o nome de seu tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="6433a-156">Os membros de enumeração não têm nenhuma acessibilidade declarada--um membro de enumeração é acessível se seu tipo de enumeração que o contém está acessível.</span><span class="sxs-lookup"><span data-stu-id="6433a-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="6433a-157">O tipo System. Enum</span><span class="sxs-lookup"><span data-stu-id="6433a-157">The System.Enum type</span></span>

<span data-ttu-id="6433a-158">O tipo `System.Enum` é a classe base abstrata de todos os tipos enum (isso é distinto e diferente do tipo subjacente do tipo enum) e os membros herdados de `System.Enum` estão disponíveis em qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="6433a-159">Uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) existe de qualquer tipo enum para `System.Enum`, e uma conversão unboxing ([conversões unboxing](types.md#unboxing-conversions)) existe de `System.Enum` para qualquer tipo enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="6433a-160">Observe que `System.Enum` não é um *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="6433a-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="6433a-161">Em vez disso, é uma *class_type* da qual todos os *enum_type*são derivados.</span><span class="sxs-lookup"><span data-stu-id="6433a-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="6433a-162">O tipo `System.Enum` herda do tipo `System.ValueType` ([o tipo System. ValueType](types.md#the-systemvaluetype-type)), que, por sua vez, herda do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="6433a-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="6433a-163">Em tempo de execução, um valor do tipo `System.Enum` pode ser `null` ou uma referência a um valor em caixa de qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="6433a-164">Valores de enumeração e operações</span><span class="sxs-lookup"><span data-stu-id="6433a-164">Enum values and operations</span></span>

<span data-ttu-id="6433a-165">Cada tipo de enumeração define um tipo distinto; uma conversão de enumeração explícita ([conversões de enumeração explícitas](conversions.md#explicit-enumeration-conversions)) é necessária para converter entre um tipo de enumeração e um tipo integral ou entre dois tipos de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="6433a-166">O conjunto de valores que um tipo de enumeração pode assumir não é limitado por seus membros enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="6433a-167">Em particular, qualquer valor do tipo subjacente de uma enumeração pode ser convertido para o tipo de enumeração e é um valor válido distinto desse tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="6433a-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="6433a-168">Os membros de enumeração têm o tipo de tipo de enumeração contendo (exceto em outros inicializadores de membro de enumeração: consulte [membros de enumeração](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="6433a-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="6433a-169">O valor de um membro enum declarado no tipo enum `E` com o valor associado `v` é `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="6433a-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="6433a-170">Os operadores a seguir podem ser usados em valores de tipos de enumeração: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operadores de comparação de enumeração](expressions.md#enumeration-comparison-operators)), binary `+` (operador de[adição](expressions.md#addition-operator)), binary `-` ([operador de subtração](expressions.md#subtraction-operator)), `^`, `&`, `|` ([operadores lógicos de enumeração](expressions.md#enumeration-logical-operators)), `~` (operador de[complemento](expressions.md#bitwise-complement-operator)) `++` `--`[Incremento de sufixo e diminuição de operadores](expressions.md#postfix-increment-and-decrement-operators) e [incrementos de prefixo e de decréscimo](expressions.md#prefix-increment-and-decrement-operators)). </span><span class="sxs-lookup"><span data-stu-id="6433a-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="6433a-171">Cada tipo de enumeração deriva automaticamente da classe `System.Enum` (que, por sua vez, deriva de `System.ValueType` e `object`).</span><span class="sxs-lookup"><span data-stu-id="6433a-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="6433a-172">Assim, os métodos herdados e as propriedades dessa classe podem ser usados em valores de um tipo enum.</span><span class="sxs-lookup"><span data-stu-id="6433a-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
