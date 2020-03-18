---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485080"
---
# <a name="static-lambdas"></a><span data-ttu-id="20c80-101">Lambdas estáticos</span><span class="sxs-lookup"><span data-stu-id="20c80-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="20c80-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="20c80-102">Summary</span></span>

<span data-ttu-id="20c80-103">Suporte a lambdas que não permitem capturar o estado do escopo de delimitação.</span><span class="sxs-lookup"><span data-stu-id="20c80-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="20c80-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="20c80-104">Motivation</span></span>

<span data-ttu-id="20c80-105">Evite capturar de forma não intencional o estado do contexto delimitador.</span><span class="sxs-lookup"><span data-stu-id="20c80-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="20c80-106">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="20c80-106">Detailed design</span></span>

<span data-ttu-id="20c80-107">Um lambdas com `static` não pode capturar o estado do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="20c80-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="20c80-108">Como resultado, os locais, os parâmetros e os `this` do escopo delimitador não estão disponíveis em um lambda `static`.</span><span class="sxs-lookup"><span data-stu-id="20c80-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="20c80-109">Um `static` lambda não pode referenciar membros de instância de uma referência implícita ou explícita de `this` ou `base`.</span><span class="sxs-lookup"><span data-stu-id="20c80-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="20c80-110">Um `static` lambda pode fazer referência `static` membros do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="20c80-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="20c80-111">Um `static` lambda pode referenciar definições de `constant` do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="20c80-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="20c80-112">`nameof()` em um `static` lambda pode referenciar locais, parâmetros ou `this` ou `base` do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="20c80-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="20c80-113">As regras de acessibilidade para membros de `private` no escopo de delimitação são as mesmas para lambdas `static` e não`static`.</span><span class="sxs-lookup"><span data-stu-id="20c80-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="20c80-114">Nenhuma garantia é feita como se uma definição de lambda de `static` é emitida como um método de `static` nos metadados.</span><span class="sxs-lookup"><span data-stu-id="20c80-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="20c80-115">Isso é deixado para a implementação do compilador para otimizar.</span><span class="sxs-lookup"><span data-stu-id="20c80-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="20c80-116">Uma função local ou lambda não`static` pode capturar o estado de um `static` lambda delimitador, mas não pode capturar o estado fora do `static` lambda delimitador.</span><span class="sxs-lookup"><span data-stu-id="20c80-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="20c80-117">Um `static` lambda pode ser usado em uma árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="20c80-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="20c80-118">Remover o modificador de `static` de um lambda em um programa válido não altera o significado do programa.</span><span class="sxs-lookup"><span data-stu-id="20c80-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
