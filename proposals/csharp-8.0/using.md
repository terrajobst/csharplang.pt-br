---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485052"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="6d4a7-101">"baseado em padrões usando" e "usando declarações"</span><span class="sxs-lookup"><span data-stu-id="6d4a7-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="6d4a7-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="6d4a7-102">Summary</span></span>

<span data-ttu-id="6d4a7-103">A linguagem adicionará dois novos recursos em relação à instrução de `using` para tornar o gerenciamento de recursos mais simples: `using` deve reconhecer um padrão descartável além de `IDisposable` e adicionar uma declaração de `using` ao idioma.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="6d4a7-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="6d4a7-104">Motivation</span></span>

<span data-ttu-id="6d4a7-105">A instrução `using` é uma ferramenta eficaz para o gerenciamento de recursos hoje, mas requer um pouco de cerimônia.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="6d4a7-106">Os métodos que têm um número de recursos para gerenciar podem ser sobrecargados sintaticamente com uma série de instruções de `using`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="6d4a7-107">Essa sobrecarga de sintaxe é suficiente para que a maioria das diretrizes de estilo de codificação tenha explicitamente uma exceção em chaves para esse cenário.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="6d4a7-108">A declaração de `using` remove grande parte da cerimônia aqui e C# se obtém em comparação com outras linguagens que incluem blocos de gerenciamento de recursos.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="6d4a7-109">Além disso, a `using` baseada em padrões permite que os desenvolvedores expandam o conjunto de tipos que podem participar aqui.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="6d4a7-110">Em muitos casos, remover a necessidade de criar tipos de wrapper que existem apenas para permitir um uso de valores em uma instrução `using`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="6d4a7-111">Juntos, esses recursos permitem que os desenvolvedores simplifiquem e expandam os cenários em que `using` podem ser aplicados.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6d4a7-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="6d4a7-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="6d4a7-113">usando declaração</span><span class="sxs-lookup"><span data-stu-id="6d4a7-113">using declaration</span></span>

<span data-ttu-id="6d4a7-114">O idioma permitirá que `using` sejam adicionados a uma declaração de variável local.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="6d4a7-115">Tal declaração terá o mesmo efeito que declarar a variável em uma instrução `using` no mesmo local.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="6d4a7-116">O tempo de vida de um `using` local será estendido para o final do escopo no qual ele é declarado.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="6d4a7-117">O `using` locais será Descartado na ordem inversa na qual eles são declarados.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="6d4a7-118">Não há restrições em relação à `goto`ou a qualquer outra construção de fluxo de controle diante de uma declaração de `using`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="6d4a7-119">Em vez disso, o código funciona exatamente como seria para a instrução `using` equivalente:</span><span class="sxs-lookup"><span data-stu-id="6d4a7-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="6d4a7-120">Um local declarado em uma `using` declaração local será implicitamente somente leitura.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="6d4a7-121">Isso corresponde ao comportamento de locais declarados em uma instrução `using`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="6d4a7-122">A gramática de idioma para declarações de `using` será a seguinte:</span><span class="sxs-lookup"><span data-stu-id="6d4a7-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="6d4a7-123">Restrições em relação à declaração de `using`:</span><span class="sxs-lookup"><span data-stu-id="6d4a7-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="6d4a7-124">Pode não aparecer diretamente dentro de um rótulo de `case`, mas deve estar dentro de um bloco dentro do rótulo de `case`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="6d4a7-125">Pode não aparecer como parte de uma declaração de variável `out`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="6d4a7-126">Deve ter um inicializador para cada Declarador.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="6d4a7-127">O tipo local deve ser implicitamente conversível para `IDisposable` ou para atender ao padrão de `using`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="6d4a7-128">baseado em padrões usando</span><span class="sxs-lookup"><span data-stu-id="6d4a7-128">pattern-based using</span></span>

<span data-ttu-id="6d4a7-129">A linguagem adicionará a noção de um padrão descartável: que é um tipo que tem um método de instância `Dispose` acessível.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="6d4a7-130">Os tipos que se ajustam ao padrão descartável podem participar de uma declaração ou instrução `using` sem precisar implementar `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="6d4a7-131">Isso permitirá que os desenvolvedores aproveitem `using` em vários cenários novos:</span><span class="sxs-lookup"><span data-stu-id="6d4a7-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="6d4a7-132">`ref struct`: esses tipos não podem implementar interfaces hoje e, portanto, não podem participar de instruções `using`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="6d4a7-133">Os métodos de extensão permitirão que os desenvolvedores ampliem tipos em outros assemblies para participarem de `using` instruções.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="6d4a7-134">Na situação em que um tipo pode ser convertido implicitamente em `IDisposable` e também se adapta ao padrão descartável, `IDisposable` será preferível.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="6d4a7-135">Embora isso assuma a abordagem oposta de `foreach` (o padrão é preferencial na interface), é necessário para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="6d4a7-136">As mesmas restrições de uma instrução de `using` tradicional também se aplicam aqui: variáveis locais declaradas no `using` são somente leitura, um valor de `null` não fará com que uma exceção seja gerada, etc... A geração de código só será diferente, pois não haverá uma conversão para `IDisposable` antes de chamar Dispose:</span><span class="sxs-lookup"><span data-stu-id="6d4a7-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="6d4a7-137">Para ajustar o padrão descartável, o método `Dispose` deve ser acessível, sem parâmetros e ter um tipo de retorno `void`.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="6d4a7-138">Não há nenhuma outra restrição.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-138">There are no other restrictions.</span></span> <span data-ttu-id="6d4a7-139">Isso significa explicitamente que os métodos de extensão podem ser usados aqui.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="6d4a7-140">Considerações</span><span class="sxs-lookup"><span data-stu-id="6d4a7-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="6d4a7-141">rótulos de caso sem blocos</span><span class="sxs-lookup"><span data-stu-id="6d4a7-141">case labels without blocks</span></span>

<span data-ttu-id="6d4a7-142">Um `using declaration` é ilegal diretamente dentro de um rótulo de `case` devido a complicações em relação ao seu tempo de vida real.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="6d4a7-143">Uma solução potencial é simplesmente dar a ela o mesmo tempo de vida como um `out var` no mesmo local.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="6d4a7-144">Foi considerada a complexidade extra para a implementação do recurso e a facilidade do trabalho (basta adicionar um bloco ao rótulo de `case`) não justificava a execução dessa rota.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="6d4a7-145">Expansões futuras</span><span class="sxs-lookup"><span data-stu-id="6d4a7-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="6d4a7-146">locais fixos</span><span class="sxs-lookup"><span data-stu-id="6d4a7-146">fixed locals</span></span>

<span data-ttu-id="6d4a7-147">Uma instrução `fixed` tem todas as propriedades de instruções `using` que motivavam a capacidade de ter `using` locais.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="6d4a7-148">A consideração deve ser fornecida para estender esse recurso para `fixed` locais também.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="6d4a7-149">As regras de tempo de vida e ordenação devem se aplicar igualmente bem ao `using` e `fixed` aqui.</span><span class="sxs-lookup"><span data-stu-id="6d4a7-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
