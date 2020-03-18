---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485010"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="6c8a0-101">Parâmetros eficientes e formatação de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="6c8a0-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="6c8a0-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="6c8a0-102">Summary</span></span>
<span data-ttu-id="6c8a0-103">Essa combinação de recursos aumentará a eficiência da formatação `string` valores e a passagem de argumentos de estilo de `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="6c8a0-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="6c8a0-104">Motivation</span></span>
<span data-ttu-id="6c8a0-105">A sobrecarga de alocação da formatação `string` valores pode dominar o desempenho de muitos aplicativos baseados em texto: da penalidade de Boxing dos tipos de `struct`, a alocação de `object[]` para `params` e as alocações de `string` intermediárias durante chamadas de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="6c8a0-106">Para manter a eficiência, esses aplicativos geralmente precisam abandonar os recursos de produtividade, como `params` e `string` interpolação e mudar para soluções codificadas não padrão.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="6c8a0-107">Considere o MSBuild como exemplo.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="6c8a0-108">Isso é escrito usando vários recursos modernos C# por desenvolvedores que estão preocupados com o desempenho.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="6c8a0-109">No entanto, em um modelo de compilação de exemplo, o MSBuild gerará 262MB de alocação de `string` usando o mínimo de detalhes.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="6c8a0-110">Desse 1/2 das alocações estão as alocações de vida curta dentro de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="6c8a0-111">Esses recursos poderiam remover grande parte disso do .NET desktop e colocá-lo em quase zero no .NET Core devido à disponibilidade do `Span<T>`</span><span class="sxs-lookup"><span data-stu-id="6c8a0-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="6c8a0-112">O conjunto de recursos de linguagem descritos aqui permitirá que os aplicativos continuem usando esses recursos, com pouca ou nenhuma variação na base de código do aplicativo, ao mesmo tempo que remove a sobrecarga de alocação não intencional na maioria dos casos.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6c8a0-113">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="6c8a0-113">Detailed Design</span></span> 
<span data-ttu-id="6c8a0-114">Há um conjunto de recursos que serão usados aqui para obter esses resultados:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="6c8a0-115">Expandir `params` para dar suporte a um conjunto mais amplo de tipos de coleção.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="6c8a0-116">Permitir que os desenvolvedores personalizem como `string` interpolação é obtida.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="6c8a0-117">Permitir que o `string` interpolado se associe a sobrecargas de `string.Format` mais eficientes.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="6c8a0-118">Estendendo parâmetros</span><span class="sxs-lookup"><span data-stu-id="6c8a0-118">Extending params</span></span>
<span data-ttu-id="6c8a0-119">O idioma permitirá `params` em uma assinatura de método para ter os tipos `Span<T>`, `ReadOnlySpan<T>` e `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="6c8a0-120">As mesmas regras para invocação serão aplicadas a esses novos tipos que se aplicam a `params T[]`:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="6c8a0-121">Não é possível sobrecarregar onde a única diferença é uma palavra-chave `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="6c8a0-122">Pode invocar passando uma série de argumentos que são conversíveis implicitamente para `T` ou um único `Span<T>` / 
`ReadOnlySpan<T>` / argumento `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="6c8a0-123">Deve ser o último parâmetro em uma assinatura de método.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="6c8a0-124">Etc...</span><span class="sxs-lookup"><span data-stu-id="6c8a0-124">Etc ...</span></span> 

