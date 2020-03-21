---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108383"
---
# <a name="function-pointers"></a><span data-ttu-id="4300b-101">Ponteiros de função</span><span class="sxs-lookup"><span data-stu-id="4300b-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="4300b-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="4300b-102">Summary</span></span>

<span data-ttu-id="4300b-103">Esta proposta fornece construções de linguagem que expõem opcodes IL que não podem ser acessados no momento com eficiência C# ou, atualmente, em hoje: `ldftn` e `calli`.</span><span class="sxs-lookup"><span data-stu-id="4300b-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="4300b-104">Esses opcodes de IL podem ser importantes em código de alto desempenho e os desenvolvedores precisam de uma maneira eficiente para acessá-los.</span><span class="sxs-lookup"><span data-stu-id="4300b-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="4300b-105">Motivação</span><span class="sxs-lookup"><span data-stu-id="4300b-105">Motivation</span></span>

<span data-ttu-id="4300b-106">As motivações e o plano de fundo desse recurso são descritos no seguinte problema (como é uma implementação potencial do recurso):</span><span class="sxs-lookup"><span data-stu-id="4300b-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="4300b-107">Esta é uma proposta de design alternativa para [intrínsecos do compilador](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span><span class="sxs-lookup"><span data-stu-id="4300b-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4300b-108">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="4300b-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="4300b-109">Ponteiros de função</span><span class="sxs-lookup"><span data-stu-id="4300b-109">Function pointers</span></span>

<span data-ttu-id="4300b-110">O idioma permitirá a declaração de ponteiros de função usando a sintaxe `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="4300b-111">A sintaxe completa é descrita detalhadamente na próxima seção, mas ela se destina a se assemelhar à sintaxe usada pelas declarações de tipo `Func` e `Action`.</span><span class="sxs-lookup"><span data-stu-id="4300b-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="4300b-112">Esses tipos são representados usando o tipo de ponteiro de função, conforme descrito em ECMA-335.</span><span class="sxs-lookup"><span data-stu-id="4300b-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="4300b-113">Isso significa que a invocação de um `delegate*` usará `calli` em que a invocação de um `delegate` usará `callvirt` no método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="4300b-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="4300b-114">Sintaticamente, embora a invocação seja idêntica para ambas as construções.</span><span class="sxs-lookup"><span data-stu-id="4300b-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="4300b-115">A definição ECMA-335 dos ponteiros de método inclui a Convenção de chamada como parte da assinatura de tipo (seção 7,1).</span><span class="sxs-lookup"><span data-stu-id="4300b-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="4300b-116">A Convenção de chamada padrão será `managed`.</span><span class="sxs-lookup"><span data-stu-id="4300b-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="4300b-117">Formulários alternativos podem ser especificados adicionando o modificador apropriado após a sintaxe de `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`ou `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="4300b-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="4300b-118">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="4300b-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="4300b-119">As conversões entre os tipos de `delegate*` são feitas com base em sua assinatura, incluindo a Convenção de chamada.</span><span class="sxs-lookup"><span data-stu-id="4300b-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="4300b-120">Um tipo de `delegate*` é um tipo de ponteiro que significa que ele tem todos os recursos e restrições de um tipo de ponteiro padrão:</span><span class="sxs-lookup"><span data-stu-id="4300b-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="4300b-121">Válido somente em um contexto de `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="4300b-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="4300b-122">Os métodos que contêm um parâmetro `delegate*` ou tipo de retorno só podem ser chamados de um contexto `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="4300b-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="4300b-123">Não pode ser convertido em `object`.</span><span class="sxs-lookup"><span data-stu-id="4300b-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="4300b-124">Não pode ser usado como um argumento genérico.</span><span class="sxs-lookup"><span data-stu-id="4300b-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="4300b-125">Pode converter implicitamente `delegate*` em `void*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="4300b-126">Pode converter explicitamente de `void*` para `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="4300b-127">Restrições:</span><span class="sxs-lookup"><span data-stu-id="4300b-127">Restrictions:</span></span>

