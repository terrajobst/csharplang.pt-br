---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484618"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="84757-101">Restrição de tipo não gerenciado</span><span class="sxs-lookup"><span data-stu-id="84757-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="84757-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="84757-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="84757-103">O recurso de restrição não gerenciado fornecerá a imposição de linguagem para a classe de tipos conhecida como "tipos não gerenciados C# " na especificação da linguagem.  Isso é definido na seção 18,2 como um tipo que não é um tipo de referência e não contém campos de tipo de referência em nenhum nível de aninhamento.</span><span class="sxs-lookup"><span data-stu-id="84757-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="84757-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="84757-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="84757-105">A principal motivação é facilitar o autor do código de interoperabilidade de nível C#baixo no.</span><span class="sxs-lookup"><span data-stu-id="84757-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="84757-106">Os tipos não gerenciados são um dos principais blocos de construção do código de interoperabilidade, mas a falta de suporte em genéricos torna impossível criar rotinas reutilizáveis em todos os tipos não gerenciados.</span><span class="sxs-lookup"><span data-stu-id="84757-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="84757-107">Em vez disso, os desenvolvedores são forçados a criar o mesmo código de placa para cada tipo não gerenciado em sua biblioteca:</span><span class="sxs-lookup"><span data-stu-id="84757-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="84757-108">Para habilitar esse tipo de cenário, o idioma apresentará uma nova restrição: não gerenciado:</span><span class="sxs-lookup"><span data-stu-id="84757-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="84757-109">Essa restrição só pode ser atendida por tipos que se encaixam na definição de tipo não gerenciado C# na especificação da linguagem. Outra maneira de observar isso é que um tipo satisfaz a restrição não gerenciada IFF ela também pode ser usada como um ponteiro.</span><span class="sxs-lookup"><span data-stu-id="84757-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="84757-110">Os parâmetros de tipo com a restrição não gerenciada podem usar todos os recursos disponíveis para tipos não gerenciados: ponteiros, fixos, etc...</span><span class="sxs-lookup"><span data-stu-id="84757-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="84757-111">Essa restrição também possibilitará ter conversões eficientes entre dados estruturados e fluxos de bytes.</span><span class="sxs-lookup"><span data-stu-id="84757-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="84757-112">Essa é uma operação que é comum em pilhas de rede e camadas de serialização:</span><span class="sxs-lookup"><span data-stu-id="84757-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="84757-113">Essas rotinas são vantajosas porque são provavelmente seguras no tempo de compilação e na alocação gratuita.</span><span class="sxs-lookup"><span data-stu-id="84757-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="84757-114">Os autores de interoperabilidade atualmente não podem fazer isso (embora seja em uma camada em que o desempenho é crítico).</span><span class="sxs-lookup"><span data-stu-id="84757-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="84757-115">Em vez disso, eles precisam contar com a alocação de rotinas que têm verificações de tempo de execução caras para verificar se os valores estão corretamente não gerenciados.</span><span class="sxs-lookup"><span data-stu-id="84757-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="84757-116">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="84757-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="84757-117">O idioma apresentará uma nova restrição chamada `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="84757-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="84757-118">Para atender a essa restrição, um tipo deve ser uma struct e todos os campos do tipo devem se enquadrar em uma das seguintes categorias:</span><span class="sxs-lookup"><span data-stu-id="84757-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="84757-119">O tipo `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` ou `UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="84757-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="84757-120">Ser qualquer tipo de `enum`.</span><span class="sxs-lookup"><span data-stu-id="84757-120">Be any `enum` type.</span></span>
- <span data-ttu-id="84757-121">Ser um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="84757-121">Be a pointer type.</span></span>
- <span data-ttu-id="84757-122">Ser um struct definido pelo usuário que satsifies a restrição `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="84757-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="84757-123">Os campos de instância gerados pelo compilador, como os que fazem backup das propriedades implementadas automaticamente, também devem atender a essas restrições.</span><span class="sxs-lookup"><span data-stu-id="84757-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="84757-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="84757-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="84757-125">A restrição `unmanaged` não pode ser combinada com `struct`, `class` ou `new()`.</span><span class="sxs-lookup"><span data-stu-id="84757-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="84757-126">Essa restrição deriva do fato de que `unmanaged` implica `struct`, portanto, as outras restrições não fazem sentido.</span><span class="sxs-lookup"><span data-stu-id="84757-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="84757-127">A restrição `unmanaged` não é imposta pelo CLR, somente pelo idioma.</span><span class="sxs-lookup"><span data-stu-id="84757-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="84757-128">Para evitar o uso incorretamente por outras linguagens, os métodos que têm essa restrição serão protegidos por um mod-req. Isso impedirá que outras linguagens usem argumentos de tipo que não sejam tipos não gerenciados.</span><span class="sxs-lookup"><span data-stu-id="84757-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="84757-129">O token `unmanaged` na restrição não é uma palavra-chave nem uma palavra-chave contextual.</span><span class="sxs-lookup"><span data-stu-id="84757-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="84757-130">Em vez disso, é como `var` em que ele é avaliado nesse local e irá:</span><span class="sxs-lookup"><span data-stu-id="84757-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="84757-131">Associar ao tipo definido pelo usuário ou referenciado `unmanaged`: isso será tratado da mesma forma que qualquer outra restrição de tipo nomeado é tratada.</span><span class="sxs-lookup"><span data-stu-id="84757-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="84757-132">Associar a nenhum tipo: isso será interpretado como a restrição de `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="84757-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="84757-133">No caso de existir um tipo chamado `unmanaged` e ele estiver disponível sem qualificação no contexto atual, não haverá como usar a restrição `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="84757-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="84757-134">Isso paraleliza as regras que envolvem o `var` de recursos e os tipos definidos pelo usuário com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="84757-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="84757-135">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="84757-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="84757-136">A principal desvantagem desse recurso é que ele atende a um pequeno número de desenvolvedores: normalmente, autores ou estruturas de biblioteca de nível baixo.</span><span class="sxs-lookup"><span data-stu-id="84757-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="84757-137">Portanto, está gastando um tempo de linguagem precioso para um pequeno número de desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="84757-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="84757-138">Ainda assim, essas estruturas são a base para a maioria dos aplicativos .NET.</span><span class="sxs-lookup"><span data-stu-id="84757-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="84757-139">Portanto, o desempenho/exatidão WINS nesse nível pode ter um efeito de ondulação no ecossistema do .NET.</span><span class="sxs-lookup"><span data-stu-id="84757-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="84757-140">Isso faz com que o recurso valha a pena considerar mesmo com o público limitado.</span><span class="sxs-lookup"><span data-stu-id="84757-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="84757-141">Alternativas</span><span class="sxs-lookup"><span data-stu-id="84757-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="84757-142">Há algumas alternativas a serem consideradas:</span><span class="sxs-lookup"><span data-stu-id="84757-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="84757-143">O status quo: o recurso não é justificado em seus próprios méritos e os desenvolvedores continuam a usar o comportamento de consentimento implícito.</span><span class="sxs-lookup"><span data-stu-id="84757-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="84757-144">Perguntas</span><span class="sxs-lookup"><span data-stu-id="84757-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="84757-145">Representação de metadados</span><span class="sxs-lookup"><span data-stu-id="84757-145">Metadata Representation</span></span>

