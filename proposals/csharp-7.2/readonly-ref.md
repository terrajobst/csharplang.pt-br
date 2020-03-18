---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485108"
---
# <a name="readonly-references"></a><span data-ttu-id="e1504-101">Referências somente leitura</span><span class="sxs-lookup"><span data-stu-id="e1504-101">Readonly references</span></span>

* <span data-ttu-id="e1504-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="e1504-102">[x] Proposed</span></span>
* <span data-ttu-id="e1504-103">[x] protótipo</span><span class="sxs-lookup"><span data-stu-id="e1504-103">[x] Prototype</span></span>
* <span data-ttu-id="e1504-104">[x] implementação: iniciada</span><span class="sxs-lookup"><span data-stu-id="e1504-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="e1504-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="e1504-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="e1504-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="e1504-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e1504-107">O recurso "referências somente leitura" é, na verdade, um grupo de recursos que aproveitam a eficiência da passagem de variáveis por referência, mas sem expor os dados às modificações:</span><span class="sxs-lookup"><span data-stu-id="e1504-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="e1504-108">`in` parâmetros</span><span class="sxs-lookup"><span data-stu-id="e1504-108">`in` parameters</span></span>
- <span data-ttu-id="e1504-109">Retornos de `ref readonly`</span><span class="sxs-lookup"><span data-stu-id="e1504-109">`ref readonly` returns</span></span>
- <span data-ttu-id="e1504-110">`readonly` structs</span><span class="sxs-lookup"><span data-stu-id="e1504-110">`readonly` structs</span></span>
- <span data-ttu-id="e1504-111">`ref`/métodos de extensão `in`</span><span class="sxs-lookup"><span data-stu-id="e1504-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="e1504-112">locais `ref readonly`</span><span class="sxs-lookup"><span data-stu-id="e1504-112">`ref readonly` locals</span></span>
- <span data-ttu-id="e1504-113">`ref` expressões condicionais</span><span class="sxs-lookup"><span data-stu-id="e1504-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="e1504-114">Passando argumentos como referências somente leitura.</span><span class="sxs-lookup"><span data-stu-id="e1504-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="e1504-115">Há uma proposta existente que toca neste tópico https://github.com/dotnet/roslyn/issues/115 como um caso especial de parâmetros ReadOnly sem entrar em muitos detalhes.</span><span class="sxs-lookup"><span data-stu-id="e1504-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="e1504-116">Aqui, quero apenas reconhecer que a ideia propriamente dita não é muito nova.</span><span class="sxs-lookup"><span data-stu-id="e1504-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="e1504-117">Motivação</span><span class="sxs-lookup"><span data-stu-id="e1504-117">Motivation</span></span>

<span data-ttu-id="e1504-118">Antes desse recurso C# não havia uma maneira eficiente de expressar um desejo de passar variáveis struct em chamadas de método para fins somente leitura sem intenção de modificar.</span><span class="sxs-lookup"><span data-stu-id="e1504-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="e1504-119">O argumento regular por valor passando implica em copiar, o que adiciona custos desnecessários.</span><span class="sxs-lookup"><span data-stu-id="e1504-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="e1504-120">Isso orienta os usuários a usar o argumento-ref passando e a contar com comentários/documentação para indicar que os dados não devem ser modificados pelo receptor.</span><span class="sxs-lookup"><span data-stu-id="e1504-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="e1504-121">Não é uma boa solução por muitos motivos.</span><span class="sxs-lookup"><span data-stu-id="e1504-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="e1504-122">Os exemplos são muitos operadores matemáticos/matrizes matemáticas em bibliotecas gráficas, como o [XNA](https://msdn.microsoft.com/library/bb194944.aspx) é conhecido por ter operandos de referência puramente devido a considerações de desempenho.</span><span class="sxs-lookup"><span data-stu-id="e1504-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="e1504-123">Há código no próprio compilador Roslyn que usa structs para evitar alocações e as passa por referência para evitar a cópia de custos.</span><span class="sxs-lookup"><span data-stu-id="e1504-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="e1504-124">Solução (parâmetros de`in`)</span><span class="sxs-lookup"><span data-stu-id="e1504-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="e1504-125">Da mesma forma que os parâmetros de `out`, `in` parâmetros são passados como referências gerenciadas com garantias adicionais do receptor.</span><span class="sxs-lookup"><span data-stu-id="e1504-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="e1504-126">Ao contrário dos parâmetros de `out` que _devem_ ser atribuídos pelo receptor antes de qualquer outro uso, `in` parâmetros não podem ser atribuídos pelo receptor.</span><span class="sxs-lookup"><span data-stu-id="e1504-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="e1504-127">Como resultado `in` parâmetros permitem a eficácia da passagem de argumentos indiretos sem expor argumentos a mutações pelo receptor.</span><span class="sxs-lookup"><span data-stu-id="e1504-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="e1504-128">Declarando parâmetros `in`</span><span class="sxs-lookup"><span data-stu-id="e1504-128">Declaring `in` parameters</span></span>

<span data-ttu-id="e1504-129">parâmetros de `in` são declarados usando a palavra-chave `in` como um modificador na assinatura de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="e1504-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="e1504-130">Para todas as finalidades, o parâmetro `in` é tratado como uma variável `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="e1504-131">A maioria das restrições sobre o uso de `in` parâmetros dentro do método são as mesmas que com `readonly` campos.</span><span class="sxs-lookup"><span data-stu-id="e1504-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="e1504-132">Na verdade, um parâmetro `in` pode representar um campo de `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="e1504-133">A similaridade de restrições não é um coincidência.</span><span class="sxs-lookup"><span data-stu-id="e1504-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="e1504-134">Por exemplo, os campos de um parâmetro de `in` que tem um tipo struct são todos classificados recursivamente como variáveis `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="e1504-135">`in` parâmetros são permitidos em qualquer lugar em que parâmetros de ByVal comuns são permitidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="e1504-136">Isso inclui indexadores, operadores (incluindo conversões), delegados, lambdas, funções locais.</span><span class="sxs-lookup"><span data-stu-id="e1504-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="e1504-137">`in` não é permitido em combinação com `out` ou com qualquer coisa que `out` não combina com.</span><span class="sxs-lookup"><span data-stu-id="e1504-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="e1504-138">Não é permitido sobrecarregar `ref`/`out`/diferenças de `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="e1504-139">É permitido sobrecarregar em diferenças comuns de ByVal e `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="e1504-140">Com a finalidade de OHI (sobrecarregar, ocultar, implementar), `in` se comporta de forma semelhante a um parâmetro `out`.</span><span class="sxs-lookup"><span data-stu-id="e1504-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="e1504-141">Todas as mesmas regras se aplicam.</span><span class="sxs-lookup"><span data-stu-id="e1504-141">All the same rules apply.</span></span>
<span data-ttu-id="e1504-142">Por exemplo, o método de substituição terá que Coincidir `in` parâmetros com `in` parâmetros de um tipo conversível de identidade.</span><span class="sxs-lookup"><span data-stu-id="e1504-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="e1504-143">Para a finalidade das conversões de grupo delegate/lambda/Method, `in` se comporta de forma semelhante a um parâmetro `out`.</span><span class="sxs-lookup"><span data-stu-id="e1504-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="e1504-144">As lambdas e os candidatos à conversão do grupo de métodos aplicáveis terão que corresponder `in` parâmetros do delegado de destino com parâmetros `in` de um tipo de conversível de identidade.</span><span class="sxs-lookup"><span data-stu-id="e1504-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="e1504-145">Para fins de variância genérica, `in` parâmetros são nonvariant.</span><span class="sxs-lookup"><span data-stu-id="e1504-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="e1504-146">Observação: não há avisos sobre `in` parâmetros que têm tipos de referência ou primitivos.</span><span class="sxs-lookup"><span data-stu-id="e1504-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="e1504-147">Pode ser inútil em geral, mas, em alguns casos, o usuário deve/deseja passar primitivos como `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="e1504-148">Exemplos – substituindo um método genérico como `Method(in T param)` quando `T` foi substituído para ser `int`ou ao ter métodos como `Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="e1504-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="e1504-149">É concebível ter um analisador que avisa em casos de uso ineficiente de parâmetros de `in`, mas as regras para essa análise seriam muito difusas para fazer parte de uma especificação de linguagem.</span><span class="sxs-lookup"><span data-stu-id="e1504-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="e1504-150">Uso de `in` em sites de chamada.</span><span class="sxs-lookup"><span data-stu-id="e1504-150">Use of `in` at call sites.</span></span> <span data-ttu-id="e1504-151">(argumentos de`in`)</span><span class="sxs-lookup"><span data-stu-id="e1504-151">(`in` arguments)</span></span>

