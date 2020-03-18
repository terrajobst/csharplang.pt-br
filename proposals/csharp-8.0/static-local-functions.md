---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485094"
---
# <a name="static-local-functions"></a><span data-ttu-id="9734b-101">Funções locais estáticas</span><span class="sxs-lookup"><span data-stu-id="9734b-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="9734b-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="9734b-102">Summary</span></span>

<span data-ttu-id="9734b-103">Suporte a funções locais que não permitem capturar o estado do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="9734b-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="9734b-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="9734b-104">Motivation</span></span>

<span data-ttu-id="9734b-105">Evite capturar de forma não intencional o estado do contexto delimitador.</span><span class="sxs-lookup"><span data-stu-id="9734b-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="9734b-106">Permita que as funções locais sejam usadas em cenários em que um método de `static` é necessário.</span><span class="sxs-lookup"><span data-stu-id="9734b-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9734b-107">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="9734b-107">Detailed design</span></span>

<span data-ttu-id="9734b-108">Uma função local declarada `static` não pode capturar o estado do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="9734b-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="9734b-109">Como resultado, os locais, os parâmetros e os `this` do escopo delimitador não estão disponíveis em uma função local `static`.</span><span class="sxs-lookup"><span data-stu-id="9734b-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="9734b-110">Uma `static` função local não pode referenciar membros de instância de uma referência implícita ou explícita `this` ou `base`.</span><span class="sxs-lookup"><span data-stu-id="9734b-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="9734b-111">Uma `static` função local pode referenciar `static` membros do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="9734b-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="9734b-112">Uma `static` função local pode referenciar `constant` definições do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="9734b-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="9734b-113">`nameof()` em uma função local `static` pode referenciar locais, parâmetros ou `this` ou `base` do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="9734b-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="9734b-114">As regras de acessibilidade para membros de `private` no escopo delimitador são as mesmas para funções locais `static` e não`static`.</span><span class="sxs-lookup"><span data-stu-id="9734b-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="9734b-115">Uma definição de função local `static` é emitida como um método `static` nos metadados, mesmo que seja usada apenas em um delegado.</span><span class="sxs-lookup"><span data-stu-id="9734b-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="9734b-116">Uma função local ou lambda não`static` pode capturar o estado de uma função local de `static` delimitadora, mas não pode capturar o estado fora da função local `static` de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="9734b-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="9734b-117">Uma função local `static` não pode ser invocada em uma árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="9734b-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="9734b-118">Uma chamada para uma função local é emitida como `call` em vez de `callvirt`, independentemente de a função local ser `static`.</span><span class="sxs-lookup"><span data-stu-id="9734b-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="9734b-119">A resolução de sobrecarga de uma chamada em uma função local não afetada pelo fato de a função local ser `static`.</span><span class="sxs-lookup"><span data-stu-id="9734b-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="9734b-120">Remover o modificador de `static` de uma função local em um programa válido não altera o significado do programa.</span><span class="sxs-lookup"><span data-stu-id="9734b-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="9734b-121">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="9734b-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
