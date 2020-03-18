---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484555"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="a8990-101">Os operadores devem ser expostos para `System.IntPtr` e `System.UIntPtr`</span><span class="sxs-lookup"><span data-stu-id="a8990-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="a8990-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="a8990-102">[x] Proposed</span></span>
* <span data-ttu-id="a8990-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="a8990-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="a8990-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="a8990-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="a8990-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="a8990-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="a8990-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="a8990-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a8990-107">O CLR dá suporte a um conjunto de operadores para os tipos de `System.IntPtr` e `System.UIntPtr` (`native int`).</span><span class="sxs-lookup"><span data-stu-id="a8990-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="a8990-108">Esses operadores podem ser vistos em `III.1.5` da especificação de Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="a8990-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="a8990-109">No entanto, esses operadores não são C#suportados pelo.</span><span class="sxs-lookup"><span data-stu-id="a8990-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="a8990-110">O suporte a idiomas deve ser fornecido para o conjunto completo de operadores com suporte pelo `System.IntPtr` e `System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="a8990-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="a8990-111">Esses operadores são: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span><span class="sxs-lookup"><span data-stu-id="a8990-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="a8990-112">Motivação</span><span class="sxs-lookup"><span data-stu-id="a8990-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a8990-113">Hoje, os usuários podem facilmente C# escrever aplicativos direcionados a várias plataformas usando várias ferramentas e estruturas, como: `Xamarin`, `.NET Core`, `Mono`, etc...</span><span class="sxs-lookup"><span data-stu-id="a8990-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="a8990-114">Ao escrever código de plataforma cruzada, muitas vezes é necessário escrever código de interoperabilidade que interaja com uma plataforma de destino específica de uma maneira específica.</span><span class="sxs-lookup"><span data-stu-id="a8990-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="a8990-115">Isso pode incluir a escrita de código de gráficos, a chamada de alguma API do sistema ou a interação com uma biblioteca nativa existente.</span><span class="sxs-lookup"><span data-stu-id="a8990-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="a8990-116">Esse código de interoperabilidade geralmente precisa lidar com identificadores, memória não gerenciada ou até apenas números inteiros específicos da plataforma.</span><span class="sxs-lookup"><span data-stu-id="a8990-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="a8990-117">O tempo de execução fornece suporte para isso definindo um conjunto de operadores que podem ser usados nos tipos primitivos `native int` (`System.IntPtr`) e `native unsigned int` (`System.UIntPtr`).</span><span class="sxs-lookup"><span data-stu-id="a8990-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="a8990-118">C#o nunca tem suporte para esses operadores e, portanto, os usuários precisam contornar o problema.</span><span class="sxs-lookup"><span data-stu-id="a8990-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="a8990-119">Isso geralmente aumenta a complexidade do código e reduz a facilidade de manutenção do código.</span><span class="sxs-lookup"><span data-stu-id="a8990-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="a8990-120">Como tal, a linguagem deve começar a dar suporte a esses operadores para ajudar a avançar a linguagem para dar melhor suporte a esses requisitos.</span><span class="sxs-lookup"><span data-stu-id="a8990-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a8990-121">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="a8990-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a8990-122">O conjunto completo de operadores com suporte é definido em `III.1.5` da especificação de Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="a8990-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="a8990-123">A especificação está disponível aqui: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span><span class="sxs-lookup"><span data-stu-id="a8990-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="a8990-124">Um resumo dos operadores é fornecido abaixo para sua conveniência.</span><span class="sxs-lookup"><span data-stu-id="a8990-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="a8990-125">Os operadores não verificáveis definidos pela especificação da CLI não estão listados e não fazem parte desta proposta no momento (embora também possa valer a pena considerar isso).</span><span class="sxs-lookup"><span data-stu-id="a8990-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="a8990-126">Fornecer uma palavra-chave (como `nint` e `nuint`) nem fornecer uma maneira de permitir que os literais sejam declarados para `System.IntPtr` e `System.UIntPtr` (como 0N) não fazem parte dessa proposta (embora possa valer a pena considerar isso também).</span><span class="sxs-lookup"><span data-stu-id="a8990-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="a8990-127">Operador de adição unário</span><span class="sxs-lookup"><span data-stu-id="a8990-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="a8990-128">Operador de subtração unário</span><span class="sxs-lookup"><span data-stu-id="a8990-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="a8990-129">Operador de complemento de bits</span><span class="sxs-lookup"><span data-stu-id="a8990-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="a8990-130">Operadores cast</span><span class="sxs-lookup"><span data-stu-id="a8990-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="a8990-131">Operador de multiplicação</span><span class="sxs-lookup"><span data-stu-id="a8990-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="a8990-132">Operador de divisão</span><span class="sxs-lookup"><span data-stu-id="a8990-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="a8990-133">Operador restante</span><span class="sxs-lookup"><span data-stu-id="a8990-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="a8990-134">Operador de adição</span><span class="sxs-lookup"><span data-stu-id="a8990-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="a8990-135">Operador de subtração</span><span class="sxs-lookup"><span data-stu-id="a8990-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="a8990-136">Operadores Shift</span><span class="sxs-lookup"><span data-stu-id="a8990-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="a8990-137">Operadores de comparação de inteiros</span><span class="sxs-lookup"><span data-stu-id="a8990-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="a8990-138">Operadores lógicos de inteiros</span><span class="sxs-lookup"><span data-stu-id="a8990-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="a8990-139">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="a8990-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a8990-140">O uso real desses operadores pode ser pequeno e limitado aos usuários finais que estão escrevendo bibliotecas de nível inferior ou código de interoperabilidade.</span><span class="sxs-lookup"><span data-stu-id="a8990-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="a8990-141">A maioria dos usuários finais provavelmente estavam consumindo essas bibliotecas de nível inferior, o que teria os números inteiros, identificadores e código de interoperabilidade nativos, abstratos.</span><span class="sxs-lookup"><span data-stu-id="a8990-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="a8990-142">Dessa forma, eles não precisariam dos operadores propriamente ditos.</span><span class="sxs-lookup"><span data-stu-id="a8990-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a8990-143">Alternativas</span><span class="sxs-lookup"><span data-stu-id="a8990-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a8990-144">Faça com que a estrutura implemente os operadores necessários escrevendo-os diretamente no IL.</span><span class="sxs-lookup"><span data-stu-id="a8990-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="a8990-145">Além disso, o tempo de execução pode fornecer suporte intrínseco para os operadores definidos pela estrutura, para otimizar melhor o desempenho final.</span><span class="sxs-lookup"><span data-stu-id="a8990-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a8990-146">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="a8990-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="a8990-147">Quais partes do design ainda são TBD?</span><span class="sxs-lookup"><span data-stu-id="a8990-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a8990-148">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="a8990-148">Design meetings</span></span>

<span data-ttu-id="a8990-149">Vincule a anotações de design que afetam essa proposta e descreva em uma frase para cada alteração que elas levaram.</span><span class="sxs-lookup"><span data-stu-id="a8990-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>