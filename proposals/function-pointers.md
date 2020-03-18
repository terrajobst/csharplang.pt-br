---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79485500"
---
# <a name="function-pointers"></a><span data-ttu-id="68fcf-101">Ponteiros de função</span><span class="sxs-lookup"><span data-stu-id="68fcf-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="68fcf-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="68fcf-102">Summary</span></span>

<span data-ttu-id="68fcf-103">Esta proposta fornece construções de linguagem que expõem opcodes IL que não podem ser acessados no momento com eficiência C# ou, atualmente, em hoje: `ldftn` e `calli`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="68fcf-104">Esses opcodes de IL podem ser importantes em código de alto desempenho e os desenvolvedores precisam de uma maneira eficiente para acessá-los.</span><span class="sxs-lookup"><span data-stu-id="68fcf-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="68fcf-105">Motivação</span><span class="sxs-lookup"><span data-stu-id="68fcf-105">Motivation</span></span>

<span data-ttu-id="68fcf-106">As motivações e o plano de fundo desse recurso são descritos no seguinte problema (como é uma implementação potencial do recurso):</span><span class="sxs-lookup"><span data-stu-id="68fcf-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="68fcf-107">Esta é uma proposta de design alternativa para [intrínsecos do compilador](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span><span class="sxs-lookup"><span data-stu-id="68fcf-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="68fcf-108">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="68fcf-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="68fcf-109">Ponteiros de função</span><span class="sxs-lookup"><span data-stu-id="68fcf-109">Function pointers</span></span>

<span data-ttu-id="68fcf-110">O idioma permitirá a declaração de ponteiros de função usando a sintaxe `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="68fcf-111">A sintaxe completa é descrita detalhadamente na próxima seção, mas ela se destina a se assemelhar à sintaxe usada pelas declarações de tipo `Func` e `Action`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="68fcf-112">Esses tipos são representados usando o tipo de ponteiro de função, conforme descrito em ECMA-335.</span><span class="sxs-lookup"><span data-stu-id="68fcf-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="68fcf-113">Isso significa que a invocação de um `delegate*` usará `calli` em que a invocação de um `delegate` usará `callvirt` no método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="68fcf-114">Sintaticamente, embora a invocação seja idêntica para ambas as construções.</span><span class="sxs-lookup"><span data-stu-id="68fcf-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="68fcf-115">A definição ECMA-335 dos ponteiros de método inclui a Convenção de chamada como parte da assinatura de tipo (seção 7,1).</span><span class="sxs-lookup"><span data-stu-id="68fcf-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="68fcf-116">A Convenção de chamada padrão será `managed`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="68fcf-117">Formulários alternativos podem ser especificados adicionando o modificador apropriado após a sintaxe de `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`ou `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="68fcf-118">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="68fcf-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="68fcf-119">As conversões entre os tipos de `delegate*` são feitas com base em sua assinatura, incluindo a Convenção de chamada.</span><span class="sxs-lookup"><span data-stu-id="68fcf-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

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

<span data-ttu-id="68fcf-120">Um tipo de `delegate*` é um tipo de ponteiro que significa que ele tem todos os recursos e restrições de um tipo de ponteiro padrão:</span><span class="sxs-lookup"><span data-stu-id="68fcf-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="68fcf-121">Válido somente em um contexto de `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="68fcf-122">Os métodos que contêm um parâmetro `delegate*` ou tipo de retorno só podem ser chamados de um contexto `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="68fcf-123">Não pode ser convertido em `object`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="68fcf-124">Não pode ser usado como um argumento genérico.</span><span class="sxs-lookup"><span data-stu-id="68fcf-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="68fcf-125">Pode converter implicitamente `delegate*` em `void*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="68fcf-126">Pode converter explicitamente de `void*` para `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="68fcf-127">Restrições:</span><span class="sxs-lookup"><span data-stu-id="68fcf-127">Restrictions:</span></span>

