---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484562"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="04cde-101">Intrínsecos do compilador</span><span class="sxs-lookup"><span data-stu-id="04cde-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="04cde-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="04cde-102">Summary</span></span>

<span data-ttu-id="04cde-103">Esta proposta fornece construções de linguagem que expõem opcodes IL de nível baixo que atualmente não podem ser acessados com eficiência, ou em todos: `ldftn`, `ldvirtftn`, `ldtoken` e `calli`.</span><span class="sxs-lookup"><span data-stu-id="04cde-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="04cde-104">Esses opcodes de baixo nível podem ser importantes em código de alto desempenho e os desenvolvedores precisam de uma maneira eficiente para acessá-los.</span><span class="sxs-lookup"><span data-stu-id="04cde-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="04cde-105">Motivação</span><span class="sxs-lookup"><span data-stu-id="04cde-105">Motivation</span></span>

<span data-ttu-id="04cde-106">As motivações e o plano de fundo desse recurso são descritos no seguinte problema (como é uma implementação potencial do recurso):</span><span class="sxs-lookup"><span data-stu-id="04cde-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="04cde-107">Essa proposta de design alternativa vem depois de revisar uma implementação de protótipo da proposta original por @msjabby, bem como o uso em uma base de código significativa.</span><span class="sxs-lookup"><span data-stu-id="04cde-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="04cde-108">Esse design foi feito com uma entrada significativa do @mjsabby, @tmat e @jkotas.</span><span class="sxs-lookup"><span data-stu-id="04cde-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="04cde-109">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="04cde-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="04cde-110">Permitir endereço de métodos de destino</span><span class="sxs-lookup"><span data-stu-id="04cde-110">Allow address of to target methods</span></span>

<span data-ttu-id="04cde-111">Os grupos de métodos agora serão permitidos como argumentos para uma expressão de endereço.</span><span class="sxs-lookup"><span data-stu-id="04cde-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="04cde-112">O tipo dessa expressão será `void*`.</span><span class="sxs-lookup"><span data-stu-id="04cde-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="04cde-113">Considerando que não há nenhuma conversão de delegado aqui, o único mecanismo para filtrar Membros no grupo de métodos é por acesso estático/de instância.</span><span class="sxs-lookup"><span data-stu-id="04cde-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="04cde-114">Se não for possível distinguir os membros, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="04cde-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="04cde-115">A expressão AddressOf neste contexto será implementada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="04cde-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="04cde-116">Ldftn: quando o método é não virtual.</span><span class="sxs-lookup"><span data-stu-id="04cde-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="04cde-117">ldvirtftn: quando o método é virtual.</span><span class="sxs-lookup"><span data-stu-id="04cde-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="04cde-118">Restrições deste recurso:</span><span class="sxs-lookup"><span data-stu-id="04cde-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="04cde-119">Os métodos de instância só podem ser especificados ao usar uma expressão de invocação em um valor</span><span class="sxs-lookup"><span data-stu-id="04cde-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="04cde-120">As funções locais não podem ser usadas em `&`.</span><span class="sxs-lookup"><span data-stu-id="04cde-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="04cde-121">Os detalhes de implementação desses métodos são deliberadamente não especificados pelo idioma.</span><span class="sxs-lookup"><span data-stu-id="04cde-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="04cde-122">Isso inclui se eles são estáticos versus instância ou exatamente em qual assinatura eles são emitidos.</span><span class="sxs-lookup"><span data-stu-id="04cde-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="04cde-123">handleof</span><span class="sxs-lookup"><span data-stu-id="04cde-123">handleof</span></span>

