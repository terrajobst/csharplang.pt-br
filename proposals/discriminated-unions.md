---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485122"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="03ceb-101">Uniões discriminadas/`enum class`</span><span class="sxs-lookup"><span data-stu-id="03ceb-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="03ceb-102">os `enum class`es são um novo tipo de declaração de tipo, às vezes chamados de uniões discriminadas, em que cada instância possível do tipo é listada e cada instância não se sobrepõe.</span><span class="sxs-lookup"><span data-stu-id="03ceb-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="03ceb-103">Um `enum class` é definido usando a seguinte sintaxe:</span><span class="sxs-lookup"><span data-stu-id="03ceb-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="03ceb-104">Sintaxe de exemplo:</span><span class="sxs-lookup"><span data-stu-id="03ceb-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="03ceb-105">Semântica</span><span class="sxs-lookup"><span data-stu-id="03ceb-105">Semantics</span></span>

<span data-ttu-id="03ceb-106">Uma definição de `enum class` define um tipo de raiz, que é uma classe abstrata de mesmo nome que a declaração de `enum class` e um conjunto de membros, cada um com um tipo que é um subtipo do tipo raiz.</span><span class="sxs-lookup"><span data-stu-id="03ceb-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="03ceb-107">Se houver várias definições de `partial enum class`, todos os membros serão considerados membros da definição de classe de enumeração.</span><span class="sxs-lookup"><span data-stu-id="03ceb-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="03ceb-108">Ao contrário de uma definição de classe abstrata definida pelo usuário, o tipo de raiz `enum class` é parcial por padrão e definido para ter um construtor padrão sem parâmetros *privados* .</span><span class="sxs-lookup"><span data-stu-id="03ceb-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="03ceb-109">Observe que, como o tipo de raiz é definido para ser uma classe abstrata parcial, as definições parciais do *tipo raiz* também podem ser adicionadas, onde são permitidas formas de sintaxe padrão para um corpo de classe.</span><span class="sxs-lookup"><span data-stu-id="03ceb-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="03ceb-110">No entanto, nenhum tipo pode herdar diretamente do tipo raiz em qualquer declaração, exceto aqueles especificados como membros `enum class`.</span><span class="sxs-lookup"><span data-stu-id="03ceb-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="03ceb-111">Além disso, nenhum construtor definido pelo usuário é permitido para o tipo de raiz.</span><span class="sxs-lookup"><span data-stu-id="03ceb-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="03ceb-112">Há quatro tipos de declarações de membro `enum class`:</span><span class="sxs-lookup"><span data-stu-id="03ceb-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="03ceb-113">Membros de classe simples</span><span class="sxs-lookup"><span data-stu-id="03ceb-113">simple class members</span></span>

* <span data-ttu-id="03ceb-114">Membros de classe complexa</span><span class="sxs-lookup"><span data-stu-id="03ceb-114">complex class members</span></span>

* <span data-ttu-id="03ceb-115">Membros do `enum class`</span><span class="sxs-lookup"><span data-stu-id="03ceb-115">`enum class` members</span></span>

* <span data-ttu-id="03ceb-116">Membros de valor.</span><span class="sxs-lookup"><span data-stu-id="03ceb-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="03ceb-117">Membros de classe simples</span><span class="sxs-lookup"><span data-stu-id="03ceb-117">Simple class members</span></span>

<span data-ttu-id="03ceb-118">Uma declaração de membro de classe simples define uma nova classe "registro" aninhada (intencionalmente deixada indefinida neste documento) com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="03ceb-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="03ceb-119">A classe aninhada herda do tipo raiz.</span><span class="sxs-lookup"><span data-stu-id="03ceb-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="03ceb-120">Dado o código de exemplo acima,</span><span class="sxs-lookup"><span data-stu-id="03ceb-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="03ceb-121">a declaração de `enum class` tem semântica equivalente à declaração a seguir</span><span class="sxs-lookup"><span data-stu-id="03ceb-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="03ceb-122">Membros de classe complexa</span><span class="sxs-lookup"><span data-stu-id="03ceb-122">Complex class members</span></span>

<span data-ttu-id="03ceb-123">Você também pode aninhar uma declaração de classe inteira abaixo de uma declaração de `enum class`.</span><span class="sxs-lookup"><span data-stu-id="03ceb-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="03ceb-124">Ele será tratado como uma classe aninhada do tipo raiz.</span><span class="sxs-lookup"><span data-stu-id="03ceb-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="03ceb-125">A sintaxe permite qualquer declaração de classe, mas é necessária para que o membro de classe complexa herde da declaração direta contendo `enum class`.</span><span class="sxs-lookup"><span data-stu-id="03ceb-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="03ceb-126">Membros do `enum class`</span><span class="sxs-lookup"><span data-stu-id="03ceb-126">`enum class` members</span></span>