- <span data-ttu-id="68fcf-128">Atributos personalizados não podem ser aplicados a um `delegate*` ou a qualquer um de seus elementos.</span><span class="sxs-lookup"><span data-stu-id="68fcf-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="68fcf-129">Um parâmetro `delegate*` não pode ser marcado como `params`</span><span class="sxs-lookup"><span data-stu-id="68fcf-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="68fcf-130">Um tipo de `delegate*` tem todas as restrições de um tipo de ponteiro normal.</span><span class="sxs-lookup"><span data-stu-id="68fcf-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="68fcf-131">Sintaxe de ponteiro de função</span><span class="sxs-lookup"><span data-stu-id="68fcf-131">Function pointer syntax</span></span>

<span data-ttu-id="68fcf-132">A sintaxe de ponteiro de função completa é representada pela seguinte gramática:</span><span class="sxs-lookup"><span data-stu-id="68fcf-132">The full function pointer syntax is represented by the following grammar:</span></span>

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

<span data-ttu-id="68fcf-133">A Convenção de chamada `unmanaged` representa a Convenção de chamada padrão para código nativo na plataforma atual e é codificada como Winapi tenha.</span><span class="sxs-lookup"><span data-stu-id="68fcf-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="68fcf-134">Todas as `calling_convention`s são palavras-chave contextuais quando precedidas por um `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

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

### <a name="function-pointer-conversions"></a><span data-ttu-id="68fcf-135">Conversões de ponteiro de função</span><span class="sxs-lookup"><span data-stu-id="68fcf-135">Function pointer conversions</span></span>

<span data-ttu-id="68fcf-136">Em um contexto sem segurança, o conjunto de conversões implícitas disponíveis (conversões implícitas) é estendido para incluir as seguintes conversões de ponteiro implícitas:</span><span class="sxs-lookup"><span data-stu-id="68fcf-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="68fcf-137">_Conversões existentes_</span><span class="sxs-lookup"><span data-stu-id="68fcf-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="68fcf-138">Em _funcptr\_tipo_ `F0` a outro _\_tipo_ `F1`, desde que todas as seguintes opções sejam verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="68fcf-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="68fcf-139">`F0` e `F1` têm o mesmo número de parâmetros, e cada `D0n` de parâmetro no `F0` tem os mesmos `ref`, `out`ou `in` modificadores que o parâmetro correspondente `D1n` em `F1`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="68fcf-140">Para cada parâmetro de valor (um parâmetro sem `ref`, `out`ou modificador de `in`), uma conversão de identidade, conversão de referência implícita ou conversão de ponteiro implícita existe do tipo de parâmetro em `F0` para o tipo de parâmetro correspondente no `F1`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="68fcf-141">Para cada parâmetro `ref`, `out`ou `in`, o tipo de parâmetro em `F0` é o mesmo que o tipo de parâmetro correspondente em `F1`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="68fcf-142">Se o tipo de retorno for por valor (sem `ref` ou `ref readonly`), uma identidade, referência implícita ou conversão implícita do ponteiro existirá do tipo de retorno de `F1` para o tipo de retorno de `F0`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="68fcf-143">Se o tipo de retorno for por referência (`ref` ou `ref readonly`), o tipo de retorno e os modificadores de `ref` de `F1` serão os mesmos do tipo de retorno e os modificadores de `ref` de `F0`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="68fcf-144">A Convenção de chamada de `F0` é igual à Convenção de chamada de `F1`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="68fcf-145">Permitir endereço para métodos de destino</span><span class="sxs-lookup"><span data-stu-id="68fcf-145">Allow address-of to target methods</span></span>

<span data-ttu-id="68fcf-146">Os grupos de métodos agora serão permitidos como argumentos para uma expressão de endereço.</span><span class="sxs-lookup"><span data-stu-id="68fcf-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="68fcf-147">O tipo de tal expressão será um `delegate*` que tem a assinatura equivalente do método de destino e uma Convenção de chamada gerenciada:</span><span class="sxs-lookup"><span data-stu-id="68fcf-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

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

