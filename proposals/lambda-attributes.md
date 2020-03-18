---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484569"
---
# <a name="lambda-attributes"></a><span data-ttu-id="d31f6-101">Atributos lambda</span><span class="sxs-lookup"><span data-stu-id="d31f6-101">Lambda Attributes</span></span>

* <span data-ttu-id="d31f6-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="d31f6-102">[x] Proposed</span></span>
* <span data-ttu-id="d31f6-103">[] Protótipo</span><span class="sxs-lookup"><span data-stu-id="d31f6-103">[ ] Prototype</span></span>
* <span data-ttu-id="d31f6-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="d31f6-104">[ ] Implementation</span></span>
* <span data-ttu-id="d31f6-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="d31f6-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="d31f6-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="d31f6-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d31f6-107">Permite que os atributos sejam aplicados a lambdas (e métodos anônimos) e a parâmetros de método Lambda/anônimo, pois eles podem estar em métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="d31f6-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="d31f6-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="d31f6-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d31f6-109">Duas motivações principais:</span><span class="sxs-lookup"><span data-stu-id="d31f6-109">Two primary motivations:</span></span>

1. <span data-ttu-id="d31f6-110">Para fornecer metadados visíveis para analisadores em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="d31f6-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="d31f6-111">Para fornecer metadados visíveis para reflexão e ferramentas em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="d31f6-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="d31f6-112">Como um exemplo de (1): para código sensível ao desempenho, é útil poder ter um analisador que sinaliza quando fechamentos e delegados estão sendo alocados para lambdas que fecham o estado.</span><span class="sxs-lookup"><span data-stu-id="d31f6-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="d31f6-113">Geralmente, um desenvolvedor desse código desaparecerá de sua maneira de evitar a captura de qualquer Estado, de modo que o compilador possa gerar um método estático e um delegado armazenável em cache para o método, ou o desenvolvedor garantirá que o único Estado que está sendo fechado seja `this`, permitindo ao compilador pelo menos para evitar alocar um objeto de fechamento.</span><span class="sxs-lookup"><span data-stu-id="d31f6-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="d31f6-114">Mas, sem o suporte de idioma para limitar o que pode ser capturado, é muito fácil fechar acidentalmente o estado.</span><span class="sxs-lookup"><span data-stu-id="d31f6-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="d31f6-115">Seria importante se um desenvolvedor pudesse anotar lambdas com atributos para indicar em que estado eles têm permissão para fechar, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="d31f6-116">Em seguida, um analisador pode ser escrito para sinalizar quando o estado é capturado incorretamente, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="d31f6-117">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="d31f6-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="d31f6-118">Usando a mesma sintaxe de atributo que em métodos normais, os atributos podem ser aplicados no início de um método lambda ou anônimo, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="d31f6-119">Para evitar ambigüidade como se um atributo se aplica ao método lambda ou a um dos argumentos, os atributos só podem ser usados quando parênteses são usados em todos os argumentos, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="d31f6-120">Com os métodos anônimos, os parênteses não são necessários para aplicar um atributo ao método antes da palavra-chave `delegate`, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="d31f6-121">Vários atributos podem ser aplicados, seja por meio de sintaxe padrão delimitada por vírgula ou por meio da sintaxe de atributo completo, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="d31f6-122">Os atributos podem ser aplicados aos parâmetros para um método anônimo ou lambda, mas somente quando parênteses são usados em qualquer argumento, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="d31f6-123">Vários atributos podem ser aplicados aos parâmetros de um método anônimo ou lambda, usando a sintaxe delimitada por vírgula ou de atributo completo, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="d31f6-124">os atributos de `return`de destino também podem ser usados em lambdas, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d31f6-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="d31f6-125">O compilador gera os atributos para o método gerado e os argumentos para esses métodos como faria para qualquer outro método.</span><span class="sxs-lookup"><span data-stu-id="d31f6-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d31f6-126">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="d31f6-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d31f6-127">N/D</span><span class="sxs-lookup"><span data-stu-id="d31f6-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="d31f6-128">Alternativas</span><span class="sxs-lookup"><span data-stu-id="d31f6-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d31f6-129">N/D</span><span class="sxs-lookup"><span data-stu-id="d31f6-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d31f6-130">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="d31f6-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="d31f6-131">N/D</span><span class="sxs-lookup"><span data-stu-id="d31f6-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d31f6-132">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="d31f6-132">Design meetings</span></span>

<span data-ttu-id="d31f6-133">N/D</span><span class="sxs-lookup"><span data-stu-id="d31f6-133">n/a</span></span>