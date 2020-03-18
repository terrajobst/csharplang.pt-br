---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485255"
---
# <a name="async-streams"></a><span data-ttu-id="b1f0a-101">Fluxos assíncronos</span><span class="sxs-lookup"><span data-stu-id="b1f0a-101">Async Streams</span></span>

* <span data-ttu-id="b1f0a-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="b1f0a-102">[x] Proposed</span></span>
* <span data-ttu-id="b1f0a-103">[x] protótipo</span><span class="sxs-lookup"><span data-stu-id="b1f0a-103">[x] Prototype</span></span>
* <span data-ttu-id="b1f0a-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="b1f0a-104">[ ] Implementation</span></span>
* <span data-ttu-id="b1f0a-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="b1f0a-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="b1f0a-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="b1f0a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b1f0a-107">C#tem suporte para métodos iteradores e métodos assíncronos, mas sem suporte para um método que seja um iterador e um método assíncrono.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="b1f0a-108">Devemos corrigir isso permitindo que o `await` seja usado em uma nova forma de `async` iterador, um que retorne um `IAsyncEnumerable<T>` ou `IAsyncEnumerator<T>` em vez de um `IEnumerable<T>` ou `IEnumerator<T>`, com `IAsyncEnumerable<T>` consumível em um novo `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="b1f0a-109">Uma interface `IAsyncDisposable` também é usada para habilitar a limpeza assíncrona.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="b1f0a-110">Discussão relacionada</span><span class="sxs-lookup"><span data-stu-id="b1f0a-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="b1f0a-111">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="b1f0a-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="b1f0a-112">Interfaces</span><span class="sxs-lookup"><span data-stu-id="b1f0a-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="b1f0a-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="b1f0a-113">IAsyncDisposable</span></span>