<span data-ttu-id="68fcf-148">Em um contexto sem segurança, um método `M` é compatível com um tipo de ponteiro de função `F` se todas as seguintes opções forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="68fcf-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="68fcf-149">`M` e `F` têm o mesmo número de parâmetros, e cada parâmetro em `D` tem os mesmos `ref`, `out`ou modificadores de `in` como o parâmetro correspondente no `F`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="68fcf-150">Para cada parâmetro de valor (um parâmetro sem `ref`, `out`ou modificador de `in`), uma conversão de identidade, conversão de referência implícita ou conversão de ponteiro implícita existe do tipo de parâmetro em `M` para o tipo de parâmetro correspondente no `F`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="68fcf-151">Para cada parâmetro `ref`, `out`ou `in`, o tipo de parâmetro em `M` é o mesmo que o tipo de parâmetro correspondente em `F`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="68fcf-152">Se o tipo de retorno for por valor (sem `ref` ou `ref readonly`), uma identidade, referência implícita ou conversão implícita do ponteiro existirá do tipo de retorno de `F` para o tipo de retorno de `M`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="68fcf-153">Se o tipo de retorno for por referência (`ref` ou `ref readonly`), o tipo de retorno e os modificadores de `ref` de `F` serão os mesmos do tipo de retorno e os modificadores de `ref` de `M`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="68fcf-154">A Convenção de chamada de `M` é igual à Convenção de chamada de `F`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="68fcf-155">`M` é um método estático.</span><span class="sxs-lookup"><span data-stu-id="68fcf-155">`M` is a static method.</span></span>

<span data-ttu-id="68fcf-156">Em um contexto não seguro, existe uma conversão implícita de uma expressão de endereço cujo destino é um grupo de métodos `E` a um tipo de ponteiro de função compatível `F` se `E` contiver pelo menos um método aplicável em seu formato normal a uma lista de argumentos construída pelo uso dos tipos de parâmetro e modificadores de `F`, conforme descrito no seguinte.</span><span class="sxs-lookup"><span data-stu-id="68fcf-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="68fcf-157">Um único método `M` é selecionado correspondente a uma invocação de método do formulário `E(A)` com as seguintes modificações:</span><span class="sxs-lookup"><span data-stu-id="68fcf-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="68fcf-158">A lista de argumentos `A` é uma lista de expressões, cada uma classificada como uma variável e com o tipo e o modificador (`ref`, `out`ou `in`) do _parâmetro\_formal correspondente\_lista_ de `D`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="68fcf-159">Os métodos candidatos são apenas aqueles métodos que são aplicáveis em seu formato normal, não aqueles aplicáveis em sua forma expandida.</span><span class="sxs-lookup"><span data-stu-id="68fcf-159">The candidate methods are only those methods that are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
- <span data-ttu-id="68fcf-160">Se o algoritmo das invocações de método produzir um erro, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="68fcf-160">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="68fcf-161">Caso contrário, o algoritmo produz um único método melhor `M` ter o mesmo número de parâmetros que `F` e a conversão é considerada como existe.</span><span class="sxs-lookup"><span data-stu-id="68fcf-161">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="68fcf-162">O método selecionado `M` deve ser compatível (conforme definido acima) com o tipo de ponteiro de função `F`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-162">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="68fcf-163">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="68fcf-163">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="68fcf-164">O resultado da conversão é um ponteiro de função do tipo `F`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-164">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="68fcf-165">Há uma conversão implícita de uma expressão de endereço cujo destino é um grupo de métodos `E` para `void*` se houver apenas um método estático `M` em `E`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-165">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="68fcf-166">Se houver um método estático, o melhor método de `E` será `M`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-166">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="68fcf-167">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="68fcf-167">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="68fcf-168">Isso significa que os desenvolvedores podem depender das regras de resolução de sobrecarga para trabalhar em conjunto com o operador address-of:</span><span class="sxs-lookup"><span data-stu-id="68fcf-168">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

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

<span data-ttu-id="68fcf-169">O operador address-of será implementado usando a instrução `ldftn`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-169">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="68fcf-170">Restrições deste recurso:</span><span class="sxs-lookup"><span data-stu-id="68fcf-170">Restrictions of this feature:</span></span>

- <span data-ttu-id="68fcf-171">Aplica-se somente a métodos marcados como `static`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-171">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="68fcf-172">Funções locais não`static` não podem ser usadas em `&`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-172">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="68fcf-173">Os detalhes de implementação desses métodos são deliberadamente não especificados pelo idioma.</span><span class="sxs-lookup"><span data-stu-id="68fcf-173">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="68fcf-174">Isso inclui se eles são estáticos versus instância ou exatamente em qual assinatura eles são emitidos.</span><span class="sxs-lookup"><span data-stu-id="68fcf-174">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="68fcf-175">Melhor membro da função</span><span class="sxs-lookup"><span data-stu-id="68fcf-175">Better function member</span></span>

