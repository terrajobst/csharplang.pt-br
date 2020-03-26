---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281938"
---
# <a name="simple-programs"></a><span data-ttu-id="25abf-101">Programas simples</span><span class="sxs-lookup"><span data-stu-id="25abf-101">Simple programs</span></span>

* <span data-ttu-id="25abf-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="25abf-102">[x] Proposed</span></span>
* <span data-ttu-id="25abf-103">[x] protótipo: iniciado</span><span class="sxs-lookup"><span data-stu-id="25abf-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="25abf-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="25abf-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="25abf-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="25abf-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="25abf-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="25abf-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="25abf-107">Permita que uma sequência de *instruções* ocorra logo antes da *namespace_member_declaration*s de um *compilation_unit* (ou seja, o arquivo de origem).</span><span class="sxs-lookup"><span data-stu-id="25abf-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="25abf-108">A semântica é que se tal sequência de *instruções* estiver presente, a seguinte declaração de tipo, módulo, o nome do tipo real e o nome do método, seria emitido:</span><span class="sxs-lookup"><span data-stu-id="25abf-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="25abf-109">Consulte também https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="25abf-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="25abf-110">Motivação</span><span class="sxs-lookup"><span data-stu-id="25abf-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="25abf-111">Há uma certa quantidade de clichê em torno mesmo dos programas mais simples, devido à necessidade de um método `Main` explícito.</span><span class="sxs-lookup"><span data-stu-id="25abf-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="25abf-112">Isso parece chegar à forma de aprendizado de idioma e clareza do programa.</span><span class="sxs-lookup"><span data-stu-id="25abf-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="25abf-113">O objetivo principal do recurso é, portanto, permitir C# programas sem texto clichê desnecessário em relação a eles, para fins de aprendizes e a clareza do código.</span><span class="sxs-lookup"><span data-stu-id="25abf-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="25abf-114">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="25abf-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="25abf-115">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="25abf-115">Syntax</span></span>

<span data-ttu-id="25abf-116">A única sintaxe adicional é permitir uma sequência de *instruções*s em uma unidade de compilação, logo antes do *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="25abf-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="25abf-117">Somente um *compilation_unit* pode ter a *instrução*s.</span><span class="sxs-lookup"><span data-stu-id="25abf-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="25abf-118">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="25abf-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="25abf-119">Semântica</span><span class="sxs-lookup"><span data-stu-id="25abf-119">Semantics</span></span>

<span data-ttu-id="25abf-120">Se quaisquer instruções de nível superior estiverem presentes em qualquer unidade de compilação do programa, o significado será como se elas fossem combinadas no corpo do bloco de um método `Main` de uma classe `Program` no namespace global, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="25abf-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="25abf-121">Observe que os nomes "Program" e "Main" são usados apenas para fins ilustrativos, os nomes reais usados pelo compilador são dependentes de implementação e nem o tipo, nem o método pode ser referenciado pelo nome do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="25abf-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="25abf-122">O método é designado como o ponto de entrada do programa.</span><span class="sxs-lookup"><span data-stu-id="25abf-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="25abf-123">Métodos explicitamente declarados que por convenção podem ser considerados como candidatos de ponto de entrada são ignorados.</span><span class="sxs-lookup"><span data-stu-id="25abf-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="25abf-124">Um aviso é relatado quando isso acontece.</span><span class="sxs-lookup"><span data-stu-id="25abf-124">A warning is reported when that happens.</span></span> <span data-ttu-id="25abf-125">É um erro especificar `-main:<type>` switch de compilador quando há instruções de nível superior.</span><span class="sxs-lookup"><span data-stu-id="25abf-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="25abf-126">Operações assíncronas são permitidas em instruções de nível superior para o grau em que são permitidas em instruções dentro de um método de ponto de entrada assíncrono regular.</span><span class="sxs-lookup"><span data-stu-id="25abf-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="25abf-127">No entanto, eles não são necessários, se `await` expressões e outras operações assíncronas forem omitidas, nenhum aviso será produzido.</span><span class="sxs-lookup"><span data-stu-id="25abf-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="25abf-128">Em vez disso, a assinatura do método de ponto de entrada gerado é equivalente a</span><span class="sxs-lookup"><span data-stu-id="25abf-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="25abf-129">O exemplo acima produziria a seguinte declaração de método de `$Main`:</span><span class="sxs-lookup"><span data-stu-id="25abf-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="25abf-130">Ao mesmo tempo, um exemplo como este:</span><span class="sxs-lookup"><span data-stu-id="25abf-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="25abf-131">produziria:</span><span class="sxs-lookup"><span data-stu-id="25abf-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="25abf-132">Escopo de variáveis locais de nível superior e funções locais</span><span class="sxs-lookup"><span data-stu-id="25abf-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="25abf-133">Embora as variáveis e funções locais de nível superior sejam "encapsuladas" no método de ponto de entrada gerado, elas ainda devem estar no escopo em todo o programa em cada unidade de compilação.</span><span class="sxs-lookup"><span data-stu-id="25abf-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="25abf-134">Para fins de avaliação de nome simples, depois que o namespace global for atingido:</span><span class="sxs-lookup"><span data-stu-id="25abf-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="25abf-135">Primeiro, é feita uma tentativa de avaliar o nome dentro do método de ponto de entrada gerado e somente se essa tentativa falhar</span><span class="sxs-lookup"><span data-stu-id="25abf-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="25abf-136">A avaliação "regular" dentro da declaração de namespace global é executada.</span><span class="sxs-lookup"><span data-stu-id="25abf-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="25abf-137">Isso pode levar ao nome sombreamento de namespaces e tipos declarados dentro do namespace global, bem como para sombreamento de nomes importados.</span><span class="sxs-lookup"><span data-stu-id="25abf-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="25abf-138">Se a avaliação de nome simples ocorrer fora das instruções de nível superior e a avaliação gerar uma variável ou função local de nível superior, isso deverá levar a um erro.</span><span class="sxs-lookup"><span data-stu-id="25abf-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="25abf-139">Dessa forma, protegemos nossa capacidade futura de abordar melhor as "funções de nível superior" (cenário 2 em https://github.com/dotnet/csharplang/issues/3117)e são capazes de fornecer diagnósticos úteis aos usuários que, por engano, acreditam que eles tenham suporte.</span><span class="sxs-lookup"><span data-stu-id="25abf-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

