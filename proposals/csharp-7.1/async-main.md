---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484793"
---
# <a name="async-main"></a>Assíncrono principal

* [x] proposta
* [] Protótipo
* [] Implementação
* [] Especificação

## <a name="summary"></a>Resumo
[summary]: #summary

Permita que `await` seja usado no método Main/EntryPoint de um aplicativo, permitindo que o ponto de entrada retorne `Task` / `Task<int>` e seja marcado `async`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

É muito comum ao aprender C#, ao escrever utilitários baseados em console e ao escrever aplicativos de teste pequenos para desejarem chamar e `await` `async` métodos do principal.  Hoje, adicionamos um nível de complexidade, forçando tal `await`"a ser feito em um método assíncrono separado, o que faz com que os desenvolvedores precisem escrever clichê como o seguinte apenas para começar:

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

Podemos remover a necessidade desse texto clichê e torná-lo mais fácil de começar simplesmente permitindo que o principal seja `async` de forma que `await`s possa ser usado.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

No momento, as seguintes assinaturas são de entryPoints permitidas:

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

Estendemos a lista de entryPoints permitidos para incluir:

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

Para evitar riscos de compatibilidade, essas novas assinaturas só serão consideradas como entryPoints válidos se não houver sobrecargas do conjunto anterior.
O idioma/compilador não exigirá que o EntryPoint seja marcado como `async`, embora espere que a grande maioria dos usos será marcada como tal.

Quando um deles é identificado como EntryPoint, o compilador sintetiza um método EntryPoint real que chama um desses métodos codificados:
- ```static Task Main()``` resultará no compilador que emite o equivalente de ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` resultará no compilador que emite o equivalente de ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` resultará no compilador que emite o equivalente de ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` resultará no compilador que emite o equivalente de ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

Exemplo de uso:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

A principal desvantagem é simplesmente a complexidade adicional de dar suporte a assinaturas de EntryPoint adicionais.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Outras variantes consideradas:

Permitindo `async void`.  Precisamos manter a semântica o mesmo para o código chamando diretamente, o que dificultaria um ponto de entrada gerado para chamá-lo (nenhuma tarefa foi retornada).  Poderíamos resolver isso gerando dois outros métodos, por exemplo,

```csharp
public static async void Main()
{
   ... // await code
}
```

torna-se

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

Também há preocupações em relação à incentivação do uso de `async void`.

Usando "MainAsync" em vez de "Main" como o nome.  Embora o sufixo assíncrono seja recomendado para métodos de retorno de tarefas, trata-se principalmente da funcionalidade da biblioteca, que principal não é, e o suporte a nomes de EntryPoint adicionais além de "Main" não vale a pena.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

N/D

## <a name="design-meetings"></a>Criar reuniões

N/D
