---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484478"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="6e838-101">parâmetros e locais ReadOnly</span><span class="sxs-lookup"><span data-stu-id="6e838-101">readonly locals and parameters</span></span>

* <span data-ttu-id="6e838-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="6e838-102">[x] Proposed</span></span>
* <span data-ttu-id="6e838-103">[] Protótipo</span><span class="sxs-lookup"><span data-stu-id="6e838-103">[ ] Prototype</span></span>
* <span data-ttu-id="6e838-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="6e838-104">[ ] Implementation</span></span>
* <span data-ttu-id="6e838-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="6e838-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="6e838-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="6e838-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6e838-107">Permitir que locais e parâmetros sejam anotados como ReadOnly para evitar a mutação superficial desses locais e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="6e838-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="6e838-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="6e838-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="6e838-109">Hoje, a palavra-chave `readonly` pode ser aplicada a campos; Isso tem o efeito de garantir que um campo só possa ser gravado durante a construção (construção estática no caso de um campo estático ou de uma construção de instância no caso de um campo de instância), o que ajuda os desenvolvedores a evitar erros, substituindo acidentalmente o estado que não deve ser modificado.</span><span class="sxs-lookup"><span data-stu-id="6e838-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="6e838-110">Mas os campos não são os únicos locais que os desenvolvedores querem garantir que os valores não sejam modificados.</span><span class="sxs-lookup"><span data-stu-id="6e838-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="6e838-111">Em particular, é comum criar uma variável local para armazenar o estado temporário, e a atualização acidental desse estado temporário pode resultar em cálculos errados e em outros bugs, especialmente quando tais "locais" são capturados em lambdas, em que ponto eles são levantados em campos, mas não há nenhuma maneira de marcar tais campos levantados como `readonly`.</span><span class="sxs-lookup"><span data-stu-id="6e838-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6e838-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="6e838-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="6e838-113">Os locais também serão anotados como `readonly`, com o compilador, garantindo que eles sejam definidos apenas no momento da declaração (determinados locais no C# já estão implicitamente ReadOnly, como a variável de iteração em um loop ' foreach ' ou a variável usada em um bloco ' Using ', mas atualmente um desenvolvedor não tem a capacidade de marcar outros locais como `readonly`).</span><span class="sxs-lookup"><span data-stu-id="6e838-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="6e838-114">Tais `readonly` locais devem ter um inicializador:</span><span class="sxs-lookup"><span data-stu-id="6e838-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="6e838-115">E, como abreviação de `readonly var`, a palavra-chave contextual existente `let` pode ser usada, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="6e838-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="6e838-116">Não há nenhuma restrição especial para o que o inicializador pode ser e pode ser qualquer coisa válida no momento como um inicializador para locais, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="6e838-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="6e838-117">`readonly` em locais é particularmente valiosa ao trabalhar com lambdas e fechamentos.</span><span class="sxs-lookup"><span data-stu-id="6e838-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="6e838-118">Quando um método anônimo ou lambda acessa o estado local do escopo delimitador, esse estado é capturado em um fechamento pelo compilador, que é representado por uma "classe de exibição".</span><span class="sxs-lookup"><span data-stu-id="6e838-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="6e838-119">Cada "local" capturado é um campo nessa classe, ainda que o compilador esteja gerando esse campo em seu nome, você não tem oportunidade de anotá-lo como `readonly` para impedir que o lambda grave erroneamente no "local" (entre aspas, pois não é realmente local, pelo menos não no MSIL resultante).</span><span class="sxs-lookup"><span data-stu-id="6e838-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="6e838-120">Com `readonly` locais, o compilador pode impedir que o lambda grave em local, o que é particularmente valioso em cenários que envolvem multithread, em que uma gravação errada poderia resultar em um bug de simultaneidade perigoso, mas raro e difícil de encontrar.</span><span class="sxs-lookup"><span data-stu-id="6e838-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="6e838-121">Como uma forma especial de locais, os parâmetros também serão anotados como `readonly`.</span><span class="sxs-lookup"><span data-stu-id="6e838-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="6e838-122">Isso não teria nenhum efeito sobre o que o chamador do método pode passar para o parâmetro (assim como não há nenhuma restrição sobre quais valores podem ser armazenados em um campo de `readonly`), mas como acontece com qualquer `readonly` local, o compilador proibiria o código de gravar no parâmetro após a declaração, o que significa que o corpo do método é proibido de gravar no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e838-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="6e838-123">`readonly` parâmetros não afetam a assinatura/os metadados emitidos pelo compilador para esse método e simplesmente afetam como o compilador trata a compilação do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="6e838-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="6e838-124">Portanto, por exemplo, um método virtual base pode ter um parâmetro `readonly`, e esse parâmetro poderia ser gravável em uma substituição.</span><span class="sxs-lookup"><span data-stu-id="6e838-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="6e838-125">Assim como nos campos, `readonly` para locais e parâmetros é superficial, afetando o local de armazenamento, mas não afetando transitivamente o grafo do objeto.</span><span class="sxs-lookup"><span data-stu-id="6e838-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="6e838-126">No entanto, também como acontece com campos, chamar um método em um `readonly` struct local/Parameter realmente fará uma cópia da struct e chamará o método na cópia, a fim de evitar a mutação interna de `this`.</span><span class="sxs-lookup"><span data-stu-id="6e838-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="6e838-127">`readonly` locais e parâmetros não podem ser passados como argumentos `ref` ou `out`, a menos que/até `ref readonly` também tenha suporte.</span><span class="sxs-lookup"><span data-stu-id="6e838-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="6e838-128">Alternativas</span><span class="sxs-lookup"><span data-stu-id="6e838-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="6e838-129">`val` pode ser usado como uma abreviação alternativa para `let`.</span><span class="sxs-lookup"><span data-stu-id="6e838-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="6e838-130">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="6e838-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="6e838-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: deixei a pergunta de como lidar com `ref readonly` como separado dessa proposta.</span><span class="sxs-lookup"><span data-stu-id="6e838-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="6e838-132">Esta proposta não lida com tipos de structs/imutáveis de ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="6e838-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="6e838-133">Que é deixado por uma proposta separada.</span><span class="sxs-lookup"><span data-stu-id="6e838-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="6e838-134">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="6e838-134">Design meetings</span></span>

- <span data-ttu-id="6e838-135">Discutido brevemente em 21 de janeiro de 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span><span class="sxs-lookup"><span data-stu-id="6e838-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