<span data-ttu-id="6c8a0-125">As variantes `Span<T>` e `ReadOnlySpan<T>` serão referidas como `Span<T>` abaixo para manter a simplicidade.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="6c8a0-126">Nos casos em que o comportamento de `ReadOnlySpan<T>` difere, ele será explicitamente chamado.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="6c8a0-127">A vantagem do `Span<T>` variantes do `params` fornece isso, proporciona ao compilador uma grande flexibilidade na forma como aloca o armazenamento de backup para o valor de `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="6c8a0-128">Com um `params T[]` o compilador deve alocar um novo `T[]` para cada invocação de um método `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="6c8a0-129">Não é possível reutilizar porque ele deve assumir o receptor armazenado e reutilizado o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="6c8a0-130">Isso pode levar a uma grande ineficiência em métodos com muitas invocações de `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="6c8a0-131">Dadas `Span<T>` variantes são `ref struct` o receptor não pode armazenar o argumento.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="6c8a0-132">Portanto, o compilador pode otimizar os sites de chamada executando ações como reutilizar o argumento.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="6c8a0-133">Isso pode tornar as invocações repetidas muito eficientes em comparação com `T[]`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="6c8a0-134">No entanto, o idioma não fará nenhuma garantia específica sobre como esses CallSites são otimizados.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="6c8a0-135">Observe apenas que o compilador é livre para usar valores diferentes de `T[]` ao invocar um método de `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="6c8a0-136">Uma dessas implementações potenciais é a seguinte.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-136">One such potential implementation is the following.</span></span> <span data-ttu-id="6c8a0-137">Considere a invocação de todos os `params` em um corpo de método.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="6c8a0-138">O compilador poderia alocar uma matriz que tem um tamanho igual à maior invocação de `params` e usá-la para todas as invocações criando instâncias de `Span<T>` de tamanho adequado na matriz.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="6c8a0-139">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-139">For example:</span></span>

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

<span data-ttu-id="6c8a0-140">O compilador pode optar por emitir o corpo de `Go` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