- <span data-ttu-id="4300b-128">Atributos personalizados não podem ser aplicados a um `delegate*` ou a qualquer um de seus elementos.</span><span class="sxs-lookup"><span data-stu-id="4300b-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="4300b-129">Um parâmetro `delegate*` não pode ser marcado como `params`</span><span class="sxs-lookup"><span data-stu-id="4300b-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="4300b-130">Um tipo de `delegate*` tem todas as restrições de um tipo de ponteiro normal.</span><span class="sxs-lookup"><span data-stu-id="4300b-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="4300b-131">Sintaxe de ponteiro de função</span><span class="sxs-lookup"><span data-stu-id="4300b-131">Function pointer syntax</span></span>

<span data-ttu-id="4300b-132">A sintaxe de ponteiro de função completa é representada pela seguinte gramática:</span><span class="sxs-lookup"><span data-stu-id="4300b-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="4300b-133">A Convenção de chamada `unmanaged` representa a Convenção de chamada padrão para código nativo na plataforma atual e é codificada como Winapi tenha.</span><span class="sxs-lookup"><span data-stu-id="4300b-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="4300b-134">Todas as `calling_convention`s são palavras-chave contextuais quando precedidas por um `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="4300b-135">Conversões de ponteiro de função</span><span class="sxs-lookup"><span data-stu-id="4300b-135">Function pointer conversions</span></span>

<span data-ttu-id="4300b-136">Em um contexto sem segurança, o conjunto de conversões implícitas disponíveis (conversões implícitas) é estendido para incluir as seguintes conversões de ponteiro implícitas:</span><span class="sxs-lookup"><span data-stu-id="4300b-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="4300b-137">_Conversões existentes_</span><span class="sxs-lookup"><span data-stu-id="4300b-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="4300b-138">Em _funcptr\_tipo_ `F0` a outro _\_tipo_ `F1`, desde que todas as seguintes opções sejam verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="4300b-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="4300b-139">`F0` e `F1` têm o mesmo número de parâmetros, e cada `D0n` de parâmetro no `F0` tem os mesmos `ref`, `out`ou `in` modificadores que o parâmetro correspondente `D1n` em `F1`.</span><span class="sxs-lookup"><span data-stu-id="4300b-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="4300b-140">Para cada parâmetro de valor (um parâmetro sem `ref`, `out`ou modificador de `in`), uma conversão de identidade, conversão de referência implícita ou conversão de ponteiro implícita existe do tipo de parâmetro em `F0` para o tipo de parâmetro correspondente no `F1`.</span><span class="sxs-lookup"><span data-stu-id="4300b-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="4300b-141">Para cada parâmetro `ref`, `out`ou `in`, o tipo de parâmetro em `F0` é o mesmo que o tipo de parâmetro correspondente em `F1`.</span><span class="sxs-lookup"><span data-stu-id="4300b-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="4300b-142">Se o tipo de retorno for por valor (sem `ref` ou `ref readonly`), uma identidade, referência implícita ou conversão implícita do ponteiro existirá do tipo de retorno de `F1` para o tipo de retorno de `F0`.</span><span class="sxs-lookup"><span data-stu-id="4300b-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="4300b-143">Se o tipo de retorno for por referência (`ref` ou `ref readonly`), o tipo de retorno e os modificadores de `ref` de `F1` serão os mesmos do tipo de retorno e os modificadores de `ref` de `F0`.</span><span class="sxs-lookup"><span data-stu-id="4300b-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="4300b-144">A Convenção de chamada de `F0` é igual à Convenção de chamada de `F1`.</span><span class="sxs-lookup"><span data-stu-id="4300b-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="4300b-145">Permitir endereço para métodos de destino</span><span class="sxs-lookup"><span data-stu-id="4300b-145">Allow address-of to target methods</span></span>

<span data-ttu-id="4300b-146">Os grupos de métodos agora serão permitidos como argumentos para uma expressão de endereço.</span><span class="sxs-lookup"><span data-stu-id="4300b-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="4300b-147">O tipo de tal expressão será um `delegate*` que tem a assinatura equivalente do método de destino e uma Convenção de chamada gerenciada:</span><span class="sxs-lookup"><span data-stu-id="4300b-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="4300b-148">Em um contexto sem segurança, um método `M` é compatível com um tipo de ponteiro de função `F` se todas as seguintes opções forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="4300b-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="4300b-149">`M` e `F` têm o mesmo número de parâmetros, e cada parâmetro em `D` tem os mesmos `ref`, `out`ou modificadores de `in` como o parâmetro correspondente no `F`.</span><span class="sxs-lookup"><span data-stu-id="4300b-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="4300b-150">Para cada parâmetro de valor (um parâmetro sem `ref`, `out`ou modificador de `in`), uma conversão de identidade, conversão de referência implícita ou conversão de ponteiro implícita existe do tipo de parâmetro em `M` para o tipo de parâmetro correspondente no `F`.</span><span class="sxs-lookup"><span data-stu-id="4300b-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="4300b-151">Para cada parâmetro `ref`, `out`ou `in`, o tipo de parâmetro em `M` é o mesmo que o tipo de parâmetro correspondente em `F`.</span><span class="sxs-lookup"><span data-stu-id="4300b-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="4300b-152">Se o tipo de retorno for por valor (sem `ref` ou `ref readonly`), uma identidade, referência implícita ou conversão implícita do ponteiro existirá do tipo de retorno de `F` para o tipo de retorno de `M`.</span><span class="sxs-lookup"><span data-stu-id="4300b-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="4300b-153">Se o tipo de retorno for por referência (`ref` ou `ref readonly`), o tipo de retorno e os modificadores de `ref` de `F` serão os mesmos do tipo de retorno e os modificadores de `ref` de `M`.</span><span class="sxs-lookup"><span data-stu-id="4300b-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="4300b-154">A Convenção de chamada de `M` é igual à Convenção de chamada de `F`.</span><span class="sxs-lookup"><span data-stu-id="4300b-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="4300b-155">`M` é um método estático.</span><span class="sxs-lookup"><span data-stu-id="4300b-155">`M` is a static method.</span></span>

<span data-ttu-id="4300b-156">Em um contexto não seguro, existe uma conversão implícita de uma expressão de endereço cujo destino é um grupo de métodos `E` a um tipo de ponteiro de função compatível `F` se `E` contiver pelo menos um método aplicável em seu formato normal a uma lista de argumentos construída pelo uso dos tipos de parâmetro e modificadores de `F`, conforme descrito no seguinte.</span><span class="sxs-lookup"><span data-stu-id="4300b-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="4300b-157">Um único método `M` é selecionado correspondente a uma invocação de método do formulário `E(A)` com as seguintes modificações:</span><span class="sxs-lookup"><span data-stu-id="4300b-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="4300b-158">A lista de argumentos `A` é uma lista de expressões, cada uma classificada como uma variável e com o tipo e o modificador (`ref`, `out`ou `in`) do _parâmetro\_formal correspondente\_lista_ de `D`.</span><span class="sxs-lookup"><span data-stu-id="4300b-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="4300b-159">Os métodos candidatos são apenas os métodos que são aplicáveis em seu formato normal, não aqueles aplicáveis em sua forma expandida.</span><span class="sxs-lookup"><span data-stu-id="4300b-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="4300b-160">Os métodos candidatos são apenas os métodos que são estáticos.</span><span class="sxs-lookup"><span data-stu-id="4300b-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="4300b-161">Se o algoritmo das invocações de método produzir um erro, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4300b-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="4300b-162">Caso contrário, o algoritmo produz um único método melhor `M` ter o mesmo número de parâmetros que `F` e a conversão é considerada como existe.</span><span class="sxs-lookup"><span data-stu-id="4300b-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="4300b-163">O método selecionado `M` deve ser compatível (conforme definido acima) com o tipo de ponteiro de função `F`.</span><span class="sxs-lookup"><span data-stu-id="4300b-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="4300b-164">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4300b-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="4300b-165">O resultado da conversão é um ponteiro de função do tipo `F`.</span><span class="sxs-lookup"><span data-stu-id="4300b-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="4300b-166">Há uma conversão implícita de uma expressão de endereço cujo destino é um grupo de métodos `E` para `void*` se houver apenas um método estático `M` em `E`.</span><span class="sxs-lookup"><span data-stu-id="4300b-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="4300b-167">Se houver um método estático, o melhor método de `E` será `M`.</span><span class="sxs-lookup"><span data-stu-id="4300b-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="4300b-168">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4300b-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="4300b-169">Isso significa que os desenvolvedores podem depender das regras de resolução de sobrecarga para trabalhar em conjunto com o operador address-of:</span><span class="sxs-lookup"><span data-stu-id="4300b-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="4300b-170">O operador address-of será implementado usando a instrução `ldftn`.</span><span class="sxs-lookup"><span data-stu-id="4300b-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="4300b-171">Restrições deste recurso:</span><span class="sxs-lookup"><span data-stu-id="4300b-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="4300b-172">Aplica-se somente a métodos marcados como `static`.</span><span class="sxs-lookup"><span data-stu-id="4300b-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="4300b-173">Funções locais não`static` não podem ser usadas em `&`.</span><span class="sxs-lookup"><span data-stu-id="4300b-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="4300b-174">Os detalhes de implementação desses métodos são deliberadamente não especificados pelo idioma.</span><span class="sxs-lookup"><span data-stu-id="4300b-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="4300b-175">Isso inclui se eles são estáticos versus instância ou exatamente em qual assinatura eles são emitidos.</span><span class="sxs-lookup"><span data-stu-id="4300b-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="4300b-176">Melhor membro da função</span><span class="sxs-lookup"><span data-stu-id="4300b-176">Better function member</span></span>

<span data-ttu-id="4300b-177">A melhor especificação de membro de função será alterada para incluir a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="4300b-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="4300b-178">Uma `delegate*` é mais específica do que `void*`</span><span class="sxs-lookup"><span data-stu-id="4300b-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="4300b-179">Isso significa que é possível sobrecarregar `void*` e uma `delegate*` e ainda facilmente usar o operador address-of.</span><span class="sxs-lookup"><span data-stu-id="4300b-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="4300b-180">Problemas em aberto</span><span class="sxs-lookup"><span data-stu-id="4300b-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="4300b-181">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="4300b-181">NativeCallableAttribute</span></span>

<span data-ttu-id="4300b-182">Esse é um atributo usado pelo CLR para evitar o prólogo gerenciado para nativo ao invocar.</span><span class="sxs-lookup"><span data-stu-id="4300b-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="4300b-183">Os métodos marcados por este atributo só podem ser chamados de código nativo, não gerenciado (não é possível chamar métodos, criar um delegado, etc...).</span><span class="sxs-lookup"><span data-stu-id="4300b-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="4300b-184">O atributo não é especial para mscorlib; o tempo de execução tratará qualquer atributo com esse nome com a mesma semântica.</span><span class="sxs-lookup"><span data-stu-id="4300b-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="4300b-185">É possível que o tempo de execução e a linguagem funcionem juntos para dar suporte total a isso.</span><span class="sxs-lookup"><span data-stu-id="4300b-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="4300b-186">O idioma pode optar por tratar os membros de `static` de endereço com um atributo `NativeCallable` como um `delegate*` com a Convenção de chamada especificada.</span><span class="sxs-lookup"><span data-stu-id="4300b-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="4300b-187">Além disso, a linguagem provavelmente também desejaria:</span><span class="sxs-lookup"><span data-stu-id="4300b-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="4300b-188">Sinalizar todas as chamadas gerenciadas para um método marcado com `NativeCallable` como um erro.</span><span class="sxs-lookup"><span data-stu-id="4300b-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="4300b-189">Considerando que a função não pode ser invocada do código gerenciado, o compilador deve impedir que os desenvolvedores tentem essa invocação.</span><span class="sxs-lookup"><span data-stu-id="4300b-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="4300b-190">Impeça conversões de grupo de métodos para `delegate` quando o método é marcado com `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="4300b-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="4300b-191">No entanto, isso não é necessário para dar suporte a `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="4300b-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="4300b-192">O compilador pode dar suporte ao atributo `NativeCallable` como está usando a sintaxe existente.</span><span class="sxs-lookup"><span data-stu-id="4300b-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="4300b-193">O programa simplesmente precisaria ser convertido em `void*` antes da conversão para a assinatura de `delegate*` correta.</span><span class="sxs-lookup"><span data-stu-id="4300b-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="4300b-194">Isso não seria pior do que o suporte atualmente.</span><span class="sxs-lookup"><span data-stu-id="4300b-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="4300b-195">Conjunto extensível de convenções de chamada não gerenciadas</span><span class="sxs-lookup"><span data-stu-id="4300b-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="4300b-196">O conjunto de convenções de chamada não gerenciadas com suporte das codificações ECMA-335 atuais está desatualizado.</span><span class="sxs-lookup"><span data-stu-id="4300b-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="4300b-197">Vimos solicitações para adicionar suporte para mais convenções de chamada não gerenciadas, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4300b-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="4300b-198">https://github.com/dotnet/coreclr/issues/12120 [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall)</span><span class="sxs-lookup"><span data-stu-id="4300b-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="4300b-199">StdCall com este https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 explícito</span><span class="sxs-lookup"><span data-stu-id="4300b-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="4300b-200">O design desse recurso deve permitir a extensão do conjunto de convenções de chamada não gerenciadas conforme necessário no futuro.</span><span class="sxs-lookup"><span data-stu-id="4300b-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="4300b-201">Os problemas incluem espaço limitado para codificar convenções de chamada (12 de 16 valores são obtidos em `IMAGE_CEE_CS_CALLCONV_MASK`) e número de casas que precisam ser tocadas para adicionar uma nova Convenção de chamada.</span><span class="sxs-lookup"><span data-stu-id="4300b-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="4300b-202">Uma solução potencial é apresentar uma nova codificação que representa a Convenção de chamada usando [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enumeração.</span><span class="sxs-lookup"><span data-stu-id="4300b-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="4300b-203">Para referência, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h tem a lista de convenções de chamada com suporte do LLVM.</span><span class="sxs-lookup"><span data-stu-id="4300b-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="4300b-204">Embora seja improvável que o .NET precise dar suporte a todos eles, ele demonstra que o espaço das convenções de chamada é muito avançado.</span><span class="sxs-lookup"><span data-stu-id="4300b-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="4300b-205">Considerações</span><span class="sxs-lookup"><span data-stu-id="4300b-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="4300b-206">Permitir métodos de instância</span><span class="sxs-lookup"><span data-stu-id="4300b-206">Allow instance methods</span></span>