<span data-ttu-id="68fcf-176">A melhor especificação de membro de função será alterada para incluir a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="68fcf-176">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="68fcf-177">Uma `delegate*` é mais específica do que `void*`</span><span class="sxs-lookup"><span data-stu-id="68fcf-177">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="68fcf-178">Isso significa que é possível sobrecarregar `void*` e uma `delegate*` e ainda facilmente usar o operador address-of.</span><span class="sxs-lookup"><span data-stu-id="68fcf-178">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="68fcf-179">Problemas em aberto</span><span class="sxs-lookup"><span data-stu-id="68fcf-179">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="68fcf-180">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="68fcf-180">NativeCallableAttribute</span></span>

<span data-ttu-id="68fcf-181">Esse é um atributo usado pelo CLR para evitar o prólogo gerenciado para nativo ao invocar.</span><span class="sxs-lookup"><span data-stu-id="68fcf-181">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="68fcf-182">Os métodos marcados por este atributo só podem ser chamados de código nativo, não gerenciado (não é possível chamar métodos, criar um delegado, etc...).</span><span class="sxs-lookup"><span data-stu-id="68fcf-182">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="68fcf-183">O atributo não é especial para mscorlib; o tempo de execução tratará qualquer atributo com esse nome com a mesma semântica.</span><span class="sxs-lookup"><span data-stu-id="68fcf-183">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="68fcf-184">É possível que o tempo de execução e a linguagem funcionem juntos para dar suporte total a isso.</span><span class="sxs-lookup"><span data-stu-id="68fcf-184">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="68fcf-185">O idioma pode optar por tratar os membros de `static` de endereço com um atributo `NativeCallable` como um `delegate*` com a Convenção de chamada especificada.</span><span class="sxs-lookup"><span data-stu-id="68fcf-185">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

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

<span data-ttu-id="68fcf-186">Além disso, a linguagem provavelmente também desejaria:</span><span class="sxs-lookup"><span data-stu-id="68fcf-186">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="68fcf-187">Sinalizar todas as chamadas gerenciadas para um método marcado com `NativeCallable` como um erro.</span><span class="sxs-lookup"><span data-stu-id="68fcf-187">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="68fcf-188">Considerando que a função não pode ser invocada do código gerenciado, o compilador deve impedir que os desenvolvedores tentem essa invocação.</span><span class="sxs-lookup"><span data-stu-id="68fcf-188">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="68fcf-189">Impeça conversões de grupo de métodos para `delegate` quando o método é marcado com `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-189">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="68fcf-190">No entanto, isso não é necessário para dar suporte a `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-190">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="68fcf-191">O compilador pode dar suporte ao atributo `NativeCallable` como está usando a sintaxe existente.</span><span class="sxs-lookup"><span data-stu-id="68fcf-191">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="68fcf-192">O programa simplesmente precisaria ser convertido em `void*` antes da conversão para a assinatura de `delegate*` correta.</span><span class="sxs-lookup"><span data-stu-id="68fcf-192">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="68fcf-193">Isso não seria pior do que o suporte atualmente.</span><span class="sxs-lookup"><span data-stu-id="68fcf-193">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="68fcf-194">Conjunto extensível de convenções de chamada não gerenciadas</span><span class="sxs-lookup"><span data-stu-id="68fcf-194">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="68fcf-195">O conjunto de convenções de chamada não gerenciadas com suporte das codificações ECMA-335 atuais está desatualizado.</span><span class="sxs-lookup"><span data-stu-id="68fcf-195">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="68fcf-196">Vimos solicitações para adicionar suporte para mais convenções de chamada não gerenciadas, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="68fcf-196">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="68fcf-197">https://github.com/dotnet/coreclr/issues/12120 [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall)</span><span class="sxs-lookup"><span data-stu-id="68fcf-197">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="68fcf-198">StdCall com este https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 explícito</span><span class="sxs-lookup"><span data-stu-id="68fcf-198">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="68fcf-199">O design desse recurso deve permitir a extensão do conjunto de convenções de chamada não gerenciadas conforme necessário no futuro.</span><span class="sxs-lookup"><span data-stu-id="68fcf-199">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="68fcf-200">Os problemas incluem espaço limitado para codificar convenções de chamada (12 de 16 valores são obtidos em `IMAGE_CEE_CS_CALLCONV_MASK`) e número de casas que precisam ser tocadas para adicionar uma nova Convenção de chamada.</span><span class="sxs-lookup"><span data-stu-id="68fcf-200">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="68fcf-201">Uma solução potencial é apresentar uma nova codificação que representa a Convenção de chamada usando [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enumeração.</span><span class="sxs-lookup"><span data-stu-id="68fcf-201">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="68fcf-202">Para referência, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h tem a lista de convenções de chamada com suporte do LLVM.</span><span class="sxs-lookup"><span data-stu-id="68fcf-202">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="68fcf-203">Embora seja improvável que o .NET precise dar suporte a todos eles, ele demonstra que o espaço das convenções de chamada é muito avançado.</span><span class="sxs-lookup"><span data-stu-id="68fcf-203">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="68fcf-204">Considerações</span><span class="sxs-lookup"><span data-stu-id="68fcf-204">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="68fcf-205">Permitir métodos de instância</span><span class="sxs-lookup"><span data-stu-id="68fcf-205">Allow instance methods</span></span>

