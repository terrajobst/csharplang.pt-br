---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509503"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="7a1a4-101">Atributos em funções locais</span><span class="sxs-lookup"><span data-stu-id="7a1a4-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="7a1a4-102">Atributos</span><span class="sxs-lookup"><span data-stu-id="7a1a4-102">Attributes</span></span>

<span data-ttu-id="7a1a4-103">Agora, as declarações de função local têm permissão para ter [atributos](../spec/attributes.md).</span><span class="sxs-lookup"><span data-stu-id="7a1a4-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="7a1a4-104">Parâmetros e parâmetros de tipo em funções locais também podem ter atributos.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="7a1a4-105">Atributos com um significado especificado quando aplicados a um método, seus parâmetros ou seus parâmetros de tipo terão o mesmo significado quando aplicados a uma função local, seus parâmetros ou seus parâmetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="7a1a4-106">Uma função local pode se tornar condicional no mesmo sentido que um [método condicional](../spec/attributes.md#the-conditional-attribute) , decorando-a com um `[ConditionalAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="7a1a4-107">Uma função local condicional também deve ser `static`.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="7a1a4-108">Todas as restrições em métodos condicionais também se aplicam a funções locais condicionais, incluindo que o tipo de retorno deve ser `void`.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="7a1a4-109">Externo</span><span class="sxs-lookup"><span data-stu-id="7a1a4-109">Extern</span></span>

<span data-ttu-id="7a1a4-110">O modificador de `extern` agora é permitido em funções locais.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="7a1a4-111">Isso torna a função local externa no mesmo sentido que um [método externo](../spec/classes.md#external-methods).</span><span class="sxs-lookup"><span data-stu-id="7a1a4-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="7a1a4-112">Da mesma forma que um método externo, o *corpo da função local* de uma função local externa deve ser um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="7a1a4-113">Um *corpo de função local* de ponto e vírgula só é permitido em uma função local externa.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="7a1a4-114">Uma função local externa também deve ser `static`.</span><span class="sxs-lookup"><span data-stu-id="7a1a4-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="7a1a4-115">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="7a1a4-115">Syntax</span></span>

<span data-ttu-id="7a1a4-116">A [gramática de funções locais](csharp-7.0/local-functions.md#syntax-grammar) é modificada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="7a1a4-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