<span data-ttu-id="04cde-124">O `handleof` palavra-chave contextual converterá um campo, membro ou tipo em seu tipo equivalente de `RuntimeHandle` usando a instrução `ldtoken`.</span><span class="sxs-lookup"><span data-stu-id="04cde-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="04cde-125">O tipo exato da expressão dependerá do tipo de nome em `handleof`:</span><span class="sxs-lookup"><span data-stu-id="04cde-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="04cde-126">campo: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="04cde-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="04cde-127">Tipo: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="04cde-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="04cde-128">método: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="04cde-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="04cde-129">Os argumentos para `handleof` são idênticos aos `nameof`.</span><span class="sxs-lookup"><span data-stu-id="04cde-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="04cde-130">Ele deve ser um nome simples, um nome qualificado, um acesso de membro, um acesso de base com um membro especificado ou esse acesso com um membro especificado.</span><span class="sxs-lookup"><span data-stu-id="04cde-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="04cde-131">A expressão de argumento identifica uma definição de código, mas ela nunca é avaliada.</span><span class="sxs-lookup"><span data-stu-id="04cde-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="04cde-132">A expressão de `handleof` é avaliada em tempo de execução e tem um tipo de retorno de `RuntimeHandle`.</span><span class="sxs-lookup"><span data-stu-id="04cde-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="04cde-133">Isso pode ser executado em código seguro, bem como não seguro.</span><span class="sxs-lookup"><span data-stu-id="04cde-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="04cde-134">Restrições deste recurso:</span><span class="sxs-lookup"><span data-stu-id="04cde-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="04cde-135">As propriedades não podem ser usadas em uma expressão `handleof`.</span><span class="sxs-lookup"><span data-stu-id="04cde-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="04cde-136">A expressão de `handleof` não pode ser usada quando há um nome de `handleof` existente no escopo.</span><span class="sxs-lookup"><span data-stu-id="04cde-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="04cde-137">Por exemplo, um tipo, namespace, etc...</span><span class="sxs-lookup"><span data-stu-id="04cde-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="04cde-138">Calli</span><span class="sxs-lookup"><span data-stu-id="04cde-138">calli</span></span>

<span data-ttu-id="04cde-139">O compilador adicionará suporte para um novo tipo de `extern` função que se traduz com eficiência em uma instrução `.calli`.</span><span class="sxs-lookup"><span data-stu-id="04cde-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="04cde-140">O atributo externo será marcado com um atributo da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="04cde-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="04cde-141">Isso permite que os desenvolvedores definam métodos no seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="04cde-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="04cde-142">Restrições no método que tem o atributo `CallIndirect` aplicado:</span><span class="sxs-lookup"><span data-stu-id="04cde-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="04cde-143">Não é possível ter um atributo `DllImport`.</span><span class="sxs-lookup"><span data-stu-id="04cde-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="04cde-144">Não pode ser genérico.</span><span class="sxs-lookup"><span data-stu-id="04cde-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="04cde-145">Problemas em aberto</span><span class="sxs-lookup"><span data-stu-id="04cde-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="04cde-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="04cde-146">CallingConvention</span></span>

<span data-ttu-id="04cde-147">O `CallIndirectAttribute` como projetado usa o `CallingConvention` enum que não tem uma entrada para convenções de chamada gerenciadas.</span><span class="sxs-lookup"><span data-stu-id="04cde-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="04cde-148">A enumeração precisa ser estendida para incluir essa Convenção de chamada ou o atributo precisa usar uma abordagem diferente.</span><span class="sxs-lookup"><span data-stu-id="04cde-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="04cde-149">Considerações</span><span class="sxs-lookup"><span data-stu-id="04cde-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="04cde-150">Grupos de métodos de desambiguidade</span><span class="sxs-lookup"><span data-stu-id="04cde-150">Disambiguating method groups</span></span>

