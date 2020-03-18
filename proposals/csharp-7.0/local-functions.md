---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484436"
---
# <a name="local-functions"></a><span data-ttu-id="ab274-101">Funções locais</span><span class="sxs-lookup"><span data-stu-id="ab274-101">Local functions</span></span>

<span data-ttu-id="ab274-102">Estendemos C# para dar suporte à declaração de funções no escopo de bloco.</span><span class="sxs-lookup"><span data-stu-id="ab274-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="ab274-103">As funções locais podem usar variáveis (capturar) do escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="ab274-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="ab274-104">O compilador usa a análise de fluxo para detectar quais variáveis uma função local usa antes de atribuir um valor a ela.</span><span class="sxs-lookup"><span data-stu-id="ab274-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="ab274-105">Cada chamada da função requer que essas variáveis sejam definitivamente atribuídas.</span><span class="sxs-lookup"><span data-stu-id="ab274-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="ab274-106">Da mesma forma, o compilador determina quais variáveis são definitivamente atribuídas no retorno.</span><span class="sxs-lookup"><span data-stu-id="ab274-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="ab274-107">Essas variáveis são consideradas definitivamente atribuídas depois que a função local é invocada.</span><span class="sxs-lookup"><span data-stu-id="ab274-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="ab274-108">As funções locais podem ser chamadas de um ponto léxico antes de sua definição.</span><span class="sxs-lookup"><span data-stu-id="ab274-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="ab274-109">As instruções de declaração de função local não causam um aviso quando não estão acessíveis.</span><span class="sxs-lookup"><span data-stu-id="ab274-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="ab274-110">TODO: _criar especificação_</span><span class="sxs-lookup"><span data-stu-id="ab274-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="ab274-111">Gramática de sintaxe</span><span class="sxs-lookup"><span data-stu-id="ab274-111">Syntax grammar</span></span>

<span data-ttu-id="ab274-112">Essa gramática é representada como uma comparação da gramática de especificações atual.</span><span class="sxs-lookup"><span data-stu-id="ab274-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="ab274-113">As funções locais podem usar variáveis definidas no escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="ab274-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="ab274-114">A implementação atual requer que todas as variáveis lidas dentro de uma função local sejam definitivamente atribuídas, como se estiver executando a função local em seu ponto de definição.</span><span class="sxs-lookup"><span data-stu-id="ab274-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="ab274-115">Além disso, a definição da função local deve ter sido "executada" em qualquer ponto de uso.</span><span class="sxs-lookup"><span data-stu-id="ab274-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="ab274-116">Depois de experimentar esse bit (por exemplo, não é possível definir duas funções locais recursivas mutuamente), já que revisamos como queremos que a atribuição definitiva funcione.</span><span class="sxs-lookup"><span data-stu-id="ab274-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="ab274-117">A revisão (ainda não implementada) é que todas as variáveis locais lidas em uma função local devem ser definitivamente atribuídas em cada invocação da função local.</span><span class="sxs-lookup"><span data-stu-id="ab274-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="ab274-118">Isso é, na verdade, mais sutil do que parece, e há um monte de trabalho restante para fazê-lo funcionar.</span><span class="sxs-lookup"><span data-stu-id="ab274-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="ab274-119">Depois de fazer isso, você poderá mover suas funções locais para o final de seu bloco delimitador.</span><span class="sxs-lookup"><span data-stu-id="ab274-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="ab274-120">As novas regras de atribuição definitivas são incompatíveis com a inferência do tipo de retorno de uma função local, portanto, provavelmente, vamos remover o suporte para inferir o tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="ab274-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="ab274-121">A menos que você converta uma função local em um delegado, a captura é feita em quadros que são tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="ab274-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="ab274-122">Isso significa que você não obtém nenhuma pressão de GC do uso de funções locais com a captura.</span><span class="sxs-lookup"><span data-stu-id="ab274-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="ab274-123">Acessibilidade</span><span class="sxs-lookup"><span data-stu-id="ab274-123">Reachability</span></span>

<span data-ttu-id="ab274-124">Adicionamos à especificação</span><span class="sxs-lookup"><span data-stu-id="ab274-124">We add to the spec</span></span>

> <span data-ttu-id="ab274-125">O corpo de uma expressão lambda apto para de instrução ou função local é considerado acessível.</span><span class="sxs-lookup"><span data-stu-id="ab274-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
