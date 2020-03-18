---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484604"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="29887-101">Inicializadores de matriz stackalloc</span><span class="sxs-lookup"><span data-stu-id="29887-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="29887-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="29887-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="29887-103">Permitir que a sintaxe do inicializador de matriz seja usada com `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="29887-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="29887-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="29887-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="29887-105">Matrizes comuns podem ter seus elementos inicializados no momento da criação.</span><span class="sxs-lookup"><span data-stu-id="29887-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="29887-106">Parece razoável permitir isso em `stackalloc` caso.</span><span class="sxs-lookup"><span data-stu-id="29887-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="29887-107">A pergunta sobre por que essa sintaxe não é permitida com `stackalloc` surge com muita frequência.</span><span class="sxs-lookup"><span data-stu-id="29887-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="29887-108">Consulte, por exemplo, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="29887-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="29887-109">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="29887-109">Detailed design</span></span>

<span data-ttu-id="29887-110">Matrizes comuns podem ser criadas por meio da seguinte sintaxe:</span><span class="sxs-lookup"><span data-stu-id="29887-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="29887-111">Devemos permitir que matrizes alocadas da pilha sejam criadas por meio de:</span><span class="sxs-lookup"><span data-stu-id="29887-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="29887-112">A semântica de todos os casos é praticamente a mesma que com matrizes.</span><span class="sxs-lookup"><span data-stu-id="29887-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="29887-113">Por exemplo: no último caso, o tipo de elemento é inferido do inicializador e deve ser um tipo "não gerenciado".</span><span class="sxs-lookup"><span data-stu-id="29887-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="29887-114">Observação: o recurso não depende do destino ser um `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="29887-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="29887-115">Ele é tão aplicável em `T*` caso, portanto, não parece razoável para predicar a ti em `Span<T>` caso.</span><span class="sxs-lookup"><span data-stu-id="29887-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="29887-116">{1&gt;Tradução&lt;1}</span><span class="sxs-lookup"><span data-stu-id="29887-116">Translation</span></span>

<span data-ttu-id="29887-117">A implementação ingênua poderia simplesmente inicializar a matriz logo após a criação por meio de uma série de atribuições de elemento.</span><span class="sxs-lookup"><span data-stu-id="29887-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="29887-118">De maneira semelhante ao caso com matrizes, pode ser possível e desejável detectar casos em que todos ou a maioria dos elementos são tipos blittable e usar técnicas mais eficientes copiando o estado pré-criado de todos os elementos constantes.</span><span class="sxs-lookup"><span data-stu-id="29887-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="29887-119">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="29887-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="29887-120">Alternativas</span><span class="sxs-lookup"><span data-stu-id="29887-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="29887-121">Esse é um recurso de conveniência.</span><span class="sxs-lookup"><span data-stu-id="29887-121">This is a convenience feature.</span></span> <span data-ttu-id="29887-122">É possível simplesmente não fazer nada.</span><span class="sxs-lookup"><span data-stu-id="29887-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="29887-123">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="29887-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="29887-124">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="29887-124">Design meetings</span></span>

<span data-ttu-id="29887-125">Nenhum ainda.</span><span class="sxs-lookup"><span data-stu-id="29887-125">None yet.</span></span> 
