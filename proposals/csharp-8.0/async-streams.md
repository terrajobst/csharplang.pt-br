---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485255"
---
# <a name="async-streams"></a>Fluxos assíncronos

* [x] proposta
* [x] protótipo
* [] Implementação
* [] Especificação

## <a name="summary"></a>Resumo
[summary]: #summary

C#tem suporte para métodos iteradores e métodos assíncronos, mas sem suporte para um método que seja um iterador e um método assíncrono.  Devemos corrigir isso permitindo que o `await` seja usado em uma nova forma de `async` iterador, um que retorne um `IAsyncEnumerable<T>` ou `IAsyncEnumerator<T>` em vez de um `IEnumerable<T>` ou `IEnumerator<T>`, com `IAsyncEnumerable<T>` consumível em um novo `await foreach`.  Uma interface `IAsyncDisposable` também é usada para habilitar a limpeza assíncrona.

## <a name="related-discussion"></a>Discussão relacionada
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

## <a name="interfaces"></a>Interfaces

### <a name="iasyncdisposable"></a>IAsyncDisposable

Houve muita discussão sobre `IAsyncDisposable` (por exemplo, https://github.com/dotnet/roslyn/issues/114) e se é uma boa ideia.  No entanto, é um conceito necessário para adicionar suporte a iteradores assíncronos.  Como os blocos de `finally` podem conter `await`s e, como os blocos de `finally` precisam ser executados como parte do descarte de iteradores, precisamos de uma alienação assíncrona.  Ele também é geralmente útil sempre que a limpeza de recursos pode levar algum tempo, por exemplo, fechar arquivos (exigindo liberações), cancelar o registro de retornos de chamada e fornecer uma maneira de saber quando o cancelamento do registro foi concluído, etc.

A interface a seguir é adicionada às bibliotecas principais do .NET (por exemplo, System. privado. CoreLib/System. Runtime):
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
Assim como ocorre com `Dispose`, invocar `DisposeAsync` várias vezes é aceitável, e as invocações subsequentes após o primeiro devem ser tratadas como NOPs, retornando uma tarefa bem-sucedida sincronizada de forma síncrona (`DisposeAsync` não precisam ser thread-safe, porém, e não precisam dar suporte à invocação simultânea).  Além disso, os tipos podem implementar `IDisposable` e `IAsyncDisposable`e, se fizerem, é aceitável invocar `Dispose` e, em seguida, `DisposeAsync` ou vice-versa, mas somente o primeiro deve ser significativo e as invocações subsequentes de devem ser um NOP.  Assim, se um tipo implementar ambos, os consumidores serão incentivados a chamar uma única vez e apenas uma vez o método mais relevante com base no contexto, `Dispose` em contextos síncronos e `DisposeAsync` em assíncronos.

(Estou deixando a discussão de como `IAsyncDisposable` interage com `using` a uma discussão separada.  E a cobertura de como ela interage com `foreach` é tratada posteriormente nesta proposta.)

Alternativas consideradas:
- _`DisposeAsync` aceitando um `CancellationToken`_ : em teoria, faz sentido que qualquer coisa assíncrona possa ser cancelada, a alienação é sobre a limpeza, o fechamento das coisas, os recursos de free'ing, etc., que geralmente não é algo que deve ser cancelado; a limpeza ainda é importante para o trabalho que foi cancelado.  O mesmo `CancellationToken` que causou o trabalho real a ser cancelado normalmente seria o mesmo token passado para `DisposeAsync`, fazendo `DisposeAsync` inúteis porque o cancelamento do trabalho faria com que `DisposeAsync` fosse um NOP.  Se alguém quiser evitar ser bloqueado aguardando a alienação, ele poderá evitar esperar o `ValueTask`resultante ou esperar por um período de tempo.
- _`DisposeAsync` retornando um `Task`_ : agora que um `ValueTask` não genérico existe e pode ser construído de um `IValueTaskSource`, retornar `ValueTask` de `DisposeAsync` permite que um objeto existente seja reutilizado como a promessa que representa a eventual conclusão assíncrona de `DisposeAsync`, salvando uma alocação de `Task` no caso em que `DisposeAsync` é concluído de forma assíncrona.
- _Configurando `DisposeAsync` com um `bool continueOnCapturedContext` (`ConfigureAwait`)_ : embora possa haver problemas relacionados à forma como esse conceito é exposto a `using`, `foreach`e outras construções de linguagem que consomem isso, de uma perspectiva de interface não está realmente fazendo nenhuma `await`' ing e não há nada para configurar... os consumidores do `ValueTask` podem consumi-lo, no entanto, eles desejam.
- _`IAsyncDisposable` herdando `IDisposable`_ : como apenas uma ou outra deve ser usada, não faz sentido forçar os tipos a implementar ambos.
- _`IDisposableAsync` em vez de `IAsyncDisposable`_ : estamos seguindo a nomenclatura de que as coisas/tipos são um "assíncrono de algo", enquanto operações são "feitas assincronamente", de modo que os tipos têm "Async" como um prefixo e os métodos têm "Async" como um sufixo.

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable/IAsyncEnumerator

Duas interfaces são adicionadas às principais bibliotecas do .NET:

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

O consumo típico (sem recursos de linguagem adicional) seria semelhante a:

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

Opções descartadas consideradas:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : usar `Task<bool>` ofereceria suporte ao uso de um objeto de tarefa em cache para representar chamadas de `MoveNextAsync` síncronas bem-sucedidas, mas uma alocação ainda seria necessária para a conclusão assíncrona.  Ao retornar `ValueTask<bool>`, habilitamos o objeto enumerador para que ele mesmo implemente `IValueTaskSource<bool>` e seja usado como o backup para o `ValueTask<bool>` retornado de `MoveNextAsync`, o que, por sua vez, permite sobrecargas significativamente reduzidos.
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : não é apenas mais difícil de consumir, mas isso significa que `T` não pode mais ser covariantes.
- _`ValueTask<T?> TryMoveNextAsync();`_ : não covariant.
- _`Task<T?> TryMoveNextAsync();`_ : não covariant, alocações em todas as chamadas, etc.
- _`ITask<T?> TryMoveNextAsync();`_ : não covariant, alocações em todas as chamadas, etc.
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : não covariant, alocações em todas as chamadas, etc.
- _`Task<bool> TryMoveNextAsync(out T result);`_ : o resultado de `out` precisaria ser definido quando a operação retorna de forma síncrona, não quando ele conclui a tarefa de maneira assíncrona, em algum momento, no futuro, em que não haveria nenhuma maneira de comunicar o resultado.
- _`IAsyncEnumerator<T>` não está implementando `IAsyncDisposable`_ : poderíamos optar por separá-las.  No entanto, isso complica algumas outras áreas da proposta, pois o código deve ser capaz de lidar com a possibilidade de um enumerador não fornecer descarte, o que dificulta a gravação de auxiliares baseados em padrões.  Além disso, será comum que os enumeradores tenham uma necessidade de descarte (por exemplo, qualquer C# iterador assíncrono que tenha um bloco finally, a maioria das coisas que enumeram dados de uma conexão de rede, etc.) e, se não houver, será simples implementar o método puramente como `public ValueTask DisposeAsync() => default(ValueTask);` com sobrecarga adicional mínima.
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: nenhum parâmetro de token de cancelamento.

#### <a name="viable-alternative"></a>Alternativa viável:

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

`TryGetNext` é usado em um loop interno para consumir itens com uma única chamada de interface, desde que estejam disponíveis de forma síncrona.  Quando o próximo item não pode ser recuperado de forma síncrona, ele retorna false e, sempre que retorna false, um chamador deve posteriormente invocar `WaitForNextAsync` para esperar que o próximo item esteja disponível ou para determinar que nunca haverá outro item. O consumo típico (sem recursos de linguagem adicional) seria semelhante a:

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

A vantagem disso é duas dobras, uma secundária e uma maior:
- _Minor: permite que um enumerador dê suporte a vários consumidores_. Pode haver cenários em que é valioso para um enumerador dar suporte a vários consumidores simultâneos.  Isso não pode ser obtido quando `MoveNextAsync` e `Current` são separados, de modo que uma implementação não pode fazer seu uso atômico.  Por outro lado, essa abordagem fornece um único método `TryGetNext` que dá suporte ao envio do enumerador para frente e para o próximo item, para que o enumerador possa habilitar a atomicidade, se desejado.  No entanto, é provável que esses cenários também possam ser habilitados fornecendo a cada consumidor seu próprio enumerador de um Enumerable compartilhado.  Além disso, não queremos impor que todos os enumeradores suportam uso simultâneo, pois isso adicionaria sobrecargas não triviais ao caso principal que não exige isso, o que significa que um consumidor da interface geralmente não podia depender dessa forma.
- _Principal: desempenho_. A abordagem `MoveNextAsync`/`Current` requer duas chamadas de interface por operação, enquanto o melhor caso para `WaitForNextAsync`/`TryGetNext` é que a maioria das iterações é concluída de forma síncrona, permitindo um loop interno rígido com `TryGetNext`, de modo que temos apenas uma chamada de interface por operação.  Isso pode ter um impacto mensurável em situações em que a interface chama o dominando o cálculo.

No entanto, há desvantagens não triviais, incluindo uma complexidade significativamente maior ao consumi-las manualmente e uma maior chance de introduzir bugs ao usá-los.  E, embora os benefícios de desempenho sejam mostrados em netbenchmarks, não acreditamos que eles serão impactados na grande maioria do uso real.  Se isso acontece, podemos introduzir um segundo conjunto de interfaces de um modo claro.

Opções descartadas consideradas:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: parâmetros de `out` não podem ser covariantes.  Há também um pequeno impacto aqui (um problema com o padrão try em geral) que isso provavelmente incorre em uma barreira de gravação em tempo de execução para resultados do tipo de referência.

#### <a name="cancellation"></a>Cancelamento

Há várias abordagens possíveis para dar suporte ao cancelamento:
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` estão cancelamentos independentes: `CancellationToken` não aparece em nenhum lugar.  O cancelamento é conseguido trazendor logicamente a `CancellationToken` no enumerável e/ou no enumerador de qualquer maneira que seja apropriada, por exemplo, ao chamar um iterador, passar o `CancellationToken` como um argumento para o método iterador e usá-lo no corpo do iterador, como é feito com qualquer outro parâmetro.
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: você passa um `CancellationToken` para `GetAsyncEnumerator`, e as operações de `MoveNextAsync` subsequentes respeitam, no entanto, isso pode.
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: você passa um `CancellationToken` para cada chamada de `MoveNextAsync` individual.
4. 1 & & 2: você insere `CancellationToken`s em seu enumerável/enumerador e passa `CancellationToken`s para `GetAsyncEnumerator`.
5. 1 & & 3: você insere `CancellationToken`s em seu enumerável/enumerador e passa `CancellationToken`s para `MoveNextAsync`.

De uma perspectiva puramente teórica, (5) é a mais robusta, no (a) `MoveNextAsync` aceitar um `CancellationToken` permite o controle mais refinado sobre o que foi cancelado e (b) `CancellationToken` é apenas qualquer outro tipo que possa passar como um argumento em iteradores, inseridos em tipos arbitrários, etc.

No entanto, há vários problemas com essa abordagem:
- Como um `CancellationToken` passado para `GetAsyncEnumerator` transformá-lo no corpo do iterador?  Poderíamos expor uma nova palavra-chave `iterator` da qual você poderia retirar para obter acesso ao `CancellationToken` passado para `GetEnumerator`, mas a) que é uma grande quantidade de máquinas adicionais, b) estamos fazendo dele um cidadão de primeira classe, e c) o caso de 99% pareceria ser o mesmo código que chamamos um iterador e chamando `GetAsyncEnumerator` nele; nesse caso, ele pode apenas passar o `CancellationToken` como um argumento para o método.
- Como um `CancellationToken` passado para `MoveNextAsync` entrar no corpo do método?  Isso é ainda pior, como se fosse exposto a partir de um `iterator` objeto local, seu valor pode ser alterado em Awaits, o que significa que qualquer código registrado com o token precisaria cancelar o registro dele antes de aguardar e, em seguida, registrar novamente depois; também é potencialmente muito caro precisar fazer tal registro e cancelamento de registro em cada `MoveNextAsync` chamada, independentemente de ser implementado pelo compilador em um iterador ou por um desenvolvedor manualmente.
- Como um desenvolvedor cancela um loop de `foreach`?  Se for feito fornecendo um `CancellationToken` a um enumerável/enumerador e, em seguida, um), precisamos dar suporte aos enumeradores de `foreach`' ing over ', que os eleva a ser cidadãos de primeira classe e agora você precisa começar a pensar em um ecossistema criado em torno de enumeradores (por exemplo, métodos LINQ) ou b), precisamos inserir o `CancellationToken` no enumerable de qualquer forma, tendo algum `WithCancellation` método de extensão de `IAsyncEnumerable<T>` que armazenaria o token fornecido e, em seguida, passá-lo para o `GetAsyncEnumerator` Enumerable disposto quando o `GetAsyncEnumerator` na estrutura retornada é invocado (ignorando esse token).  Ou, você pode usar apenas o `CancellationToken` no corpo do foreach.
- Se/quando houver suporte a compreensão de consulta, como o `CancellationToken` fornecido para `GetEnumerator` ou `MoveNextAsync` ser passado para cada cláusula?  A maneira mais fácil seria simplesmente para a cláusula capturá-la e, nesse ponto, qualquer token é passado para `GetAsyncEnumerator`/`MoveNextAsync` é ignorado.

Uma versão anterior deste documento é recomendada (1), mas, como mudamos para (4).

Os dois principais problemas com (1):
- os produtores de enumeráveis canceláveis precisam implementar algum texto clichê e só podem aproveitar o suporte do compilador para iteradores assíncronos para implementar um método de `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)`.
- é provável que muitos produtores sejam tentados a simplesmente adicionar um parâmetro `CancellationToken` à sua assinatura Async-Enumerable, o que impedirá que os consumidores passem o token de cancelamento que desejam quando receberem um tipo de `IAsyncEnumerable`.

Há dois cenários de consumo principais:
1. `await foreach (var i in GetData(token)) ...` em que o consumidor chama o método Async-Iterator,
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` em que o consumidor lida com uma determinada instância de `IAsyncEnumerable`.

Descobrimos que um comprometimento razoável para dar suporte a ambos os cenários de uma maneira que seja conveniente para produtores e consumidores de transmissões assíncronas é usar um parâmetro especialmente anotado no método Async-Iterator. O atributo `[EnumeratorCancellation]` é usado para essa finalidade. Colocar esse atributo em um parâmetro informa ao compilador que, se um token for passado para o método `GetAsyncEnumerator`, esse token deverá ser usado em vez do valor passado originalmente para o parâmetro.

Considere o `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`. O implementador desse método pode simplesmente usar o parâmetro no corpo do método. O consumidor pode usar os padrões de consumo acima:
1. Se você usar `GetData(token)`, o token será salvo no Async-Enumerable e será usado na iteração,
2. Se você usar `givenIAsyncEnumerable.WithCancellation(token)`, o token passado para `GetAsyncEnumerator` substituirá qualquer token salvo no Async-Enumerable.

## <a name="foreach"></a>foreach

`foreach` será aumentado para dar suporte a `IAsyncEnumerable<T>` além de seu suporte existente para `IEnumerable<T>`.  E ele dará suporte ao equivalente de `IAsyncEnumerable<T>` como um padrão se os membros relevantes forem expostos publicamente, voltando ao uso da interface diretamente, caso contrário, para habilitar as extensões baseadas em struct que evitam a alocação, bem como o uso de awaitables alternativo como o tipo de retorno de `MoveNextAsync` e `DisposeAsync`.

### <a name="syntax"></a>Sintaxe

Usando a sintaxe:

```csharp
foreach (var i in enumerable)
```

C#continuará a tratar `enumerable` como um Enumerable síncrono, de modo que, mesmo que ele exponha as APIs relevantes para enumeráveis assíncronos (expondo o padrão ou implementando a interface), ele considerará apenas as APIs síncronas.

Para forçar `foreach` em vez disso, considere apenas as APIs assíncronas, `await` é inserido da seguinte maneira:

```csharp
await foreach (var i in enumerable)
```

Nenhuma sintaxe seria fornecida para dar suporte ao uso das APIs Async ou Sync; o desenvolvedor deve escolher com base na sintaxe usada.

Opções descartadas consideradas:
- _`foreach (var i in await enumerable)`_ : essa sintaxe já é válida e alterar seu significado seria uma alteração significativa.  Isso significa `await` o `enumerable`, obter algo de forma síncrona iterável a partir dele e, em seguida, iterar de forma síncrona.
- _`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_ : todos sugerem que estamos aguardando o próximo item, mas há outros Awaits envolvidos em foreach, especialmente se o Enumerable for um `IAsyncDisposable`, será `await`' ing seu descarte assíncrono.  O Await é como o escopo do foreach em vez de para cada elemento individual e, portanto, a palavra-chave `await` merece estar no nível de `foreach`.  Além disso, tê-lo associado ao `foreach` nos dá uma maneira de descrever o `foreach` com um termo diferente, por exemplo, um "Await foreach".  Mas o mais importante é que há um valor para considerar `foreach` sintaxe ao mesmo tempo que `using` sintaxe, para que eles permaneçam consistentes entre si e `using (await ...)` já seja uma sintaxe válida.
- _`foreach await (var i in enumerable)`_

Você ainda deve considerar:
- `foreach` hoje não oferece suporte à iteração por meio de um enumerador.  Esperamos que seja mais comum ter `IAsyncEnumerator<T>`s em um lugar e, portanto, é tentador dar suporte a `await foreach` com `IAsyncEnumerable<T>` e `IAsyncEnumerator<T>`.  Mas depois de adicionar esse suporte, ele apresenta a pergunta se `IAsyncEnumerator<T>` é um cidadão de primeira classe e se precisamos ter sobrecargas de combinadores que operam em enumeradores além de enumeráveis?    Queremos incentivar métodos para retornar enumeradores em vez de enumeráveis? Devemos continuar a discutir isso.  Se decidirmos que não queremos dar suporte a ela, talvez queiramos introduzir um método de extensão `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` que permitisse que um enumerador ainda fosse `foreach`.  Se decidirmos que queremos dar suporte a ela, também precisaremos decidir se a `await foreach` seria responsável por chamar `DisposeAsync` no enumerador, e a resposta é provavelmente "não, o controle sobre a alienação deve ser tratado por quem chamou `GetEnumerator`".

### <a name="pattern-based-compilation"></a>Compilação baseada em padrões

O compilador se associará às APIs baseadas em padrão, se existirem, preferirem aquelas usando a interface (o padrão pode ser satisfeito com métodos de instância ou métodos de extensão).  Os requisitos para o padrão são:
- O enumerável deve expor um método `GetAsyncEnumerator` que possa ser chamado sem argumentos e que retorne um enumerador que atenda ao padrão relevante.
- O enumerador deve expor um método `MoveNextAsync` que pode ser chamado sem argumentos e que retorna algo que pode ser `await`Ed e cujo `GetResult()` retorna um `bool`.
- O enumerador também deve expor `Current` propriedade cujo getter retorna um `T` representando o tipo de dados que está sendo enumerado.
- O enumerador pode, opcionalmente, expor um método `DisposeAsync` que pode ser invocado sem argumentos e que retorna algo que pode ser `await`Ed e cujo `GetResult()` retorna `void`.

Este código:

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

é convertido para o equivalente de:

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

Se o tipo iterado não expor o padrão correto, as interfaces serão usadas.

### <a name="configureawait"></a>ConfigureAwait

Essa compilação baseada em padrão permitirá que `ConfigureAwait` seja usado em todos os Awaits, por meio de um método de extensão `ConfigureAwait`:

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

Isso se baseará em tipos que também adicionaremos ao .NET, provavelmente a System. Threading. Tasks. Extensions. dll:

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

Observe que essa abordagem não permitirá que `ConfigureAwait` seja usado com enumeráveis baseados em padrões, mas, novamente, já é o caso de o `ConfigureAwait` ser exposto apenas como uma extensão em `Task`/`Task<T>`/`ValueTask`/e não pode ser aplicado a coisas notentes arbitrárias, pois faz sentido apenas quando aplicado às tarefas (ele controla um comportamento implementado no suporte à continuação da tarefa) e, portanto, não faz sentido ao usar um padrão em que as coisas awaitable podem não ser tarefas.`ValueTask<T>`  Qualquer pessoa que retornar coisas awaitable pode fornecer seu próprio comportamento personalizado nesses cenários avançados.

(Se pudermos surgir alguma maneira de dar suporte a uma solução de `ConfigureAwait` de nível de escopo ou assembly, isso não será necessário.)

## <a name="async-iterators"></a>Iteradores assíncronos

O idioma/compilador dará suporte à produção de `IAsyncEnumerable<T>`s e `IAsyncEnumerator<T>`s, além de consumi-los. Atualmente, a linguagem dá suporte à gravação de um iterador como:

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

Mas `await` não pode ser usado no corpo desses iteradores.  Adicionaremos esse suporte.

### <a name="syntax"></a>Sintaxe

O suporte de idioma existente para iteradores infere a natureza do iterador do método com base no fato de ele conter qualquer `yield`s.  O mesmo será verdadeiro para iteradores assíncronos.  Esses iteradores assíncronos serão demarcadas e diferenciados dos iteradores síncronos por meio da adição de `async` à assinatura e, em seguida, também devem ter `IAsyncEnumerable<T>` ou `IAsyncEnumerator<T>` como seu tipo de retorno.  Por exemplo, o exemplo acima poderia ser escrito como um iterador assíncrono da seguinte maneira:

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

Alternativas consideradas:
- _Não usar `async` na assinatura_: usar `async` provavelmente é tecnicamente exigido pelo compilador, pois ele o utiliza para determinar se `await` é válido nesse contexto.  Mas mesmo que não seja necessário, estabelecemos que `await` só podem ser usadas em métodos marcados como `async`, e parece importante manter a consistência.
- _Habilitando construtores personalizados para `IAsyncEnumerable<T>`_ : isso é algo que poderíamos examinar para o futuro, mas a maquinaidade é complicada e não damos suporte a isso para as contrapartes síncronas.
- _Tendo um `iterator` palavra-chave na assinatura: os_iteradores assíncronos usariam `async iterator` na assinatura e `yield` só poderiam ser usados em métodos `async` que incluíam `iterator`; em seguida, `iterator` se tornaria opcional em iteradores síncronos.  Dependendo da sua perspectiva, isso tem a vantagem de torná-lo muito claro pela assinatura do método se `yield` é permitido e se o método é realmente destinado a retornar instâncias do tipo `IAsyncEnumerable<T>` em vez da fabricação do compilador, com base em se o código usa `yield` ou não.  Mas é diferente dos iteradores síncronos, que não são e não podem ser feitos para exigir um.  Além disso, alguns desenvolvedores não gostam da sintaxe extra.  Se estivessemos projetando a partir do zero, provavelmente teríamos que fazer isso, mas neste ponto há muito mais valor em manter iteradores assíncronos próximos aos iteradores de sincronização.

## <a name="linq"></a>LINQ

Há mais de ~ 200 sobrecargas de métodos na classe `System.Linq.Enumerable`, todos os quais funcionam em termos de `IEnumerable<T>`; algumas dessas aceitam `IEnumerable<T>`, algumas delas produzem `IEnumerable<T>`e muitas fazem as duas.  Adicionar suporte a LINQ para `IAsyncEnumerable<T>` provavelmente envolveria a duplicação de todas essas sobrecargas para ela, para outro ~ 200.  E, como `IAsyncEnumerator<T>` é provavelmente mais comum como uma entidade autônoma no mundo assíncrono do que `IEnumerator<T>` está no mundo síncrono, poderíamos potencialmente precisar de outras sobrecargas de 200 que funcionam com `IAsyncEnumerator<T>`.  Além disso, um grande número de sobrecargas lida com predicados (por exemplo, `Where` que usa um `Func<T, bool>`), e pode ser desejável ter sobrecargas com base em `IAsyncEnumerable<T>`que lidam com predicados síncronos e assíncronos (por exemplo, `Func<T, ValueTask<bool>>` além de `Func<T, bool>`).  Embora isso não seja aplicável a todas as novas sobrecargas de agora ~ 400, um cálculo aproximado é que seria aplicável à metade, o que significa que outras ~ 200 sobrecargas, para um total de ~ 600 novos métodos.

Esse é um número cada vez maior de APIs, com o potencial para ainda mais quando bibliotecas de extensão como extensões interativas (IX) são consideradas.  Mas o IX já tem uma implementação de muitos desses, e parece que não há um grande motivo para duplicar esse trabalho; em vez disso, devemos ajudar a Comunidade a melhorar o IX e recomendar isso para quando os desenvolvedores desejarem usar o LINQ com `IAsyncEnumerable<T>`.

Também há o problema de sintaxe de compreensão da consulta.  A natureza baseada em padrões das compreensão das consultas permitiria "simplesmente funcionar" com alguns operadores, por exemplo, se o IX fornecer os seguintes métodos:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

Então, C# esse código vai "apenas funcionar":

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

No entanto, não há nenhuma sintaxe de compreensão de consulta que ofereça suporte ao uso de `await` nas cláusulas, portanto, se IX for adicionado, por exemplo:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

Então, isso "simplesmente funciona":

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

Mas não haveria nenhuma maneira de escrevê-lo com o `await` embutido na cláusula `select`.  Como um esforço separado, poderíamos examinar a adição de expressões de `async { ... }` ao idioma, no ponto em que poderíamos permitir que elas sejam usadas em compreensãos de consulta e, em vez disso, a seguinte poderia ser escrita como:

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

ou para permitir que `await` seja usado diretamente em expressões, como ao dar suporte a `async from`.  No entanto, é improvável que um design aqui afete o restante do conjunto de recursos de uma maneira ou o outro, e essa não é uma coisa particularmente de alto valor para investir no momento, portanto, a proposta é não fazer nada mais aqui no momento.

## <a name="integration-with-other-asynchronous-frameworks"></a>Integração com outras estruturas assíncronas

A integração com `IObservable<T>` e outras estruturas assíncronas (por exemplo, fluxos reativos) seria feita no nível da biblioteca, e não no nível de linguagem.  Por exemplo, todos os dados de um `IAsyncEnumerator<T>` podem ser publicados em um `IObserver<T>` simplesmente pelo `await foreach`' enatravés do enumerador e `OnNext`os dados para o observador, para que um método de extensão `AsObservable<T>` seja possível.  O consumo de um `IObservable<T>` em um `await foreach` exige o armazenamento em buffer dos dados (caso outro item seja enviado por push enquanto o item anterior ainda está sendo processado), mas esse adaptador pode ser facilmente implementado para permitir que um `IObservable<T>` seja extraído com um `IAsyncEnumerator<T>`.  Diante.  O RX/IX já fornece protótipos dessas implementações, e bibliotecas como https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels fornecem vários tipos de estruturas de dados em buffer.  O idioma não precisa estar envolvido neste estágio.
