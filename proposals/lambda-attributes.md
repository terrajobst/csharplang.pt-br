---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484569"
---
# <a name="lambda-attributes"></a>Atributos lambda

* [x] proposta
* [] Protótipo
* [] Implementação
* [] Especificação

## <a name="summary"></a>Resumo
[summary]: #summary

Permite que os atributos sejam aplicados a lambdas (e métodos anônimos) e a parâmetros de método Lambda/anônimo, pois eles podem estar em métodos regulares.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Duas motivações principais:

1. Para fornecer metadados visíveis para analisadores em tempo de compilação.
2. Para fornecer metadados visíveis para reflexão e ferramentas em tempo de execução.

Como um exemplo de (1): para código sensível ao desempenho, é útil poder ter um analisador que sinaliza quando fechamentos e delegados estão sendo alocados para lambdas que fecham o estado.  Geralmente, um desenvolvedor desse código desaparecerá de sua maneira de evitar a captura de qualquer Estado, de modo que o compilador possa gerar um método estático e um delegado armazenável em cache para o método, ou o desenvolvedor garantirá que o único Estado que está sendo fechado seja `this`, permitindo ao compilador pelo menos para evitar alocar um objeto de fechamento.  Mas, sem o suporte de idioma para limitar o que pode ser capturado, é muito fácil fechar acidentalmente o estado.  Seria importante se um desenvolvedor pudesse anotar lambdas com atributos para indicar em que estado eles têm permissão para fechar, por exemplo:

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

Em seguida, um analisador pode ser escrito para sinalizar quando o estado é capturado incorretamente, por exemplo:

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

- Usando a mesma sintaxe de atributo que em métodos normais, os atributos podem ser aplicados no início de um método lambda ou anônimo, por exemplo:

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- Para evitar ambigüidade como se um atributo se aplica ao método lambda ou a um dos argumentos, os atributos só podem ser usados quando parênteses são usados em todos os argumentos, por exemplo:

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- Com os métodos anônimos, os parênteses não são necessários para aplicar um atributo ao método antes da palavra-chave `delegate`, por exemplo:

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- Vários atributos podem ser aplicados, seja por meio de sintaxe padrão delimitada por vírgula ou por meio da sintaxe de atributo completo, por exemplo:

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- Os atributos podem ser aplicados aos parâmetros para um método anônimo ou lambda, mas somente quando parênteses são usados em qualquer argumento, por exemplo:

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- Vários atributos podem ser aplicados aos parâmetros de um método anônimo ou lambda, usando a sintaxe delimitada por vírgula ou de atributo completo, por exemplo:

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- os atributos de `return`de destino também podem ser usados em lambdas, por exemplo:

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- O compilador gera os atributos para o método gerado e os argumentos para esses métodos como faria para qualquer outro método.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

N/D

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

N/D

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

N/D

## <a name="design-meetings"></a>Criar reuniões

N/D