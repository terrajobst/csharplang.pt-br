---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484527"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="3c00d-101">Nullable-tipo comum aprimorado</span><span class="sxs-lookup"><span data-stu-id="3c00d-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="3c00d-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="3c00d-102">[x] Proposed</span></span>
* <span data-ttu-id="3c00d-103">[] Protótipo: nenhum</span><span class="sxs-lookup"><span data-stu-id="3c00d-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="3c00d-104">[] Implementação: nenhuma</span><span class="sxs-lookup"><span data-stu-id="3c00d-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="3c00d-105">[] Especificação: Veja abaixo</span><span class="sxs-lookup"><span data-stu-id="3c00d-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="3c00d-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="3c00d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3c00d-107">Há uma situação na qual os resultados atuais de algoritmos de tipo comum são de contador e resultam no programador que adiciona o que se parece com uma conversão redundante no código.</span><span class="sxs-lookup"><span data-stu-id="3c00d-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="3c00d-108">Com essa alteração, uma expressão como `condition ? 1 : null` resultaria em um valor do tipo `int?`, e uma expressão como `condition ? x : 1.0` em que `x` é do tipo `int?` resultaria em um valor do tipo `double?`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="3c00d-109">Motivação</span><span class="sxs-lookup"><span data-stu-id="3c00d-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3c00d-110">Essa é uma causa comum do que se sente ao programador, como código clichê indesejado.</span><span class="sxs-lookup"><span data-stu-id="3c00d-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3c00d-111">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="3c00d-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3c00d-112">Modificamos a especificação para [encontrar o melhor tipo comum de um conjunto de expressões](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) para afetar as seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="3c00d-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="3c00d-113">Se uma expressão for de um tipo de valor não anulável `T` e a outra for um literal nulo, o resultado será do tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="3c00d-114">Se uma expressão for de um tipo de valor anulável `T?` e a outra for de um tipo de valor `U`e houver uma conversão implícita de `T` para `U`, o resultado será do tipo `U?`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="3c00d-115">Espera-se que isso afete os seguintes aspectos do idioma:</span><span class="sxs-lookup"><span data-stu-id="3c00d-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="3c00d-116">a [expressão Ternário](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="3c00d-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="3c00d-117">[expressão de criação de matriz](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions) digitada implicitamente</span><span class="sxs-lookup"><span data-stu-id="3c00d-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="3c00d-118">inferindo o [tipo de retorno de um lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) para inferência de tipos</span><span class="sxs-lookup"><span data-stu-id="3c00d-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="3c00d-119">casos que envolvem genéricos, como invocar `M<T>(T a, T b)` como `M(1, null)`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="3c00d-120">Mais precisamente, alteramos as seguintes seções da especificação (inserções em negrito, exclusões em tachado):</span><span class="sxs-lookup"><span data-stu-id="3c00d-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="3c00d-121">Inferências de tipo de saída</span><span class="sxs-lookup"><span data-stu-id="3c00d-121">Output type inferences</span></span>
> 
> <span data-ttu-id="3c00d-122">Uma *inferência de tipo de saída* é feita *de* uma expressão `E` *para* um tipo `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3c00d-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="3c00d-123">Se `E` for uma função anônima com tipo de retorno inferido `U`[(tipo de retorno inferido](expressions.md#inferred-return-type)) e `T` for um tipo delegado ou tipo de árvore de expressão com tipo de retorno `Tb`, uma *inferência de limite inferior* ([inferências de limite inferior](expressions.md#lower-bound-inferences)) será feita *de* `U` *para* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="3c00d-124">Caso contrário, se `E` for um grupo de métodos e `T` for um tipo delegado ou tipo de árvore de expressão com tipos de parâmetro `T1...Tk` e tipo de retorno `Tb`, e a resolução de sobrecarga de `E` com os tipos `T1...Tk` gerar um único método com o tipo de retorno `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="3c00d-125">\* \* Caso contrário, se `E` for uma expressão com o tipo de valor anulável `U?`, uma *inferência de limite inferior* será feita *de* `U` *para* `T` e um *limite nulo* será adicionado ao `T`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="3c00d-126">Caso contrário, se `E` for uma expressão com o tipo `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `T`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="3c00d-127">**Caso contrário, se `E` for uma expressão constante com valor `null`, um *limite nulo* será adicionado a `T`**</span><span class="sxs-lookup"><span data-stu-id="3c00d-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="3c00d-128">Caso contrário, nenhuma inferência será feita.</span><span class="sxs-lookup"><span data-stu-id="3c00d-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="3c00d-129">Resolver</span><span class="sxs-lookup"><span data-stu-id="3c00d-129">Fixing</span></span>
> 
> <span data-ttu-id="3c00d-130">Uma variável de tipo não *fixa* `Xi` com um conjunto de limites é *fixa* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3c00d-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="3c00d-131">O conjunto de *tipos candidatos* `Uj` começa como o conjunto de todos os tipos no conjunto de limites para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="3c00d-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="3c00d-132">Em seguida, examinamos cada associado por `Xi` por sua vez: para cada `U` limite exato de `Xi` todos os tipos `Uj` que não são idênticos aos `U` são removidos do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="3c00d-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="3c00d-133">Para cada `U` limite inferior de `Xi` todos os tipos `Uj` para o qual *não* há uma conversão implícita de `U` são removidos do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="3c00d-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="3c00d-134">Para cada `U` limite superior de `Xi` todos os tipos `Uj` de onde *não* há uma conversão implícita para `U` são removidos do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="3c00d-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="3c00d-135">Se entre os tipos candidatos restantes `Uj` houver um tipo exclusivo `V` do qual há uma conversão implícita para todos os outros tipos candidatos, ~~`Xi` será corrigido para `V`.~~</span><span class="sxs-lookup"><span data-stu-id="3c00d-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="3c00d-136">**Se `V` for um tipo de valor e houver um *limite nulo* para `Xi`, `Xi` será corrigido para `V?`**</span><span class="sxs-lookup"><span data-stu-id="3c00d-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="3c00d-137">**Caso contrário `Xi` é corrigido para `V`**</span><span class="sxs-lookup"><span data-stu-id="3c00d-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="3c00d-138">Caso contrário, a inferência de tipos falhará.</span><span class="sxs-lookup"><span data-stu-id="3c00d-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3c00d-139">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="3c00d-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3c00d-140">Pode haver algumas incompatibilidades introduzidas por essa proposta.</span><span class="sxs-lookup"><span data-stu-id="3c00d-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3c00d-141">Alternativas</span><span class="sxs-lookup"><span data-stu-id="3c00d-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3c00d-142">None.</span><span class="sxs-lookup"><span data-stu-id="3c00d-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3c00d-143">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="3c00d-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="3c00d-144">[] Qual é a gravidade da incompatibilidade introduzida por esta proposta, se houver, e como ela pode ser moderada?</span><span class="sxs-lookup"><span data-stu-id="3c00d-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3c00d-145">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="3c00d-145">Design meetings</span></span>

<span data-ttu-id="3c00d-146">None.</span><span class="sxs-lookup"><span data-stu-id="3c00d-146">None.</span></span>