<span data-ttu-id="b1f0a-114">Houve muita discussão sobre `IAsyncDisposable` (por exemplo, https://github.com/dotnet/roslyn/issues/114) e se é uma boa ideia.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="b1f0a-115">No entanto, é um conceito necessário para adicionar suporte a iteradores assíncronos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="b1f0a-116">Como os blocos de `finally` podem conter `await`s e, como os blocos de `finally` precisam ser executados como parte do descarte de iteradores, precisamos de uma alienação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="b1f0a-117">Ele também é geralmente útil sempre que a limpeza de recursos pode levar algum tempo, por exemplo, fechar arquivos (exigindo liberações), cancelar o registro de retornos de chamada e fornecer uma maneira de saber quando o cancelamento do registro foi concluído, etc.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="b1f0a-118">A interface a seguir é adicionada às bibliotecas principais do .NET (por exemplo, System. privado. CoreLib/System. Runtime):</span><span class="sxs-lookup"><span data-stu-id="b1f0a-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="b1f0a-119">Assim como ocorre com `Dispose`, invocar `DisposeAsync` várias vezes é aceitável, e as invocações subsequentes após o primeiro devem ser tratadas como NOPs, retornando uma tarefa bem-sucedida sincronizada de forma síncrona (`DisposeAsync` não precisam ser thread-safe, porém, e não precisam dar suporte à invocação simultânea).</span><span class="sxs-lookup"><span data-stu-id="b1f0a-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="b1f0a-120">Além disso, os tipos podem implementar `IDisposable` e `IAsyncDisposable`e, se fizerem, é aceitável invocar `Dispose` e, em seguida, `DisposeAsync` ou vice-versa, mas somente o primeiro deve ser significativo e as invocações subsequentes de devem ser um NOP.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="b1f0a-121">Assim, se um tipo implementar ambos, os consumidores serão incentivados a chamar uma única vez e apenas uma vez o método mais relevante com base no contexto, `Dispose` em contextos síncronos e `DisposeAsync` em assíncronos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="b1f0a-122">(Estou deixando a discussão de como `IAsyncDisposable` interage com `using` a uma discussão separada.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="b1f0a-123">E a cobertura de como ela interage com `foreach` é tratada posteriormente nesta proposta.)</span><span class="sxs-lookup"><span data-stu-id="b1f0a-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="b1f0a-124">Alternativas consideradas:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-124">Alternatives considered:</span></span>
- <span data-ttu-id="b1f0a-125">_`DisposeAsync` aceitando um `CancellationToken`_ : em teoria, faz sentido que qualquer coisa assíncrona possa ser cancelada, a alienação é sobre a limpeza, o fechamento das coisas, os recursos de free'ing, etc., que geralmente não é algo que deve ser cancelado; a limpeza ainda é importante para o trabalho que foi cancelado.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="b1f0a-126">O mesmo `CancellationToken` que causou o trabalho real a ser cancelado normalmente seria o mesmo token passado para `DisposeAsync`, fazendo `DisposeAsync` inúteis porque o cancelamento do trabalho faria com que `DisposeAsync` fosse um NOP.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="b1f0a-127">Se alguém quiser evitar ser bloqueado aguardando a alienação, ele poderá evitar esperar o `ValueTask`resultante ou esperar por um período de tempo.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="b1f0a-128">_`DisposeAsync` retornando um `Task`_ : agora que um `ValueTask` não genérico existe e pode ser construído de um `IValueTaskSource`, retornar `ValueTask` de `DisposeAsync` permite que um objeto existente seja reutilizado como a promessa que representa a eventual conclusão assíncrona de `DisposeAsync`, salvando uma alocação de `Task` no caso em que `DisposeAsync` é concluído de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="b1f0a-129">_Configurando `DisposeAsync` com um `bool continueOnCapturedContext` (`ConfigureAwait`)_ : embora possa haver problemas relacionados à forma como esse conceito é exposto a `using`, `foreach`e outras construções de linguagem que consomem isso, de uma perspectiva de interface não está realmente fazendo nenhuma `await`' ing e não há nada para configurar... os consumidores do `ValueTask` podem consumi-lo, no entanto, eles desejam.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="b1f0a-130">_`IAsyncDisposable` herdando `IDisposable`_ : como apenas uma ou outra deve ser usada, não faz sentido forçar os tipos a implementar ambos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="b1f0a-131">_`IDisposableAsync` em vez de `IAsyncDisposable`_ : estamos seguindo a nomenclatura de que as coisas/tipos são um "assíncrono de algo", enquanto operações são "feitas assincronamente", de modo que os tipos têm "Async" como um prefixo e os métodos têm "Async" como um sufixo.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="b1f0a-132">IAsyncEnumerable/IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="b1f0a-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="b1f0a-133">Duas interfaces são adicionadas às principais bibliotecas do .NET:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="b1f0a-134">O consumo típico (sem recursos de linguagem adicional) seria semelhante a:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="b1f0a-135">Opções descartadas consideradas:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-135">Discarded options considered:</span></span>
- <span data-ttu-id="b1f0a-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : usar `Task<bool>` ofereceria suporte ao uso de um objeto de tarefa em cache para representar chamadas de `MoveNextAsync` síncronas bem-sucedidas, mas uma alocação ainda seria necessária para a conclusão assíncrona.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="b1f0a-137">Ao retornar `ValueTask<bool>`, habilitamos o objeto enumerador para que ele mesmo implemente `IValueTaskSource<bool>` e seja usado como o backup para o `ValueTask<bool>` retornado de `MoveNextAsync`, o que, por sua vez, permite sobrecargas significativamente reduzidos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="b1f0a-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : não é apenas mais difícil de consumir, mas isso significa que `T` não pode mais ser covariantes.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="b1f0a-139">_`ValueTask<T?> TryMoveNextAsync();`_ : não covariant.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="b1f0a-140">_`Task<T?> TryMoveNextAsync();`_ : não covariant, alocações em todas as chamadas, etc.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="b1f0a-141">_`ITask<T?> TryMoveNextAsync();`_ : não covariant, alocações em todas as chamadas, etc.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="b1f0a-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : não covariant, alocações em todas as chamadas, etc.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="b1f0a-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : o resultado de `out` precisaria ser definido quando a operação retorna de forma síncrona, não quando ele conclui a tarefa de maneira assíncrona, em algum momento, no futuro, em que não haveria nenhuma maneira de comunicar o resultado.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="b1f0a-144">_`IAsyncEnumerator<T>` não está implementando `IAsyncDisposable`_ : poderíamos optar por separá-las.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="b1f0a-145">No entanto, isso complica algumas outras áreas da proposta, pois o código deve ser capaz de lidar com a possibilidade de um enumerador não fornecer descarte, o que dificulta a gravação de auxiliares baseados em padrões.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="b1f0a-146">Além disso, será comum que os enumeradores tenham uma necessidade de descarte (por exemplo, qualquer C# iterador assíncrono que tenha um bloco finally, a maioria das coisas que enumeram dados de uma conexão de rede, etc.) e, se não houver, será simples implementar o método puramente como `public ValueTask DisposeAsync() => default(ValueTask);` com sobrecarga adicional mínima.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="b1f0a-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: nenhum parâmetro de token de cancelamento.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="b1f0a-148">Alternativa viável:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="b1f0a-149">`TryGetNext` é usado em um loop interno para consumir itens com uma única chamada de interface, desde que estejam disponíveis de forma síncrona.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="b1f0a-150">Quando o próximo item não pode ser recuperado de forma síncrona, ele retorna false e, sempre que retorna false, um chamador deve posteriormente invocar `WaitForNextAsync` para esperar que o próximo item esteja disponível ou para determinar que nunca haverá outro item.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="b1f0a-151">O consumo típico (sem recursos de linguagem adicional) seria semelhante a:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="b1f0a-152">A vantagem disso é duas dobras, uma secundária e uma maior:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="b1f0a-153">_Minor: permite que um enumerador dê suporte a vários consumidores_.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="b1f0a-154">Pode haver cenários em que é valioso para um enumerador dar suporte a vários consumidores simultâneos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="b1f0a-155">Isso não pode ser obtido quando `MoveNextAsync` e `Current` são separados, de modo que uma implementação não pode fazer seu uso atômico.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="b1f0a-156">Por outro lado, essa abordagem fornece um único método `TryGetNext` que dá suporte ao envio do enumerador para frente e para o próximo item, para que o enumerador possa habilitar a atomicidade, se desejado.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="b1f0a-157">No entanto, é provável que esses cenários também possam ser habilitados fornecendo a cada consumidor seu próprio enumerador de um Enumerable compartilhado.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="b1f0a-158">Além disso, não queremos impor que todos os enumeradores suportam uso simultâneo, pois isso adicionaria sobrecargas não triviais ao caso principal que não exige isso, o que significa que um consumidor da interface geralmente não podia depender dessa forma.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="b1f0a-159">_Principal: desempenho_.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-159">_Major: Performance_.</span></span> <span data-ttu-id="b1f0a-160">A abordagem `MoveNextAsync`/`Current` requer duas chamadas de interface por operação, enquanto o melhor caso para `WaitForNextAsync`/`TryGetNext` é que a maioria das iterações é concluída de forma síncrona, permitindo um loop interno rígido com `TryGetNext`, de modo que temos apenas uma chamada de interface por operação.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="b1f0a-161">Isso pode ter um impacto mensurável em situações em que a interface chama o dominando o cálculo.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="b1f0a-162">No entanto, há desvantagens não triviais, incluindo uma complexidade significativamente maior ao consumi-las manualmente e uma maior chance de introduzir bugs ao usá-los.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="b1f0a-163">E, embora os benefícios de desempenho sejam mostrados em netbenchmarks, não acreditamos que eles serão impactados na grande maioria do uso real.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="b1f0a-164">Se isso acontece, podemos introduzir um segundo conjunto de interfaces de um modo claro.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="b1f0a-165">Opções descartadas consideradas:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-165">Discarded options considered:</span></span>
- <span data-ttu-id="b1f0a-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: parâmetros de `out` não podem ser covariantes.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="b1f0a-167">Há também um pequeno impacto aqui (um problema com o padrão try em geral) que isso provavelmente incorre em uma barreira de gravação em tempo de execução para resultados do tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="b1f0a-168">Cancelamento</span><span class="sxs-lookup"><span data-stu-id="b1f0a-168">Cancellation</span></span>

<span data-ttu-id="b1f0a-169">Há várias abordagens possíveis para dar suporte ao cancelamento:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="b1f0a-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` estão cancelamentos independentes: `CancellationToken` não aparece em nenhum lugar.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="b1f0a-171">O cancelamento é conseguido trazendor logicamente a `CancellationToken` no enumerável e/ou no enumerador de qualquer maneira que seja apropriada, por exemplo, ao chamar um iterador, passar o `CancellationToken` como um argumento para o método iterador e usá-lo no corpo do iterador, como é feito com qualquer outro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="b1f0a-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: você passa um `CancellationToken` para `GetAsyncEnumerator`, e as operações de `MoveNextAsync` subsequentes respeitam, no entanto, isso pode.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="b1f0a-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: você passa um `CancellationToken` para cada chamada de `MoveNextAsync` individual.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="b1f0a-174">1 & & 2: você insere `CancellationToken`s em seu enumerável/enumerador e passa `CancellationToken`s para `GetAsyncEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="b1f0a-175">1 & & 3: você insere `CancellationToken`s em seu enumerável/enumerador e passa `CancellationToken`s para `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="b1f0a-176">De uma perspectiva puramente teórica, (5) é a mais robusta, no (a) `MoveNextAsync` aceitar um `CancellationToken` permite o controle mais refinado sobre o que foi cancelado e (b) `CancellationToken` é apenas qualquer outro tipo que possa passar como um argumento em iteradores, inseridos em tipos arbitrários, etc.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="b1f0a-177">No entanto, há vários problemas com essa abordagem:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="b1f0a-178">Como um `CancellationToken` passado para `GetAsyncEnumerator` transformá-lo no corpo do iterador?</span><span class="sxs-lookup"><span data-stu-id="b1f0a-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="b1f0a-179">Poderíamos expor uma nova palavra-chave `iterator` da qual você poderia retirar para obter acesso ao `CancellationToken` passado para `GetEnumerator`, mas a) que é uma grande quantidade de máquinas adicionais, b) estamos fazendo dele um cidadão de primeira classe, e c) o caso de 99% pareceria ser o mesmo código que chamamos um iterador e chamando `GetAsyncEnumerator` nele; nesse caso, ele pode apenas passar o `CancellationToken` como um argumento para o método.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="b1f0a-180">Como um `CancellationToken` passado para `MoveNextAsync` entrar no corpo do método?</span><span class="sxs-lookup"><span data-stu-id="b1f0a-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="b1f0a-181">Isso é ainda pior, como se fosse exposto a partir de um `iterator` objeto local, seu valor pode ser alterado em Awaits, o que significa que qualquer código registrado com o token precisaria cancelar o registro dele antes de aguardar e, em seguida, registrar novamente depois; também é potencialmente muito caro precisar fazer tal registro e cancelamento de registro em cada `MoveNextAsync` chamada, independentemente de ser implementado pelo compilador em um iterador ou por um desenvolvedor manualmente.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="b1f0a-182">Como um desenvolvedor cancela um loop de `foreach`?</span><span class="sxs-lookup"><span data-stu-id="b1f0a-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="b1f0a-183">Se for feito fornecendo um `CancellationToken` a um enumerável/enumerador e, em seguida, um), precisamos dar suporte aos enumeradores de `foreach`' ing over ', que os eleva a ser cidadãos de primeira classe e agora você precisa começar a pensar em um ecossistema criado em torno de enumeradores (por exemplo, métodos LINQ) ou b), precisamos inserir o `CancellationToken` no enumerable de qualquer forma, tendo algum `WithCancellation` método de extensão de `IAsyncEnumerable<T>` que armazenaria o token fornecido e, em seguida, passá-lo para o `GetAsyncEnumerator` Enumerable disposto quando o `GetAsyncEnumerator` na estrutura retornada é invocado (ignorando esse token).</span><span class="sxs-lookup"><span data-stu-id="b1f0a-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="b1f0a-184">Ou, você pode usar apenas o `CancellationToken` no corpo do foreach.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="b1f0a-185">Se/quando houver suporte a compreensão de consulta, como o `CancellationToken` fornecido para `GetEnumerator` ou `MoveNextAsync` ser passado para cada cláusula?</span><span class="sxs-lookup"><span data-stu-id="b1f0a-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="b1f0a-186">A maneira mais fácil seria simplesmente para a cláusula capturá-la e, nesse ponto, qualquer token é passado para `GetAsyncEnumerator`/`MoveNextAsync` é ignorado.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="b1f0a-187">Uma versão anterior deste documento é recomendada (1), mas, como mudamos para (4).</span><span class="sxs-lookup"><span data-stu-id="b1f0a-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="b1f0a-188">Os dois principais problemas com (1):</span><span class="sxs-lookup"><span data-stu-id="b1f0a-188">The two main problems with (1):</span></span>
- <span data-ttu-id="b1f0a-189">os produtores de enumeráveis canceláveis precisam implementar algum texto clichê e só podem aproveitar o suporte do compilador para iteradores assíncronos para implementar um método de `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="b1f0a-190">é provável que muitos produtores sejam tentados a simplesmente adicionar um parâmetro `CancellationToken` à sua assinatura Async-Enumerable, o que impedirá que os consumidores passem o token de cancelamento que desejam quando receberem um tipo de `IAsyncEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="b1f0a-191">Há dois cenários de consumo principais:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="b1f0a-192">`await foreach (var i in GetData(token)) ...` em que o consumidor chama o método Async-Iterator,</span><span class="sxs-lookup"><span data-stu-id="b1f0a-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="b1f0a-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` em que o consumidor lida com uma determinada instância de `IAsyncEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="b1f0a-194">Descobrimos que um comprometimento razoável para dar suporte a ambos os cenários de uma maneira que seja conveniente para produtores e consumidores de transmissões assíncronas é usar um parâmetro especialmente anotado no método Async-Iterator.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="b1f0a-195">O atributo `[EnumeratorCancellation]` é usado para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="b1f0a-196">Colocar esse atributo em um parâmetro informa ao compilador que, se um token for passado para o método `GetAsyncEnumerator`, esse token deverá ser usado em vez do valor passado originalmente para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="b1f0a-197">Considere o `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="b1f0a-198">O implementador desse método pode simplesmente usar o parâmetro no corpo do método.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="b1f0a-199">O consumidor pode usar os padrões de consumo acima:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="b1f0a-200">Se você usar `GetData(token)`, o token será salvo no Async-Enumerable e será usado na iteração,</span><span class="sxs-lookup"><span data-stu-id="b1f0a-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="b1f0a-201">Se você usar `givenIAsyncEnumerable.WithCancellation(token)`, o token passado para `GetAsyncEnumerator` substituirá qualquer token salvo no Async-Enumerable.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="b1f0a-202">foreach</span><span class="sxs-lookup"><span data-stu-id="b1f0a-202">foreach</span></span>

<span data-ttu-id="b1f0a-203">`foreach` será aumentado para dar suporte a `IAsyncEnumerable<T>` além de seu suporte existente para `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="b1f0a-204">E ele dará suporte ao equivalente de `IAsyncEnumerable<T>` como um padrão se os membros relevantes forem expostos publicamente, voltando ao uso da interface diretamente, caso contrário, para habilitar as extensões baseadas em struct que evitam a alocação, bem como o uso de awaitables alternativo como o tipo de retorno de `MoveNextAsync` e `DisposeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="b1f0a-205">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="b1f0a-205">Syntax</span></span>

<span data-ttu-id="b1f0a-206">Usando a sintaxe:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="b1f0a-207">C#continuará a tratar `enumerable` como um Enumerable síncrono, de modo que, mesmo que ele exponha as APIs relevantes para enumeráveis assíncronos (expondo o padrão ou implementando a interface), ele considerará apenas as APIs síncronas.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="b1f0a-208">Para forçar `foreach` em vez disso, considere apenas as APIs assíncronas, `await` é inserido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="b1f0a-209">Nenhuma sintaxe seria fornecida para dar suporte ao uso das APIs Async ou Sync; o desenvolvedor deve escolher com base na sintaxe usada.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="b1f0a-210">Opções descartadas consideradas:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-210">Discarded options considered:</span></span>
- <span data-ttu-id="b1f0a-211">_`foreach (var i in await enumerable)`_ : essa sintaxe já é válida e alterar seu significado seria uma alteração significativa.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="b1f0a-212">Isso significa `await` o `enumerable`, obter algo de forma síncrona iterável a partir dele e, em seguida, iterar de forma síncrona.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="b1f0a-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_ : todos sugerem que estamos aguardando o próximo item, mas há outros Awaits envolvidos em foreach, especialmente se o Enumerable for um `IAsyncDisposable`, será `await`' ing seu descarte assíncrono.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="b1f0a-214">O Await é como o escopo do foreach em vez de para cada elemento individual e, portanto, a palavra-chave `await` merece estar no nível de `foreach`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="b1f0a-215">Além disso, tê-lo associado ao `foreach` nos dá uma maneira de descrever o `foreach` com um termo diferente, por exemplo, um "Await foreach".</span><span class="sxs-lookup"><span data-stu-id="b1f0a-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="b1f0a-216">Mas o mais importante é que há um valor para considerar `foreach` sintaxe ao mesmo tempo que `using` sintaxe, para que eles permaneçam consistentes entre si e `using (await ...)` já seja uma sintaxe válida.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="b1f0a-217">Você ainda deve considerar:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-217">Still to consider:</span></span>
- <span data-ttu-id="b1f0a-218">`foreach` hoje não oferece suporte à iteração por meio de um enumerador.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="b1f0a-219">Esperamos que seja mais comum ter `IAsyncEnumerator<T>`s em um lugar e, portanto, é tentador dar suporte a `await foreach` com `IAsyncEnumerable<T>` e `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="b1f0a-220">Mas depois de adicionar esse suporte, ele apresenta a pergunta se `IAsyncEnumerator<T>` é um cidadão de primeira classe e se precisamos ter sobrecargas de combinadores que operam em enumeradores além de enumeráveis?</span><span class="sxs-lookup"><span data-stu-id="b1f0a-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="b1f0a-221">Queremos incentivar métodos para retornar enumeradores em vez de enumeráveis?</span><span class="sxs-lookup"><span data-stu-id="b1f0a-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="b1f0a-222">Devemos continuar a discutir isso.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-222">We should continue to discuss this.</span></span>  <span data-ttu-id="b1f0a-223">Se decidirmos que não queremos dar suporte a ela, talvez queiramos introduzir um método de extensão `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` que permitisse que um enumerador ainda fosse `foreach`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="b1f0a-224">Se decidirmos que queremos dar suporte a ela, também precisaremos decidir se a `await foreach` seria responsável por chamar `DisposeAsync` no enumerador, e a resposta é provavelmente "não, o controle sobre a alienação deve ser tratado por quem chamou `GetEnumerator`".</span><span class="sxs-lookup"><span data-stu-id="b1f0a-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="b1f0a-225">Compilação baseada em padrões</span><span class="sxs-lookup"><span data-stu-id="b1f0a-225">Pattern-based Compilation</span></span>