<span data-ttu-id="6c8a0-141">Isso pode reduzir significativamente o número de matrizes alocadas em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="6c8a0-142">As alocações podem ser ainda mais reduzidas se o tempo de execução fornecer utilitários para alocação de pilha mais inteligente de matrizes.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="6c8a0-143">No entanto, essa otimização nem sempre pode ser aplicada.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="6c8a0-144">Embora o receptor não possa capturar o argumento `params`, ele ainda pode ser capturado no chamador quando há um `ref` ou um parâmetro `out / ref` que, por sua vez, é um tipo `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="6c8a0-145">No entanto, esses casos são detectáveis estaticamente.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-145">These cases are statically detectable though.</span></span> <span data-ttu-id="6c8a0-146">Ele potencialmente ocorre sempre que há um `ref` retorno ou um parâmetro `ref struct` passado por `out` ou `ref`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="6c8a0-147">Nesse caso, o compilador deve alocar uma `T[]` atualizada para cada invocação.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="6c8a0-148">Várias outras estratégias de otimização potenciais são discutidas no final deste documento.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="6c8a0-149">A variante de `IEnumerable<T>` é meramente uma sobrecarga de conveniência.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="6c8a0-150">Ele é útil em cenários que têm usos frequentes de `IEnumerable<T>`, mas também têm muito uso `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="6c8a0-151">Quando invocado no `T` argumento, o armazenamento de backup será alocado como um `T[]` assim que `params T[]` for feito hoje.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="6c8a0-152">alterações de resolução de sobrecarga de params</span><span class="sxs-lookup"><span data-stu-id="6c8a0-152">params overload resolution changes</span></span>
<span data-ttu-id="6c8a0-153">Essa proposta significa que a linguagem agora tem quatro variantes de `params` em que antes dela tinha uma.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="6c8a0-154">É sensato que os métodos definam sobrecargas de métodos que diferem apenas no tipo de declarações de `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="6c8a0-155">Considere que `StringBuilder.AppendFormat` certamente adicionaria uma sobrecarga de `params ReadOnlySpan<object>` além da `params object[]`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="6c8a0-156">Isso permitiria que ele melhorasse substancialmente o desempenho, reduzindo as alocações de coleta sem exigir nenhuma alteração no código de chamada.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="6c8a0-157">Para facilitar isso, a linguagem apresentará a seguinte regra de quebra de empate de resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="6c8a0-158">Quando os métodos candidatos diferem somente pelo parâmetro `params`, os candidatos serão preferidos na seguinte ordem:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="6c8a0-159">Essa ordem é a mais do que o menos eficiente para o caso geral.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="6c8a0-160">Variante</span><span class="sxs-lookup"><span data-stu-id="6c8a0-160">Variant</span></span>
<span data-ttu-id="6c8a0-161">CoreFX está criando um protótipo de um novo tipo gerenciado chamado [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span><span class="sxs-lookup"><span data-stu-id="6c8a0-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="6c8a0-162">Esse tipo destina-se a ser usado em APIs que esperam valores heterogêneos, mas que não desejam a sobrecarga trazida usando `object` como o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="6c8a0-163">O tipo de `Variant` fornece armazenamento universal, mas evita a alocação Boxing para os tipos usados com mais frequência.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="6c8a0-164">Usar esse tipo em APIs como `string.Format` pode eliminar a sobrecarga de Boxing na maioria dos casos.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="6c8a0-165">Esse tipo em si não é necessariamente especial para o idioma.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="6c8a0-166">Ele está sendo apresentado neste documento separadamente, embora se torne um detalhe de implementação de outras partes da proposta.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="6c8a0-167">Cadeias de caracteres interpoladas eficientes</span><span class="sxs-lookup"><span data-stu-id="6c8a0-167">Efficient interpolated strings</span></span>
<span data-ttu-id="6c8a0-168">As cadeias de caracteres interpoladas são um recurso popular C#, mas ineficiente, no.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="6c8a0-169">A sintaxe mais comum, usando um `string` interpolado como um `string`, se traduz em uma chamada de `string.Format(string, params object[])`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="6c8a0-170">Isso incorrerá em alocações de Boxing para todos os tipos de valor, as alocações de `string` intermediárias como a implementação usa amplamente `object.ToString` para formatação, bem como as alocações de matriz, uma vez que o número de argumentos excede a quantidade de parâmetros nas sobrecargas "rápidas" de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="6c8a0-171">O idioma alterará sua interpolação abaixo para considerar sobrecargas alternativas de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="6c8a0-172">Ele considerará todas as formas de `string.Format(string, params)` e escolherá a sobrecarga "melhor", que satisfaz os tipos de argumentos.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="6c8a0-173">A sobrecarga de `params` "melhor" será determinada pelas regras discutidas acima.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="6c8a0-174">Isso significa que os `string` interpolados agora podem ser associados a sobrecargas muito eficientes, como `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="6c8a0-175">Em muitos casos, isso removerá todas as alocações intermediárias.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="6c8a0-176">Cadeias de caracteres interpoladas personalizáveis</span><span class="sxs-lookup"><span data-stu-id="6c8a0-176">Customizable interpolated strings</span></span>
<span data-ttu-id="6c8a0-177">Os desenvolvedores podem personalizar o comportamento de cadeias de caracteres interpoladas com `FormattableString`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="6c8a0-178">Ele contém os dados que entram em uma cadeia de caracteres interpolada: o formato `string` e os argumentos como uma matriz.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="6c8a0-179">Embora ainda tenha a conversão boxing e a matriz de argumentos, bem como a alocação para `FormattableString` (é um `abstract class`).</span><span class="sxs-lookup"><span data-stu-id="6c8a0-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="6c8a0-180">Portanto, é de pouco uso para aplicativos que são de alocação pesada na formatação `string`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="6c8a0-181">Para tornar a formatação de cadeia de caracteres interpolada eficiente, a linguagem reconhecerá um novo tipo: `System.ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="6c8a0-182">Todas as cadeias de caracteres interpoladas terão uma conversão de tipo de destino para esse tipo.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="6c8a0-183">Isso será implementado pela conversão da cadeia de caracteres interpolada na chamada `ValueFormattableString.Create` exatamente como é feito para `FormattableString.Create` hoje.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="6c8a0-184">O idioma dará suporte a todas as opções de `params` descritas neste documento ao procurar o método de `ValueFormattableString.Create` mais adequado.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

