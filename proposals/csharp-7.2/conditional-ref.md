---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484695"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="ecd1a-101">Expressões de referência condicional</span><span class="sxs-lookup"><span data-stu-id="ecd1a-101">Conditional ref expressions</span></span>

<span data-ttu-id="ecd1a-102">O padrão de associação de uma variável ref a uma ou outra expressão condicionalmente não é expresso no C#momento no.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="ecd1a-103">A solução alternativa típica é introduzir um método como:</span><span class="sxs-lookup"><span data-stu-id="ecd1a-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="ecd1a-104">Observe que isso não é uma substituição exata de um ternário, já que todos os argumentos devem ser avaliados no site de chamada.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="ecd1a-105">O seguinte não funcionará conforme o esperado:</span><span class="sxs-lookup"><span data-stu-id="ecd1a-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="ecd1a-106">A sintaxe proposta teria a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="ecd1a-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="ecd1a-107">A tentativa acima com "Choice" pode ser gravada _corretamente_ usando ref ternário como:</span><span class="sxs-lookup"><span data-stu-id="ecd1a-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="ecd1a-108">A diferença da escolha é que a consequência e as expressões alternativas são acessadas de maneira _realmente_ condicional, portanto, não vemos uma falha se ```arr == null```</span><span class="sxs-lookup"><span data-stu-id="ecd1a-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="ecd1a-109">A referência ternário é apenas um ternário em que tanto a alternativa quanto a consequência são refs.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="ecd1a-110">Naturalmente, isso exigirá que os operandos/conseqüências alternativos sejam LValue.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="ecd1a-111">Ele também exigirá que a consequência e a alternativa tenham tipos que são conversíveis de identidade entre si.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="ecd1a-112">O tipo da expressão será computado de forma semelhante à do ternário regular.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="ecd1a-113">I.E.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-113">I.E.</span></span> <span data-ttu-id="ecd1a-114">caso a consequência e a alternativa tenham identidade conversível, mas tipos diferentes, as regras de mesclagem de tipos existentes serão aplicadas.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="ecd1a-115">A segurança a ser retornada será presumida de forma conservadora dos operandos condicionais.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="ecd1a-116">Se não for seguro retornar, não será seguro retornar o item inteiro.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="ecd1a-117">Ref ternário é um LValue e, como tal, pode ser passado/atribuído/retornado por referência;</span><span class="sxs-lookup"><span data-stu-id="ecd1a-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="ecd1a-118">Sendo um LValue, ele também pode ser atribuído a.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="ecd1a-119">Ref ternário também pode ser usado em um contexto regular (não ref).</span><span class="sxs-lookup"><span data-stu-id="ecd1a-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="ecd1a-120">Embora isso não seja comum, já que você poderia usar também um ternário regular.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="ecd1a-121">Notas de implementação:</span><span class="sxs-lookup"><span data-stu-id="ecd1a-121">Implementation notes:</span></span> 

<span data-ttu-id="ecd1a-122">A complexidade da implementação parece ser o tamanho de uma correção de bug moderada para grande.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="ecd1a-123">-I. E não é muito caro.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-123">- I.E not very expensive.</span></span>
<span data-ttu-id="ecd1a-124">Não acho que precisamos de nenhuma alteração na sintaxe ou análise.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="ecd1a-125">Não há nenhum efeito nos metadados ou na interoperabilidade.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="ecd1a-126">O recurso é baseado em expressão completamente.</span><span class="sxs-lookup"><span data-stu-id="ecd1a-126">The feature is completely expression based.</span></span>
<span data-ttu-id="ecd1a-127">Nenhum efeito na depuração/PDB pode ser</span><span class="sxs-lookup"><span data-stu-id="ecd1a-127">No effect on debugging/PDB either</span></span>