<span data-ttu-id="68fcf-206">A proposta pode ser estendida para dar suporte a métodos de instância aproveitando a Convenção de chamada da CLI `EXPLICITTHIS` C# (chamada `instance` no código).</span><span class="sxs-lookup"><span data-stu-id="68fcf-206">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="68fcf-207">Essa forma de ponteiros de função da CLI coloca o parâmetro `this` como um primeiro parâmetro explícito da sintaxe do ponteiro de função.</span><span class="sxs-lookup"><span data-stu-id="68fcf-207">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="68fcf-208">Isso é bom, mas adiciona certa complicação à proposta.</span><span class="sxs-lookup"><span data-stu-id="68fcf-208">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="68fcf-209">Particularmente, como os ponteiros de função que diferem pela Convenção de chamada `instance` e `managed` seriam incompatíveis, embora ambos os casos sejam usados para invocar métodos gerenciados com a mesma C# assinatura.</span><span class="sxs-lookup"><span data-stu-id="68fcf-209">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="68fcf-210">Além disso, em cada caso, considerado como seria valioso ter uma solução simples: Use um `static` função local.</span><span class="sxs-lookup"><span data-stu-id="68fcf-210">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="68fcf-211">Não requer segurança na declaração</span><span class="sxs-lookup"><span data-stu-id="68fcf-211">Don't require unsafe at declaration</span></span>

<span data-ttu-id="68fcf-212">Em vez de exigir `unsafe` em cada uso de um `delegate*`, só é necessário no ponto em que um grupo de métodos é convertido em um `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-212">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="68fcf-213">É aí que os problemas de segurança principal entram em cena (sabendo que o assembly recipiente não pode ser descarregado enquanto o valor estiver ativo).</span><span class="sxs-lookup"><span data-stu-id="68fcf-213">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="68fcf-214">Exigir `unsafe` em outros locais pode ser visto como excessivo.</span><span class="sxs-lookup"><span data-stu-id="68fcf-214">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="68fcf-215">É assim que o design foi originalmente planejado.</span><span class="sxs-lookup"><span data-stu-id="68fcf-215">This is how the design was originally intended.</span></span> <span data-ttu-id="68fcf-216">Mas as regras de linguagem resultantes pareciam muito estranhas.</span><span class="sxs-lookup"><span data-stu-id="68fcf-216">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="68fcf-217">É impossível ocultar o fato de que esse é um valor de ponteiro e que ele continua exibindo até mesmo sem a palavra-chave `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-217">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="68fcf-218">Por exemplo, a conversão em `object` não pode ser permitida, não pode ser um membro de um `class`, etc... O C# design é exigir `unsafe` para todos os usos de ponteiro e, portanto, esse design é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="68fcf-218">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="68fcf-219">Os desenvolvedores ainda poderão apresentar um wrapper _seguro_ sobre `delegate*` valores da mesma maneira que fazem para tipos de ponteiros normais hoje em dia.</span><span class="sxs-lookup"><span data-stu-id="68fcf-219">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="68fcf-220">Considere:</span><span class="sxs-lookup"><span data-stu-id="68fcf-220">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="68fcf-221">Usando delegados</span><span class="sxs-lookup"><span data-stu-id="68fcf-221">Using delegates</span></span>