<span data-ttu-id="6c8a0-185">As regras de resolução de sobrecarga serão alteradas para preferir `ValueFormattableString` sobre `string` quando o argumento for uma cadeia de caracteres interpolada.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="6c8a0-186">Isso significa que será valioso ter sobrecargas que diferem apenas em `string` e `ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="6c8a0-187">Atualmente, essa sobrecarga com `FormattableString` não é valiosa, pois o compilador sempre preferirá a versão `string` (a menos que o desenvolvedor Use uma conversão explícita).</span><span class="sxs-lookup"><span data-stu-id="6c8a0-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="6c8a0-188">Problemas em aberto</span><span class="sxs-lookup"><span data-stu-id="6c8a0-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="6c8a0-189">ValueFormattableString alteração significativa</span><span class="sxs-lookup"><span data-stu-id="6c8a0-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="6c8a0-190">A alteração para preferir `ValueFormattableString` durante a resolução de sobrecarga sobre `string` é uma alteração significativa.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="6c8a0-191">É possível que um desenvolvedor tenha definido um tipo chamado `ValueFormattableString` hoje e usá-lo em sobrecargas de método com `string`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="6c8a0-192">Essa alteração proposta faria com que o compilador selecionasse uma sobrecarga diferente depois que esse conjunto de recursos fosse implementado.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="6c8a0-193">A possibilidade de isso parece razoavelmente baixa.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="6c8a0-194">O tipo precisaria do nome completo `System.ValueFormattableString` e precisaria ter `static` métodos chamados `Create`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="6c8a0-195">Considerando que os desenvolvedores são altamente desencorajados de definir qualquer tipo no namespace de `System`, essa interrupção parece um comprometimento razoável.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="6c8a0-196">Expandindo para mais tipos</span><span class="sxs-lookup"><span data-stu-id="6c8a0-196">Expanding to more types</span></span>
<span data-ttu-id="6c8a0-197">Dado que estamos na área que devemos considerar adicionar `IList<T>`, `ICollection<T>` e `IReadOnlyList<T>` ao conjunto de coleções para os quais `params` tem suporte.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="6c8a0-198">Em termos de implementação, ele custará uma pequena quantia em relação ao outro trabalho aqui.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="6c8a0-199">O LDM precisa decidir se a complicação do idioma vale a pena.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="6c8a0-200">A adição de `IEnumerable<T>` remove um ponto de conflito muito específico.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="6c8a0-201">Sem esse `params` solução, muitos clientes foram forçados a alocar `T[]` de um `IEnumerable<T>` ao chamar um método `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="6c8a0-202">No entanto, a adição de `IEnumerable<T>` corrige isso.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="6c8a0-203">Não há um ponto de conflito específico que as outras interfaces corrigem aqui.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="6c8a0-204">Considerações</span><span class="sxs-lookup"><span data-stu-id="6c8a0-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="6c8a0-205">Variant2 e Variant3</span><span class="sxs-lookup"><span data-stu-id="6c8a0-205">Variant2 and Variant3</span></span>
<span data-ttu-id="6c8a0-206">A equipe do CoreFX também tem um conjunto de tipos de armazenamento não alocados para até três argumentos `Variant`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="6c8a0-207">Esses são um único `Variant`, `Variant2` e `Variant3`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="6c8a0-208">Todos têm um par de métodos para obter uma alocação livre `Span<Variant>` deles: `CreateSpan` e `KeepAlive`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="6c8a0-209">Isso significa que para um `params Span<Variant>` de até três argumentos, o site de chamada pode ser totalmente livre de alocação.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="6c8a0-210">O método `Go` pode ser reduzido para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-210">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

