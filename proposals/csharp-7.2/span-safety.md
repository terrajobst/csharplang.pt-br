---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485038"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="f9d49-101">Tempo de compilação imposição de segurança para tipos semelhantes a ref</span><span class="sxs-lookup"><span data-stu-id="f9d49-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="f9d49-102">Introdução</span><span class="sxs-lookup"><span data-stu-id="f9d49-102">Introduction</span></span>

<span data-ttu-id="f9d49-103">O principal motivo para as regras de segurança adicionais ao lidar com tipos como `Span<T>` e `ReadOnlySpan<T>` é que esses tipos devem ser confinados para a pilha de execução.</span><span class="sxs-lookup"><span data-stu-id="f9d49-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="f9d49-104">Há dois motivos pelos quais `Span<T>` e tipos semelhantes devem ser tipos somente de pilha.</span><span class="sxs-lookup"><span data-stu-id="f9d49-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="f9d49-105">`Span<T>` é semanticamente uma struct que contém uma referência e um `(ref T data, int length)`de intervalo.</span><span class="sxs-lookup"><span data-stu-id="f9d49-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="f9d49-106">Independentemente da implementação real, as gravações em tal struct não seriam atômicas.</span><span class="sxs-lookup"><span data-stu-id="f9d49-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="f9d49-107">A "divisão" simultânea dessa estrutura levaria à possibilidade de `length` não corresponder à `data`, causando acessos fora do intervalo e violações de segurança de tipo, o que, por fim, pode resultar na corrupção de heap de GC no código "seguro" aparentemente.</span><span class="sxs-lookup"><span data-stu-id="f9d49-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="f9d49-108">Algumas implementações de `Span<T>` literalmente contêm um ponteiro gerenciado em um de seus campos.</span><span class="sxs-lookup"><span data-stu-id="f9d49-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="f9d49-109">Não há suporte para ponteiros gerenciados, pois campos de objetos de heap e código que gerencia para colocar um ponteiro gerenciado no heap de GC normalmente falham no momento do JIT.</span><span class="sxs-lookup"><span data-stu-id="f9d49-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="f9d49-110">Todos os problemas acima seriam aliviados se instâncias de `Span<T>` estiverem restritas a existir somente na pilha de execução.</span><span class="sxs-lookup"><span data-stu-id="f9d49-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="f9d49-111">Um problema adicional surge devido à composição.</span><span class="sxs-lookup"><span data-stu-id="f9d49-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="f9d49-112">Geralmente, seria desejável criar tipos de dados mais complexos que incorporaram `Span<T>` e `ReadOnlySpan<T>` instâncias.</span><span class="sxs-lookup"><span data-stu-id="f9d49-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="f9d49-113">Esses tipos compostos teriam de ser structos e compartilhariam todos os riscos e requisitos de `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="f9d49-114">Como resultado, as regras de segurança descritas aqui devem ser exibidas como aplicáveis a todo o intervalo de **_tipos de referência_** .</span><span class="sxs-lookup"><span data-stu-id="f9d49-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="f9d49-115">A [especificação de idioma de rascunho](#draft-language-specification) destina-se a garantir que os valores de um tipo de referência ocorra somente na pilha.</span><span class="sxs-lookup"><span data-stu-id="f9d49-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="f9d49-116">Tipos de `ref-like` generalizados no código-fonte</span><span class="sxs-lookup"><span data-stu-id="f9d49-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="f9d49-117">`ref-like` structs são explicitamente marcadas no código-fonte usando o modificador `ref`:</span><span class="sxs-lookup"><span data-stu-id="f9d49-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="f9d49-118">A designação de uma struct como ref permitirá que a estrutura tenha campos de instância semelhantes a ref e também fará com que todos os requisitos de tipos de referência sejam aplicáveis à estrutura.</span><span class="sxs-lookup"><span data-stu-id="f9d49-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="f9d49-119">Representação de metadados ou estruturas semelhantes a ref</span><span class="sxs-lookup"><span data-stu-id="f9d49-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="f9d49-120">As estruturas semelhantes a ref serão marcadas com o atributo **System. Runtime. CompilerServices. IsRefLikeAttribute** .</span><span class="sxs-lookup"><span data-stu-id="f9d49-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="f9d49-121">O atributo será adicionado às bibliotecas base comuns, como `mscorlib`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="f9d49-122">Em um caso, se o atributo não estiver disponível, o compilador irá gerar um interno de forma semelhante a outros atributos incorporados sob demanda, como `IsReadOnlyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="f9d49-123">Uma medida adicional será tomada para evitar o uso de structs de referência em compiladores que não estejam familiarizados com as regras de segurança ( C# isso inclui compiladores anteriores ao em que esse recurso é implementado).</span><span class="sxs-lookup"><span data-stu-id="f9d49-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="f9d49-124">Não tendo outras boas alternativas que funcionem em compiladores antigos sem manutenção, um atributo `Obsolete` com uma cadeia de caracteres conhecida será adicionado a todas as estruturas semelhantes a ref.</span><span class="sxs-lookup"><span data-stu-id="f9d49-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="f9d49-125">Os compiladores que sabem como usar tipos de referência irão ignorar essa forma específica de `Obsolete`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="f9d49-126">Uma representação de metadados típica:</span><span class="sxs-lookup"><span data-stu-id="f9d49-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="f9d49-127">Observação: não é o objetivo de fazê-lo para que qualquer uso de tipos de referência em compiladores antigos falhe em 100%.</span><span class="sxs-lookup"><span data-stu-id="f9d49-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="f9d49-128">Isso é difícil de atingir e não é estritamente necessário.</span><span class="sxs-lookup"><span data-stu-id="f9d49-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="f9d49-129">Por exemplo, sempre há uma maneira de contornar o `Obsolete` usando código dinâmico ou, por exemplo, criando uma matriz de tipos de referência por meio de reflexão.</span><span class="sxs-lookup"><span data-stu-id="f9d49-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="f9d49-130">Em particular, se o usuário quiser colocar um atributo `Obsolete` ou `Deprecated` em um tipo de referência, não teremos nenhuma outra opção além de não emitir o predefinido, uma vez que `Obsolete` atributo não pode ser aplicado mais de uma vez.</span><span class="sxs-lookup"><span data-stu-id="f9d49-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="f9d49-131">Exemplos:</span><span class="sxs-lookup"><span data-stu-id="f9d49-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="f9d49-132">Especificação da linguagem de rascunho</span><span class="sxs-lookup"><span data-stu-id="f9d49-132">Draft language specification</span></span>

<span data-ttu-id="f9d49-133">Abaixo, descrevemos um conjunto de regras de segurança para tipos do tipo ref (`ref struct`s) para garantir que os valores desses tipos ocorram somente na pilha.</span><span class="sxs-lookup"><span data-stu-id="f9d49-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="f9d49-134">Um conjunto de regras de segurança diferente e mais simples seria possível se os locais não puderem ser passados por referência.</span><span class="sxs-lookup"><span data-stu-id="f9d49-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="f9d49-135">Essa especificação também permitiria a reatribuição segura de locais de referência.</span><span class="sxs-lookup"><span data-stu-id="f9d49-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="f9d49-136">Visão geral</span><span class="sxs-lookup"><span data-stu-id="f9d49-136">Overview</span></span>

<span data-ttu-id="f9d49-137">Nós associamos a cada expressão em tempo de compilação o conceito de qual escopo a expressão tem permissão para escapar, "seguro para escapar".</span><span class="sxs-lookup"><span data-stu-id="f9d49-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="f9d49-138">Da mesma forma, para cada lvalue, mantemos um conceito de qual escopo uma referência a ele tem permissão para escapar, "ref-safe-to-escape".</span><span class="sxs-lookup"><span data-stu-id="f9d49-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="f9d49-139">Para uma determinada expressão lvalue, elas podem ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="f9d49-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="f9d49-140">Eles são análogos ao "seguro para retornar" do recurso de locais de referência, mas é mais refinado.</span><span class="sxs-lookup"><span data-stu-id="f9d49-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="f9d49-141">Onde o "seguro-para-retornar" de uma expressão registra apenas se (ou não) ele pode escapar do método delimitador como um todo, os registros seguros para o escape para os quais o escopo pode escapar (em qual escopo ele pode não escapar).</span><span class="sxs-lookup"><span data-stu-id="f9d49-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="f9d49-142">O mecanismo de segurança básico é imposto da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="f9d49-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="f9d49-143">Devido a uma atribuição de uma expressão E1 com um escopo de segurança para escape S1, para uma expressão (lvalue) E2 com escopo seguro para escape S2, será um erro se S2 for um escopo maior do que S1.</span><span class="sxs-lookup"><span data-stu-id="f9d49-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="f9d49-144">Por construção, os dois escopos S1 e S2 estão em uma relação de aninhamento, porque uma expressão legal sempre é segura para retornar de algum escopo delimitando a expressão.</span><span class="sxs-lookup"><span data-stu-id="f9d49-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="f9d49-145">Por enquanto, é o suficiente para a finalidade da análise, dar suporte a apenas dois escopos – externos ao método e ao escopo de nível superior do método.</span><span class="sxs-lookup"><span data-stu-id="f9d49-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="f9d49-146">Isso ocorre porque os valores like ref com escopos internos não podem ser criados e os locais de referência não dão suporte à reatribuição.</span><span class="sxs-lookup"><span data-stu-id="f9d49-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="f9d49-147">As regras, no entanto, podem dar suporte a mais de dois níveis de escopo.</span><span class="sxs-lookup"><span data-stu-id="f9d49-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="f9d49-148">As regras precisas para a computação do status de *segurança para retorno* de uma expressão e as regras que regem a legalidade de expressões, seguem.</span><span class="sxs-lookup"><span data-stu-id="f9d49-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="f9d49-149">ref-safe-to-escape</span><span class="sxs-lookup"><span data-stu-id="f9d49-149">ref-safe-to-escape</span></span>

<span data-ttu-id="f9d49-150">O *ref-safe-to-escape* é um escopo, colocando uma expressão lvalue, à qual é seguro para uma ref ao lvalue para escapar.</span><span class="sxs-lookup"><span data-stu-id="f9d49-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="f9d49-151">Se esse escopo for o método inteiro, dizemos que uma referência ao lvalue é *segura para retornar* do método.</span><span class="sxs-lookup"><span data-stu-id="f9d49-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="f9d49-152">seguro para escapar</span><span class="sxs-lookup"><span data-stu-id="f9d49-152">safe-to-escape</span></span>

<span data-ttu-id="f9d49-153">O *seguro para escapar* é um escopo, delimitando uma expressão, à qual é seguro para o valor escapar.</span><span class="sxs-lookup"><span data-stu-id="f9d49-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="f9d49-154">Se esse escopo for o método inteiro, dizemos que um valor é *seguro para retornar* do método.</span><span class="sxs-lookup"><span data-stu-id="f9d49-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="f9d49-155">Uma expressão cujo tipo não é um tipo de `ref struct` é *seguro para retornar* de todo o método delimitador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="f9d49-156">Caso contrário, nos referimos às regras abaixo.</span><span class="sxs-lookup"><span data-stu-id="f9d49-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="f9d49-157">Parâmetros</span><span class="sxs-lookup"><span data-stu-id="f9d49-157">Parameters</span></span>

<span data-ttu-id="f9d49-158">Um lvalue que designa um parâmetro formal é *ref-safe-to-escape* (por referência) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9d49-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="f9d49-159">Se o parâmetro for um parâmetro `ref`, `out`ou `in`, ele será de *ref-safe-to-escape* de todo o método (por exemplo, por uma instrução `return ref`); ,</span><span class="sxs-lookup"><span data-stu-id="f9d49-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="f9d49-160">Se o parâmetro for o parâmetro `this` de um tipo struct, ele será de *referência segura a escape* para o escopo de nível superior do método (mas não de todo o próprio método); [Exemplo](#struct-this-escape) de</span><span class="sxs-lookup"><span data-stu-id="f9d49-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="f9d49-161">Caso contrário, o parâmetro é um parâmetro de valor e ele é de *referência segura para escape* para o escopo de nível superior do método (mas não do próprio método).</span><span class="sxs-lookup"><span data-stu-id="f9d49-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="f9d49-162">Uma expressão que é um Rvalue que designa o uso de um parâmetro formal é *segura para escapar* (por valor) do método inteiro (por exemplo, por uma instrução `return`).</span><span class="sxs-lookup"><span data-stu-id="f9d49-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="f9d49-163">Isso se aplica ao parâmetro `this` também.</span><span class="sxs-lookup"><span data-stu-id="f9d49-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="f9d49-164">Locais</span><span class="sxs-lookup"><span data-stu-id="f9d49-164">Locals</span></span>

<span data-ttu-id="f9d49-165">Um lvalue que designa uma variável local é *ref-safe-to-escape* (por referência) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9d49-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="f9d49-166">Se a variável for uma variável `ref`, seu *ref-safe-to-escape* será obtido do *ref-safe-to-escape* de sua expressão de inicialização; ,</span><span class="sxs-lookup"><span data-stu-id="f9d49-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="f9d49-167">A variável é *ref-safe-to-escape* no escopo no qual ele foi declarado.</span><span class="sxs-lookup"><span data-stu-id="f9d49-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="f9d49-168">Uma expressão que é um Rvalue que designa o uso de uma variável local é *segura para escapar* (por valor) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9d49-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="f9d49-169">Mas a regra geral acima, um local cujo tipo não é um tipo de `ref struct` é *seguro para retornar* de todo o método delimitador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="f9d49-170">Se a variável for uma variável de iteração de um loop de `foreach`, o escopo de *segurança para escape* da variável será o mesmo que o *seguro para escapar* da expressão do loop de `foreach`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="f9d49-171">Um local de `ref struct` tipo e não inicializado no ponto de declaração é *seguro para retornar* de todo o método delimitador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="f9d49-172">Caso contrário, o tipo da variável é um tipo de `ref struct`, e a declaração da variável requer um inicializador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="f9d49-173">O escopo de *segurança para escape* da variável é o mesmo que o de *segurança para escapar* de seu inicializador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="f9d49-174">Referência de campo</span><span class="sxs-lookup"><span data-stu-id="f9d49-174">Field reference</span></span>

<span data-ttu-id="f9d49-175">Um lvalue que designa uma referência a um campo `e.F`é *ref-safe-to-escape* (por referência) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9d49-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="f9d49-176">Se `e` for de um tipo de referência, ele será de *ref-safe-to-escape* de todo o método; ,</span><span class="sxs-lookup"><span data-stu-id="f9d49-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="f9d49-177">Se `e` for de um tipo de valor, seu *ref-safe-to-escape* será tirado da *referência-safe-to-escape* de `e`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="f9d49-178">Um Rvalue que designa uma referência a um campo, `e.F`, tem um escopo *seguro para escapar* , que é o mesmo que o *seguro para escapar* do `e`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="f9d49-179">Operadores, incluindo `?:`</span><span class="sxs-lookup"><span data-stu-id="f9d49-179">Operators including `?:`</span></span>

<span data-ttu-id="f9d49-180">O aplicativo de um operador definido pelo usuário é tratado como uma invocação de método.</span><span class="sxs-lookup"><span data-stu-id="f9d49-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="f9d49-181">Para um operador que produz um Rvalue, como `e1 + e2` ou `c ? e1 : e2`, o seguro- *para-escape* do resultado é o escopo mais estreito entre os operandos de *segurança para escape* do operador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="f9d49-182">Em consequência disso, para um operador unário que produz um Rvalue, como `+e`, o *seguro para escapar* do resultado é o *seguro para escapar* do operando.</span><span class="sxs-lookup"><span data-stu-id="f9d49-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="f9d49-183">Para um operador que produz um lvalue, como `c ? ref e1 : ref e2`</span><span class="sxs-lookup"><span data-stu-id="f9d49-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="f9d49-184">a *ref-safe-to-escape* do resultado é o escopo mais estreito entre o *ref-safe-to-escape* dos operandos do operador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="f9d49-185">o *seguro para escapar* dos operandos deve concordar, e isso é o *seguro para escapar* do lvalue resultante.</span><span class="sxs-lookup"><span data-stu-id="f9d49-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="f9d49-186">Invocação de método</span><span class="sxs-lookup"><span data-stu-id="f9d49-186">Method invocation</span></span>

<span data-ttu-id="f9d49-187">Um lvalue resultante de uma invocação de método de retorno de referência `e1.M(e2, ...)` é de *referência segura para escapar* do menor dos seguintes escopos:</span><span class="sxs-lookup"><span data-stu-id="f9d49-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="f9d49-188">O método de circunscrição inteiro</span><span class="sxs-lookup"><span data-stu-id="f9d49-188">The entire enclosing method</span></span>
- <span data-ttu-id="f9d49-189">o *ref-safe-to-escape* de todas as expressões de argumento `ref` e `out` (excluindo o receptor)</span><span class="sxs-lookup"><span data-stu-id="f9d49-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="f9d49-190">Para cada parâmetro de `in` do método, se houver uma expressão correspondente que seja um lvalue, sua *ref-safe-to-escape*, caso contrário, o escopo de delimitação mais próximo</span><span class="sxs-lookup"><span data-stu-id="f9d49-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="f9d49-191">o *seguro para escapar* de todas as expressões de argumento (incluindo o receptor)</span><span class="sxs-lookup"><span data-stu-id="f9d49-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="f9d49-192">Observação: o último marcador é necessário para manipular código como</span><span class="sxs-lookup"><span data-stu-id="f9d49-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="f9d49-193">ou</span><span class="sxs-lookup"><span data-stu-id="f9d49-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="f9d49-194">Um Rvalue resultante de uma invocação de método `e1.M(e2, ...)` é *seguro para escapar* do menor dos seguintes escopos:</span><span class="sxs-lookup"><span data-stu-id="f9d49-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="f9d49-195">O método de circunscrição inteiro</span><span class="sxs-lookup"><span data-stu-id="f9d49-195">The entire enclosing method</span></span>
- <span data-ttu-id="f9d49-196">o *seguro para escapar* de todas as expressões de argumento (incluindo o receptor)</span><span class="sxs-lookup"><span data-stu-id="f9d49-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="f9d49-197">Um Rvalue</span><span class="sxs-lookup"><span data-stu-id="f9d49-197">An Rvalue</span></span>
<span data-ttu-id="f9d49-198">Um Rvalue é *ref-safe-to-escape* do escopo de delimitação mais próximo.</span><span class="sxs-lookup"><span data-stu-id="f9d49-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="f9d49-199">Isso ocorre, por exemplo, em uma invocação, como `M(ref d.Length)` em que `d` é do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="f9d49-200">Ele também é consistente com (e talvez incorporou) nossa manipulação de argumentos correspondentes a parâmetros de `in`. \*</span><span class="sxs-lookup"><span data-stu-id="f9d49-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="f9d49-201">Invocações de propriedade</span><span class="sxs-lookup"><span data-stu-id="f9d49-201">Property invocations</span></span>

<span data-ttu-id="f9d49-202">Uma invocação de propriedade (`get` ou `set`) tratada como uma invocação de método do método subjacente pelas regras acima.</span><span class="sxs-lookup"><span data-stu-id="f9d49-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="f9d49-203">Uma expressão stackalloc é um Rvalue que é *seguro para escapar* do escopo de nível superior do método (mas não de todo o próprio método).</span><span class="sxs-lookup"><span data-stu-id="f9d49-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="f9d49-204">Invocações de Construtor</span><span class="sxs-lookup"><span data-stu-id="f9d49-204">Constructor invocations</span></span>

<span data-ttu-id="f9d49-205">Uma `new` expressão que invoca um Construtor obedece às mesmas regras que uma invocação de método que é considerada para retornar o tipo que está sendo construído.</span><span class="sxs-lookup"><span data-stu-id="f9d49-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="f9d49-206">Além disso, o *safe-to-escape* não é mais largo do que o menor de *todos os argumentos* /operandos das expressões de inicializador de objeto, recursivamente, se o inicializador estiver presente.</span><span class="sxs-lookup"><span data-stu-id="f9d49-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="f9d49-207">Construtor de span</span><span class="sxs-lookup"><span data-stu-id="f9d49-207">Span constructor</span></span>
<span data-ttu-id="f9d49-208">O idioma depende de `Span<T>` não ter um construtor da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="f9d49-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="f9d49-209">Esse construtor faz `Span<T>` que são usados como campos indistinguíveis de um campo `ref`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="f9d49-210">As regras de segurança descritas neste documento dependem de `ref` campos que não são uma construção C#válida no ou no .net.</span><span class="sxs-lookup"><span data-stu-id="f9d49-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="f9d49-211">Expressões `default`</span><span class="sxs-lookup"><span data-stu-id="f9d49-211">`default` expressions</span></span>

<span data-ttu-id="f9d49-212">Uma expressão de `default` é *segura para escapar* de todo o método delimitador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="f9d49-213">Restrições de idioma</span><span class="sxs-lookup"><span data-stu-id="f9d49-213">Language Constraints</span></span>

<span data-ttu-id="f9d49-214">Gostaríamos de garantir que nenhuma variável local `ref` e nenhuma variável de `ref struct` tipo, se refere à memória de pilha ou a variáveis que não estão mais ativas.</span><span class="sxs-lookup"><span data-stu-id="f9d49-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="f9d49-215">Portanto, temos as seguintes restrições de idioma:</span><span class="sxs-lookup"><span data-stu-id="f9d49-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="f9d49-216">Nem um parâmetro ref, nem um local ref, nem um parâmetro ou local de um tipo de `ref struct` podem ser divididos em uma função lambda ou local.</span><span class="sxs-lookup"><span data-stu-id="f9d49-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="f9d49-217">Nem um parâmetro ref nem um parâmetro de um tipo `ref struct` pode ser um argumento em um método Iterator ou um método `async`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="f9d49-218">Nem um local de referência, nem um local de um tipo de `ref struct` pode estar no escopo no ponto de uma instrução `yield return` ou uma expressão `await`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="f9d49-219">Um tipo de `ref struct` não pode ser usado como um argumento de tipo ou como um tipo de elemento em um tipo de tupla.</span><span class="sxs-lookup"><span data-stu-id="f9d49-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="f9d49-220">Um tipo de `ref struct` não pode ser o tipo declarado de um campo, exceto que ele pode ser o tipo declarado de um campo de instância de outro `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="f9d49-221">Um tipo de `ref struct` não pode ser o tipo de elemento de uma matriz.</span><span class="sxs-lookup"><span data-stu-id="f9d49-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="f9d49-222">Um valor de um tipo de `ref struct` não pode ser Boxed:</span><span class="sxs-lookup"><span data-stu-id="f9d49-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="f9d49-223">Não há nenhuma conversão de um tipo de `ref struct` para o tipo `object` ou o tipo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="f9d49-224">Um tipo de `ref struct` não pode ser declarado para implementar qualquer interface</span><span class="sxs-lookup"><span data-stu-id="f9d49-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="f9d49-225">Nenhum método de instância declarado em `object` ou em `System.ValueType`, mas não substituído em um tipo `ref struct` pode ser chamado com um destinatário desse tipo de `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="f9d49-226">Nenhum método de instância de um tipo de `ref struct` pode ser capturado pela conversão de método para um tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="f9d49-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="f9d49-227">Para uma `ref e1 = ref e2`de reatribuição de referência, o *ref-safe-to-escape* de `e2` deve ser pelo menos tão amplo quanto o escopo de *ref-safe-to-escape* de `e1`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="f9d49-228">Para uma instrução de retorno de ref `return ref e1`, o *ref-safe-to-escape* de `e1` deve ser *ref-safe-to-escape* de todo o método.</span><span class="sxs-lookup"><span data-stu-id="f9d49-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="f9d49-229">(TODO: também precisamos de uma regra que `e1` deve ser *segura para escapar* do método inteiro ou que seja redundante?)</span><span class="sxs-lookup"><span data-stu-id="f9d49-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="f9d49-230">Para uma instrução de retorno `return e1`, a *segurança* de `e1` deve ser *segura para escapar* do método inteiro.</span><span class="sxs-lookup"><span data-stu-id="f9d49-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="f9d49-231">Para uma atribuição `e1 = e2`, se o tipo de `e1` for um tipo de `ref struct`, o *seguro para escape* de `e2` deve ser pelo menos tão amplo quanto o escopo da `e1`de *segurança para escape* .</span><span class="sxs-lookup"><span data-stu-id="f9d49-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="f9d49-232">Para uma invocação de método se houver um argumento `ref` ou `out` de um tipo de `ref struct` (incluindo o receptor), com E1 de *segurança para escape* , nenhum argumento (incluindo o receptor) poderá ter um limite *mais* estreito do que o E1.</span><span class="sxs-lookup"><span data-stu-id="f9d49-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="f9d49-233">Amostra</span><span class="sxs-lookup"><span data-stu-id="f9d49-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="f9d49-234">Uma função local ou anônima pode não se referir a um local ou parâmetro do tipo `ref struct` declarado em um escopo delimitador.</span><span class="sxs-lookup"><span data-stu-id="f9d49-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="f9d49-235">***Abrir problema:*** Precisamos de alguma regra que nos permita produzir um erro quando precisar despejar um valor de pilha de um tipo de `ref struct` em uma expressão Await, por exemplo, no código</span><span class="sxs-lookup"><span data-stu-id="f9d49-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="f9d49-236">Sobre</span><span class="sxs-lookup"><span data-stu-id="f9d49-236">Explanations</span></span>
<span data-ttu-id="f9d49-237">Essas explicações e exemplos ajudam a explicar por que muitas das regras de segurança acima existem</span><span class="sxs-lookup"><span data-stu-id="f9d49-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="f9d49-238">Argumentos de método devem corresponder</span><span class="sxs-lookup"><span data-stu-id="f9d49-238">Method Arguments Must Match</span></span>
<span data-ttu-id="f9d49-239">Ao invocar um método em que há um `out`, `ref` parâmetro que é um `ref struct` incluindo o destinatário, todas as `ref struct` precisam ter o mesmo tempo de vida.</span><span class="sxs-lookup"><span data-stu-id="f9d49-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="f9d49-240">Isso é necessário porque C# o deve tomar todas as decisões sobre a segurança do tempo de vida com base nas informações disponíveis na assinatura do método e no tempo de vida dos valores no site de chamada.</span><span class="sxs-lookup"><span data-stu-id="f9d49-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="f9d49-241">Quando há `ref` parâmetros que são `ref struct`, há o possibilidade que podem alternar em seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="f9d49-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="f9d49-242">Portanto, no local de chamada, devemos garantir que todas essas trocas **potenciais** sejam compatíveis.</span><span class="sxs-lookup"><span data-stu-id="f9d49-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="f9d49-243">Se o idioma não tiver aplicado isso, ele permitirá um código inadequado como o seguinte.</span><span class="sxs-lookup"><span data-stu-id="f9d49-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="f9d49-244">A restrição no receptor é necessária porque, embora nenhum dos seus conteúdos seja ref-safe-to-escape, ele pode armazenar os valores fornecidos.</span><span class="sxs-lookup"><span data-stu-id="f9d49-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="f9d49-245">Isso significa que, com tempos de vida incompatíveis, você poderia criar um buraco de segurança de tipo da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9d49-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="f9d49-246">Estruturar este escape</span><span class="sxs-lookup"><span data-stu-id="f9d49-246">Struct This Escape</span></span>
<span data-ttu-id="f9d49-247">Quando se trata de estender regras de segurança, o valor de `this` em um membro de instância é modelado como um parâmetro para o membro.</span><span class="sxs-lookup"><span data-stu-id="f9d49-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="f9d49-248">Agora, para um `struct` o tipo de `this` é, na verdade, `ref S` onde em uma `class` é simplesmente `S` (para membros de um `class / struct` chamado S).</span><span class="sxs-lookup"><span data-stu-id="f9d49-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="f9d49-249">Ainda `this` tem diferentes regras de escape do que outros parâmetros de `ref`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="f9d49-250">Especificamente, não é válido para ref-safe-to-escape enquanto outros parâmetros são:</span><span class="sxs-lookup"><span data-stu-id="f9d49-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="f9d49-251">A razão para essa restrição realmente tem pouco a ver com `struct` invocação de membro.</span><span class="sxs-lookup"><span data-stu-id="f9d49-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="f9d49-252">Há algumas regras que precisam ser configuradas em relação à invocação de membro em `struct` Membros em que o receptor é um Rvalue.</span><span class="sxs-lookup"><span data-stu-id="f9d49-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="f9d49-253">Mas isso é muito acessível.</span><span class="sxs-lookup"><span data-stu-id="f9d49-253">But that is very approachable.</span></span> 

<span data-ttu-id="f9d49-254">A razão para essa restrição é, na verdade, sobre a invocação de interface.</span><span class="sxs-lookup"><span data-stu-id="f9d49-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="f9d49-255">Especificamente, isso se resume se o exemplo a seguir deve ou não ser compilado;</span><span class="sxs-lookup"><span data-stu-id="f9d49-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="f9d49-256">Considere o caso em que `T` é instanciado como um `struct`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="f9d49-257">Se o parâmetro `this` for ref-safe-to-escape, o retorno de `p.Get` poderá apontar para a pilha (especificamente, poderia ser um campo dentro do tipo instanciado de `T`).</span><span class="sxs-lookup"><span data-stu-id="f9d49-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="f9d49-258">Isso significa que o idioma não pôde permitir a compilação deste exemplo, pois ele poderia retornar um `ref` para um local de pilha.</span><span class="sxs-lookup"><span data-stu-id="f9d49-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="f9d49-259">Por outro lado, se `this` não for ref-safe-to-escape, `p.Get` não poderá se referir à pilha e, portanto, será seguro retornar.</span><span class="sxs-lookup"><span data-stu-id="f9d49-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="f9d49-260">É por isso que a saída de `this` em uma `struct` é, na verdade, todas as interfaces.</span><span class="sxs-lookup"><span data-stu-id="f9d49-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="f9d49-261">Ele pode ser absolutamente feito para funcionar, mas tem uma compensação.</span><span class="sxs-lookup"><span data-stu-id="f9d49-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="f9d49-262">O design eventualmente surgiu em favor de tornar as interfaces mais flexíveis.</span><span class="sxs-lookup"><span data-stu-id="f9d49-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="f9d49-263">No entanto, é possível relaxar isso no futuro.</span><span class="sxs-lookup"><span data-stu-id="f9d49-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="f9d49-264">Considerações futuras</span><span class="sxs-lookup"><span data-stu-id="f9d49-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="f9d49-265">Comprimento um\<T > sobre valores de referência</span><span class="sxs-lookup"><span data-stu-id="f9d49-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="f9d49-266">Embora não seja legal hoje, há casos em que criar um comprimento `Span<T>` instância em um valor seria benéfico:</span><span class="sxs-lookup"><span data-stu-id="f9d49-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="f9d49-267">Esse recurso será mais atraente se compararmos as restrições em [buffers de tamanho fixo](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , pois permitiria `Span<T>` instâncias de comprimento ainda maior.</span><span class="sxs-lookup"><span data-stu-id="f9d49-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="f9d49-268">Se houver alguma necessidade de reduzir esse caminho, o idioma poderá acomodar isso, garantindo que instâncias de `Span<T>` estavam apenas para frente.</span><span class="sxs-lookup"><span data-stu-id="f9d49-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="f9d49-269">Isso é que, no momento *,* eles eram seguros para o escopo no qual foram criados.</span><span class="sxs-lookup"><span data-stu-id="f9d49-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="f9d49-270">Isso garante que o idioma nunca tenha que considerar um valor `ref` escapar de um método por meio de um `ref struct` retorno ou campo de `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="f9d49-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="f9d49-271">Isso provavelmente também exigiria outras alterações para reconhecer esses construtores como capturar um parâmetro de `ref` desse modo.</span><span class="sxs-lookup"><span data-stu-id="f9d49-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
