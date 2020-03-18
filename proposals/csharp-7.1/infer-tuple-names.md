---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484716"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="27418-101">Inferir nomes de tupla (também conhecido como.</span><span class="sxs-lookup"><span data-stu-id="27418-101">Infer tuple names (aka.</span></span> <span data-ttu-id="27418-102">inicializadores de projeção de tupla)</span><span class="sxs-lookup"><span data-stu-id="27418-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="27418-103">Resumo</span><span class="sxs-lookup"><span data-stu-id="27418-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="27418-104">Em vários casos comuns, esse recurso permite que os nomes de elementos de tupla sejam omitidos e, em vez disso, sejam inferidos.</span><span class="sxs-lookup"><span data-stu-id="27418-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="27418-105">Por exemplo, em vez de digitar `(f1: x.f1, f2: x?.f2)`, os nomes de elemento "F1" e "F2" podem ser inferidos de `(x.f1, x?.f2)`.</span><span class="sxs-lookup"><span data-stu-id="27418-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="27418-106">Isso paraleliza o comportamento de tipos anônimos, que permitem inferir nomes de membros durante a criação.</span><span class="sxs-lookup"><span data-stu-id="27418-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="27418-107">Por exemplo, `new { x.f1, y?.f2 }` declara os membros "F1" e "F2".</span><span class="sxs-lookup"><span data-stu-id="27418-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="27418-108">Isso é particularmente útil ao usar tuplas no LINQ:</span><span class="sxs-lookup"><span data-stu-id="27418-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="27418-109">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="27418-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="27418-110">Há duas partes para a alteração:</span><span class="sxs-lookup"><span data-stu-id="27418-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="27418-111">Tente inferir um nome de candidato para cada elemento de tupla que não tem um nome explícito:</span><span class="sxs-lookup"><span data-stu-id="27418-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="27418-112">Usando as mesmas regras que a inferência de nomes para tipos anônimos.</span><span class="sxs-lookup"><span data-stu-id="27418-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="27418-113">No C#, isso permite três casos: `y` (identificador), `x.y` (acesso de membro simples) e `x?.y` (acesso condicional).</span><span class="sxs-lookup"><span data-stu-id="27418-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="27418-114">No VB, isso permite casos adicionais, como `x.y()`.</span><span class="sxs-lookup"><span data-stu-id="27418-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="27418-115">Rejeitar nomes de tupla reservada (diferencia maiúsculas C#de minúsculas, não diferencia maiúsculas de minúsculas no VB), pois elas são proibidas ou já implícitas.</span><span class="sxs-lookup"><span data-stu-id="27418-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="27418-116">Por exemplo, como `ItemN`, `Rest`e `ToString`.</span><span class="sxs-lookup"><span data-stu-id="27418-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="27418-117">Se qualquer nome de candidato for duplicado (diferencia maiúsculas C#de minúsculas, não diferencia maiúsculas de minúsculas no VB) em toda a tupla, descartamos esses candidatos,</span><span class="sxs-lookup"><span data-stu-id="27418-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="27418-118">Durante as conversões (que verificam e avisam sobre descartar nomes de literais de tupla), nomes deduzidos não produzirão nenhum aviso.</span><span class="sxs-lookup"><span data-stu-id="27418-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="27418-119">Isso evita a interrupção do código de tupla existente.</span><span class="sxs-lookup"><span data-stu-id="27418-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="27418-120">Observe que a regra para lidar com duplicatas é diferente daquela para tipos anônimos.</span><span class="sxs-lookup"><span data-stu-id="27418-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="27418-121">Por exemplo, `new { x.f1, x.f1 }` produz um erro, mas `(x.f1, x.f1)` ainda seria permitido (apenas sem nenhum nome deduzido).</span><span class="sxs-lookup"><span data-stu-id="27418-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="27418-122">Isso evita a interrupção do código de tupla existente.</span><span class="sxs-lookup"><span data-stu-id="27418-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="27418-123">Para consistência, o mesmo se aplicaria a tuplas produzidas por degroup-assignments (in C#):</span><span class="sxs-lookup"><span data-stu-id="27418-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="27418-124">O mesmo também se aplicaria a tuplas VB, usando as regras específicas do VB para inferir o nome da expressão e comparações de nome que não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="27418-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="27418-125">Ao usar o C# compilador 7,1 (ou posterior) com a versão de idioma "7,0", os nomes de elemento serão inferidos (apesar de o recurso não estar disponível), mas haverá um erro de site de uso para tentar acessá-los.</span><span class="sxs-lookup"><span data-stu-id="27418-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="27418-126">Isso limitará adições de novo código que posteriormente enfrentaria o problema de compatibilidade (descrito abaixo).</span><span class="sxs-lookup"><span data-stu-id="27418-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="27418-127">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="27418-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="27418-128">A principal desvantagem é que isso introduz uma interrupção de compatibilidade C# de 7,0:</span><span class="sxs-lookup"><span data-stu-id="27418-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="27418-129">O Conselho de compatibilidade descobriu essa interrupção aceitável, Considerando que ela é limitada e a janela de tempo desde que as tuplas enviadas (em C# 7,0) é curta.</span><span class="sxs-lookup"><span data-stu-id="27418-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="27418-130">Referências</span><span class="sxs-lookup"><span data-stu-id="27418-130">References</span></span>
- [<span data-ttu-id="27418-131">LDM 4º 2017 de abril</span><span class="sxs-lookup"><span data-stu-id="27418-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="27418-132">[Discussão do GitHub](https://github.com/dotnet/csharplang/issues/370) (Obrigado @alrz por trazer esse problema para cima)</span><span class="sxs-lookup"><span data-stu-id="27418-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="27418-133">Design de tuplas</span><span class="sxs-lookup"><span data-stu-id="27418-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
