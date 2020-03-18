---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484492"
---
# <a name="null-conditional-await"></a><span data-ttu-id="bbae1-101">NULL-condicional Await</span><span class="sxs-lookup"><span data-stu-id="bbae1-101">null-conditional await</span></span>

* <span data-ttu-id="bbae1-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="bbae1-102">[x] Proposed</span></span>
* <span data-ttu-id="bbae1-103">[] Protótipo: nenhum</span><span class="sxs-lookup"><span data-stu-id="bbae1-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="bbae1-104">[] Implementação: nenhuma</span><span class="sxs-lookup"><span data-stu-id="bbae1-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="bbae1-105">[] Especificação: iniciada, abaixo</span><span class="sxs-lookup"><span data-stu-id="bbae1-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="bbae1-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="bbae1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="bbae1-107">Dá suporte a uma expressão do formulário `await? e`, que aguarda `e` se ele não for nulo, caso contrário, ele resultará em `null`.</span><span class="sxs-lookup"><span data-stu-id="bbae1-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="bbae1-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="bbae1-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="bbae1-109">Esse é um padrão de codificação comum, e esse recurso teria uma boa sinergia com os operadores existentes de propagação de nulos e de União nula.</span><span class="sxs-lookup"><span data-stu-id="bbae1-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="bbae1-110">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="bbae1-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="bbae1-111">Adicionamos um novo formulário da *await_expression*:</span><span class="sxs-lookup"><span data-stu-id="bbae1-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="bbae1-112">O operador de `await` condicional nulo aguardará seu operando somente se esse operando for não nulo.</span><span class="sxs-lookup"><span data-stu-id="bbae1-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="bbae1-113">Caso contrário, o resultado da aplicação do operador é NULL.</span><span class="sxs-lookup"><span data-stu-id="bbae1-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="bbae1-114">O tipo do resultado é computado usando as [regras para o operador NULL-Conditional](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span><span class="sxs-lookup"><span data-stu-id="bbae1-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="bbae1-115">**Observação:** Se `e` for do tipo `Task`, `await? e;` não faria nada se `e` for `null`e Await `e` se não for `null`.</span><span class="sxs-lookup"><span data-stu-id="bbae1-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="bbae1-116">Se `e` for do tipo `Task<K>` em que `K` é um tipo de valor, `await? e` resultaria em um valor do tipo `K?`.</span><span class="sxs-lookup"><span data-stu-id="bbae1-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="bbae1-117">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="bbae1-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="bbae1-118">Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.</span><span class="sxs-lookup"><span data-stu-id="bbae1-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="bbae1-119">Alternativas</span><span class="sxs-lookup"><span data-stu-id="bbae1-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="bbae1-120">Embora exija algum código clichê, os usos desse operador geralmente podem ser substituídos por uma expressão algo como `(e == null) ? null : await e` ou uma instrução como `if (e != null) await e`.</span><span class="sxs-lookup"><span data-stu-id="bbae1-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="bbae1-121">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="bbae1-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="bbae1-122">[] Requer revisão LDM</span><span class="sxs-lookup"><span data-stu-id="bbae1-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="bbae1-123">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="bbae1-123">Design meetings</span></span>

<span data-ttu-id="bbae1-124">None.</span><span class="sxs-lookup"><span data-stu-id="bbae1-124">None.</span></span>