<span data-ttu-id="84757-146">O F# idioma codifica a restrição no arquivo de assinatura, o que significa C# que não é possível reutilizar sua representação.</span><span class="sxs-lookup"><span data-stu-id="84757-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="84757-147">Será necessário escolher um novo atributo para essa restrição.</span><span class="sxs-lookup"><span data-stu-id="84757-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="84757-148">Além disso, um método que tem essa restrição deve ser protegido por um mod-req.</span><span class="sxs-lookup"><span data-stu-id="84757-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="84757-149">Blittable versus não gerenciado</span><span class="sxs-lookup"><span data-stu-id="84757-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="84757-150">O F# idioma tem um [recurso muito semelhante](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) que usa a palavra-chave unmanaged.</span><span class="sxs-lookup"><span data-stu-id="84757-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="84757-151">O nome blittable é proveniente do uso em Midori.</span><span class="sxs-lookup"><span data-stu-id="84757-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="84757-152">Talvez você queira verificar a precedência aqui e usar não gerenciado.</span><span class="sxs-lookup"><span data-stu-id="84757-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="84757-153">**Resolução** A linguagem decide usar não gerenciado</span><span class="sxs-lookup"><span data-stu-id="84757-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="84757-154">Verificação</span><span class="sxs-lookup"><span data-stu-id="84757-154">Verifier</span></span>

<span data-ttu-id="84757-155">O verificador/tempo de execução precisa ser atualizado para entender o uso de ponteiros para parâmetros de tipo genérico?</span><span class="sxs-lookup"><span data-stu-id="84757-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="84757-156">Ou pode simplesmente funcionar como está sem alterações?</span><span class="sxs-lookup"><span data-stu-id="84757-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="84757-157">**Resolução** Nenhuma alteração é necessária.</span><span class="sxs-lookup"><span data-stu-id="84757-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="84757-158">Todos os tipos de ponteiro simplesmente não são verificáveis.</span><span class="sxs-lookup"><span data-stu-id="84757-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="84757-159">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="84757-159">Design meetings</span></span>

<span data-ttu-id="84757-160">N/D</span><span class="sxs-lookup"><span data-stu-id="84757-160">n/a</span></span>