<span data-ttu-id="68fcf-222">Em vez de usar um novo elemento de sintaxe, `delegate*`, basta usar tipos de `delegate` existentes com um `*` seguindo o tipo:</span><span class="sxs-lookup"><span data-stu-id="68fcf-222">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="68fcf-223">A manipulação de Convenção de chamada pode ser feita anotando os tipos de `delegate` com um atributo que especifica um valor de `CallingConvention`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-223">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="68fcf-224">A falta de um atributo significaria a Convenção de chamada gerenciada.</span><span class="sxs-lookup"><span data-stu-id="68fcf-224">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="68fcf-225">Codificar isso no IL é problemático.</span><span class="sxs-lookup"><span data-stu-id="68fcf-225">Encoding this in IL is problematic.</span></span> <span data-ttu-id="68fcf-226">O valor subjacente precisa ser representado como um ponteiro, ainda que ele também deva:</span><span class="sxs-lookup"><span data-stu-id="68fcf-226">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="68fcf-227">Ter um tipo exclusivo para permitir sobrecargas com tipos diferentes de ponteiro de função.</span><span class="sxs-lookup"><span data-stu-id="68fcf-227">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="68fcf-228">Ser equivalente para fins de OHI em limites de assembly.</span><span class="sxs-lookup"><span data-stu-id="68fcf-228">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="68fcf-229">O último ponto é particularmente problemático.</span><span class="sxs-lookup"><span data-stu-id="68fcf-229">The last point is particularly problematic.</span></span> <span data-ttu-id="68fcf-230">Isso significa que cada assembly que usa `Func<int>*` deve codificar um tipo equivalente em metadados, mesmo que `Func<int>*` seja definido em um assembly, embora não controle.</span><span class="sxs-lookup"><span data-stu-id="68fcf-230">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="68fcf-231">Além disso, qualquer outro tipo que é definido com o nome `System.Func<T>` em um assembly que não seja mscorlib deve ser diferente da versão definida em mscorlib.</span><span class="sxs-lookup"><span data-stu-id="68fcf-231">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="68fcf-232">Uma opção que foi explorada foi emitir um ponteiro como `mod_req(Func<int>) void*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-232">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="68fcf-233">Embora isso não funcione como um `mod_req` não pode se associar a um `TypeSpec` e, portanto, não pode direcionar instanciações genéricas.</span><span class="sxs-lookup"><span data-stu-id="68fcf-233">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="68fcf-234">Ponteiros de função nomeados</span><span class="sxs-lookup"><span data-stu-id="68fcf-234">Named function pointers</span></span>