<span data-ttu-id="b1f0a-226">O compilador se associará às APIs baseadas em padrão, se existirem, preferirem aquelas usando a interface (o padrão pode ser satisfeito com métodos de instância ou métodos de extensão).</span><span class="sxs-lookup"><span data-stu-id="b1f0a-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="b1f0a-227">Os requisitos para o padrão são:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="b1f0a-228">O enumerável deve expor um método `GetAsyncEnumerator` que possa ser chamado sem argumentos e que retorne um enumerador que atenda ao padrão relevante.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="b1f0a-229">O enumerador deve expor um método `MoveNextAsync` que pode ser chamado sem argumentos e que retorna algo que pode ser `await`Ed e cujo `GetResult()` retorna um `bool`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="b1f0a-230">O enumerador também deve expor `Current` propriedade cujo getter retorna um `T` representando o tipo de dados que está sendo enumerado.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="b1f0a-231">O enumerador pode, opcionalmente, expor um método `DisposeAsync` que pode ser invocado sem argumentos e que retorna algo que pode ser `await`Ed e cujo `GetResult()` retorna `void`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="b1f0a-232">Este código:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="b1f0a-233">é convertido para o equivalente de:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="b1f0a-234">Se o tipo iterado não expor o padrão correto, as interfaces serão usadas.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="b1f0a-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="b1f0a-235">ConfigureAwait</span></span>

