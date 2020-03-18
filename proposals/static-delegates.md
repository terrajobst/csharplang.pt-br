---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484471"
---
# <a name="static-delegates"></a><span data-ttu-id="da625-101">Delegados estáticos</span><span class="sxs-lookup"><span data-stu-id="da625-101">Static Delegates</span></span>

* <span data-ttu-id="da625-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="da625-102">[x] Proposed</span></span>
* <span data-ttu-id="da625-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="da625-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="da625-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="da625-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="da625-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="da625-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="da625-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="da625-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="da625-107">Forneça um recurso de retorno de chamada leve de uso geral C# para a linguagem.</span><span class="sxs-lookup"><span data-stu-id="da625-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="da625-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="da625-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="da625-109">Hoje, os usuários têm a capacidade de criar retornos de chamada usando o tipo de `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="da625-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="da625-110">No entanto, elas são bastante pesadas (como exigir uma alocação de heap e sempre ter tratamento para encadear retornos de chamada juntos).</span><span class="sxs-lookup"><span data-stu-id="da625-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="da625-111">Além disso, `System.Delegate` não fornece a melhor interoperabilidade com ponteiros de função não gerenciados, ou seja, por ser não-blittable e exigir Marshalling a qualquer momento, ele cruza o limite gerenciado/não gerenciado.</span><span class="sxs-lookup"><span data-stu-id="da625-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="da625-112">Com alguns pequenos ajustes, poderíamos fornecer um novo tipo de delegado que seja leve, de uso geral e de interoperabilidade bem com código nativo.</span><span class="sxs-lookup"><span data-stu-id="da625-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="da625-113">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="da625-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="da625-114">Um declararia um delegado estático por meio do seguinte:</span><span class="sxs-lookup"><span data-stu-id="da625-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="da625-115">Além disso, é possível atribuir a declaração com algo semelhante a `System.Runtime.InteropServices.UnmanagedFunctionPointer` para que a Convenção de chamada, o Marshalling de cadeia de caracteres e o comportamento de definir o último erro sejam controlados.</span><span class="sxs-lookup"><span data-stu-id="da625-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="da625-116">Observação: usar `System.Runtime.InteropServices.UnmanagedFunctionPointer` em si não funcionará, pois ele só pode ser usado em delegados.</span><span class="sxs-lookup"><span data-stu-id="da625-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="da625-117">A declaração seria convertida em uma representação interna pelo compilador que é semelhante ao seguinte</span><span class="sxs-lookup"><span data-stu-id="da625-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="da625-118">Isso significa que ele é representado internamente por uma struct que tem um único membro do tipo `IntPtr` (tal struct é blittable e não incorre em nenhuma alocação de heap):</span><span class="sxs-lookup"><span data-stu-id="da625-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="da625-119">O membro contém o endereço da função que deve ser o retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="da625-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="da625-120">O tipo declara um método que corresponde à assinatura do método do retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="da625-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="da625-121">O nome da estrutura não deve ser User-constructible (como fazemos com outras estruturas de backup geradas internamente).</span><span class="sxs-lookup"><span data-stu-id="da625-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="da625-122">Por exemplo: buffers de tamanho fixo geram uma struct com um nome no formato de `<name>e__FixedBuffer` (`<` e `>` fazem parte do identificador e tornam o identificador não constructibledo C#, mas ainda utilizáveis em Il).</span><span class="sxs-lookup"><span data-stu-id="da625-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="da625-123">O nome da declaração do método deve ser um nome bem conhecido usado em todos os tipos delegados estáticos (isso permite que o compilador saiba o nome a ser pesquisado ao determinar a assinatura).</span><span class="sxs-lookup"><span data-stu-id="da625-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="da625-124">O valor do delegado estático só pode ser associado a um método estático que corresponda à assinatura do retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="da625-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="da625-125">Não há suporte para o encadeamento de retornos de chamada juntos.</span><span class="sxs-lookup"><span data-stu-id="da625-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="da625-126">A invocação do retorno de chamada seria implementada pela instrução `calli`.</span><span class="sxs-lookup"><span data-stu-id="da625-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="da625-127">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="da625-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="da625-128">Delegados estáticos não funcionariam com APIs existentes que usam delegados regulares (seria necessário encapsular um delegado estático em um delegado regular da mesma assinatura).</span><span class="sxs-lookup"><span data-stu-id="da625-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="da625-129">Considerando que `System.Delegate` é representado internamente como um conjunto de campos de `object` e `IntPtr` (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), seria possível permitir a conversão implícita de um delegado estático em um `System.Delegate` que tenha uma assinatura de método correspondente.</span><span class="sxs-lookup"><span data-stu-id="da625-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="da625-130">Também seria possível fornecer uma conversão explícita na direção oposta, desde que o `System.Delegate` esteja em conformidade com todos os requisitos de ser um delegado estático.</span><span class="sxs-lookup"><span data-stu-id="da625-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="da625-131">Um trabalho adicional seria necessário para tornar o delegado estático prontamente utilizável na estrutura principal.</span><span class="sxs-lookup"><span data-stu-id="da625-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="da625-132">Alternativas</span><span class="sxs-lookup"><span data-stu-id="da625-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="da625-133">TBD</span><span class="sxs-lookup"><span data-stu-id="da625-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="da625-134">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="da625-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="da625-135">Quais partes do design ainda são TBD?</span><span class="sxs-lookup"><span data-stu-id="da625-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="da625-136">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="da625-136">Design meetings</span></span>

<span data-ttu-id="da625-137">Vincule a anotações de design que afetam essa proposta e descreva em uma frase para cada alteração que elas levaram.</span><span class="sxs-lookup"><span data-stu-id="da625-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