<span data-ttu-id="68fcf-235">A sintaxe do ponteiro de função pode ser complicada, particularmente em casos complexos, como ponteiros de funções aninhadas.</span><span class="sxs-lookup"><span data-stu-id="68fcf-235">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="68fcf-236">Em vez de os desenvolvedores digitarem a assinatura sempre que o idioma puder permitir declarações nomeadas de ponteiros de função, como é feito com `delegate`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-236">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="68fcf-237">Parte do problema aqui é que o primitivo de CLI subjacente não tem nomes, portanto, isso seria C# puramente uma invenção e requer um pouco de trabalho de metadados para habilitar.</span><span class="sxs-lookup"><span data-stu-id="68fcf-237">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="68fcf-238">Isso é factível, mas é um grande respeito do trabalho.</span><span class="sxs-lookup"><span data-stu-id="68fcf-238">That is doable but is a significant about of work.</span></span> <span data-ttu-id="68fcf-239">Basicamente, é C# necessário ter um complemento para a tabela de tipo def puramente para esses nomes.</span><span class="sxs-lookup"><span data-stu-id="68fcf-239">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="68fcf-240">Além disso, quando os argumentos dos ponteiros de função nomeados foram examinados, eles poderiam ser aplicados igualmente bem a vários outros cenários.</span><span class="sxs-lookup"><span data-stu-id="68fcf-240">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="68fcf-241">Por exemplo, seria conveniente declarar tuplas nomeadas para reduzir a necessidade de digitar a assinatura completa em todos os casos.</span><span class="sxs-lookup"><span data-stu-id="68fcf-241">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="68fcf-242">Após a discussão, decidimos não permitir a declaração nomeada de tipos de `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-242">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="68fcf-243">Se encontrarmos uma necessidade significativa para isso com base nos comentários de uso do cliente, investigaremos uma solução de nomenclatura que funciona para ponteiros de função, tuplas, genéricos, etc... Isso provavelmente será semelhante em forma de outras sugestões, como suporte completo a `typedef` no idioma.</span><span class="sxs-lookup"><span data-stu-id="68fcf-243">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="68fcf-244">Considerações futuras</span><span class="sxs-lookup"><span data-stu-id="68fcf-244">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="68fcf-245">funções locais estáticas</span><span class="sxs-lookup"><span data-stu-id="68fcf-245">static local functions</span></span>

<span data-ttu-id="68fcf-246">Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/1565) para permitir o modificador de `static` em funções locais.</span><span class="sxs-lookup"><span data-stu-id="68fcf-246">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="68fcf-247">Essa função seria garantida para ser emitida como `static` e com a assinatura exata especificada no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="68fcf-247">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="68fcf-248">Essa função deve ser um argumento válido para `&`, pois ela contém nenhum dos problemas que as funções locais têm hoje</span><span class="sxs-lookup"><span data-stu-id="68fcf-248">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="68fcf-249">delegados estáticos</span><span class="sxs-lookup"><span data-stu-id="68fcf-249">static delegates</span></span>

<span data-ttu-id="68fcf-250">Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/302) para permitir a declaração de tipos de `delegate` que só podem se referir a membros `static`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-250">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="68fcf-251">A vantagem é que essas instâncias de `delegate` podem ser de alocação gratuita e melhor em cenários de desempenho.</span><span class="sxs-lookup"><span data-stu-id="68fcf-251">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="68fcf-252">Se o recurso de ponteiro de função for implementado, a proposta de `static delegate` provavelmente será fechada. A vantagem proposta desse recurso é a natureza livre de alocação.</span><span class="sxs-lookup"><span data-stu-id="68fcf-252">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="68fcf-253">No entanto, foram encontradas investigações recentes que não podem ser obtidas devido ao descarregamento do assembly.</span><span class="sxs-lookup"><span data-stu-id="68fcf-253">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="68fcf-254">Deve haver um identificador forte do `static delegate` ao método ao qual se refere para impedir que o assembly seja descarregado de dentro dele.</span><span class="sxs-lookup"><span data-stu-id="68fcf-254">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="68fcf-255">Para manter cada instância de `static delegate` seria necessário alocar um novo identificador que executa um contador para as metas da proposta.</span><span class="sxs-lookup"><span data-stu-id="68fcf-255">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="68fcf-256">Havia alguns designs em que a alocação poderia ser amortizada para uma única alocação por site de chamada, mas isso era um pouco complexo e não parece que vale a pena compensar.</span><span class="sxs-lookup"><span data-stu-id="68fcf-256">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="68fcf-257">Isso significa que os desenvolvedores têm, essencialmente, decidir entre as seguintes compensações:</span><span class="sxs-lookup"><span data-stu-id="68fcf-257">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="68fcf-258">Segurança diante do descarregamento do assembly: isso requer alocações e, portanto, `delegate` já é uma opção suficiente.</span><span class="sxs-lookup"><span data-stu-id="68fcf-258">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="68fcf-259">Não há segurança em face de descarregamento de assembly: Use um `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="68fcf-259">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="68fcf-260">Isso pode ser encapsulado em um `struct` para permitir o uso fora de um contexto de `unsafe` no restante do código.</span><span class="sxs-lookup"><span data-stu-id="68fcf-260">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