<span data-ttu-id="b1f0a-236">Essa compilação baseada em padrão permitirá que `ConfigureAwait` seja usado em todos os Awaits, por meio de um método de extensão `ConfigureAwait`:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="b1f0a-237">Isso se baseará em tipos que também adicionaremos ao .NET, provavelmente a System. Threading. Tasks. Extensions. dll:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="b1f0a-238">Observe que essa abordagem não permitirá que `ConfigureAwait` seja usado com enumeráveis baseados em padrões, mas, novamente, já é o caso de o `ConfigureAwait` ser exposto apenas como uma extensão em `Task`/`Task<T>`/`ValueTask`/e não pode ser aplicado a coisas notentes arbitrárias, pois faz sentido apenas quando aplicado às tarefas (ele controla um comportamento implementado no suporte à continuação da tarefa) e, portanto, não faz sentido ao usar um padrão em que as coisas awaitable podem não ser tarefas.`ValueTask<T>`</span><span class="sxs-lookup"><span data-stu-id="b1f0a-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="b1f0a-239">Qualquer pessoa que retornar coisas awaitable pode fornecer seu próprio comportamento personalizado nesses cenários avançados.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="b1f0a-240">(Se pudermos surgir alguma maneira de dar suporte a uma solução de `ConfigureAwait` de nível de escopo ou assembly, isso não será necessário.)</span><span class="sxs-lookup"><span data-stu-id="b1f0a-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="b1f0a-241">Iteradores assíncronos</span><span class="sxs-lookup"><span data-stu-id="b1f0a-241">Async Iterators</span></span>

