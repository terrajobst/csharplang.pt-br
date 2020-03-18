---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484450"
---
# <a name="covariant-return-types"></a><span data-ttu-id="fdbcc-101">tipos de retorno covariantes</span><span class="sxs-lookup"><span data-stu-id="fdbcc-101">covariant return types</span></span>

* <span data-ttu-id="fdbcc-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="fdbcc-102">[x] Proposed</span></span>
* <span data-ttu-id="fdbcc-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="fdbcc-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="fdbcc-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="fdbcc-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="fdbcc-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="fdbcc-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="fdbcc-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="fdbcc-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="fdbcc-107">Suporte a _tipos de retorno covariantes_.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-107">Support _covariant return types_.</span></span> <span data-ttu-id="fdbcc-108">Especificamente, permitir que um método de substituição tenha um tipo de referência mais derivado do que o método que ele substitui.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="fdbcc-109">Motivação</span><span class="sxs-lookup"><span data-stu-id="fdbcc-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="fdbcc-110">É um padrão comum no código que nomes de métodos diferentes precisam ser inventados para contornar a restrição de idioma que as substituições devem retornar o mesmo tipo do método substituído.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="fdbcc-111">Veja abaixo um exemplo da base de código Roslyn.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="fdbcc-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="fdbcc-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="fdbcc-113">Suporte a _tipos de retorno covariantes_.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-113">Support _covariant return types_.</span></span> <span data-ttu-id="fdbcc-114">Especificamente, permitir que um método de substituição tenha um tipo de referência mais derivado do que o método que ele substitui.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="fdbcc-115">Isso se aplicaria a métodos e propriedades e terá suporte em classes e interfaces.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="fdbcc-116">Isso seria útil no padrão de fábrica.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="fdbcc-117">Por exemplo, na base de código Roslyn, teríamos</span><span class="sxs-lookup"><span data-stu-id="fdbcc-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="fdbcc-118">A implementação disso seria para o compilador emitir o método de substituição como um método virtual "New" que oculta o método de classe base, junto com um método de _ponte_ que implementa o método de classe base com uma chamada para o método de classe derivada.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="fdbcc-119">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="fdbcc-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="fdbcc-120">[] Cada alteração de idioma deve pagar por si mesma.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="fdbcc-121">[] Devemos garantir que o desempenho seja razoável, mesmo no caso de hierarquias de herança profundas</span><span class="sxs-lookup"><span data-stu-id="fdbcc-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="fdbcc-122">[] Devemos garantir que os artefatos da estratégia de tradução não afetem a semântica da linguagem, mesmo ao consumir o novo IL de compiladores antigos.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="fdbcc-123">Alternativas</span><span class="sxs-lookup"><span data-stu-id="fdbcc-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="fdbcc-124">Poderíamos relaxar ligeiramente as regras de linguagem para permitir, na origem,</span><span class="sxs-lookup"><span data-stu-id="fdbcc-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="fdbcc-125">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="fdbcc-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="fdbcc-126">[] Como as APIs que foram compiladas para usar esse recurso funcionam em versões mais antigas do idioma?</span><span class="sxs-lookup"><span data-stu-id="fdbcc-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="fdbcc-127">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="fdbcc-127">Design meetings</span></span>

<span data-ttu-id="fdbcc-128">Nenhum ainda.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-128">None yet.</span></span> <span data-ttu-id="fdbcc-129">Houve alguma discussão em <https://github.com/dotnet/roslyn/issues/357>.</span><span class="sxs-lookup"><span data-stu-id="fdbcc-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>