<span data-ttu-id="4300b-207">A proposta pode ser estendida para dar suporte a métodos de instância aproveitando a Convenção de chamada da CLI `EXPLICITTHIS` C# (chamada `instance` no código).</span><span class="sxs-lookup"><span data-stu-id="4300b-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="4300b-208">Essa forma de ponteiros de função da CLI coloca o parâmetro `this` como um primeiro parâmetro explícito da sintaxe do ponteiro de função.</span><span class="sxs-lookup"><span data-stu-id="4300b-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="4300b-209">Isso é bom, mas adiciona certa complicação à proposta.</span><span class="sxs-lookup"><span data-stu-id="4300b-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="4300b-210">Particularmente, como os ponteiros de função que diferem pela Convenção de chamada `instance` e `managed` seriam incompatíveis, embora ambos os casos sejam usados para invocar métodos gerenciados com a mesma C# assinatura.</span><span class="sxs-lookup"><span data-stu-id="4300b-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="4300b-211">Além disso, em cada caso, considerado como seria valioso ter uma solução simples: Use um `static` função local.</span><span class="sxs-lookup"><span data-stu-id="4300b-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="4300b-212">Não requer segurança na declaração</span><span class="sxs-lookup"><span data-stu-id="4300b-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="4300b-213">Em vez de exigir `unsafe` em cada uso de um `delegate*`, só é necessário no ponto em que um grupo de métodos é convertido em um `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="4300b-214">É aí que os problemas de segurança principal entram em cena (sabendo que o assembly recipiente não pode ser descarregado enquanto o valor estiver ativo).</span><span class="sxs-lookup"><span data-stu-id="4300b-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="4300b-215">Exigir `unsafe` em outros locais pode ser visto como excessivo.</span><span class="sxs-lookup"><span data-stu-id="4300b-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="4300b-216">É assim que o design foi originalmente planejado.</span><span class="sxs-lookup"><span data-stu-id="4300b-216">This is how the design was originally intended.</span></span> <span data-ttu-id="4300b-217">Mas as regras de linguagem resultantes pareciam muito estranhas.</span><span class="sxs-lookup"><span data-stu-id="4300b-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="4300b-218">É impossível ocultar o fato de que esse é um valor de ponteiro e que ele continua exibindo até mesmo sem a palavra-chave `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="4300b-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="4300b-219">Por exemplo, a conversão em `object` não pode ser permitida, não pode ser um membro de um `class`, etc... O C# design é exigir `unsafe` para todos os usos de ponteiro e, portanto, esse design é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="4300b-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="4300b-220">Os desenvolvedores ainda poderão apresentar um wrapper _seguro_ sobre `delegate*` valores da mesma maneira que fazem para tipos de ponteiros normais hoje em dia.</span><span class="sxs-lookup"><span data-stu-id="4300b-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="4300b-221">Considere:</span><span class="sxs-lookup"><span data-stu-id="4300b-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="4300b-222">Usando delegados</span><span class="sxs-lookup"><span data-stu-id="4300b-222">Using delegates</span></span>