<span data-ttu-id="b1f0a-242">O idioma/compilador dará suporte à produção de `IAsyncEnumerable<T>`s e `IAsyncEnumerator<T>`s, além de consumi-los.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="b1f0a-243">Atualmente, a linguagem dá suporte à gravação de um iterador como:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="b1f0a-244">Mas `await` não pode ser usado no corpo desses iteradores.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="b1f0a-245">Adicionaremos esse suporte.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="b1f0a-246">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="b1f0a-246">Syntax</span></span>

<span data-ttu-id="b1f0a-247">O suporte de idioma existente para iteradores infere a natureza do iterador do método com base no fato de ele conter qualquer `yield`s.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="b1f0a-248">O mesmo será verdadeiro para iteradores assíncronos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="b1f0a-249">Esses iteradores assíncronos serão demarcadas e diferenciados dos iteradores síncronos por meio da adição de `async` à assinatura e, em seguida, também devem ter `IAsyncEnumerable<T>` ou `IAsyncEnumerator<T>` como seu tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="b1f0a-250">Por exemplo, o exemplo acima poderia ser escrito como um iterador assíncrono da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="b1f0a-251">Alternativas consideradas:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-251">Alternatives considered:</span></span>
- <span data-ttu-id="b1f0a-252">_Não usar `async` na assinatura_: usar `async` provavelmente é tecnicamente exigido pelo compilador, pois ele o utiliza para determinar se `await` é válido nesse contexto.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="b1f0a-253">Mas mesmo que não seja necessário, estabelecemos que `await` só podem ser usadas em métodos marcados como `async`, e parece importante manter a consistência.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="b1f0a-254">_Habilitando construtores personalizados para `IAsyncEnumerable<T>`_ : isso é algo que poderíamos examinar para o futuro, mas a maquinaidade é complicada e não damos suporte a isso para as contrapartes síncronas.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="b1f0a-255">_Tendo um `iterator` palavra-chave na assinatura: os_iteradores assíncronos usariam `async iterator` na assinatura e `yield` só poderiam ser usados em métodos `async` que incluíam `iterator`; em seguida, `iterator` se tornaria opcional em iteradores síncronos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="b1f0a-256">Dependendo da sua perspectiva, isso tem a vantagem de torná-lo muito claro pela assinatura do método se `yield` é permitido e se o método é realmente destinado a retornar instâncias do tipo `IAsyncEnumerable<T>` em vez da fabricação do compilador, com base em se o código usa `yield` ou não.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="b1f0a-257">Mas é diferente dos iteradores síncronos, que não são e não podem ser feitos para exigir um.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="b1f0a-258">Além disso, alguns desenvolvedores não gostam da sintaxe extra.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="b1f0a-259">Se estivessemos projetando a partir do zero, provavelmente teríamos que fazer isso, mas neste ponto há muito mais valor em manter iteradores assíncronos próximos aos iteradores de sincronização.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="b1f0a-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="b1f0a-260">LINQ</span></span>

