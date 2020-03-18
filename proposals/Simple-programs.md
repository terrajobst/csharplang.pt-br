---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485220"
---
# <a name="simple-programs"></a><span data-ttu-id="8256b-101">Programas simples</span><span class="sxs-lookup"><span data-stu-id="8256b-101">Simple programs</span></span>

* <span data-ttu-id="8256b-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="8256b-102">[x] Proposed</span></span>
* <span data-ttu-id="8256b-103">[x] protótipo: iniciado</span><span class="sxs-lookup"><span data-stu-id="8256b-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="8256b-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="8256b-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="8256b-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="8256b-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="8256b-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="8256b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="8256b-107">Permita que uma sequência de *instruções* ocorra logo antes da *namespace_member_declaration*s de um *compilation_unit* (ou seja, o arquivo de origem).</span><span class="sxs-lookup"><span data-stu-id="8256b-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="8256b-108">A semântica é que se tal sequência de *instruções* estiver presente, a seguinte declaração de tipo, módulo, o nome do tipo real e o nome do método, seria emitido:</span><span class="sxs-lookup"><span data-stu-id="8256b-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="8256b-109">Consulte também https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="8256b-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="8256b-110">Motivação</span><span class="sxs-lookup"><span data-stu-id="8256b-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="8256b-111">Há uma certa quantidade de clichê em torno mesmo dos programas mais simples, devido à necessidade de um método `Main` explícito.</span><span class="sxs-lookup"><span data-stu-id="8256b-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="8256b-112">Isso parece chegar à forma de aprendizado de idioma e clareza do programa.</span><span class="sxs-lookup"><span data-stu-id="8256b-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="8256b-113">O objetivo principal do recurso é, portanto, permitir C# programas sem texto clichê desnecessário em relação a eles, para fins de aprendizes e a clareza do código.</span><span class="sxs-lookup"><span data-stu-id="8256b-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="8256b-114">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="8256b-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="8256b-115">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="8256b-115">Syntax</span></span>

<span data-ttu-id="8256b-116">A única sintaxe adicional é permitir uma sequência de *instruções*s em uma unidade de compilação, logo antes do *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="8256b-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="8256b-117">Em todos, exceto um *compilation_unit* a *instrução*s deve ser declarações de função local.</span><span class="sxs-lookup"><span data-stu-id="8256b-117">In all but one *compilation_unit* the *statement*s must all be local function declarations.</span></span> 

<span data-ttu-id="8256b-118">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="8256b-118">Example:</span></span>

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="8256b-119">Semântica</span><span class="sxs-lookup"><span data-stu-id="8256b-119">Semantics</span></span>

<span data-ttu-id="8256b-120">Se quaisquer instruções de nível superior estiverem presentes em qualquer unidade de compilação do programa, o significado será como se elas fossem combinadas no corpo do bloco de um método `Main` de uma classe `Program` no namespace global, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="8256b-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

<span data-ttu-id="8256b-121">Observe que os nomes "Program" e "Main" são usados apenas para fins ilustrativos, os nomes reais usados pelo compilador são dependentes de implementação e nem o tipo, nem o método pode ser referenciado pelo nome do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="8256b-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="8256b-122">O método é designado como o ponto de entrada do programa.</span><span class="sxs-lookup"><span data-stu-id="8256b-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="8256b-123">Métodos explicitamente declarados que por convenção podem ser considerados como candidatos de ponto de entrada são ignorados.</span><span class="sxs-lookup"><span data-stu-id="8256b-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="8256b-124">Um aviso é relatado quando isso acontece.</span><span class="sxs-lookup"><span data-stu-id="8256b-124">A warning is reported when that happens.</span></span> <span data-ttu-id="8256b-125">É um erro especificar `-main:<type>` switch do compilador.</span><span class="sxs-lookup"><span data-stu-id="8256b-125">It is an error to specify `-main:<type>` compiler switch.</span></span>

