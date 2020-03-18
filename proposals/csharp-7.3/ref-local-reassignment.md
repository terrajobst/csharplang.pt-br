---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484597"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="84e06-101">Reatribuição local de referência</span><span class="sxs-lookup"><span data-stu-id="84e06-101">Ref Local Reassignment</span></span>

<span data-ttu-id="84e06-102">Em C# 7,3, adicionamos suporte para reassociar o Referent de uma variável local ref ou um parâmetro ref.</span><span class="sxs-lookup"><span data-stu-id="84e06-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="84e06-103">Adicionamos o seguinte ao conjunto de `assignment_operator`s.</span><span class="sxs-lookup"><span data-stu-id="84e06-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="84e06-104">O operador de `=ref` é chamado de ***operador de atribuição de referência***.</span><span class="sxs-lookup"><span data-stu-id="84e06-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="84e06-105">Não é um *operador de atribuição composta*.</span><span class="sxs-lookup"><span data-stu-id="84e06-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="84e06-106">O operando esquerdo deve ser uma expressão que seja associada a uma variável local ref, um parâmetro ref (diferente de `this`) ou um parâmetro out.</span><span class="sxs-lookup"><span data-stu-id="84e06-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="84e06-107">O operando à direita deve ser uma expressão que produz um lvalue que designa um valor do mesmo tipo que o operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="84e06-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="84e06-108">O operando à direita deve ser definitivamente atribuído no ponto da atribuição de referência.</span><span class="sxs-lookup"><span data-stu-id="84e06-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="84e06-109">Quando o operando esquerdo for associado a um parâmetro `out`, será um erro se esse parâmetro de `out` não tiver sido atribuído definitivamente no início do operador de atribuição de referência.</span><span class="sxs-lookup"><span data-stu-id="84e06-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="84e06-110">Se o operando esquerdo for uma ref gravável (ou seja, ele designa algo diferente de um `ref readonly` local ou `in` parâmetro), o operando à direita deverá ser um lvalue gravável.</span><span class="sxs-lookup"><span data-stu-id="84e06-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="84e06-111">O operador ref Assignment produz um lvalue do tipo atribuído.</span><span class="sxs-lookup"><span data-stu-id="84e06-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="84e06-112">É gravável se o operando esquerdo é gravável (ou seja, não `ref readonly` ou `in`).</span><span class="sxs-lookup"><span data-stu-id="84e06-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="84e06-113">As regras de segurança para esse operador são:</span><span class="sxs-lookup"><span data-stu-id="84e06-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="84e06-114">Para uma `e1 = ref e2`de reatribuição de referência, o *ref-safe-to-escape* de `e2` deve ser pelo menos tão amplo quanto o escopo de *ref-safe-to-escape* de `e1`.</span><span class="sxs-lookup"><span data-stu-id="84e06-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="84e06-115">Onde *ref-safe-to-escape* é definido em [segurança para tipos do tipo ref](../csharp-7.2/span-safety.md)</span><span class="sxs-lookup"><span data-stu-id="84e06-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