<span data-ttu-id="e1504-152">Há duas maneiras de passar argumentos para `in` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e1504-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="e1504-153">`in` argumentos podem corresponder `in` parâmetros:</span><span class="sxs-lookup"><span data-stu-id="e1504-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="e1504-154">Um argumento com um modificador de `in` no site de chamada pode corresponder `in` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e1504-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="e1504-155">`in` argumento deve ser um LValue _legível_ (\*).</span><span class="sxs-lookup"><span data-stu-id="e1504-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="e1504-156">Exemplo: `M1(in 42)` é inválido</span><span class="sxs-lookup"><span data-stu-id="e1504-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="e1504-157">(\*) A noção de [LValue/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) varia entre os idiomas.</span><span class="sxs-lookup"><span data-stu-id="e1504-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="e1504-158">Aqui, por LValue, quero dizer uma expressão que representa um local que pode ser referenciado diretamente.</span><span class="sxs-lookup"><span data-stu-id="e1504-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="e1504-159">E RValue significa uma expressão que produz um resultado temporário que não persiste por conta própria.</span><span class="sxs-lookup"><span data-stu-id="e1504-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="e1504-160">Em particular, é válido passar `readonly` campos, `in` parâmetros ou outras variáveis de `readonly` formalmente como argumentos de `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="e1504-161">Exemplo: `dictionary[in Guid.Empty]` é legal.</span><span class="sxs-lookup"><span data-stu-id="e1504-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="e1504-162">`Guid.Empty` é um campo somente leitura estático.</span><span class="sxs-lookup"><span data-stu-id="e1504-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="e1504-163">`in` argumento deve ter o tipo _Identity-conversível_ para o tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="e1504-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="e1504-164">Exemplo: `M1<object>(in Guid.Empty)` é inválido.</span><span class="sxs-lookup"><span data-stu-id="e1504-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="e1504-165">`Guid.Empty` não é _conversível em identidade_ para `object`</span><span class="sxs-lookup"><span data-stu-id="e1504-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="e1504-166">A motivação para as regras acima é que `in` argumentos garantem a _alias_ da variável de argumento.</span><span class="sxs-lookup"><span data-stu-id="e1504-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="e1504-167">O receptor sempre recebe uma referência direta para o mesmo local, conforme representado pelo argumento.</span><span class="sxs-lookup"><span data-stu-id="e1504-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="e1504-168">em raras situações em que os argumentos de `in` devem ser despejados em pilha devido a `await` expressões usadas como operandos da mesma chamada, o comportamento é o mesmo que os argumentos `out` e `ref`-se a variável não puder ser despejada de maneira referencialmente transparente, um erro será relatado.</span><span class="sxs-lookup"><span data-stu-id="e1504-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="e1504-169">Exemplos:</span><span class="sxs-lookup"><span data-stu-id="e1504-169">Examples:</span></span>
1. <span data-ttu-id="e1504-170">`M1(in staticField, await SomethingAsync())` é válido.</span><span class="sxs-lookup"><span data-stu-id="e1504-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="e1504-171">`staticField` é um campo estático que pode ser acessado mais de uma vez sem efeitos colaterais observáveis.</span><span class="sxs-lookup"><span data-stu-id="e1504-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="e1504-172">Portanto, a ordem dos efeitos colaterais e os requisitos de alias podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="e1504-173">`M1(in RefReturningMethod(), await SomethingAsync())` produzirá um erro.</span><span class="sxs-lookup"><span data-stu-id="e1504-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="e1504-174">`RefReturningMethod()` é um método `ref` retornando.</span><span class="sxs-lookup"><span data-stu-id="e1504-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="e1504-175">Uma chamada de método pode ter efeitos colaterais observáveis, portanto, ela deve ser avaliada antes do operando de `SomethingAsync()`.</span><span class="sxs-lookup"><span data-stu-id="e1504-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="e1504-176">No entanto, o resultado da invocação é uma referência que não pode ser preservada no ponto de suspensão `await` que torna impossível o requisito de referência direta.</span><span class="sxs-lookup"><span data-stu-id="e1504-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="e1504-177">Observação: os erros de despejo de pilha são considerados limitações específicas da implementação.</span><span class="sxs-lookup"><span data-stu-id="e1504-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="e1504-178">Portanto, eles não têm efeito sobre a resolução de sobrecarga ou a inferência de lambda.</span><span class="sxs-lookup"><span data-stu-id="e1504-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="e1504-179">Argumentos de ByVal comuns podem corresponder a parâmetros `in`:</span><span class="sxs-lookup"><span data-stu-id="e1504-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="e1504-180">Argumentos regulares sem modificadores podem corresponder `in` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e1504-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="e1504-181">Nesse caso, os argumentos têm as mesmas restrições relaxadas que os argumentos comuns de ByVal teriam.</span><span class="sxs-lookup"><span data-stu-id="e1504-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="e1504-182">A motivação para esse cenário é que `in` parâmetros nas APIs podem resultar em inconveniências para o usuário quando argumentos não podem ser passados como referência direta-ex: literais, resultados computados ou `await`-Ed ou argumentos que ocorrem para ter tipos mais específicos.</span><span class="sxs-lookup"><span data-stu-id="e1504-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="e1504-183">Todos esses casos têm uma solução trivial de armazenar o valor do argumento em um local temporário do tipo apropriado e passar esse local como um argumento `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="e1504-184">Para reduzir a necessidade de tal compilador de código clichê pode executar a mesma transformação, se necessário, quando `in` modificador não está presente no site de chamada.</span><span class="sxs-lookup"><span data-stu-id="e1504-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="e1504-185">Além disso, em alguns casos, como invocação de operadores ou `in` métodos de extensão, não há nenhuma maneira sintática de especificar `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="e1504-186">Isso sozinho requer a especificação do comportamento de argumentos de ByVal comuns quando eles correspondem `in` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e1504-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="e1504-187">Em particular:</span><span class="sxs-lookup"><span data-stu-id="e1504-187">In particular:</span></span>