<span data-ttu-id="4300b-223">Em vez de usar um novo elemento de sintaxe, `delegate*`, basta usar tipos de `delegate` existentes com um `*` seguindo o tipo:</span><span class="sxs-lookup"><span data-stu-id="4300b-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="4300b-224">A manipulação de Convenção de chamada pode ser feita anotando os tipos de `delegate` com um atributo que especifica um valor de `CallingConvention`.</span><span class="sxs-lookup"><span data-stu-id="4300b-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="4300b-225">A falta de um atributo significaria a Convenção de chamada gerenciada.</span><span class="sxs-lookup"><span data-stu-id="4300b-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="4300b-226">Codificar isso no IL é problemático.</span><span class="sxs-lookup"><span data-stu-id="4300b-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="4300b-227">O valor subjacente precisa ser representado como um ponteiro, ainda que ele também deva:</span><span class="sxs-lookup"><span data-stu-id="4300b-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="4300b-228">Ter um tipo exclusivo para permitir sobrecargas com tipos diferentes de ponteiro de função.</span><span class="sxs-lookup"><span data-stu-id="4300b-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="4300b-229">Ser equivalente para fins de OHI em limites de assembly.</span><span class="sxs-lookup"><span data-stu-id="4300b-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="4300b-230">O último ponto é particularmente problemático.</span><span class="sxs-lookup"><span data-stu-id="4300b-230">The last point is particularly problematic.</span></span> <span data-ttu-id="4300b-231">Isso significa que cada assembly que usa `Func<int>*` deve codificar um tipo equivalente em metadados, mesmo que `Func<int>*` seja definido em um assembly, embora não controle.</span><span class="sxs-lookup"><span data-stu-id="4300b-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="4300b-232">Além disso, qualquer outro tipo que é definido com o nome `System.Func<T>` em um assembly que não seja mscorlib deve ser diferente da versão definida em mscorlib.</span><span class="sxs-lookup"><span data-stu-id="4300b-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="4300b-233">Uma opção que foi explorada foi emitir um ponteiro como `mod_req(Func<int>) void*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="4300b-234">Embora isso não funcione como um `mod_req` não pode se associar a um `TypeSpec` e, portanto, não pode direcionar instanciações genéricas.</span><span class="sxs-lookup"><span data-stu-id="4300b-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="4300b-235">Ponteiros de função nomeados</span><span class="sxs-lookup"><span data-stu-id="4300b-235">Named function pointers</span></span>

