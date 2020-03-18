---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484520"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="4149f-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="4149f-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="4149f-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="4149f-102">[x] Proposed</span></span>
* <span data-ttu-id="4149f-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="4149f-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="4149f-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="4149f-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="4149f-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="4149f-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="4149f-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="4149f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4149f-107">Permitir que os desenvolvedores capturem as expressões passadas para um método, a fim de permitir melhores mensagens de erro em APIs de diagnóstico/teste e reduzir pressionamentos de teclas.</span><span class="sxs-lookup"><span data-stu-id="4149f-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="4149f-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="4149f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4149f-109">Quando uma validação de asserção ou argumento falha, o desenvolvedor deseja saber o máximo possível sobre onde e por que ele falhou.</span><span class="sxs-lookup"><span data-stu-id="4149f-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="4149f-110">No entanto, as APIs de diagnóstico atuais não facilitam totalmente isso.</span><span class="sxs-lookup"><span data-stu-id="4149f-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="4149f-111">Considere o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="4149f-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="4149f-112">Quando uma das declarações falhar, somente o nome do arquivo, o número da linha e o do método serão fornecidos no rastreamento de pilha.</span><span class="sxs-lookup"><span data-stu-id="4149f-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="4149f-113">O desenvolvedor não conseguirá informar qual declaração falhou nessas informações--(s) ele terá que abrir o arquivo e navegar até o número de linha fornecido para ver o que deu errado.</span><span class="sxs-lookup"><span data-stu-id="4149f-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="4149f-114">Isso também é o motivo pelo qual as estruturas de teste precisam fornecer uma variedade de métodos Assert.</span><span class="sxs-lookup"><span data-stu-id="4149f-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="4149f-115">Com xUnit, `Assert.True` e `Assert.False` não são frequentemente usados porque não fornecem contexto suficiente sobre o que falhou.</span><span class="sxs-lookup"><span data-stu-id="4149f-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="4149f-116">Embora a situação seja um pouco melhor para a validação de argumento porque os nomes de argumentos inválidos são mostrados para o desenvolvedor, o desenvolvedor deve passar esses nomes para exceções manualmente.</span><span class="sxs-lookup"><span data-stu-id="4149f-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="4149f-117">Se o exemplo acima tiver sido reescrito para usar a validação de argumento tradicional em vez de `Debug.Assert`, ele ficaria como</span><span class="sxs-lookup"><span data-stu-id="4149f-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

<span data-ttu-id="4149f-118">Observe que `nameof(array)` deve ser passado para cada exceção, embora já esteja claro do contexto qual argumento é inválido.</span><span class="sxs-lookup"><span data-stu-id="4149f-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4149f-119">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="4149f-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="4149f-120">Nos exemplos acima, incluindo a cadeia de caracteres `"array != null"` ou `"array.Length == 1"` na mensagem Assert ajudaria o desenvolvedor a determinar o que falhou.</span><span class="sxs-lookup"><span data-stu-id="4149f-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="4149f-121">Insira `CallerArgumentExpression`: é um atributo que a estrutura pode usar para obter a cadeia de caracteres associada a um argumento de método específico.</span><span class="sxs-lookup"><span data-stu-id="4149f-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="4149f-122">Vamos adicioná-lo a `Debug.Assert` como</span><span class="sxs-lookup"><span data-stu-id="4149f-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="4149f-123">O código-fonte no exemplo acima permaneceria o mesmo.</span><span class="sxs-lookup"><span data-stu-id="4149f-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="4149f-124">No entanto, o código que o compilador realmente emite corresponde a</span><span class="sxs-lookup"><span data-stu-id="4149f-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="4149f-125">O compilador reconhece especialmente o atributo em `Debug.Assert`.</span><span class="sxs-lookup"><span data-stu-id="4149f-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="4149f-126">Ele passa a cadeia de caracteres associada ao argumento referido no construtor do atributo (nesse caso, `condition`) no site de chamada.</span><span class="sxs-lookup"><span data-stu-id="4149f-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="4149f-127">Quando a declaração falha, o desenvolvedor mostrará a condição que foi falsa e saberá qual delas falhou.</span><span class="sxs-lookup"><span data-stu-id="4149f-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="4149f-128">Para validação de argumento, o atributo não pode ser usado diretamente, mas pode ser usado por meio de uma classe auxiliar:</span><span class="sxs-lookup"><span data-stu-id="4149f-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

<span data-ttu-id="4149f-129">Uma proposta para adicionar tal classe auxiliar à estrutura está em andamento em https://github.com/dotnet/corefx/issues/17068.</span><span class="sxs-lookup"><span data-stu-id="4149f-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="4149f-130">Se esse recurso de idioma foi implementado, a proposta poderá ser atualizada para aproveitar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="4149f-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="4149f-131">Métodos de extensão</span><span class="sxs-lookup"><span data-stu-id="4149f-131">Extension methods</span></span>

