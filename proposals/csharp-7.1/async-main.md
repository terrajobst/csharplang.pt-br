---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484793"
---
# <a name="async-main"></a><span data-ttu-id="171f3-101">Assíncrono principal</span><span class="sxs-lookup"><span data-stu-id="171f3-101">Async Main</span></span>

* <span data-ttu-id="171f3-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="171f3-102">[x] Proposed</span></span>
* <span data-ttu-id="171f3-103">[] Protótipo</span><span class="sxs-lookup"><span data-stu-id="171f3-103">[ ] Prototype</span></span>
* <span data-ttu-id="171f3-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="171f3-104">[ ] Implementation</span></span>
* <span data-ttu-id="171f3-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="171f3-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="171f3-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="171f3-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="171f3-107">Permita que `await` seja usado no método Main/EntryPoint de um aplicativo, permitindo que o ponto de entrada retorne `Task` / `Task<int>` e seja marcado `async`.</span><span class="sxs-lookup"><span data-stu-id="171f3-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="171f3-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="171f3-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="171f3-109">É muito comum ao aprender C#, ao escrever utilitários baseados em console e ao escrever aplicativos de teste pequenos para desejarem chamar e `await` `async` métodos do principal.</span><span class="sxs-lookup"><span data-stu-id="171f3-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="171f3-110">Hoje, adicionamos um nível de complexidade, forçando tal `await`"a ser feito em um método assíncrono separado, o que faz com que os desenvolvedores precisem escrever clichê como o seguinte apenas para começar:</span><span class="sxs-lookup"><span data-stu-id="171f3-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="171f3-111">Podemos remover a necessidade desse texto clichê e torná-lo mais fácil de começar simplesmente permitindo que o principal seja `async` de forma que `await`s possa ser usado.</span><span class="sxs-lookup"><span data-stu-id="171f3-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="171f3-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="171f3-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="171f3-113">No momento, as seguintes assinaturas são de entryPoints permitidas:</span><span class="sxs-lookup"><span data-stu-id="171f3-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="171f3-114">Estendemos a lista de entryPoints permitidos para incluir:</span><span class="sxs-lookup"><span data-stu-id="171f3-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="171f3-115">Para evitar riscos de compatibilidade, essas novas assinaturas só serão consideradas como entryPoints válidos se não houver sobrecargas do conjunto anterior.</span><span class="sxs-lookup"><span data-stu-id="171f3-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="171f3-116">O idioma/compilador não exigirá que o EntryPoint seja marcado como `async`, embora espere que a grande maioria dos usos será marcada como tal.</span><span class="sxs-lookup"><span data-stu-id="171f3-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="171f3-117">Quando um deles é identificado como EntryPoint, o compilador sintetiza um método EntryPoint real que chama um desses métodos codificados:</span><span class="sxs-lookup"><span data-stu-id="171f3-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="171f3-118">```static Task Main()``` resultará no compilador que emite o equivalente de ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="171f3-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="171f3-119">```static Task Main(string[])``` resultará no compilador que emite o equivalente de ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="171f3-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="171f3-120">```static Task<int> Main()``` resultará no compilador que emite o equivalente de ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="171f3-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="171f3-121">```static Task<int> Main(string[])``` resultará no compilador que emite o equivalente de ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="171f3-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="171f3-122">Exemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="171f3-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="171f3-123">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="171f3-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="171f3-124">A principal desvantagem é simplesmente a complexidade adicional de dar suporte a assinaturas de EntryPoint adicionais.</span><span class="sxs-lookup"><span data-stu-id="171f3-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="171f3-125">Alternativas</span><span class="sxs-lookup"><span data-stu-id="171f3-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="171f3-126">Outras variantes consideradas:</span><span class="sxs-lookup"><span data-stu-id="171f3-126">Other variants considered:</span></span>

<span data-ttu-id="171f3-127">Permitindo `async void`.</span><span class="sxs-lookup"><span data-stu-id="171f3-127">Allowing `async void`.</span></span>  <span data-ttu-id="171f3-128">Precisamos manter a semântica o mesmo para o código chamando diretamente, o que dificultaria um ponto de entrada gerado para chamá-lo (nenhuma tarefa foi retornada).</span><span class="sxs-lookup"><span data-stu-id="171f3-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="171f3-129">Poderíamos resolver isso gerando dois outros métodos, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="171f3-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="171f3-130">torna-se</span><span class="sxs-lookup"><span data-stu-id="171f3-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="171f3-131">Também há preocupações em relação à incentivação do uso de `async void`.</span><span class="sxs-lookup"><span data-stu-id="171f3-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="171f3-132">Usando "MainAsync" em vez de "Main" como o nome.</span><span class="sxs-lookup"><span data-stu-id="171f3-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="171f3-133">Embora o sufixo assíncrono seja recomendado para métodos de retorno de tarefas, trata-se principalmente da funcionalidade da biblioteca, que principal não é, e o suporte a nomes de EntryPoint adicionais além de "Main" não vale a pena.</span><span class="sxs-lookup"><span data-stu-id="171f3-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="171f3-134">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="171f3-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="171f3-135">N/D</span><span class="sxs-lookup"><span data-stu-id="171f3-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="171f3-136">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="171f3-136">Design meetings</span></span>

<span data-ttu-id="171f3-137">N/D</span><span class="sxs-lookup"><span data-stu-id="171f3-137">n/a</span></span>