<span data-ttu-id="4300b-236">A sintaxe do ponteiro de função pode ser complicada, particularmente em casos complexos, como ponteiros de funções aninhadas.</span><span class="sxs-lookup"><span data-stu-id="4300b-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="4300b-237">Em vez de os desenvolvedores digitarem a assinatura sempre que o idioma puder permitir declarações nomeadas de ponteiros de função, como é feito com `delegate`.</span><span class="sxs-lookup"><span data-stu-id="4300b-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="4300b-238">Parte do problema aqui é que o primitivo de CLI subjacente não tem nomes, portanto, isso seria C# puramente uma invenção e requer um pouco de trabalho de metadados para habilitar.</span><span class="sxs-lookup"><span data-stu-id="4300b-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="4300b-239">Isso é factível, mas é um grande respeito do trabalho.</span><span class="sxs-lookup"><span data-stu-id="4300b-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="4300b-240">Basicamente, é C# necessário ter um complemento para a tabela de tipo def puramente para esses nomes.</span><span class="sxs-lookup"><span data-stu-id="4300b-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="4300b-241">Além disso, quando os argumentos dos ponteiros de função nomeados foram examinados, eles poderiam ser aplicados igualmente bem a vários outros cenários.</span><span class="sxs-lookup"><span data-stu-id="4300b-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="4300b-242">Por exemplo, seria conveniente declarar tuplas nomeadas para reduzir a necessidade de digitar a assinatura completa em todos os casos.</span><span class="sxs-lookup"><span data-stu-id="4300b-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="4300b-243">Após a discussão, decidimos não permitir a declaração nomeada de tipos de `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="4300b-244">Se encontrarmos uma necessidade significativa para isso com base nos comentários de uso do cliente, investigaremos uma solução de nomenclatura que funciona para ponteiros de função, tuplas, genéricos, etc... Isso provavelmente será semelhante em forma de outras sugestões, como suporte completo a `typedef` no idioma.</span><span class="sxs-lookup"><span data-stu-id="4300b-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="4300b-245">Considerações futuras</span><span class="sxs-lookup"><span data-stu-id="4300b-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="4300b-246">funções locais estáticas</span><span class="sxs-lookup"><span data-stu-id="4300b-246">static local functions</span></span>