<span data-ttu-id="6c8a0-211">Isso requer muito pouco trabalho sobre a proposta para reutilizar `T[]` entre chamadas `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="6c8a0-212">O compilador já precisa gerenciar um temporário por chamada e fazer a limpeza do trabalho após (mesmo se, em um caso, estiver apenas marcando um temp interno como livre).</span><span class="sxs-lookup"><span data-stu-id="6c8a0-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="6c8a0-213">Observação: a função `KeepAlive` só é necessária na área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="6c8a0-214">No .NET Core, o método não estará disponível e, portanto, o compilador não emitirá uma chamada para ele.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="6c8a0-215">Auxiliares de alocação de pilha do CLR</span><span class="sxs-lookup"><span data-stu-id="6c8a0-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="6c8a0-216">O CLR só fornece [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) para alocação de pilha de memória contígua.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="6c8a0-217">Essa instrução é limitada, pois funciona apenas para tipos de `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="6c8a0-218">Isso significa que ele não pode ser usado como uma solução universal para alocar com eficiência o armazenamento de backup para `params 
 Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="6c8a0-219">Essa limitação não é uma restrição fundamental, mas, em vez disso, mais um artefato de histórico.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="6c8a0-220">O CLR pode optar por adicionar novos códigos de op/intrínsecos que fornecem alocação de pilha universal.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="6c8a0-221">Eles podem então ser usados para alocar o armazenamento de backup para a maioria das chamadas `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="6c8a0-222">O método `Go` pode ser reduzido para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-222">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

<span data-ttu-id="6c8a0-223">Embora essa abordagem seja muito eficiente de heap, isso causa um uso de pilha extra.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="6c8a0-224">Em um algoritmo que tem uma pilha profunda e muitos `params` uso, é possível que isso possa fazer com que uma `StackOverflowException` seja gerada onde uma simples alocação de `T[]` seria bem sucedido.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="6c8a0-225">Infelizmente C# , não está configurado para o tipo de análise entre métodos, onde poderia fazer uma determinação informada de se a chamada deve ou não usar a alocação de pilha ou heap de `params`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="6c8a0-226">Ele só pode realmente considerar cada chamada por conta própria.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="6c8a0-227">O CLR é a melhor configuração para fazer esse tipo de determinação no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="6c8a0-228">Portanto, provavelmente, o tempo de execução forneceria dois métodos para a alocação de pilha universal:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="6c8a0-229">`Span<T> StackAlloc<T>(int length)`: isso tem os mesmos comportamentos e limitações do `localloc`, exceto que ele pode funcionar em qualquer tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="6c8a0-230">`Span<T> MaybeStackAlloc<T>(int length)`: esse tempo de execução pode optar por implementar isso fazendo uma alocação de pilha ou heap.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="6c8a0-231">Em seguida, o tempo de execução pode usar o contexto de execução no qual é chamado para determinar como o `Span<T>` é alocado.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="6c8a0-232">O chamador, no entanto, sempre o tratará como se fosse uma pilha alocada.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="6c8a0-233">Para casos muito simples, como um para dois argumentos, o C# compilador sempre poderia usar `StackAlloc<T>` variante.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="6c8a0-234">Isso é improvável de contribuir significativamente para esgotamento de pilha na maioria dos casos.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="6c8a0-235">Para outros casos, o compilador pode optar por usar `MaybeStackAlloc<T>` em vez disso e permitir que o tempo de execução faça a chamada.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="6c8a0-236">A forma como escolheremos provavelmente exigirá uma investigação mais profunda e o exame de aplicativos do mundo real.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="6c8a0-237">Mas se esses novos intrínsecos estiverem disponíveis, eles fornecerão esse tipo de flexibilidade.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="6c8a0-238">Por que não é varargs?</span><span class="sxs-lookup"><span data-stu-id="6c8a0-238">Why not varargs?</span></span> 
<span data-ttu-id="6c8a0-239">O recurso de [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) existente foi considerado como uma solução possível.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="6c8a0-240">No entanto, esse recurso destina C++-se principalmente a cenários de/CLI e tem buracos conhecidos para outros cenários.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="6c8a0-241">Além disso, há um custo significativo na portabilidade para o UNIX.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="6c8a0-242">Portanto, ele não foi visto como uma solução viável.</span><span class="sxs-lookup"><span data-stu-id="6c8a0-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="6c8a0-243">Problemas relacionados</span><span class="sxs-lookup"><span data-stu-id="6c8a0-243">Related Issues</span></span>
<span data-ttu-id="6c8a0-244">Esta especificação está relacionada aos seguintes problemas:</span><span class="sxs-lookup"><span data-stu-id="6c8a0-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