<span data-ttu-id="b1f0a-261">Há mais de ~ 200 sobrecargas de métodos na classe `System.Linq.Enumerable`, todos os quais funcionam em termos de `IEnumerable<T>`; algumas dessas aceitam `IEnumerable<T>`, algumas delas produzem `IEnumerable<T>`e muitas fazem as duas.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="b1f0a-262">Adicionar suporte a LINQ para `IAsyncEnumerable<T>` provavelmente envolveria a duplicação de todas essas sobrecargas para ela, para outro ~ 200.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="b1f0a-263">E, como `IAsyncEnumerator<T>` é provavelmente mais comum como uma entidade autônoma no mundo assíncrono do que `IEnumerator<T>` está no mundo síncrono, poderíamos potencialmente precisar de outras sobrecargas de 200 que funcionam com `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="b1f0a-264">Além disso, um grande número de sobrecargas lida com predicados (por exemplo, `Where` que usa um `Func<T, bool>`), e pode ser desejável ter sobrecargas com base em `IAsyncEnumerable<T>`que lidam com predicados síncronos e assíncronos (por exemplo, `Func<T, ValueTask<bool>>` além de `Func<T, bool>`).</span><span class="sxs-lookup"><span data-stu-id="b1f0a-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="b1f0a-265">Embora isso não seja aplicável a todas as novas sobrecargas de agora ~ 400, um cálculo aproximado é que seria aplicável à metade, o que significa que outras ~ 200 sobrecargas, para um total de ~ 600 novos métodos.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="b1f0a-266">Esse é um número cada vez maior de APIs, com o potencial para ainda mais quando bibliotecas de extensão como extensões interativas (IX) são consideradas.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="b1f0a-267">Mas o IX já tem uma implementação de muitos desses, e parece que não há um grande motivo para duplicar esse trabalho; em vez disso, devemos ajudar a Comunidade a melhorar o IX e recomendar isso para quando os desenvolvedores desejarem usar o LINQ com `IAsyncEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="b1f0a-268">Também há o problema de sintaxe de compreensão da consulta.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="b1f0a-269">A natureza baseada em padrões das compreensão das consultas permitiria "simplesmente funcionar" com alguns operadores, por exemplo, se o IX fornecer os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="b1f0a-270">Então, C# esse código vai "apenas funcionar":</span><span class="sxs-lookup"><span data-stu-id="b1f0a-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="b1f0a-271">No entanto, não há nenhuma sintaxe de compreensão de consulta que ofereça suporte ao uso de `await` nas cláusulas, portanto, se IX for adicionado, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="b1f0a-272">Então, isso "simplesmente funciona":</span><span class="sxs-lookup"><span data-stu-id="b1f0a-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="b1f0a-273">Mas não haveria nenhuma maneira de escrevê-lo com o `await` embutido na cláusula `select`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="b1f0a-274">Como um esforço separado, poderíamos examinar a adição de expressões de `async { ... }` ao idioma, no ponto em que poderíamos permitir que elas sejam usadas em compreensãos de consulta e, em vez disso, a seguinte poderia ser escrita como:</span><span class="sxs-lookup"><span data-stu-id="b1f0a-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="b1f0a-275">ou para permitir que `await` seja usado diretamente em expressões, como ao dar suporte a `async from`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="b1f0a-276">No entanto, é improvável que um design aqui afete o restante do conjunto de recursos de uma maneira ou o outro, e essa não é uma coisa particularmente de alto valor para investir no momento, portanto, a proposta é não fazer nada mais aqui no momento.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="b1f0a-277">Integração com outras estruturas assíncronas</span><span class="sxs-lookup"><span data-stu-id="b1f0a-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="b1f0a-278">A integração com `IObservable<T>` e outras estruturas assíncronas (por exemplo, fluxos reativos) seria feita no nível da biblioteca, e não no nível de linguagem.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="b1f0a-279">Por exemplo, todos os dados de um `IAsyncEnumerator<T>` podem ser publicados em um `IObserver<T>` simplesmente pelo `await foreach`' enatravés do enumerador e `OnNext`os dados para o observador, para que um método de extensão `AsObservable<T>` seja possível.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="b1f0a-280">O consumo de um `IObservable<T>` em um `await foreach` exige o armazenamento em buffer dos dados (caso outro item seja enviado por push enquanto o item anterior ainda está sendo processado), mas esse adaptador pode ser facilmente implementado para permitir que um `IObservable<T>` seja extraído com um `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="b1f0a-281">Diante.  O RX/IX já fornece protótipos dessas implementações, e bibliotecas como https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels fornecem vários tipos de estruturas de dados em buffer.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="b1f0a-282">O idioma não precisa estar envolvido neste estágio.</span><span class="sxs-lookup"><span data-stu-id="b1f0a-282">The language need not be involved at this stage.</span></span>
