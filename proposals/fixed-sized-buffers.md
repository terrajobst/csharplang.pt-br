---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484583"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="c952f-101">Buffers de tamanho fixo</span><span class="sxs-lookup"><span data-stu-id="c952f-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="c952f-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="c952f-102">[x] Proposed</span></span>
* <span data-ttu-id="c952f-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="c952f-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="c952f-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="c952f-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="c952f-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="c952f-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="c952f-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="c952f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c952f-107">Forneça um mecanismo de uso geral e seguro para declarar buffers de tamanho fixo para C# o idioma.</span><span class="sxs-lookup"><span data-stu-id="c952f-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="c952f-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="c952f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c952f-109">Hoje, os usuários têm a capacidade de criar buffers de tamanho fixo em um contexto não seguro.</span><span class="sxs-lookup"><span data-stu-id="c952f-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="c952f-110">No entanto, isso exige que o usuário lide com ponteiros, execute verificações de limites manualmente e só dê suporte a um conjunto limitado de tipos (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`e `double`).</span><span class="sxs-lookup"><span data-stu-id="c952f-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="c952f-111">A reclamação mais comum é que os buffers de tamanho fixo não podem ser indexados em código seguro.</span><span class="sxs-lookup"><span data-stu-id="c952f-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="c952f-112">A incapacidade de usar mais tipos é a segunda.</span><span class="sxs-lookup"><span data-stu-id="c952f-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="c952f-113">Com alguns ajustes secundários, poderíamos fornecer buffers de tamanho fixo de uso geral que oferecem suporte a qualquer tipo, pode ser usado em um contexto seguro e ter a verificação de limites automáticos realizada.</span><span class="sxs-lookup"><span data-stu-id="c952f-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c952f-114">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="c952f-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c952f-115">Um declararia um buffer de tamanho fixo seguro por meio do seguinte:</span><span class="sxs-lookup"><span data-stu-id="c952f-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="c952f-116">A declaração seria convertida em uma representação interna pelo compilador que é semelhante ao seguinte</span><span class="sxs-lookup"><span data-stu-id="c952f-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="c952f-117">Como esses buffers de tamanho fixo não exigem mais o uso de `fixed`, faz sentido permitir qualquer tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="c952f-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="c952f-118">Observação: `fixed` ainda terá suporte, mas somente se o tipo de elemento for `blittable`</span><span class="sxs-lookup"><span data-stu-id="c952f-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="c952f-119">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="c952f-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="c952f-120">Pode haver alguns desafios com compatibilidade com versões anteriores, mas, Considerando que os buffers de tamanho fixo existentes funcionem apenas com uma seleção de tipos primitivos, deve ser possível que o compilador Continue "apenas trabalhando" se o usuário tratar o buffer fixo como um refere.</span><span class="sxs-lookup"><span data-stu-id="c952f-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="c952f-121">Construções incompatíveis podem precisar usar codificação de `v2` ligeiramente diferente para ocultar os campos do compilador antigo.</span><span class="sxs-lookup"><span data-stu-id="c952f-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="c952f-122">A embalagem não está bem definida na especificação de IL para tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="c952f-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="c952f-123">Embora a abordagem deva funcionar, estamos nos estornos em comportamento não documentado.</span><span class="sxs-lookup"><span data-stu-id="c952f-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="c952f-124">Devemos fazer isso documentado e verificar se outros JITss, como o mono, têm o mesmo comportamento.</span><span class="sxs-lookup"><span data-stu-id="c952f-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="c952f-125">A especificação de um tipo separado para cada comprimento (possivelmente outro para `readonly` campos, se houver suporte) terá impacto sobre os metadados.</span><span class="sxs-lookup"><span data-stu-id="c952f-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="c952f-126">Ele será associado pelo número de matrizes de tamanhos diferentes no aplicativo especificado.</span><span class="sxs-lookup"><span data-stu-id="c952f-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="c952f-127">`ref` matemática não é formalmente verificável (pois não é segura).</span><span class="sxs-lookup"><span data-stu-id="c952f-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="c952f-128">Precisaremos encontrar uma maneira de atualizar as regras de verificação para saber que nosso uso está ok.</span><span class="sxs-lookup"><span data-stu-id="c952f-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c952f-129">Alternativas</span><span class="sxs-lookup"><span data-stu-id="c952f-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c952f-130">Declare manualmente suas estruturas e use código não seguro para construir indexadores.</span><span class="sxs-lookup"><span data-stu-id="c952f-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="c952f-131">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="c952f-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="c952f-132">Devemos permitir `readonly`?</span><span class="sxs-lookup"><span data-stu-id="c952f-132">should we allow `readonly`?</span></span>  <span data-ttu-id="c952f-133">(com o indexador ReadOnly)</span><span class="sxs-lookup"><span data-stu-id="c952f-133">(with readonly indexer)</span></span>
- <span data-ttu-id="c952f-134">Devemos permitir inicializadores de matriz?</span><span class="sxs-lookup"><span data-stu-id="c952f-134">should we allow array initializers?</span></span>
- <span data-ttu-id="c952f-135">`fixed` palavra-chave é necessária?</span><span class="sxs-lookup"><span data-stu-id="c952f-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="c952f-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="c952f-136">`foreach`?</span></span>
- <span data-ttu-id="c952f-137">apenas campos de instância em structs?</span><span class="sxs-lookup"><span data-stu-id="c952f-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="c952f-138">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="c952f-138">Design meetings</span></span>

<span data-ttu-id="c952f-139">Vincule a anotações de design que afetam essa proposta e descreva em uma frase para cada alteração que elas levaram.</span><span class="sxs-lookup"><span data-stu-id="c952f-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>