- <span data-ttu-id="e1504-188">é válido passar RValues.</span><span class="sxs-lookup"><span data-stu-id="e1504-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="e1504-189">Uma referência a um temporário é passada nesse caso.</span><span class="sxs-lookup"><span data-stu-id="e1504-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="e1504-190">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="e1504-191">conversões implícitas são permitidas.</span><span class="sxs-lookup"><span data-stu-id="e1504-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="e1504-192">Isso é, na verdade, um caso especial para passar um RValue</span><span class="sxs-lookup"><span data-stu-id="e1504-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="e1504-193">Uma referência a um valor temporário em retenção é transmitida nesse caso.</span><span class="sxs-lookup"><span data-stu-id="e1504-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="e1504-194">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="e1504-195">em um caso de um receptor de um método de extensão `in` (em oposição aos métodos de extensão `ref`), os RValues ou as _conversões implícitas desse argumento_ são permitidas.</span><span class="sxs-lookup"><span data-stu-id="e1504-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="e1504-196">Uma referência a um valor temporário em retenção é transmitida nesse caso.</span><span class="sxs-lookup"><span data-stu-id="e1504-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="e1504-197">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="e1504-198">Mais informações sobre `ref`/métodos de extensão `in` são fornecidas mais adiante neste documento.</span><span class="sxs-lookup"><span data-stu-id="e1504-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="e1504-199">o despejo do argumento devido a `await` operandos pode despejar "por valor", se necessário.</span><span class="sxs-lookup"><span data-stu-id="e1504-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="e1504-200">Em cenários em que não é possível fornecer uma referência direta ao argumento devido ao intervir `await` uma cópia do valor do argumento é despejada.</span><span class="sxs-lookup"><span data-stu-id="e1504-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="e1504-201">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="e1504-202">Como o resultado de uma invocação de efeito colateral é uma referência que não pode ser preservada em `await` suspensão, um temporário contendo o valor real será preservado (como faria em um caso de parâmetro ByVal comum).</span><span class="sxs-lookup"><span data-stu-id="e1504-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="e1504-203">Argumentos opcionais omitidos</span><span class="sxs-lookup"><span data-stu-id="e1504-203">Omitted optional arguments</span></span>

<span data-ttu-id="e1504-204">É permitido que um parâmetro `in` especifique um valor padrão.</span><span class="sxs-lookup"><span data-stu-id="e1504-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="e1504-205">Isso torna o argumento correspondente opcional.</span><span class="sxs-lookup"><span data-stu-id="e1504-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="e1504-206">Omitir o argumento opcional no site de chamada resulta na passagem do valor padrão por meio de um temporário.</span><span class="sxs-lookup"><span data-stu-id="e1504-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="e1504-207">Comportamento de alias em geral</span><span class="sxs-lookup"><span data-stu-id="e1504-207">Aliasing behavior in general</span></span>

