---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484702"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="b61d7-101">correspondência de padrões com genéricos</span><span class="sxs-lookup"><span data-stu-id="b61d7-101">pattern-matching with generics</span></span>

* <span data-ttu-id="b61d7-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="b61d7-102">[x] Proposed</span></span>
* <span data-ttu-id="b61d7-103">[] Protótipo:</span><span class="sxs-lookup"><span data-stu-id="b61d7-103">[ ] Prototype:</span></span>
* <span data-ttu-id="b61d7-104">[] Implementação:</span><span class="sxs-lookup"><span data-stu-id="b61d7-104">[ ] Implementation:</span></span>
* <span data-ttu-id="b61d7-105">[] Especificação:</span><span class="sxs-lookup"><span data-stu-id="b61d7-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="b61d7-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="b61d7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b61d7-107">A especificação para o [operador C# as existente](../../spec/expressions.md#the-as-operator) permite que não haja nenhuma conversão entre o tipo do operando e o tipo especificado quando um é um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="b61d7-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="b61d7-108">No entanto C# , no 7, o padrão de `Type identifier` requer que haja uma conversão entre o tipo de entrada e o tipo fornecido.</span><span class="sxs-lookup"><span data-stu-id="b61d7-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="b61d7-109">Sugerimos relaxar isso e alterar `expression is Type identifier`, além de serem permitidas nas condições, quando permitido em C# 7, também serão permitidas quando `expression as Type` seriam permitidas.</span><span class="sxs-lookup"><span data-stu-id="b61d7-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="b61d7-110">Especificamente, os novos casos são casos em que o tipo da expressão ou o tipo especificado é um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="b61d7-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="b61d7-111">Motivação</span><span class="sxs-lookup"><span data-stu-id="b61d7-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b61d7-112">Casos em que a correspondência de padrões deve ser permitida no momento com falha na compilação.</span><span class="sxs-lookup"><span data-stu-id="b61d7-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="b61d7-113">Consulte, por exemplo, https://github.com/dotnet/roslyn/issues/16195.</span><span class="sxs-lookup"><span data-stu-id="b61d7-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b61d7-114">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="b61d7-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b61d7-115">Alteramos o parágrafo na especificação de correspondência de padrões (a adição proposta é mostrada em negrito):</span><span class="sxs-lookup"><span data-stu-id="b61d7-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="b61d7-116">Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="b61d7-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="b61d7-117">Um valor de tipo estático `E` é chamado de *padrão compatível* com o tipo `T` se houver uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão unboxing de `E` para `T`**ou se `E` ou `T` for um tipo aberto**.</span><span class="sxs-lookup"><span data-stu-id="b61d7-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="b61d7-118">É um erro de tempo de compilação se uma expressão do tipo `E` não for compatível com o tipo em um padrão de tipo com o qual ele é correspondido.</span><span class="sxs-lookup"><span data-stu-id="b61d7-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b61d7-119">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="b61d7-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b61d7-120">None.</span><span class="sxs-lookup"><span data-stu-id="b61d7-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b61d7-121">Alternativas</span><span class="sxs-lookup"><span data-stu-id="b61d7-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b61d7-122">None.</span><span class="sxs-lookup"><span data-stu-id="b61d7-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b61d7-123">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="b61d7-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="b61d7-124">None.</span><span class="sxs-lookup"><span data-stu-id="b61d7-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b61d7-125">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="b61d7-125">Design meetings</span></span>

<span data-ttu-id="b61d7-126">O LDM considerou essa pergunta e sentiu que era uma alteração no nível de correção de bug.</span><span class="sxs-lookup"><span data-stu-id="b61d7-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="b61d7-127">Estamos tratando-o como um recurso de idioma separado porque apenas fazer a alteração depois que o idioma foi liberado introduzirá uma incompatibilidade posterior.</span><span class="sxs-lookup"><span data-stu-id="b61d7-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="b61d7-128">O uso da alteração proposta requer que o programador especifique a versão de idioma 7,1.</span><span class="sxs-lookup"><span data-stu-id="b61d7-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