<span data-ttu-id="03ceb-127">`enum classes` pode ser aninhada entre si, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="03ceb-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="03ceb-128">Isso é quase idêntico à semântica de um `enum class`de nível superior, exceto pelo fato de que a classe enum aninhada define um tipo raiz aninhado, e tudo abaixo da classe enum aninhada é um subtipo do tipo raiz aninhado, em vez do tipo raiz de nível superior.</span><span class="sxs-lookup"><span data-stu-id="03ceb-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="03ceb-129">Membros de valor</span><span class="sxs-lookup"><span data-stu-id="03ceb-129">Value members</span></span>

<span data-ttu-id="03ceb-130">`enum classes` também pode conter membros de valor.</span><span class="sxs-lookup"><span data-stu-id="03ceb-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="03ceb-131">Membros de valor definem propriedades estáticas públicas somente obtenção no tipo raiz que também retornam o tipo raiz, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="03ceb-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="03ceb-132">tem propriedades equivalentes a</span><span class="sxs-lookup"><span data-stu-id="03ceb-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="03ceb-133">A semântica completa é considerada um detalhe de implementação, mas é garantido que uma instância exclusiva será retornada para cada propriedade e a mesma instância será retornada em invocações repetidas.</span><span class="sxs-lookup"><span data-stu-id="03ceb-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="03ceb-134">Alternar expressão e padrões</span><span class="sxs-lookup"><span data-stu-id="03ceb-134">Switch expression and patterns</span></span>

<span data-ttu-id="03ceb-135">Há alguns ajustes propostos na correspondência de padrões e na expressão switch para manipular `enum classes`.</span><span class="sxs-lookup"><span data-stu-id="03ceb-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="03ceb-136">Expressões de switch já podem corresponder tipos por meio do padrão variável, mas, para os tipos de referência, no momento, nenhum conjunto de braços de switch na expressão switch é considerado concluído, exceto para correspondência com o tipo estático do argumento ou um subtipo.</span><span class="sxs-lookup"><span data-stu-id="03ceb-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="03ceb-137">As expressões switches seriam alteradas de modo que, se o tipo raiz de um `enum class` for o tipo estático do argumento para a expressão switch, e houver um conjunto de padrões correspondentes a todos os membros da enumeração, a opção será considerada exaustiva.</span><span class="sxs-lookup"><span data-stu-id="03ceb-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="03ceb-138">Como os membros de valor não são constantes e não definem novos tipos estáticos, eles atualmente não podem ser correspondidos por padrão.</span><span class="sxs-lookup"><span data-stu-id="03ceb-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="03ceb-139">Para tornar isso possível, um novo padrão usando a sintaxe de padrão constante será adicionado para permitir a correspondência em relação aos membros `enum class` valor.</span><span class="sxs-lookup"><span data-stu-id="03ceb-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="03ceb-140">A correspondência é definida para ser bem sucedido se e somente se o argumento para o padrão corresponder e o valor retornado pelo membro de `enum class` valor for igual ao mesmo, embora a implementação não seja necessária para executar essa verificação.</span><span class="sxs-lookup"><span data-stu-id="03ceb-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="03ceb-141">Perguntas abertas</span><span class="sxs-lookup"><span data-stu-id="03ceb-141">Open questions</span></span>

- <span data-ttu-id="03ceb-142">[] O que o algoritmo de tipo comum diz sobre `enum class` Membros?</span><span class="sxs-lookup"><span data-stu-id="03ceb-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="03ceb-143">Este código é válido?</span><span class="sxs-lookup"><span data-stu-id="03ceb-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="03ceb-144">[] A adição de um novo padrão apenas para membros de valor parece pesada.</span><span class="sxs-lookup"><span data-stu-id="03ceb-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="03ceb-145">Existe uma construção de versão mais geral que faz sentido?</span><span class="sxs-lookup"><span data-stu-id="03ceb-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="03ceb-146">[] Os membros de valor também não mapeiam bem para uma construção de classe aninhada paralela por causa disso</span><span class="sxs-lookup"><span data-stu-id="03ceb-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="03ceb-147">[] Está alternando para um argumento com um `enum class` tipo estático garantido como tempo constante?</span><span class="sxs-lookup"><span data-stu-id="03ceb-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="03ceb-148">[] Deve haver uma maneira de fazer `enum class`es não ser considerada concluída na expressão switch?</span><span class="sxs-lookup"><span data-stu-id="03ceb-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="03ceb-149">Prefixar com `virtual`?</span><span class="sxs-lookup"><span data-stu-id="03ceb-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="03ceb-150">[] Quais modificadores devem ser permitidos em `enum class`?</span><span class="sxs-lookup"><span data-stu-id="03ceb-150">[ ] What modifiers should be permitted on `enum class`?</span></span>