---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484982"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="ddcb9-101">Literal "padrão" de tipo de destino</span><span class="sxs-lookup"><span data-stu-id="ddcb9-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="ddcb9-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="ddcb9-102">[x] Proposed</span></span>
* <span data-ttu-id="ddcb9-103">[x] protótipo</span><span class="sxs-lookup"><span data-stu-id="ddcb9-103">[x] Prototype</span></span>
* <span data-ttu-id="ddcb9-104">[x] implementação</span><span class="sxs-lookup"><span data-stu-id="ddcb9-104">[x] Implementation</span></span>
* <span data-ttu-id="ddcb9-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="ddcb9-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="ddcb9-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="ddcb9-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ddcb9-107">O recurso de `default` de tipo de destino é uma variação de forma mais curta do operador `default(T)`, que permite que o tipo seja omitido.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="ddcb9-108">Seu tipo é inferido por digitação de destino em vez disso.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="ddcb9-109">Além disso, ele se comporta como `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ddcb9-110">Motivação</span><span class="sxs-lookup"><span data-stu-id="ddcb9-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ddcb9-111">A principal motivação é evitar digitar informações redundantes.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="ddcb9-112">Por exemplo, ao invocar `void Method(ImmutableArray<SomeType> array)`, o literal *padrão* permite `M(default)` no lugar de `M(default(ImmutableArray<SomeType>))`.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="ddcb9-113">Isso é aplicável em vários cenários, como:</span><span class="sxs-lookup"><span data-stu-id="ddcb9-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="ddcb9-114">declarando locais (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="ddcb9-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="ddcb9-115">operações ternários (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="ddcb9-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="ddcb9-116">retornando em métodos e lambdas (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="ddcb9-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="ddcb9-117">declarando valores padrão para parâmetros opcionais (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="ddcb9-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="ddcb9-118">incluindo valores padrão em expressões de criação de matriz (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="ddcb9-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="ddcb9-119">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="ddcb9-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ddcb9-120">Uma nova expressão é introduzida, a literal *padrão* .</span><span class="sxs-lookup"><span data-stu-id="ddcb9-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="ddcb9-121">Uma expressão com essa classificação pode ser convertida implicitamente em qualquer tipo, por uma *conversão literal padrão*.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="ddcb9-122">A inferência do tipo para o literal *padrão* funciona da mesma forma que para o literal *nulo* , exceto que qualquer tipo é permitido (não apenas tipos de referência).</span><span class="sxs-lookup"><span data-stu-id="ddcb9-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="ddcb9-123">Essa conversão produz o valor padrão do tipo inferido.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="ddcb9-124">O literal *padrão* pode ter um valor constante, dependendo do tipo inferido.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="ddcb9-125">Portanto, `const int x = default;` é legal, mas `const int? y = default;` não é.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="ddcb9-126">O literal *padrão* pode ser o operando de operadores de igualdade, desde que o outro operando tenha um tipo.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="ddcb9-127">Portanto, `default == x` e `x == default` são expressões válidas, mas `default == default` é ilegal.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ddcb9-128">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="ddcb9-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ddcb9-129">Uma desvantagem secundária é que o literal *padrão* pode ser usado no lugar do literal *nulo* na maioria dos contextos.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="ddcb9-130">Duas das exceções são `throw null;` e `null == null`, que são permitidas para o literal *nulo* , mas não para o literal *padrão* .</span><span class="sxs-lookup"><span data-stu-id="ddcb9-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ddcb9-131">Alternativas</span><span class="sxs-lookup"><span data-stu-id="ddcb9-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ddcb9-132">Há algumas alternativas a serem consideradas:</span><span class="sxs-lookup"><span data-stu-id="ddcb9-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="ddcb9-133">O status quo: o recurso não é justificado em seus próprios méritos e os desenvolvedores continuam a usar o operador padrão com um tipo explícito.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="ddcb9-134">Estendendo o literal nulo: essa é a abordagem do VB com `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="ddcb9-135">Poderíamos permitir `int x = null;`.</span><span class="sxs-lookup"><span data-stu-id="ddcb9-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ddcb9-136">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="ddcb9-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ddcb9-137">[x] o *padrão* deve ser permitido como o operando dos *is* operadores is *ou as?*</span><span class="sxs-lookup"><span data-stu-id="ddcb9-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="ddcb9-138">Resposta: não permitir `default is T`, permitir `x is default`, permitir `default as RefType` (com aviso sempre nulo)</span><span class="sxs-lookup"><span data-stu-id="ddcb9-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ddcb9-139">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="ddcb9-139">Design meetings</span></span>

- [<span data-ttu-id="ddcb9-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="ddcb9-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="ddcb9-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="ddcb9-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="ddcb9-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="ddcb9-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