<span data-ttu-id="e1504-208">Assim como as variáveis `ref` e `out`, `in` variáveis são referências/aliases para os locais existentes.</span><span class="sxs-lookup"><span data-stu-id="e1504-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="e1504-209">Embora o receptor não tenha permissão para gravar neles, a leitura de um parâmetro `in` pode observar valores diferentes como um efeito colateral de outras avaliações.</span><span class="sxs-lookup"><span data-stu-id="e1504-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="e1504-210">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="e1504-211">`in` parâmetros e a captura de variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="e1504-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="e1504-212">Para a finalidade da captura de lambda/Async `in` parâmetros se comportam da mesma forma que os parâmetros `out` e `ref`.</span><span class="sxs-lookup"><span data-stu-id="e1504-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="e1504-213">`in` parâmetros não podem ser capturados em um fechamento</span><span class="sxs-lookup"><span data-stu-id="e1504-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="e1504-214">parâmetros de `in` não são permitidos em métodos iteradores</span><span class="sxs-lookup"><span data-stu-id="e1504-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="e1504-215">parâmetros de `in` não são permitidos em métodos assíncronos</span><span class="sxs-lookup"><span data-stu-id="e1504-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="e1504-216">Variáveis temporárias.</span><span class="sxs-lookup"><span data-stu-id="e1504-216">Temporary variables.</span></span>  
<span data-ttu-id="e1504-217">Alguns usos de `in` passagem de parâmetro podem exigir uso indireto de uma variável local temporária:</span><span class="sxs-lookup"><span data-stu-id="e1504-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="e1504-218">`in` argumentos são sempre passados como aliases diretos quando Call-site usa `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="e1504-219">O temporário nunca é usado nesse caso.</span><span class="sxs-lookup"><span data-stu-id="e1504-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="e1504-220">os argumentos de `in` não precisam ser aliases diretos quando o site de chamada não usa `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="e1504-221">Quando o argumento não é um LValue, um temporário pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="e1504-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="e1504-222">`in` parâmetro pode ter um valor padrão.</span><span class="sxs-lookup"><span data-stu-id="e1504-222">`in` parameter may have default value.</span></span> <span data-ttu-id="e1504-223">Quando o argumento correspondente é omitido no site de chamada, o valor padrão é passado por meio de um temporário.</span><span class="sxs-lookup"><span data-stu-id="e1504-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="e1504-224">`in` argumentos podem ter conversões implícitas, incluindo aquelas que não preservam a identidade.</span><span class="sxs-lookup"><span data-stu-id="e1504-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="e1504-225">Um temporário é usado nesses casos.</span><span class="sxs-lookup"><span data-stu-id="e1504-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="e1504-226">receptores de chamadas struct comuns não podem ser LValue graváveis (**caso existente!** ).</span><span class="sxs-lookup"><span data-stu-id="e1504-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="e1504-227">Um temporário é usado nesses casos.</span><span class="sxs-lookup"><span data-stu-id="e1504-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="e1504-228">O tempo de vida do argumento temporaries corresponde ao escopo de abrangência mais próximo do site de chamada.</span><span class="sxs-lookup"><span data-stu-id="e1504-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="e1504-229">O tempo de vida formal das variáveis temporárias é semanticamente significativo em cenários que envolvem a análise de escape de variáveis retornadas por referência.</span><span class="sxs-lookup"><span data-stu-id="e1504-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="e1504-230">Representação de metadados de parâmetros de `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="e1504-231">Quando `System.Runtime.CompilerServices.IsReadOnlyAttribute` é aplicado a um parâmetro ByRef, isso significa que o parâmetro é um parâmetro `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="e1504-232">Além disso, se o método for *abstract* ou *virtual*, a assinatura desses parâmetros (e apenas esses parâmetros) deverá ter `modreq[System.Runtime.InteropServices.InAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="e1504-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="e1504-233">**Motivação**: isso é feito para garantir que, em um caso do método que substitui/implemente os parâmetros de `in` coincidam.</span><span class="sxs-lookup"><span data-stu-id="e1504-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="e1504-234">Os mesmos requisitos se aplicam aos métodos `Invoke` em delegados.</span><span class="sxs-lookup"><span data-stu-id="e1504-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="e1504-235">**Motivação**: isso é para garantir que os compiladores existentes não possam simplesmente ignorar `readonly` ao criar ou atribuir delegados.</span><span class="sxs-lookup"><span data-stu-id="e1504-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="e1504-236">Retornando por referência ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e1504-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="e1504-237">Motivação</span><span class="sxs-lookup"><span data-stu-id="e1504-237">Motivation</span></span>
<span data-ttu-id="e1504-238">A motivação para esse subrecurso é aproximadamente simétrica para os motivos para os parâmetros de `in` – evitando a cópia, mas no lado de retorno.</span><span class="sxs-lookup"><span data-stu-id="e1504-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="e1504-239">Antes desse recurso, um método ou um indexador tinha duas opções: 1) retornar por referência e ser exposto a possíveis mutações ou 2) retornar por valor, o que resulta em cópia.</span><span class="sxs-lookup"><span data-stu-id="e1504-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="e1504-240">Solução (`ref readonly` retorna)</span><span class="sxs-lookup"><span data-stu-id="e1504-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="e1504-241">O recurso permite que um membro retorne variáveis por referência sem expô-las a mutações.</span><span class="sxs-lookup"><span data-stu-id="e1504-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="e1504-242">Declarando `ref readonly` retornando Membros</span><span class="sxs-lookup"><span data-stu-id="e1504-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="e1504-243">Uma combinação de modificadores `ref readonly` na assinatura de retorno é usada para indicar que o membro retorna uma referência somente leitura.</span><span class="sxs-lookup"><span data-stu-id="e1504-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="e1504-244">Para todas as finalidades, um membro de `ref readonly` é tratado como uma variável de `readonly` semelhante a campos de `readonly` e `in` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e1504-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="e1504-245">Por exemplo, os campos de `ref readonly` membro que tem um tipo de struct são todos classificados recursivamente como variáveis de `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="e1504-246">-É permitido passá-los como `in` argumentos, mas não como `ref` ou `out` argumentos.</span><span class="sxs-lookup"><span data-stu-id="e1504-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="e1504-247">`ref readonly` retornos são permitidos nos mesmos locais `ref` retornos são permitidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="e1504-248">Isso inclui indexadores, delegados, lambdas, funções locais.</span><span class="sxs-lookup"><span data-stu-id="e1504-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="e1504-249">Não é permitido sobrecarregar `ref`/`ref readonly`/diferenças.</span><span class="sxs-lookup"><span data-stu-id="e1504-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="e1504-250">É permitido sobrecarregar em diferenças comuns de retorno de ByVal e `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="e1504-251">Com a finalidade de OHI (sobrecarregar, ocultar, implementar), `ref readonly` é semelhante, mas diferente de `ref`.</span><span class="sxs-lookup"><span data-stu-id="e1504-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="e1504-252">Por exemplo, o método que substitui `ref readonly` um, deve ser `ref readonly`do e ter tipo de identidade conversível.</span><span class="sxs-lookup"><span data-stu-id="e1504-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="e1504-253">Para a finalidade das conversões de grupo delegate/lambda/Method, `ref readonly` é semelhante, mas diferente de `ref`.</span><span class="sxs-lookup"><span data-stu-id="e1504-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="e1504-254">As lambdas e os candidatos à conversão do grupo de métodos aplicáveis precisam corresponder `ref readonly` retorno do delegado de destino com `ref readonly` retorno do tipo que tem a identidade conversível.</span><span class="sxs-lookup"><span data-stu-id="e1504-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="e1504-255">Para fins de variância genérica, `ref readonly` retornos são nonvariant.</span><span class="sxs-lookup"><span data-stu-id="e1504-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="e1504-256">Observação: não há avisos sobre `ref readonly` retornos que têm tipos de referência ou primitivos.</span><span class="sxs-lookup"><span data-stu-id="e1504-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="e1504-257">Pode ser inútil em geral, mas, em alguns casos, o usuário deve/deseja passar primitivos como `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="e1504-258">Exemplos – substituindo um método genérico como `ref readonly T Method()` quando `T` foi substituído para ser `int`.</span><span class="sxs-lookup"><span data-stu-id="e1504-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="e1504-259">É concebível ter um analisador que avisa em casos de uso ineficiente de `ref readonly` Devoluções, mas as regras para essa análise seriam muito difusas para fazer parte de uma especificação de linguagem.</span><span class="sxs-lookup"><span data-stu-id="e1504-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="e1504-260">Retornando de membros de `ref readonly`</span><span class="sxs-lookup"><span data-stu-id="e1504-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="e1504-261">Dentro do corpo do método, a sintaxe é a mesma que com retornos de referência regulares.</span><span class="sxs-lookup"><span data-stu-id="e1504-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="e1504-262">O `readonly` será inferido do método que o contém.</span><span class="sxs-lookup"><span data-stu-id="e1504-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="e1504-263">A motivação é que `return ref readonly <expression>` é desnecessária e só permite incompatibilidades na parte `readonly` que sempre resultaria em erros.</span><span class="sxs-lookup"><span data-stu-id="e1504-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="e1504-264">No entanto, o `ref` é necessário para a consistência com outros cenários em que algo é passado por meio de alias estrito vs. por valor.</span><span class="sxs-lookup"><span data-stu-id="e1504-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="e1504-265">Ao contrário do caso com `in` parâmetros, `ref readonly` retorna nunca retornar por meio de uma cópia local.</span><span class="sxs-lookup"><span data-stu-id="e1504-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="e1504-266">Considerar que a cópia deixaria de existir imediatamente ao retornar tal prática seria inútil e perigosa.</span><span class="sxs-lookup"><span data-stu-id="e1504-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="e1504-267">Portanto `ref readonly` retorna são sempre referências diretas.</span><span class="sxs-lookup"><span data-stu-id="e1504-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="e1504-268">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="e1504-269">Um argumento de `return ref` deve ser um LValue (**regra existente**)</span><span class="sxs-lookup"><span data-stu-id="e1504-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="e1504-270">Um argumento de `return ref` deve ser "Safe para retornar" (**regra existente**)</span><span class="sxs-lookup"><span data-stu-id="e1504-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="e1504-271">Em um membro `ref readonly`, um argumento de `return ref` _não precisa ser gravável_ .</span><span class="sxs-lookup"><span data-stu-id="e1504-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="e1504-272">Por exemplo, esse membro pode ser ref-retornar um campo ReadOnly ou um de seus parâmetros `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="e1504-273">Seguro para retornar regras.</span><span class="sxs-lookup"><span data-stu-id="e1504-273">Safe to Return rules.</span></span>
<span data-ttu-id="e1504-274">A segurança normal para retornar regras para referências também será aplicada a referências ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e1504-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="e1504-275">Observe que um `ref readonly` pode ser obtido de um `ref` local/parâmetro/retorno regular, mas não o contrário.</span><span class="sxs-lookup"><span data-stu-id="e1504-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="e1504-276">Caso contrário, a segurança de retornos de `ref readonly` será inferida da mesma maneira que para retornos de `ref` regulares.</span><span class="sxs-lookup"><span data-stu-id="e1504-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="e1504-277">Considerando que os RValues podem ser passados como `in` parâmetro e retornados como `ref readonly` precisamos de mais uma regra- **rvalues não são seguros para retornar por referência**.</span><span class="sxs-lookup"><span data-stu-id="e1504-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="e1504-278">Considere a situação em que um RValue é passado para um parâmetro `in` por meio de uma cópia e retornada de volta em uma forma de um `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="e1504-279">No contexto do chamador, o resultado dessa invocação é uma referência a dados locais e, como tal, não é seguro retornar.</span><span class="sxs-lookup"><span data-stu-id="e1504-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="e1504-280">Depois que RValues não são seguros de retornar, a regra existente `#6` já lida com esse caso.</span><span class="sxs-lookup"><span data-stu-id="e1504-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="e1504-281">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e1504-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="e1504-282">Regras de `safe to return` atualizadas:</span><span class="sxs-lookup"><span data-stu-id="e1504-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="e1504-283">**refs para variáveis no heap é seguro para retornar**</span><span class="sxs-lookup"><span data-stu-id="e1504-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="e1504-284">os **parâmetros REF/in são seguros para retornar**
parâmetros de `in` naturalmente só podem ser retornados como ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e1504-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="e1504-285">os **parâmetros de saída são seguros para retornar** (mas devem ser definitivamente atribuídos, como já é o caso hoje)</span><span class="sxs-lookup"><span data-stu-id="e1504-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="e1504-286">**os campos de struct da instância são seguros para retornar, contanto que o receptor seja seguro para retornar**</span><span class="sxs-lookup"><span data-stu-id="e1504-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="e1504-287">**' this ' não é seguro para retornar de membros de struct**</span><span class="sxs-lookup"><span data-stu-id="e1504-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="e1504-288">**uma ref, retornada de outro método, é segura para retornar se todos os refs/esgotamentos passados para esse método como parâmetros formais eram seguros para retornar.** 
*especificamente é irrelevante se o destinatário é seguro de retornar, independentemente de o destinatário ser uma struct, uma classe ou um parâmetro de tipo genérico.*</span><span class="sxs-lookup"><span data-stu-id="e1504-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="e1504-289">Os **rvalues não são seguros para retornar por referência.** 
 *, especificamente, os rvalues são seguros como em parâmetros.*</span><span class="sxs-lookup"><span data-stu-id="e1504-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="e1504-290">Observação: há regras adicionais sobre a segurança de retornos que entram em jogo quando tipos de referência e reatribuições de referência estão envolvidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="e1504-291">As regras se aplicam igualmente aos membros `ref` e `ref readonly` e, portanto, não são mencionadas aqui.</span><span class="sxs-lookup"><span data-stu-id="e1504-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="e1504-292">Comportamento de alias.</span><span class="sxs-lookup"><span data-stu-id="e1504-292">Aliasing behavior.</span></span>
<span data-ttu-id="e1504-293">`ref readonly` Membros fornecem o mesmo comportamento de alias que os membros comuns de `ref` (exceto para serem ReadOnly).</span><span class="sxs-lookup"><span data-stu-id="e1504-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="e1504-294">Portanto, para a finalidade de capturar em lambdas, Async, iteradores, despejo de pilha, etc... as mesmas restrições se aplicam.</span><span class="sxs-lookup"><span data-stu-id="e1504-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="e1504-295">,.</span><span class="sxs-lookup"><span data-stu-id="e1504-295">- I.E.</span></span> <span data-ttu-id="e1504-296">devido à incapacidade de capturar as referências reais e devido à natureza do efeito colateral da avaliação do membro, esses cenários não são permitidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="e1504-297">É permitido e necessário fazer uma cópia quando `ref readonly` retorno é um destinatário de métodos struct regulares, que usam `this` como uma referência gravável comum.</span><span class="sxs-lookup"><span data-stu-id="e1504-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="e1504-298">Historicamente, em todos os casos em que essas invocações são aplicadas à variável ReadOnly, é feita uma cópia local.</span><span class="sxs-lookup"><span data-stu-id="e1504-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="e1504-299">Representação de metadados.</span><span class="sxs-lookup"><span data-stu-id="e1504-299">Metadata representation.</span></span>
<span data-ttu-id="e1504-300">Quando `System.Runtime.CompilerServices.IsReadOnlyAttribute` é aplicado ao retorno de um método de retorno ByRef, isso significa que o método retorna uma referência ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e1504-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="e1504-301">Além disso, a assinatura de resultado de tais métodos (e apenas esses métodos) deve ter `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="e1504-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="e1504-302">**Motivação**: isso é para garantir que os compiladores existentes não possam simplesmente ignorar `readonly` ao invocar métodos com `ref readonly` retorna</span><span class="sxs-lookup"><span data-stu-id="e1504-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="e1504-303">Structs ReadOnly</span><span class="sxs-lookup"><span data-stu-id="e1504-303">Readonly structs</span></span>
<span data-ttu-id="e1504-304">Em um recurso curto que faz `this` parâmetro de todos os membros de instância de um struct, exceto para construtores, um parâmetro `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="e1504-305">Motivação</span><span class="sxs-lookup"><span data-stu-id="e1504-305">Motivation</span></span>
<span data-ttu-id="e1504-306">O compilador deve assumir que qualquer chamada de método em uma instância de struct pode modificar a instância.</span><span class="sxs-lookup"><span data-stu-id="e1504-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="e1504-307">Na verdade, uma referência gravável é passada para o método como `this` parâmetro e habilita totalmente esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="e1504-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="e1504-308">Para permitir essas invocações em variáveis de `readonly`, as invocações são aplicadas a cópias temporárias.</span><span class="sxs-lookup"><span data-stu-id="e1504-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="e1504-309">Isso pode ser não intuitivo e, às vezes, força as pessoas a abandonar `readonly` por motivos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="e1504-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="e1504-310">Exemplo: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="e1504-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="e1504-311">Depois de adicionar suporte para `in` parâmetros e `ref readonly` retorna o problema de cópia defensiva será pior, pois as variáveis ReadOnly se tornarão mais comuns.</span><span class="sxs-lookup"><span data-stu-id="e1504-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="e1504-312">Solução</span><span class="sxs-lookup"><span data-stu-id="e1504-312">Solution</span></span>
<span data-ttu-id="e1504-313">Permitir `readonly` modificador em declarações de struct que resultaria na `this` tratada como parâmetro `in` em todos os métodos de instância de struct, exceto para construtores.</span><span class="sxs-lookup"><span data-stu-id="e1504-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="e1504-314">Restrições em membros de struct ReadOnly</span><span class="sxs-lookup"><span data-stu-id="e1504-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="e1504-315">Os campos de instância de uma struct ReadOnly devem ser ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e1504-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="e1504-316">**Motivação:** só pode ser gravada externamente, mas não por meio de membros.</span><span class="sxs-lookup"><span data-stu-id="e1504-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="e1504-317">As propriedades autopropriedade de uma struct ReadOnly devem ser somente obtenção.</span><span class="sxs-lookup"><span data-stu-id="e1504-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="e1504-318">**Motivação:** conseqüência de restrição em campos de instância.</span><span class="sxs-lookup"><span data-stu-id="e1504-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="e1504-319">Struct ReadOnly não pode declarar eventos do tipo campo.</span><span class="sxs-lookup"><span data-stu-id="e1504-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="e1504-320">**Motivação:** conseqüência de restrição em campos de instância.</span><span class="sxs-lookup"><span data-stu-id="e1504-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="e1504-321">Representação de metadados.</span><span class="sxs-lookup"><span data-stu-id="e1504-321">Metadata representation.</span></span>
<span data-ttu-id="e1504-322">Quando `System.Runtime.CompilerServices.IsReadOnlyAttribute` é aplicado a um tipo de valor, isso significa que o tipo é um `readonly struct`.</span><span class="sxs-lookup"><span data-stu-id="e1504-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="e1504-323">Em particular:</span><span class="sxs-lookup"><span data-stu-id="e1504-323">In particular:</span></span>
-  <span data-ttu-id="e1504-324">A identidade do tipo de `IsReadOnlyAttribute` não é importante.</span><span class="sxs-lookup"><span data-stu-id="e1504-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="e1504-325">Na verdade, ele pode ser inserido pelo compilador no assembly que o contém, se necessário.</span><span class="sxs-lookup"><span data-stu-id="e1504-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="e1504-326">`ref`/métodos de extensão `in`</span><span class="sxs-lookup"><span data-stu-id="e1504-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="e1504-327">Na verdade, há uma proposta existente (https://github.com/dotnet/roslyn/issues/165) e a PR (https://github.com/dotnet/roslyn/pull/15650)de protótipo correspondente.</span><span class="sxs-lookup"><span data-stu-id="e1504-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="e1504-328">Quero apenas reconhecer que essa ideia não é totalmente nova.</span><span class="sxs-lookup"><span data-stu-id="e1504-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="e1504-329">No entanto, é relevante aqui, já que `ref readonly` remove elegantemente o problema mais contenciosos sobre tais métodos, o que fazer com os receptores de RValue.</span><span class="sxs-lookup"><span data-stu-id="e1504-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="e1504-330">A ideia geral é permitir que os métodos de extensão adotem o parâmetro `this` por referência, desde que o tipo seja conhecido como um tipo struct.</span><span class="sxs-lookup"><span data-stu-id="e1504-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="e1504-331">Os motivos para escrever esses métodos de extensão são principalmente:</span><span class="sxs-lookup"><span data-stu-id="e1504-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="e1504-332">Evite copiar quando o destinatário é um struct grande</span><span class="sxs-lookup"><span data-stu-id="e1504-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="e1504-333">Permitir a mutação de métodos de extensão em structs</span><span class="sxs-lookup"><span data-stu-id="e1504-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="e1504-334">Os motivos pelos quais não queremos permitir isso em classes</span><span class="sxs-lookup"><span data-stu-id="e1504-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="e1504-335">Seria uma finalidade muito limitada.</span><span class="sxs-lookup"><span data-stu-id="e1504-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="e1504-336">Ela iria parar muito em constante distância que uma chamada de método não pode transformar o receptor não`null` para se tornar `null` após a invocação.</span><span class="sxs-lookup"><span data-stu-id="e1504-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="e1504-337">Na verdade, atualmente uma variável não`null` não pode se tornar `null`, a menos que seja _explicitamente_ atribuída ou passada por `ref` ou `out`.</span><span class="sxs-lookup"><span data-stu-id="e1504-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="e1504-338">Isso ajuda muito a legibilidade ou outras formas da análise "pode ser uma nula aqui".</span><span class="sxs-lookup"><span data-stu-id="e1504-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="e1504-339">Seria difícil reconciliar com a semântica "avaliar uma vez" de acessos condicionais nulos.</span><span class="sxs-lookup"><span data-stu-id="e1504-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="e1504-340">Exemplo: `obj.stringField?.RefExtension(...)`-é necessário capturar uma cópia de `stringField` para tornar a verificação nula significativa, mas as atribuições para `this` dentro de RefExtension não seriam refletidas de volta para o campo.</span><span class="sxs-lookup"><span data-stu-id="e1504-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="e1504-341">Uma capacidade de declarar métodos de extensão em **structs** que usam o primeiro argumento por referência era uma solicitação de longa duração.</span><span class="sxs-lookup"><span data-stu-id="e1504-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="e1504-342">Uma das considerações de bloqueio foi "o que acontece se o destinatário não for um LValue?".</span><span class="sxs-lookup"><span data-stu-id="e1504-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="e1504-343">Há um precedente de que qualquer método de extensão também poderia ser chamado como um método estático (às vezes, é a única maneira de resolver a ambiguidade).</span><span class="sxs-lookup"><span data-stu-id="e1504-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="e1504-344">Isso ditaria que os receptores de RValue não devem ser permitidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="e1504-345">Por outro lado, há uma prática de fazer a invocação em uma cópia em situações semelhantes quando os métodos de instância de struct estão envolvidos.</span><span class="sxs-lookup"><span data-stu-id="e1504-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="e1504-346">O motivo pelo qual a "cópia implícita" existe é porque a maioria dos métodos struct não modifica realmente a estrutura, embora não seja capaz de indicar isso.</span><span class="sxs-lookup"><span data-stu-id="e1504-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="e1504-347">Portanto, a solução mais prática era simplesmente fazer a invocação em uma cópia, mas essa prática é conhecida por prejudicar o desempenho e causar bugs.</span><span class="sxs-lookup"><span data-stu-id="e1504-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="e1504-348">Agora, com a disponibilidade de parâmetros de `in`, é possível que uma extensão sinalize a intenção.</span><span class="sxs-lookup"><span data-stu-id="e1504-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="e1504-349">Portanto, o enigma pode ser resolvido exigindo-se que as extensões de `ref` sejam chamadas com receptores graváveis, enquanto as extensões de `in` permitem a cópia implícita, se necessário.</span><span class="sxs-lookup"><span data-stu-id="e1504-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="e1504-350">extensões de `in` e genéricos.</span><span class="sxs-lookup"><span data-stu-id="e1504-350">`in` extensions and generics.</span></span>
<span data-ttu-id="e1504-351">A finalidade dos métodos de extensão de `ref` é mutar o receptor diretamente ou invocar os membros mutantes.</span><span class="sxs-lookup"><span data-stu-id="e1504-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="e1504-352">Portanto, `ref this T` extensões são permitidas desde que `T` seja restrito a ser uma struct.</span><span class="sxs-lookup"><span data-stu-id="e1504-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="e1504-353">Por outro lado `in` métodos de extensão existem especificamente para reduzir a cópia implícita.</span><span class="sxs-lookup"><span data-stu-id="e1504-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="e1504-354">No entanto, qualquer uso de um parâmetro de `in T` precisará ser feito por meio de um membro de interface.</span><span class="sxs-lookup"><span data-stu-id="e1504-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="e1504-355">Como todos os membros da interface são considerados mutação, qualquer uso exigiria uma cópia.</span><span class="sxs-lookup"><span data-stu-id="e1504-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="e1504-356">-Em vez de reduzir a cópia, o efeito seria o oposto.</span><span class="sxs-lookup"><span data-stu-id="e1504-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="e1504-357">Portanto `in this T` não é permitido quando `T` é um parâmetro de tipo genérico, independentemente das restrições.</span><span class="sxs-lookup"><span data-stu-id="e1504-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="e1504-358">Tipos válidos de métodos de extensão (recapitulação):</span><span class="sxs-lookup"><span data-stu-id="e1504-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="e1504-359">Agora, são permitidas as seguintes formas de declaração de `this` em um método de extensão:</span><span class="sxs-lookup"><span data-stu-id="e1504-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="e1504-360">`this T arg`-extensão de ByVal regular.</span><span class="sxs-lookup"><span data-stu-id="e1504-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="e1504-361">(**caso existente**)</span><span class="sxs-lookup"><span data-stu-id="e1504-361">(**existing case**)</span></span>
- <span data-ttu-id="e1504-362">T pode ser qualquer tipo, incluindo tipos de referência ou parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="e1504-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="e1504-363">A instância será a mesma variável após a chamada.</span><span class="sxs-lookup"><span data-stu-id="e1504-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="e1504-364">Permite conversões implícitas desse tipo de _conversão de argumento_ .</span><span class="sxs-lookup"><span data-stu-id="e1504-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="e1504-365">Pode ser chamado em RValues.</span><span class="sxs-lookup"><span data-stu-id="e1504-365">Can be called on RValues.</span></span>

- <span data-ttu-id="e1504-366"> - `in this T self``in` extensão.</span><span class="sxs-lookup"><span data-stu-id="e1504-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="e1504-367">T deve ser um tipo struct real.</span><span class="sxs-lookup"><span data-stu-id="e1504-367">T must be an actual struct type.</span></span>
<span data-ttu-id="e1504-368">A instância será a mesma variável após a chamada.</span><span class="sxs-lookup"><span data-stu-id="e1504-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="e1504-369">Permite conversões implícitas desse tipo de _conversão de argumento_ .</span><span class="sxs-lookup"><span data-stu-id="e1504-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="e1504-370">Pode ser chamado em RValues (pode ser invocado em um temp, se necessário).</span><span class="sxs-lookup"><span data-stu-id="e1504-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="e1504-371"> - `ref this T self``ref` extensão.</span><span class="sxs-lookup"><span data-stu-id="e1504-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="e1504-372">T deve ser um tipo struct ou um parâmetro de tipo genérico restrito a ser um struct.</span><span class="sxs-lookup"><span data-stu-id="e1504-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="e1504-373">A instância pode ser gravada pela invocação.</span><span class="sxs-lookup"><span data-stu-id="e1504-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="e1504-374">Permite apenas conversões de identidade.</span><span class="sxs-lookup"><span data-stu-id="e1504-374">Allows only identity conversions.</span></span>
<span data-ttu-id="e1504-375">Deve ser chamado em LValue gravável.</span><span class="sxs-lookup"><span data-stu-id="e1504-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="e1504-376">(nunca é invocado por meio de uma Temp).</span><span class="sxs-lookup"><span data-stu-id="e1504-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="e1504-377">Locais de referência ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="e1504-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="e1504-378">Motivação.</span><span class="sxs-lookup"><span data-stu-id="e1504-378">Motivation.</span></span>
<span data-ttu-id="e1504-379">Uma vez que `ref readonly` Membros foram introduzidos, ele estava claro do uso de que precisam ser emparelhados com o tipo apropriado de local.</span><span class="sxs-lookup"><span data-stu-id="e1504-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="e1504-380">A avaliação de um membro pode produzir ou observar efeitos colaterais, portanto, se o resultado precisar ser usado mais de uma vez, ele precisará ser armazenado.</span><span class="sxs-lookup"><span data-stu-id="e1504-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="e1504-381">Os locais `ref` comuns não ajudam aqui, pois não é possível atribuir uma referência `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="e1504-382">Soluções.</span><span class="sxs-lookup"><span data-stu-id="e1504-382">Solution.</span></span>
<span data-ttu-id="e1504-383">Permitir a declaração de locais de `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="e1504-384">Esse é um novo tipo de `ref` locais que não são graváveis.</span><span class="sxs-lookup"><span data-stu-id="e1504-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="e1504-385">Como resultado `ref readonly` locais podem aceitar referências a variáveis ReadOnly sem expor essas variáveis a gravações.</span><span class="sxs-lookup"><span data-stu-id="e1504-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="e1504-386">Declarando e usando `ref readonly` locais.</span><span class="sxs-lookup"><span data-stu-id="e1504-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="e1504-387">A sintaxe de tais locais usa `ref readonly` modificadores no site da declaração (nessa ordem específica).</span><span class="sxs-lookup"><span data-stu-id="e1504-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="e1504-388">Da mesma forma que os locais `ref` comuns, `ref readonly` locais devem ser inicializados como REF na declaração.</span><span class="sxs-lookup"><span data-stu-id="e1504-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="e1504-389">Ao contrário de locais de `ref` regulares, `ref readonly` locais podem se referir a `readonly` LValues como parâmetros `in`, campos de `readonly`, métodos de `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="e1504-390">Para todas as finalidades, uma `ref readonly` local é tratada como uma variável `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="e1504-391">A maioria das restrições sobre o uso é a mesma de `readonly` campos ou parâmetros de `in`.</span><span class="sxs-lookup"><span data-stu-id="e1504-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="e1504-392">Por exemplo, os campos de um parâmetro de `in` que tem um tipo struct são todos classificados recursivamente como variáveis `readonly`.</span><span class="sxs-lookup"><span data-stu-id="e1504-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="e1504-393">Restrições no uso de locais de `ref readonly`</span><span class="sxs-lookup"><span data-stu-id="e1504-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="e1504-394">Exceto por sua natureza `readonly`, `ref readonly` locais se comportam como locais de `ref` comuns e estão sujeitos a exatamente as mesmas restrições.</span><span class="sxs-lookup"><span data-stu-id="e1504-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="e1504-395">Por exemplo, as restrições relacionadas à captura em fechamentos, declarando em `async` métodos ou na análise de `safe-to-return` se aplicam igualmente a `ref readonly` locais.</span><span class="sxs-lookup"><span data-stu-id="e1504-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="e1504-396">Expressões ternários `ref`.</span><span class="sxs-lookup"><span data-stu-id="e1504-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="e1504-397">(também conhecido como "LValues condicionais")</span><span class="sxs-lookup"><span data-stu-id="e1504-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="e1504-398">Motivação</span><span class="sxs-lookup"><span data-stu-id="e1504-398">Motivation</span></span>
<span data-ttu-id="e1504-399">O uso de `ref` e `ref readonly` locais expôs a necessidade de uma inicialização de referência desses locais com uma ou outra variável de destino com base em uma condição.</span><span class="sxs-lookup"><span data-stu-id="e1504-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="e1504-400">Uma solução alternativa típica é introduzir um método como:</span><span class="sxs-lookup"><span data-stu-id="e1504-400">A typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="e1504-401">Observe que `Choice` não é uma substituição exata de um ternário, já que _todos os_ argumentos devem ser avaliados no site de chamada, o que estava levando a um comportamento e a bugs não intuitivos.</span><span class="sxs-lookup"><span data-stu-id="e1504-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="e1504-402">O seguinte não funcionará conforme o esperado:</span><span class="sxs-lookup"><span data-stu-id="e1504-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="e1504-403">Solução</span><span class="sxs-lookup"><span data-stu-id="e1504-403">Solution</span></span>
<span data-ttu-id="e1504-404">Permitir tipo especial de expressão condicional que é avaliada como uma referência a um dos argumentos LValue com base em uma condição.</span><span class="sxs-lookup"><span data-stu-id="e1504-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="e1504-405">Usando `ref` expressão Ternário.</span><span class="sxs-lookup"><span data-stu-id="e1504-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="e1504-406">A sintaxe para o `ref` tipo de uma expressão condicional é `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="e1504-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="e1504-407">Assim como com a expressão condicional comum somente `<consequence>` ou `<alternative>` é avaliada dependendo do resultado da expressão de condição booliana.</span><span class="sxs-lookup"><span data-stu-id="e1504-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="e1504-408">Diferentemente da expressão condicional comum, `ref` expressão condicional:</span><span class="sxs-lookup"><span data-stu-id="e1504-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="e1504-409">requer que `<consequence>` e `<alternative>` sejam LValues.</span><span class="sxs-lookup"><span data-stu-id="e1504-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="e1504-410">`ref` expressão condicional em si é um LValue e</span><span class="sxs-lookup"><span data-stu-id="e1504-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="e1504-411">`ref` expressão condicional é gravável se `<consequence>` e `<alternative>` são LValue graváveis</span><span class="sxs-lookup"><span data-stu-id="e1504-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="e1504-412">Exemplos:</span><span class="sxs-lookup"><span data-stu-id="e1504-412">Examples:</span></span>  
<span data-ttu-id="e1504-413">`ref` ternário é um LValue e, como tal, pode ser passado/atribuído/retornado por referência;</span><span class="sxs-lookup"><span data-stu-id="e1504-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="e1504-414">Sendo um LValue, ele também pode ser atribuído a.</span><span class="sxs-lookup"><span data-stu-id="e1504-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="e1504-415">Pode ser usado como um receptor de uma chamada de método e ignorar a cópia, se necessário.</span><span class="sxs-lookup"><span data-stu-id="e1504-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="e1504-416">`ref` ternário também pode ser usado em um contexto regular (não ref).</span><span class="sxs-lookup"><span data-stu-id="e1504-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="e1504-417">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="e1504-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e1504-418">Posso ver dois argumentos principais em relação ao suporte aprimorado para referências e referências ReadOnly:</span><span class="sxs-lookup"><span data-stu-id="e1504-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="e1504-419">Os problemas que são resolvidos aqui são muito antigos.</span><span class="sxs-lookup"><span data-stu-id="e1504-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="e1504-420">Por que repentinamente solucioná-los agora, especialmente porque não ajudariam a usar o código existente?</span><span class="sxs-lookup"><span data-stu-id="e1504-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="e1504-421">Como encontramos C# e .net usados em novos domínios, alguns problemas se tornam mais proeminentes.</span><span class="sxs-lookup"><span data-stu-id="e1504-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="e1504-422">Como exemplos de ambientes que são mais críticos do que a média de sobrecargas de computação, posso listar</span><span class="sxs-lookup"><span data-stu-id="e1504-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="e1504-423">cenários de nuvem/datacenter em que a computação é cobrada e a capacidade de resposta é uma vantagem competitiva.</span><span class="sxs-lookup"><span data-stu-id="e1504-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="e1504-424">Jogos/VR/AR com requisitos de tempo real em latências</span><span class="sxs-lookup"><span data-stu-id="e1504-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="e1504-425">Esse recurso não sacrifica nenhum dos pontos fortes existentes, como a segurança de tipo, ao mesmo tempo que permite reduzir sobrecargas em alguns cenários comuns.</span><span class="sxs-lookup"><span data-stu-id="e1504-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="e1504-426">Podemos, razoavelmente, garantir que o receptor seja tocado pelas regras quando ele se deparar com contratos de `readonly`?</span><span class="sxs-lookup"><span data-stu-id="e1504-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="e1504-427">Temos confiança semelhante ao usar `out`.</span><span class="sxs-lookup"><span data-stu-id="e1504-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="e1504-428">A implementação incorreta de `out` pode causar comportamento não especificado, mas, na realidade, raramente acontece.</span><span class="sxs-lookup"><span data-stu-id="e1504-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="e1504-429">Fazer as regras de verificação formal familiarizadas com o `ref readonly` atenuaria ainda mais o problema de confiança.</span><span class="sxs-lookup"><span data-stu-id="e1504-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="e1504-430">Alternativas</span><span class="sxs-lookup"><span data-stu-id="e1504-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e1504-431">O design principal da concorrência é realmente "não fazer nada".</span><span class="sxs-lookup"><span data-stu-id="e1504-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="e1504-432">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="e1504-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="e1504-433">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="e1504-433">Design meetings</span></span>

<span data-ttu-id="e1504-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="e1504-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