<span data-ttu-id="8256b-126">Se qualquer uma das unidades de compilação tiver instruções diferentes das declarações de função local, as instruções dessa unidade de compilação ocorrerão primeiro.</span><span class="sxs-lookup"><span data-stu-id="8256b-126">If any one compilation unit has statements other than local function declarations, statements from that compilation unit occur first.</span></span> <span data-ttu-id="8256b-127">Isso faz com que ele seja válido para funções locais em um arquivo para referenciar variáveis locais em outra.</span><span class="sxs-lookup"><span data-stu-id="8256b-127">This causes it to be legal for local functions in one file to reference local variables in another.</span></span> <span data-ttu-id="8256b-128">A ordem das contribuições de instrução (que seriam todas as funções locais) de outras unidades de compilação é indefinida.</span><span class="sxs-lookup"><span data-stu-id="8256b-128">The order of statement contributions (which would all be local functions) from other compilation units is undefined.</span></span>

<span data-ttu-id="8256b-129">Operações assíncronas são permitidas em instruções de nível superior para o grau em que são permitidas em instruções dentro de um método de ponto de entrada assíncrono regular.</span><span class="sxs-lookup"><span data-stu-id="8256b-129">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="8256b-130">No entanto, eles não são necessários, se `await` expressões e outras operações assíncronas forem omitidas, nenhum aviso será produzido.</span><span class="sxs-lookup"><span data-stu-id="8256b-130">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="8256b-131">Em vez disso, a assinatura do método de ponto de entrada gerado é equivalente a</span><span class="sxs-lookup"><span data-stu-id="8256b-131">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="8256b-132">O exemplo acima produziria a seguinte declaração de método de `$Main`:</span><span class="sxs-lookup"><span data-stu-id="8256b-132">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="8256b-133">Ao mesmo tempo, um exemplo como este:</span><span class="sxs-lookup"><span data-stu-id="8256b-133">At the same time an example like this:</span></span>
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="8256b-134">produziria:</span><span class="sxs-lookup"><span data-stu-id="8256b-134">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="8256b-135">Escopo de variáveis locais de nível superior e funções locais</span><span class="sxs-lookup"><span data-stu-id="8256b-135">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="8256b-136">Embora as variáveis e funções locais de nível superior sejam "encapsuladas" no método de ponto de entrada gerado, elas ainda devem estar no escopo durante todo o programa.</span><span class="sxs-lookup"><span data-stu-id="8256b-136">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program.</span></span>
<span data-ttu-id="8256b-137">Para fins de avaliação de nome simples, depois que o namespace global for atingido:</span><span class="sxs-lookup"><span data-stu-id="8256b-137">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="8256b-138">Primeiro, é feita uma tentativa de avaliar o nome dentro do método de ponto de entrada gerado e somente se essa tentativa falhar</span><span class="sxs-lookup"><span data-stu-id="8256b-138">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="8256b-139">A avaliação "regular" dentro da declaração de namespace global é executada.</span><span class="sxs-lookup"><span data-stu-id="8256b-139">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="8256b-140">Isso pode levar ao nome sombreamento de namespaces e tipos declarados dentro do namespace global, bem como para sombreamento de nomes importados.</span><span class="sxs-lookup"><span data-stu-id="8256b-140">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="8256b-141">Se a avaliação de nome simples ocorrer fora das instruções de nível superior e a avaliação gerar uma variável ou função local de nível superior, isso deverá levar a um erro.</span><span class="sxs-lookup"><span data-stu-id="8256b-141">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="8256b-142">Dessa forma, protegemos nossa capacidade futura de abordar melhor as "funções de nível superior" (cenário 2 em https://github.com/dotnet/csharplang/issues/3117)e são capazes de fornecer diagnósticos úteis aos usuários que, por engano, acreditam que eles tenham suporte.</span><span class="sxs-lookup"><span data-stu-id="8256b-142">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

