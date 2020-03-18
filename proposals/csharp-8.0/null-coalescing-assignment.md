---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484954"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="83078-101">Atribuição de coalescência nula</span><span class="sxs-lookup"><span data-stu-id="83078-101">null coalescing assignment</span></span>

* <span data-ttu-id="83078-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="83078-102">[x] Proposed</span></span>
* <span data-ttu-id="83078-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="83078-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="83078-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="83078-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="83078-105">[] Especificação: abaixo</span><span class="sxs-lookup"><span data-stu-id="83078-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="83078-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="83078-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="83078-107">Simplifica um padrão de codificação comum em que uma variável é atribuída a um valor se ela for nula.</span><span class="sxs-lookup"><span data-stu-id="83078-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="83078-108">Como parte dessa proposta, nós também soltaremos os requisitos de tipo em `??` para permitir uma expressão cujo tipo é um parâmetro de tipo irrestrito a ser usado no lado esquerdo.</span><span class="sxs-lookup"><span data-stu-id="83078-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="83078-109">Motivação</span><span class="sxs-lookup"><span data-stu-id="83078-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="83078-110">É comum ver o código do formulário</span><span class="sxs-lookup"><span data-stu-id="83078-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="83078-111">Essa proposta adiciona um operador binário não sobrecarregável à linguagem que executa essa função.</span><span class="sxs-lookup"><span data-stu-id="83078-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="83078-112">Houve pelo menos oito solicitações de comunidade separadas para esse recurso.</span><span class="sxs-lookup"><span data-stu-id="83078-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="83078-113">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="83078-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="83078-114">Adicionamos um novo formulário de operador de atribuição</span><span class="sxs-lookup"><span data-stu-id="83078-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="83078-115">Que segue as [regras semânticas existentes para operadores de atribuição compostos](../../spec/expressions.md#compound-assignment), exceto que elide a atribuição se o lado esquerdo for não nulo.</span><span class="sxs-lookup"><span data-stu-id="83078-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="83078-116">As regras para esse recurso são as seguintes.</span><span class="sxs-lookup"><span data-stu-id="83078-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="83078-117">Dado `a ??= b`, em que `A` é o tipo de `a`, `B` é o tipo de `b`e `A0` é o tipo subjacente de `A` se `A` for um tipo de valor anulável:</span><span class="sxs-lookup"><span data-stu-id="83078-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="83078-118">Se `A` não existir ou for um tipo de valor não anulável, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="83078-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="83078-119">Se `B` não for implicitamente conversível para `A` ou `A0` (se `A0` existir), ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="83078-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="83078-120">Se `A0` existir e `B` for implicitamente conversível para `A0`e `B` não for dinâmico, o tipo de `a ??= b` será `A0`.</span><span class="sxs-lookup"><span data-stu-id="83078-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="83078-121">`a ??= b` é avaliado em tempo de execução como:</span><span class="sxs-lookup"><span data-stu-id="83078-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="83078-122">Exceto que `a` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="83078-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="83078-123">Caso contrário, o tipo de `a ??= b` é `A`.</span><span class="sxs-lookup"><span data-stu-id="83078-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="83078-124">`a ??= b` é avaliada em tempo de execução como `a ?? (a = b)`, exceto que `a` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="83078-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="83078-125">Para o relaxamento dos requisitos de tipo do `??`, atualizamos a especificação em que ele declara atualmente, dado `a ?? b`, em que `A` é o tipo de `a`:</span><span class="sxs-lookup"><span data-stu-id="83078-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="83078-126">Se existir e não for um tipo anulável ou um tipo de referência, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="83078-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="83078-127">Nós liberamos esse requisito para:</span><span class="sxs-lookup"><span data-stu-id="83078-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="83078-128">Se existir e for um tipo de valor não anulável, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="83078-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="83078-129">Isso permite que o operador de União nulo funcione em parâmetros de tipo irrestrito, pois o parâmetro de tipo não restrito T existe, não é um tipo anulável e não é um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="83078-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="83078-130">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="83078-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="83078-131">Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.</span><span class="sxs-lookup"><span data-stu-id="83078-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="83078-132">Alternativas</span><span class="sxs-lookup"><span data-stu-id="83078-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="83078-133">O programador pode gravar `(x = x ?? y)`, `if (x == null) x = y;`ou `x ?? (x = y)` manualmente.</span><span class="sxs-lookup"><span data-stu-id="83078-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="83078-134">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="83078-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="83078-135">[] Requer revisão LDM</span><span class="sxs-lookup"><span data-stu-id="83078-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="83078-136">[] Também há suporte para operadores de `&&=` e `||=`?</span><span class="sxs-lookup"><span data-stu-id="83078-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="83078-137">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="83078-137">Design meetings</span></span>

<span data-ttu-id="83078-138">None.</span><span class="sxs-lookup"><span data-stu-id="83078-138">None.</span></span>
