---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485136"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="48bf4-101">Permitir `stackalloc` em contextos aninhados</span><span class="sxs-lookup"><span data-stu-id="48bf4-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="48bf4-102">Alocação de pilha</span><span class="sxs-lookup"><span data-stu-id="48bf4-102">Stack allocation</span></span>

<span data-ttu-id="48bf4-103">Modificamos a [*alocação da pilha*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) de seções C# da especificação da linguagem para relaxar os locais quando uma expressão `stackalloc` pode aparecer.</span><span class="sxs-lookup"><span data-stu-id="48bf4-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="48bf4-104">Nós excluímos</span><span class="sxs-lookup"><span data-stu-id="48bf4-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="48bf4-105">e substitua-os por</span><span class="sxs-lookup"><span data-stu-id="48bf4-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="48bf4-106">Observe que a adição de um *array_initializer* para *stackalloc_initializer* (e a criação da expressão de índice opcional) era uma [extensão C# em 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) e não é descrita aqui.</span><span class="sxs-lookup"><span data-stu-id="48bf4-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="48bf4-107">O *tipo de elemento* da expressão de `stackalloc` é o *unmanaged_type* nomeado na expressão stackalloc, se houver, ou o tipo comum entre os elementos da *array_initializer* caso contrário.</span><span class="sxs-lookup"><span data-stu-id="48bf4-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="48bf4-108">O tipo de *stackalloc_initializer* com o *tipo de elemento* `K` depende de seu contexto sintático:</span><span class="sxs-lookup"><span data-stu-id="48bf4-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="48bf4-109">Se o *stackalloc_initializer* aparecer diretamente como o *local_variable_initializer* de uma instrução *local_variable_declaration* ou uma *for_initializer*, seu tipo será `K*`.</span><span class="sxs-lookup"><span data-stu-id="48bf4-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="48bf4-110">Caso contrário, seu tipo será `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="48bf4-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="48bf4-111">Conversão de stackalloc</span><span class="sxs-lookup"><span data-stu-id="48bf4-111">Stackalloc Conversion</span></span>

<span data-ttu-id="48bf4-112">A *conversão stackalloc* é uma nova conversão interna implícita da expressão.</span><span class="sxs-lookup"><span data-stu-id="48bf4-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="48bf4-113">Quando o tipo de um *stackalloc_initializer* é `K*`, há uma conversão implícita de *stackalloc* do *stackalloc_initializer* para o tipo `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="48bf4-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
