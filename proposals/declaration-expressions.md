---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484576"
---
# <a name="declaration-expressions"></a><span data-ttu-id="b778d-101">Expressões de declaração</span><span class="sxs-lookup"><span data-stu-id="b778d-101">Declaration expressions</span></span>

<span data-ttu-id="b778d-102">Suporte a atribuições de declaração como expressões.</span><span class="sxs-lookup"><span data-stu-id="b778d-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="b778d-103">Motivação</span><span class="sxs-lookup"><span data-stu-id="b778d-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b778d-104">Permita a inicialização no ponto de declaração em mais casos, simplificando o código e permitindo que `var` seja usado.</span><span class="sxs-lookup"><span data-stu-id="b778d-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="b778d-105">Permitir declarações para argumentos de `ref`, semelhante a `out var`.</span><span class="sxs-lookup"><span data-stu-id="b778d-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="b778d-106">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="b778d-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b778d-107">As expressões são estendidas para incluir a atribuição de declaração.</span><span class="sxs-lookup"><span data-stu-id="b778d-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="b778d-108">A precedência é igual à atribuição.</span><span class="sxs-lookup"><span data-stu-id="b778d-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="b778d-109">A atribuição de declaração é de um único local.</span><span class="sxs-lookup"><span data-stu-id="b778d-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="b778d-110">O tipo de uma expressão de atribuição de declaração é o tipo da declaração.</span><span class="sxs-lookup"><span data-stu-id="b778d-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="b778d-111">Se o tipo for `var`, o tipo inferido será o tipo da expressão de inicialização.</span><span class="sxs-lookup"><span data-stu-id="b778d-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="b778d-112">A expressão de atribuição de declaração pode ser um valor l, para `ref` valores de argumento em particular.</span><span class="sxs-lookup"><span data-stu-id="b778d-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="b778d-113">Se a expressão de atribuição de declaração declarar um tipo de valor e a expressão for um valor de r, o valor da expressão será uma cópia.</span><span class="sxs-lookup"><span data-stu-id="b778d-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="b778d-114">A expressão de atribuição de declaração pode declarar um `ref` local.</span><span class="sxs-lookup"><span data-stu-id="b778d-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="b778d-115">Há uma ambiguidade quando `ref` é usada para uma expressão de declaração em um argumento `ref`.</span><span class="sxs-lookup"><span data-stu-id="b778d-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="b778d-116">O inicializador da variável local determina se a declaração é um `ref` local.</span><span class="sxs-lookup"><span data-stu-id="b778d-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="b778d-117">O escopo de locais declarados em expressões de atribuição de declaração é o mesmo escopo das expressões de declaração correspondentes do C # 7.0.</span><span class="sxs-lookup"><span data-stu-id="b778d-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="b778d-118">É um erro de tempo de compilação para fazer referência a um local no texto que precede a expressão de declaração.</span><span class="sxs-lookup"><span data-stu-id="b778d-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b778d-119">Alternativas</span><span class="sxs-lookup"><span data-stu-id="b778d-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="b778d-120">Nenhuma alteração.</span><span class="sxs-lookup"><span data-stu-id="b778d-120">No change.</span></span> <span data-ttu-id="b778d-121">Esse recurso é apenas uma abreviação sintática depois de tudo.</span><span class="sxs-lookup"><span data-stu-id="b778d-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="b778d-122">Expressões de sequência mais gerais: consulte [#377](https://github.com/dotnet/csharplang/issues/377).</span><span class="sxs-lookup"><span data-stu-id="b778d-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="b778d-123">Para permitir o uso de `var` em mais casos, permita uma declaração e atribuição separada de `var` locais e inferir o tipo de atribuições de todos os caminhos de código.</span><span class="sxs-lookup"><span data-stu-id="b778d-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="b778d-124">Consulte também</span><span class="sxs-lookup"><span data-stu-id="b778d-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="b778d-125">Consulte expressão de declaração básica no [#595](https://github.com/dotnet/csharplang/issues/595).</span><span class="sxs-lookup"><span data-stu-id="b778d-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="b778d-126">Consulte declaração de desconstrução no recurso de [desconstrução](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .</span><span class="sxs-lookup"><span data-stu-id="b778d-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