<span data-ttu-id="4149f-132">O parâmetro `this` em um método de extensão pode ser referenciado por `CallerArgumentExpression`.</span><span class="sxs-lookup"><span data-stu-id="4149f-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="4149f-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4149f-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="4149f-134">`thisExpression` receberá a expressão correspondente ao objeto antes do ponto.</span><span class="sxs-lookup"><span data-stu-id="4149f-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="4149f-135">Se for chamado com a sintaxe do método estático, por exemplo, `Ext.ShouldBe(contestant.Points, 1337)`, ele se comportará como se o primeiro parâmetro não estivesse marcado como `this`.</span><span class="sxs-lookup"><span data-stu-id="4149f-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="4149f-136">Sempre deve haver uma expressão correspondente ao parâmetro `this`.</span><span class="sxs-lookup"><span data-stu-id="4149f-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="4149f-137">Mesmo que uma instância de uma classe chame um método de extensão por si só, por exemplo, `this.Single()` de dentro de um tipo de coleção, a `this` é obrigatória pelo compilador para que `"this"` seja passado.</span><span class="sxs-lookup"><span data-stu-id="4149f-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="4149f-138">Se essa regra for alterada no futuro, podemos considerar a possibilidade de passar `null` ou a cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="4149f-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="4149f-139">Detalhes adicionais</span><span class="sxs-lookup"><span data-stu-id="4149f-139">Extra details</span></span>

- <span data-ttu-id="4149f-140">Assim como os outros atributos de `Caller*`, como `CallerMemberName`, esse atributo só pode ser usado em parâmetros com valores padrão.</span><span class="sxs-lookup"><span data-stu-id="4149f-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="4149f-141">Vários parâmetros marcados com `CallerArgumentExpression` são permitidos, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="4149f-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="4149f-142">O namespace do atributo será `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="4149f-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="4149f-143">Se `null` ou uma cadeia de caracteres que não seja um nome de parâmetro (por exemplo, `"notAParameterName"`) for fornecida, o compilador passará uma cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="4149f-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="4149f-144">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="4149f-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="4149f-145">As pessoas que sabem como usar descompiladores poderão ver parte do código-fonte em sites de chamada para métodos marcados com esse atributo.</span><span class="sxs-lookup"><span data-stu-id="4149f-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="4149f-146">Isso pode ser indesejável/inesperado para software de código-fonte fechado.</span><span class="sxs-lookup"><span data-stu-id="4149f-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="4149f-147">Embora isso não seja uma falha no próprio recurso, uma fonte de preocupação pode ser que exista uma API de `Debug.Assert` hoje que usa apenas um `bool`.</span><span class="sxs-lookup"><span data-stu-id="4149f-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="4149f-148">Mesmo que a sobrecarga que pega uma mensagem tivesse seu segundo parâmetro marcado com esse atributo e tenha tornado opcional, o compilador ainda escolheria o não-Message na resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="4149f-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="4149f-149">Portanto, a sobrecarga de não mensagem teria de ser removida para tirar proveito desse recurso, que seria uma alteração significativa de quebra de binário (embora não de origem).</span><span class="sxs-lookup"><span data-stu-id="4149f-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4149f-150">Alternativas</span><span class="sxs-lookup"><span data-stu-id="4149f-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="4149f-151">Se for capaz de ver o código-fonte em sites de chamada para métodos que usam esse atributo se comprovem como um problema, podemos fazer a aceitação dos efeitos do atributo.</span><span class="sxs-lookup"><span data-stu-id="4149f-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="4149f-152">Os desenvolvedores irão habilitá-lo por meio de um atributo `[assembly: EnableCallerArgumentExpression]` em todo o assembly inseridos `AssemblyInfo.cs`.</span><span class="sxs-lookup"><span data-stu-id="4149f-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="4149f-153">No caso de os efeitos do atributo não serem habilitados, os métodos de chamada marcados com o atributo não seriam um erro, para permitir que os métodos existentes usem o atributo e mantenham a compatibilidade de origem.</span><span class="sxs-lookup"><span data-stu-id="4149f-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="4149f-154">No entanto, o atributo seria ignorado e o método seria chamado com qualquer valor padrão fornecido.</span><span class="sxs-lookup"><span data-stu-id="4149f-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- <span data-ttu-id="4149f-155">Para evitar que o [problema] [ drawbacks] de compatibilidade binária ocorra toda vez que desejarmos adicionar novas informações do chamador ao `Debug.Assert`, uma solução alternativa seria adicionar um struct `CallerInfo` à estrutura que contém todas as informações necessárias sobre o chamador.</span><span class="sxs-lookup"><span data-stu-id="4149f-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

<span data-ttu-id="4149f-156">Isso foi proposto originalmente em https://github.com/dotnet/csharplang/issues/87.</span><span class="sxs-lookup"><span data-stu-id="4149f-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="4149f-157">Há algumas desvantagens dessa abordagem:</span><span class="sxs-lookup"><span data-stu-id="4149f-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="4149f-158">Apesar de ser amigável de pagamento por reprodução, permitindo que você especifique quais propriedades são necessárias, ele ainda pode prejudicar o desempenho alocando uma matriz para as expressões/chamando `MethodBase.GetCurrentMethod` mesmo quando a declaração é aprovada.</span><span class="sxs-lookup"><span data-stu-id="4149f-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="4149f-159">Além disso, embora a passagem de um novo sinalizador para o atributo `CallerInfo` não seja uma alteração significativa, `Debug.Assert` não terá a garantia de que realmente receba esse novo parâmetro de sites de chamada que foram compilados em uma versão antiga do método.</span><span class="sxs-lookup"><span data-stu-id="4149f-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="4149f-160">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="4149f-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="4149f-161">TBD</span><span class="sxs-lookup"><span data-stu-id="4149f-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="4149f-162">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="4149f-162">Design meetings</span></span>

<span data-ttu-id="4149f-163">{1&gt;N/A&lt;1}</span><span class="sxs-lookup"><span data-stu-id="4149f-163">N/A</span></span>
