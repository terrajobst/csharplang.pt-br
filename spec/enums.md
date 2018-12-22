# <a name="enums"></a><span data-ttu-id="2b7ee-101">Enums</span><span class="sxs-lookup"><span data-stu-id="2b7ee-101">Enums</span></span>

<span data-ttu-id="2b7ee-102">Uma ***tipo de enumeração*** é um tipo de valor distinto ([tipos de valor](types.md#value-types)) que declara um conjunto de constantes nomeadas.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="2b7ee-103">O exemplo</span><span class="sxs-lookup"><span data-stu-id="2b7ee-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="2b7ee-104">declara um tipo de enumeração denominado `Color` com os membros `Red`, `Green`, e `Blue`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="2b7ee-105">Declarações de enum</span><span class="sxs-lookup"><span data-stu-id="2b7ee-105">Enum declarations</span></span>

<span data-ttu-id="2b7ee-106">Uma declaração de enumeração declara um novo tipo de enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="2b7ee-107">Uma declaração de enumeração começa com a palavra-chave `enum`e define o nome, acessibilidade, tipo subjacente e os membros da enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="2b7ee-108">Cada tipo de enumeração tem um tipo integral correspondente chamado de ***tipo subjacente*** o tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="2b7ee-109">Esse tipo subjacente deve ser capaz de representar todos os valores de enumerador definidos na enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="2b7ee-110">Uma declaração de enumeração pode declarar explicitamente um tipo subjacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="2b7ee-111">Observe que `char` não pode ser usado como um tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="2b7ee-112">Uma declaração de enumeração que não declara explicitamente um tipo subjacente tem um tipo subjacente de `int`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="2b7ee-113">O exemplo</span><span class="sxs-lookup"><span data-stu-id="2b7ee-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="2b7ee-114">declara uma enumeração com um tipo subjacente de `long`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="2b7ee-115">Um desenvolvedor pode optar por usar um tipo subjacente de `long`, conforme mostrado no exemplo, para habilitar o uso de valores que estão no intervalo de `long` , mas não está no intervalo de `int`, ou para preservar essa opção para o futuro.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="2b7ee-116">Modificadores de enum</span><span class="sxs-lookup"><span data-stu-id="2b7ee-116">Enum modifiers</span></span>

<span data-ttu-id="2b7ee-117">Uma *enum_declaration* opcionalmente pode incluir uma sequência de modificadores de enum:</span><span class="sxs-lookup"><span data-stu-id="2b7ee-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="2b7ee-118">Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="2b7ee-119">Os modificadores de uma declaração de enumeração têm o mesmo significado que aquelas de uma declaração de classe ([modificadores de classe](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="2b7ee-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="2b7ee-120">No entanto, observe que o `abstract` e `sealed` modificadores não são permitidos em uma declaração de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="2b7ee-121">Enums não pode ser abstrato e não é possível fazer a derivação.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="2b7ee-122">Membros enum</span><span class="sxs-lookup"><span data-stu-id="2b7ee-122">Enum members</span></span>

<span data-ttu-id="2b7ee-123">O corpo de uma declaração de tipo de enumeração define zero ou mais membros de enumeração, que são constantes nomeadas do tipo enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="2b7ee-124">Não há dois membros de enum podem ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="2b7ee-125">Cada membro de enumeração tem um valor de constante associado.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="2b7ee-126">O tipo desse valor é o tipo subjacente para o enum recipiente.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="2b7ee-127">O valor da constante para cada membro de enumeração deve ser no intervalo do tipo subjacente para o enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="2b7ee-128">O exemplo</span><span class="sxs-lookup"><span data-stu-id="2b7ee-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="2b7ee-129">resulta em um erro de tempo de compilação porque os valores de constantes `-1`, `-2`, e `-3` não estão no intervalo do tipo integral subjacente `uint`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="2b7ee-130">Vários membros de enum podem compartilhar o mesmo valor associado.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="2b7ee-131">O exemplo</span><span class="sxs-lookup"><span data-stu-id="2b7ee-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="2b7ee-132">mostra uma enumeração na quais dois membros de enum – `Blue` e `Max` – tem o mesmo valor associado.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="2b7ee-133">O valor associado de um membro de enumeração é atribuído implícita ou explicitamente.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="2b7ee-134">Se a declaração de membro de enumeração tem um *constant_expression* inicializador, o valor dessa expressão constante, implicitamente convertido no tipo subjacente da enumeração, é o valor associado do membro de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="2b7ee-135">Se a declaração de membro de enumeração não tem nenhum inicializador, seu valor associado é definido implicitamente, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2b7ee-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="2b7ee-136">Se o membro de enumeração é o primeiro membro de enumeração declarado no tipo enum, seu valor associado é zero.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="2b7ee-137">Caso contrário, o valor associado do membro de enumeração é obtido, aumentando o valor associado do membro enum textualmente precedente por um.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="2b7ee-138">Esse valor deve estar dentro do intervalo de valores que podem ser representados pelo tipo subjacente, caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2b7ee-139">O exemplo</span><span class="sxs-lookup"><span data-stu-id="2b7ee-139">The example</span></span>

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

<span data-ttu-id="2b7ee-140">Imprime os nomes de membro de enumeração e seus valores associados.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="2b7ee-141">A saída é:</span><span class="sxs-lookup"><span data-stu-id="2b7ee-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="2b7ee-142">pelos seguintes motivos:</span><span class="sxs-lookup"><span data-stu-id="2b7ee-142">for the following reasons:</span></span>

*  <span data-ttu-id="2b7ee-143">o membro de enumeração `Red` recebe automaticamente o valor zero (porque ele não tem nenhum inicializador e é o primeiro membro enum);</span><span class="sxs-lookup"><span data-stu-id="2b7ee-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="2b7ee-144">o membro de enumeração `Green` explicitamente recebe o valor `10`;</span><span class="sxs-lookup"><span data-stu-id="2b7ee-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="2b7ee-145">e o membro de enumeração `Blue` recebe automaticamente o valor maior do que o membro textualmente precedente.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="2b7ee-146">O valor associado de um membro de enumeração não pode, direta ou indiretamente, use o valor de seu próprio membro enum associado.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="2b7ee-147">Além dessa restrição circularidade, inicializadores de membro de enumeração livremente podem se referir a outros inicializadores de membro de enumeração, independentemente de sua posição textual.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="2b7ee-148">Dentro de um inicializador de membro de enumeração, valores de outros membros de enumeração sempre são tratados como se tivessem o tipo do seu tipo subjacente, para que as conversões não são necessárias para se referir a outros membros de enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="2b7ee-149">O exemplo</span><span class="sxs-lookup"><span data-stu-id="2b7ee-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="2b7ee-150">resulta em um erro de tempo de compilação porque as declarações das `A` e `B` são circulares.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="2b7ee-151">`A` depende `B` explicitamente, e `B` depende `A` implicitamente.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="2b7ee-152">Membros de enumeração são chamados e no escopo de uma maneira exatamente análoga às campos dentro de classes.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="2b7ee-153">O escopo de um membro de enumeração é o corpo do seu tipo recipiente do enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="2b7ee-154">Dentro do escopo, os membros de enumeração podem ser referenciados por seus nomes simples.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="2b7ee-155">De outro código, o nome de um membro de enumeração deve ser qualificado com o nome do seu tipo de enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="2b7ee-156">Membros de Enum não têm qualquer acessibilidade declarada, um membro de enumeração é acessível se seu tipo de enum recipiente estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="2b7ee-157">O tipo System. Enum</span><span class="sxs-lookup"><span data-stu-id="2b7ee-157">The System.Enum type</span></span>

<span data-ttu-id="2b7ee-158">O tipo `System.Enum` é a classe base abstrata de todos os tipos de enum (Isso é diferente do tipo subjacente do tipo enum e distintos), e os membros herdados do `System.Enum` estão disponíveis em qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="2b7ee-159">Uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) existe a partir de qualquer tipo de enumeração `System.Enum`e uma conversão unboxing ([conversões de conversão Unboxing](types.md#unboxing-conversions)) existe na `System.Enum` para qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="2b7ee-160">Observe que `System.Enum` não é em si um *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="2b7ee-161">Em vez disso, ele é um *class_type* da qual todas as *enum_type*s são derivados.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="2b7ee-162">O tipo `System.Enum` herda do tipo `System.ValueType` ([ValueType o tipo](types.md#the-systemvaluetype-type)), que, por sua vez, herda do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="2b7ee-163">Em tempo de execução, um valor do tipo `System.Enum` pode ser `null` ou uma referência a um valor demarcado de qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="2b7ee-164">Operações e valores de enumeração</span><span class="sxs-lookup"><span data-stu-id="2b7ee-164">Enum values and operations</span></span>

<span data-ttu-id="2b7ee-165">Cada tipo de enum define um tipo distinto; uma conversão explícita de enumeração ([conversões explícitas de enumeração](conversions.md#explicit-enumeration-conversions)) é necessária para converter entre um tipo de enumeração e um tipo integral ou entre dois tipos de enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="2b7ee-166">O conjunto de valores que pode assumir um tipo de enumeração não é limitado por seus membros de enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="2b7ee-167">Em particular, qualquer valor do tipo subjacente de um enum pode ser convertido para o tipo de enumeração e é um valor válido diferente daquele tipo enum.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="2b7ee-168">Membros de enumeração têm o tipo do tipo de enumeração de contenção (exceto dentro de outros inicializadores de membro de enumeração: consulte [membros de Enum](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="2b7ee-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="2b7ee-169">O valor de um membro de enumeração declarado no tipo de enumeração `E` com o valor associado `v` é `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="2b7ee-170">Os operadores a seguir podem ser usados em valores de tipos enum: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([operadores de comparação de enumeração](expressions.md#enumeration-comparison-operators)) , binário `+`  ([operador de adição](expressions.md#addition-operator)), binário `-`  ([operador de subtração](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Operadores lógicos de enumeração](expressions.md#enumeration-logical-operators)), `~`  ([operador de complemento bit a bit](expressions.md#bitwise-complement-operator)), `++` e `--`  ([Incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators) e [incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="2b7ee-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="2b7ee-171">Cada tipo de enumeração automaticamente deriva da classe `System.Enum` (que, por sua vez, deriva `System.ValueType` e `object`).</span><span class="sxs-lookup"><span data-stu-id="2b7ee-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="2b7ee-172">Assim, os métodos herdados e as propriedades dessa classe podem ser usadas nos valores de um tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="2b7ee-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
