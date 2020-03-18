---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484919"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="4d2ba-101">Variáveis de expressão em inicializadores</span><span class="sxs-lookup"><span data-stu-id="4d2ba-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="4d2ba-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="4d2ba-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4d2ba-103">Estendemos os recursos introduzidos em C# 7 para permitir expressões que contêm variáveis de expressão (saída de declarações de variável e padrões de declaração) em inicializadores de campo, inicializadores de propriedade, Construtor-inicializadores e cláusulas de consulta.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="4d2ba-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="4d2ba-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4d2ba-105">Isso conclui algumas das bordas aproximadas restantes no C# idioma devido à falta de tempo.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4d2ba-106">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="4d2ba-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="4d2ba-107">Removemos a restrição que impede a declaração de variáveis de expressão (saída de declarações de variável e padrões de declaração) em um inicializador de construtor.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="4d2ba-108">Tal variável declarada está no escopo ao longo do corpo do construtor.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="4d2ba-109">Removemos a restrição que impede a declaração de variáveis de expressão (saída de declarações de variável e padrões de declaração) em um inicializador de campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="4d2ba-110">Essa variável declarada está no escopo durante a expressão de inicialização.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="4d2ba-111">Removemos a restrição que impede a declaração de variáveis de expressão (saída de declarações de variável e padrões de declaração) em uma cláusula de expressão de consulta que é convertida no corpo de um lambda.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="4d2ba-112">Essa variável declarada está no escopo por toda a expressão da cláusula de consulta.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="4d2ba-113">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="4d2ba-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="4d2ba-114">None.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4d2ba-115">Alternativas</span><span class="sxs-lookup"><span data-stu-id="4d2ba-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4d2ba-116">O escopo apropriado para variáveis de expressão declaradas nesses contextos não é óbvio e merece mais discussão LDM.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="4d2ba-117">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="4d2ba-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="4d2ba-118">[] Qual é o escopo apropriado para essas variáveis?</span><span class="sxs-lookup"><span data-stu-id="4d2ba-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="4d2ba-119">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="4d2ba-119">Design meetings</span></span>

<span data-ttu-id="4d2ba-120">None.</span><span class="sxs-lookup"><span data-stu-id="4d2ba-120">None.</span></span>
