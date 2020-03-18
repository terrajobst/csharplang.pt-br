---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484653"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="07554-101">Declaração `fixed` baseada em padrão</span><span class="sxs-lookup"><span data-stu-id="07554-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="07554-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="07554-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="07554-103">Introduza um padrão que permitiria que os tipos participem de instruções `fixed`.</span><span class="sxs-lookup"><span data-stu-id="07554-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="07554-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="07554-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="07554-105">O idioma fornece um mecanismo para fixar dados gerenciados e obter um ponteiro nativo para o buffer subjacente.</span><span class="sxs-lookup"><span data-stu-id="07554-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="07554-106">O conjunto de tipos que podem participar de `fixed` é codificado e limitado a matrizes e `System.String`.</span><span class="sxs-lookup"><span data-stu-id="07554-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="07554-107">O código de tipos "especiais" não é dimensionado quando novos primitivos, como `ImmutableArray<T>`, `Span<T>``Utf8String` são introduzidos.</span><span class="sxs-lookup"><span data-stu-id="07554-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="07554-108">Além disso, a solução atual para `System.String` se baseia em uma API razoavelmente rígida.</span><span class="sxs-lookup"><span data-stu-id="07554-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="07554-109">A forma da API implica que `System.String` é um objeto contíguo que incorpora dados codificados UTF16 em um deslocamento fixo do cabeçalho do objeto.</span><span class="sxs-lookup"><span data-stu-id="07554-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="07554-110">Essa abordagem foi encontrada com problemas em várias propostas que poderiam exigir alterações no layout subjacente.</span><span class="sxs-lookup"><span data-stu-id="07554-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="07554-111">Seria desejável poder mudar para algo mais flexível que desassocie `System.String` objeto da sua representação interna para fins de interoperabilidade não gerenciada.</span><span class="sxs-lookup"><span data-stu-id="07554-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="07554-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="07554-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="07554-113">*Padrão*</span><span class="sxs-lookup"><span data-stu-id="07554-113">*Pattern*</span></span> ##
<span data-ttu-id="07554-114">Uma "fixa" com base em padrões viável precisa:</span><span class="sxs-lookup"><span data-stu-id="07554-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="07554-115">Forneça as referências gerenciadas para fixar a instância e inicializar o ponteiro (preferivelmente esta é a mesma referência)</span><span class="sxs-lookup"><span data-stu-id="07554-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="07554-116">Transmita sem ambigüidade o tipo do elemento não gerenciado (ou seja, "char" para "String")</span><span class="sxs-lookup"><span data-stu-id="07554-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="07554-117">Prescrever o comportamento no caso "Empty" quando não houver nada para fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="07554-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="07554-118">Não deve enviar por push autores de API para decisões de design que prejudicam o uso do tipo fora do `fixed`.</span><span class="sxs-lookup"><span data-stu-id="07554-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="07554-119">Acho que a acima pode ser satisfeita reconhecendo um membro de retorno de referência de ref-renomeado especialmente: `ref [readonly] T GetPinnableReference()`.</span><span class="sxs-lookup"><span data-stu-id="07554-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="07554-120">Para ser usado pela instrução `fixed`, as seguintes condições devem ser atendidas:</span><span class="sxs-lookup"><span data-stu-id="07554-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="07554-121">Há apenas um desses membros fornecido para um tipo.</span><span class="sxs-lookup"><span data-stu-id="07554-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="07554-122">Retorna por `ref` ou `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="07554-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="07554-123">(`readonly` é permitido para que os autores de tipos imutáveis/ReadOnly pudessem implementar o padrão sem adicionar uma API gravável que pudesse ser usada em código seguro)</span><span class="sxs-lookup"><span data-stu-id="07554-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="07554-124">T é um tipo não gerenciado.</span><span class="sxs-lookup"><span data-stu-id="07554-124">T is an unmanaged type.</span></span>
<span data-ttu-id="07554-125">(como `T*` se torna o tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="07554-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="07554-126">Naturalmente, a restrição expandirá se/quando a noção de "não gerenciado" for expandida)</span><span class="sxs-lookup"><span data-stu-id="07554-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="07554-127">Retorna `nullptr` gerenciado quando não há dados para fixar – provavelmente a maneira mais barata de transmitir a esvaziação.</span><span class="sxs-lookup"><span data-stu-id="07554-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="07554-128">(Observe que "" a cadeia de caracteres retorna uma referência a ' \ 0 ', uma vez que as cadeias são terminadas em nulo)</span><span class="sxs-lookup"><span data-stu-id="07554-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="07554-129">Como alternativa para o `#3`, podemos permitir que o resultado em casos vazios seja indefinido ou específico da implementação.</span><span class="sxs-lookup"><span data-stu-id="07554-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="07554-130">No entanto, isso pode tornar a API mais perigosa e propenso a abusos e incômodos de compatibilidade indesejados.</span><span class="sxs-lookup"><span data-stu-id="07554-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="07554-131">*Tradução*</span><span class="sxs-lookup"><span data-stu-id="07554-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="07554-132">torna-se o pseudocódigo a seguir (nem C#todos expressos no)</span><span class="sxs-lookup"><span data-stu-id="07554-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="07554-133">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="07554-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="07554-134">O GetPinnableReference destina-se a ser usado somente em `fixed`, mas nada impede seu uso em código seguro, portanto, o implementador deve ter isso em mente.</span><span class="sxs-lookup"><span data-stu-id="07554-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="07554-135">Alternativas</span><span class="sxs-lookup"><span data-stu-id="07554-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="07554-136">Os usuários podem introduzir GetPinnableReference ou membro semelhante e usá-lo como</span><span class="sxs-lookup"><span data-stu-id="07554-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="07554-137">Não há solução para `System.String` se for desejada uma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="07554-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="07554-138">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="07554-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="07554-139">[] Comportamento no estado "Empty".</span><span class="sxs-lookup"><span data-stu-id="07554-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="07554-140"> - `nullptr` ou `undefined`?</span><span class="sxs-lookup"><span data-stu-id="07554-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="07554-141">[] Os métodos de extensão devem ser considerados?</span><span class="sxs-lookup"><span data-stu-id="07554-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="07554-142">[] Se um padrão for detectado em `System.String`, ele deverá vencer?</span><span class="sxs-lookup"><span data-stu-id="07554-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="07554-143">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="07554-143">Design meetings</span></span>

<span data-ttu-id="07554-144">Nenhum ainda.</span><span class="sxs-lookup"><span data-stu-id="07554-144">None yet.</span></span> 
