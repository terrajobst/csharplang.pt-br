---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485150"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="66628-101">Parâmetros de descarte de lambda</span><span class="sxs-lookup"><span data-stu-id="66628-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="66628-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="66628-102">Summary</span></span>

<span data-ttu-id="66628-103">Permitir Descartes (`_`) a serem usados como parâmetros de lambdas e métodos anônimos.</span><span class="sxs-lookup"><span data-stu-id="66628-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="66628-104">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="66628-104">For example:</span></span>
- <span data-ttu-id="66628-105">Lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="66628-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="66628-106">métodos anônimos: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="66628-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="66628-107">Motivação</span><span class="sxs-lookup"><span data-stu-id="66628-107">Motivation</span></span>

<span data-ttu-id="66628-108">Os parâmetros não utilizados não precisam ser nomeados.</span><span class="sxs-lookup"><span data-stu-id="66628-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="66628-109">A intenção de Descartes é clara, ou seja, não são usadas/descartadas.</span><span class="sxs-lookup"><span data-stu-id="66628-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="66628-110">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="66628-110">Detailed design</span></span>

<span data-ttu-id="66628-111">[Parâmetros do método](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) Na lista de parâmetros de um método lambda ou anônimo com mais de um parâmetro chamado `_`, esses parâmetros são parâmetros de descarte.</span><span class="sxs-lookup"><span data-stu-id="66628-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="66628-112">Observação: se um único parâmetro for nomeado `_`, ele será um parâmetro regular para motivos de compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="66628-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="66628-113">Os parâmetros de descarte não introduzem nenhum nome para nenhum escopo.</span><span class="sxs-lookup"><span data-stu-id="66628-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="66628-114">Observe que isso significa que eles não fazem com que os nomes de `_` (sublinhado) fiquem ocultos.</span><span class="sxs-lookup"><span data-stu-id="66628-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="66628-115">[Nomes simples](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Se `K` for zero e o *Simple_name* aparecer em um *bloco* e se o espaço da declaração local ([declarações](basic-concepts.md#declarations)) do *bloco*(ou um *bloco*delimitador) contiver uma variável local, um parâmetro (com exceção de descartar parâmetros) ou constante com o nome `I`, o *Simple_name* se refere a essa variável local, parâmetro ou constante e é classificado como uma variável ou valor.</span><span class="sxs-lookup"><span data-stu-id="66628-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="66628-116">[Escopos](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Com exceção dos parâmetros de descarte, o escopo de um parâmetro declarado em uma *lambda_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) é a *anonymous_function_body* do *lambda_expression* com a exceção de parâmetros de descarte, o escopo de um parâmetro declarado em uma *anonymous_method_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) é o *bloco* desse *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="66628-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="66628-117">Seções de especificações relacionadas</span><span class="sxs-lookup"><span data-stu-id="66628-117">Related spec sections</span></span>
- [<span data-ttu-id="66628-118">Parâmetros correspondentes</span><span class="sxs-lookup"><span data-stu-id="66628-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