<span data-ttu-id="04cde-151">Havia algumas discussões sobre os recursos que tornaria mais fácil a ambiguidade dos grupos de métodos passados para uma expressão de endereço.</span><span class="sxs-lookup"><span data-stu-id="04cde-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="04cde-152">Por exemplo, potencialmente adicionando elementos de assinatura à sintaxe:</span><span class="sxs-lookup"><span data-stu-id="04cde-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="04cde-153">Isso foi rejeitado porque um caso convincente não pôde ser feito nem uma sintaxe simples pode ser configurada aqui.</span><span class="sxs-lookup"><span data-stu-id="04cde-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="04cde-154">Além disso, há um trabalho bastante simples: defina um outro método que não seja ambíguo e use C# o código para chamar a função desejada.</span><span class="sxs-lookup"><span data-stu-id="04cde-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="04cde-155">Isso se torna ainda mais simples se `static` funções locais entram no idioma.</span><span class="sxs-lookup"><span data-stu-id="04cde-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="04cde-156">Em seguida, a solução alternativa pode ser definida na mesma função que usou a operação de endereço ambíguo:</span><span class="sxs-lookup"><span data-stu-id="04cde-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="04cde-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="04cde-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="04cde-158">A proposta original permitida para que os tokens de metadados sejam carregados como `int` valores em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="04cde-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="04cde-159">Basicamente, tem `tokenof` que tem os mesmos argumentos que `handleof`, mas é avaliado em tempo de compilação para uma constante `int`.</span><span class="sxs-lookup"><span data-stu-id="04cde-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="04cde-160">Isso foi rejeitado, pois causa um problema significativo para regravações de IL (das quais o .NET tem muitos).</span><span class="sxs-lookup"><span data-stu-id="04cde-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="04cde-161">Esses regravadores geralmente manipulam as tabelas de metadados de uma maneira que pode invalidar esses valores.</span><span class="sxs-lookup"><span data-stu-id="04cde-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="04cde-162">Não há nenhuma maneira razoável para que esses regravadores atualizem esses valores quando são armazenados como valores `int` simples.</span><span class="sxs-lookup"><span data-stu-id="04cde-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="04cde-163">A ideia subjacente de ter um identificador opaco para entradas de metadados continuará a ser explorada pela equipe de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="04cde-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="04cde-164">Considerações futuras</span><span class="sxs-lookup"><span data-stu-id="04cde-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="04cde-165">funções locais estáticas</span><span class="sxs-lookup"><span data-stu-id="04cde-165">static local functions</span></span>

<span data-ttu-id="04cde-166">Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/1565) para permitir o modificador de `static` em funções locais.</span><span class="sxs-lookup"><span data-stu-id="04cde-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="04cde-167">Essa função seria garantida para ser emitida como `static` e com a assinatura exata especificada no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="04cde-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="04cde-168">Essa função deve ser um argumento válido para `&`, pois ela contém nenhum dos problemas que as funções locais têm hoje.</span><span class="sxs-lookup"><span data-stu-id="04cde-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="04cde-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="04cde-169">NativeCallableAttribute</span></span>

<span data-ttu-id="04cde-170">O CLR tem um recurso que permite que métodos gerenciados sejam emitidos de forma que eles sejam diretamente chamáveis de código nativo.</span><span class="sxs-lookup"><span data-stu-id="04cde-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="04cde-171">Isso é feito adicionando o `NativeCallableAttribute` aos métodos.</span><span class="sxs-lookup"><span data-stu-id="04cde-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="04cde-172">Esse método só pode ser chamado de código nativo e, portanto, deve conter apenas tipos blittable na assinatura.</span><span class="sxs-lookup"><span data-stu-id="04cde-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="04cde-173">A chamada a partir de código gerenciado resulta em um erro de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="04cde-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="04cde-174">Esse recurso seria bem padronizado com essa proposta, como seria permitido:</span><span class="sxs-lookup"><span data-stu-id="04cde-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="04cde-175">Passar uma função definida em código gerenciado para código nativo como um ponteiro de função (por endereço) sem sobrecarga no código gerenciado ou nativo.</span><span class="sxs-lookup"><span data-stu-id="04cde-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="04cde-176">O tempo de execução pode introduzir o uso de erros de site para essas funções no código gerenciado para impedir que eles sejam invocados no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="04cde-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