<span data-ttu-id="4300b-247">Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/1565) para permitir o modificador de `static` em funções locais.</span><span class="sxs-lookup"><span data-stu-id="4300b-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="4300b-248">Essa função seria garantida para ser emitida como `static` e com a assinatura exata especificada no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="4300b-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="4300b-249">Essa função deve ser um argumento válido para `&`, pois ela contém nenhum dos problemas que as funções locais têm hoje</span><span class="sxs-lookup"><span data-stu-id="4300b-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="4300b-250">delegados estáticos</span><span class="sxs-lookup"><span data-stu-id="4300b-250">static delegates</span></span>

<span data-ttu-id="4300b-251">Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/302) para permitir a declaração de tipos de `delegate` que só podem se referir a membros `static`.</span><span class="sxs-lookup"><span data-stu-id="4300b-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="4300b-252">A vantagem é que essas instâncias de `delegate` podem ser de alocação gratuita e melhor em cenários de desempenho.</span><span class="sxs-lookup"><span data-stu-id="4300b-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="4300b-253">Se o recurso de ponteiro de função for implementado, a proposta de `static delegate` provavelmente será fechada. A vantagem proposta desse recurso é a natureza livre de alocação.</span><span class="sxs-lookup"><span data-stu-id="4300b-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="4300b-254">No entanto, foram encontradas investigações recentes que não podem ser obtidas devido ao descarregamento do assembly.</span><span class="sxs-lookup"><span data-stu-id="4300b-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="4300b-255">Deve haver um identificador forte do `static delegate` ao método ao qual se refere para impedir que o assembly seja descarregado de dentro dele.</span><span class="sxs-lookup"><span data-stu-id="4300b-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="4300b-256">Para manter cada instância de `static delegate` seria necessário alocar um novo identificador que executa um contador para as metas da proposta.</span><span class="sxs-lookup"><span data-stu-id="4300b-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="4300b-257">Havia alguns designs em que a alocação poderia ser amortizada para uma única alocação por site de chamada, mas isso era um pouco complexo e não parece que vale a pena compensar.</span><span class="sxs-lookup"><span data-stu-id="4300b-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="4300b-258">Isso significa que os desenvolvedores têm, essencialmente, decidir entre as seguintes compensações:</span><span class="sxs-lookup"><span data-stu-id="4300b-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="4300b-259">Segurança diante do descarregamento do assembly: isso requer alocações e, portanto, `delegate` já é uma opção suficiente.</span><span class="sxs-lookup"><span data-stu-id="4300b-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="4300b-260">Não há segurança em face de descarregamento de assembly: Use um `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="4300b-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="4300b-261">Isso pode ser encapsulado em um `struct` para permitir o uso fora de um contexto de `unsafe` no restante do código.</span><span class="sxs-lookup"><span data-stu-id="4300b-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
