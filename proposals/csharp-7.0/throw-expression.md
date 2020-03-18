---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484541"
---
# <a name="throw-expression"></a><span data-ttu-id="ef65f-101">Expressão throw</span><span class="sxs-lookup"><span data-stu-id="ef65f-101">Throw expression</span></span>

<span data-ttu-id="ef65f-102">Estendemos o conjunto de formulários de expressões para incluir</span><span class="sxs-lookup"><span data-stu-id="ef65f-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="ef65f-103">As regras de tipo são as seguintes:</span><span class="sxs-lookup"><span data-stu-id="ef65f-103">The type rules are as follows:</span></span>

- <span data-ttu-id="ef65f-104">Um *throw_expression* não tem nenhum tipo.</span><span class="sxs-lookup"><span data-stu-id="ef65f-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="ef65f-105">Uma *throw_expression* é conversível para cada tipo por uma conversão implícita.</span><span class="sxs-lookup"><span data-stu-id="ef65f-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="ef65f-106">Uma *expressão throw* gera o valor produzido avaliando o *null_coalescing_expression*, que deve indicar um valor do tipo de classe `System.Exception`, de um tipo de classe que deriva de `System.Exception` ou de um tipo de parâmetro de tipo que tem `System.Exception` (ou uma subclasse) como sua classe base efetiva.</span><span class="sxs-lookup"><span data-stu-id="ef65f-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="ef65f-107">Se a avaliação da expressão produzir `null`, uma `System.NullReferenceException` será lançada em vez disso.</span><span class="sxs-lookup"><span data-stu-id="ef65f-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="ef65f-108">O comportamento em tempo de execução da avaliação de uma *expressão throw* é o mesmo [especificado para uma *instrução Throw*](../../spec/statements.md#the-throw-statement).</span><span class="sxs-lookup"><span data-stu-id="ef65f-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="ef65f-109">As regras de análise de fluxo são as seguintes:</span><span class="sxs-lookup"><span data-stu-id="ef65f-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="ef65f-110">Para cada variável *v*, a *v* é definitivamente atribuída antes da *null_coalescing_expression* de um *throw_expression* IFF ele é definitivamente atribuído antes da *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="ef65f-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="ef65f-111">Para cada variável *v*, *v* é definitivamente atribuído após *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="ef65f-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="ef65f-112">Uma *expressão throw* é permitida somente nos seguintes contextos sintáticos:</span><span class="sxs-lookup"><span data-stu-id="ef65f-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="ef65f-113">Como o segundo ou terceiro operando de um operador condicional Ternário `?:`</span><span class="sxs-lookup"><span data-stu-id="ef65f-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="ef65f-114">Como o segundo operando de um operador de União nulo `??`</span><span class="sxs-lookup"><span data-stu-id="ef65f-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="ef65f-115">Como o corpo de um lambda ou método apto para de expressão.</span><span class="sxs-lookup"><span data-stu-id="ef65f-115">As the body of an expression-bodied lambda or method.</span></span>
