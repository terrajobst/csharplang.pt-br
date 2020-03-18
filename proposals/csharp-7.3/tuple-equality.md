---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485248"
---
# <a name="support-for--and--on-tuple-types"></a><span data-ttu-id="f3554-101">Suporte para = = e! = em tipos de tupla</span><span class="sxs-lookup"><span data-stu-id="f3554-101">Support for == and != on tuple types</span></span>

<span data-ttu-id="f3554-102">Permitir expressões `t1 == t2` em que `t1` e `t2` são tipos de tupla ou separadoras anuláveis da mesma cardinalidade e os avaliam aproximadamente como `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (supondo `var temp1 = t1; var temp2 = t2;`).</span><span class="sxs-lookup"><span data-stu-id="f3554-102">Allow expressions `t1 == t2` where `t1` and `t2` are tuple or nullable tuple types of same cardinality, and evaluate them roughly as `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (assuming `var temp1 = t1; var temp2 = t2;`).</span></span>

<span data-ttu-id="f3554-103">Por outro lado, ele permitiria `t1 != t2` e avaliá-lo como `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.</span><span class="sxs-lookup"><span data-stu-id="f3554-103">Conversely it would allow `t1 != t2` and evaluate it as `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.</span></span>

<span data-ttu-id="f3554-104">No caso anulável, são usadas verificações adicionais para `temp1.HasValue` e `temp2.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="f3554-104">In the nullable case, additional checks for `temp1.HasValue` and `temp2.HasValue` are used.</span></span> <span data-ttu-id="f3554-105">Por exemplo, `nullableT1 == nullableT2` é avaliada como `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.</span><span class="sxs-lookup"><span data-stu-id="f3554-105">For instance, `nullableT1 == nullableT2` evaluates as `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.</span></span>

<span data-ttu-id="f3554-106">Quando uma comparação por elemento retorna um resultado não bool (por exemplo, quando um `operator ==` definido pelo usuário não booliano ou `operator !=` é usado, ou em uma comparação dinâmica), esse resultado será convertido em `bool` ou executado por `operator true` ou `operator false` para obter uma `bool`.</span><span class="sxs-lookup"><span data-stu-id="f3554-106">When an element-wise comparison returns a non-bool result (for instance, when a non-bool user-defined `operator ==` or `operator !=` is used, or in a dynamic comparison), then that result will be either converted to `bool` or run through `operator true` or `operator false` to get a `bool`.</span></span> <span data-ttu-id="f3554-107">A comparação de tupla sempre acaba retornando um `bool`.</span><span class="sxs-lookup"><span data-stu-id="f3554-107">The tuple comparison always ends up returning a `bool`.</span></span>

<span data-ttu-id="f3554-108">A partir C# de 7,2, esse código produz um erro (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), a menos que haja um `operator==`definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="f3554-108">As of C# 7.2, such code produces an error (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), unless there is a user-defined `operator==`.</span></span>

## <a name="details"></a><span data-ttu-id="f3554-109">Detalhes</span><span class="sxs-lookup"><span data-stu-id="f3554-109">Details</span></span>

<span data-ttu-id="f3554-110">Ao vincular o operador `==` (ou `!=`), as regras existentes são: (1) caso dinâmico, (2) resolução de sobrecarga e (3) falham.</span><span class="sxs-lookup"><span data-stu-id="f3554-110">When binding the `==` (or `!=`) operator, the existing rules are: (1) dynamic case, (2) overload resolution, and (3) fail.</span></span>
<span data-ttu-id="f3554-111">Essa proposta adiciona um caso de tupla entre (1) e (2): se ambos os operandos de um operador de comparação são tuplas (têm tipos de tupla ou literais de tupla) e têm cardinalidade correspondente, a comparação é executada com um elemento-Wise.</span><span class="sxs-lookup"><span data-stu-id="f3554-111">This proposal adds a tuple case between (1) and (2): if both operands of a comparison operator are tuples (have tuple types or are tuple literals) and have matching cardinality, then the comparison is performed element-wise.</span></span> <span data-ttu-id="f3554-112">Essa igualdade de tupla também é levantada em tuplas anuláveis.</span><span class="sxs-lookup"><span data-stu-id="f3554-112">This tuple equality is also lifted onto nullable tuples.</span></span>

<span data-ttu-id="f3554-113">Ambos os operandos (e, no caso de literais de tupla, seus elementos) são avaliados na ordem da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="f3554-113">Both operands (and, in the case of tuple literals, their elements) are evaluated in order from left to right.</span></span> <span data-ttu-id="f3554-114">Cada par de elementos é usado como operandos para associar o operador `==` (ou `!=`), recursivamente.</span><span class="sxs-lookup"><span data-stu-id="f3554-114">Each pair of elements is then used as operands to bind the operator `==` (or `!=`), recursively.</span></span> <span data-ttu-id="f3554-115">Qualquer elemento com o tipo de tempo de compilação `dynamic` causa um erro.</span><span class="sxs-lookup"><span data-stu-id="f3554-115">Any elements with compile-time type `dynamic` cause an error.</span></span> <span data-ttu-id="f3554-116">Os resultados dessas comparações por elemento são usados como operandos em uma cadeia de operadores condicionais e (or ou).</span><span class="sxs-lookup"><span data-stu-id="f3554-116">The results of those element-wise comparisons are used as operands in a chain of conditional AND (or OR) operators.</span></span>

<span data-ttu-id="f3554-117">Por exemplo, no contexto de `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` será avaliada como `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.</span><span class="sxs-lookup"><span data-stu-id="f3554-117">For instance, in the context of `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` would evaluate as `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.</span></span>

<span data-ttu-id="f3554-118">Quando um literal de tupla é usado como operando (em qualquer lado), ele recebe um tipo de tupla convertido formado pelas conversões de elemento-Wise que são introduzidas ao ligar o elemento `==` (ou `!=`) de operador.</span><span class="sxs-lookup"><span data-stu-id="f3554-118">When a tuple literal is used as operand (on either side), it receives a converted tuple type formed by the element-wise conversions which are introduced when binding the operator `==` (or `!=`) element-wise.</span></span> 

<span data-ttu-id="f3554-119">Por exemplo, em `(1L, 2, "hello") == (1, 2L, null)`, o tipo convertido para os literais de tupla é `(long, long, string)` e o segundo literal não tem nenhum tipo natural.</span><span class="sxs-lookup"><span data-stu-id="f3554-119">For instance, in `(1L, 2, "hello") == (1, 2L, null)`, the converted type for both tuple literals is `(long, long, string)` and the second literal has no natural type.</span></span>


### <a name="deconstruction-and-conversions-to-tuple"></a><span data-ttu-id="f3554-120">Desconstrução e conversões para tupla</span><span class="sxs-lookup"><span data-stu-id="f3554-120">Deconstruction and conversions to tuple</span></span>
<span data-ttu-id="f3554-121">Em `(a, b) == x`, o fato de que `x` pode desconstruir em dois elementos não desempenha uma função.</span><span class="sxs-lookup"><span data-stu-id="f3554-121">In `(a, b) == x`, the fact that `x` can deconstruct into two elements does not play a role.</span></span> <span data-ttu-id="f3554-122">Isso poderia estar em uma futura proposta, embora isso gere dúvidas sobre `x == y` (é uma comparação simples ou uma comparação de elemento, e se estiver usando qual cardinalidade?).</span><span class="sxs-lookup"><span data-stu-id="f3554-122">That could conceivably be in a future proposal, although it would raise questions about `x == y` (is this a simple comparison or an element-wise comparison, and if so using what cardinality?).</span></span>
<span data-ttu-id="f3554-123">Da mesma forma, as conversões para a tupla não desempenham nenhuma função.</span><span class="sxs-lookup"><span data-stu-id="f3554-123">Similarly, conversions to tuple play no role.</span></span>

### <a name="tuple-element-names"></a><span data-ttu-id="f3554-124">Nomes de elementos de tupla</span><span class="sxs-lookup"><span data-stu-id="f3554-124">Tuple element names</span></span>

<span data-ttu-id="f3554-125">Ao converter um literal de tupla, avisamos quando um nome de elemento de tupla explícito era fornecido no literal, mas ele não corresponde ao nome do elemento de tupla de destino.</span><span class="sxs-lookup"><span data-stu-id="f3554-125">When converting a tuple literal, we warn when an explicit tuple element name was provided in the literal, but it doesn't match the target tuple element name.</span></span>
<span data-ttu-id="f3554-126">Usamos a mesma regra na comparação de tuplas, de modo que supondo `(int a, int b) t` avisamos sobre `d` no `t == (c, d: 0)`.</span><span class="sxs-lookup"><span data-stu-id="f3554-126">We use the same rule in tuple comparison, so that assuming `(int a, int b) t` we warn on `d` in `t == (c, d: 0)`.</span></span>

### <a name="non-bool-element-wise-comparison-results"></a><span data-ttu-id="f3554-127">Resultados de comparação de elemento não bool</span><span class="sxs-lookup"><span data-stu-id="f3554-127">Non-bool element-wise comparison results</span></span>

<span data-ttu-id="f3554-128">Se uma comparação por elemento for dinâmica em uma igualdade de tupla, usamos uma invocação dinâmica do operador `false` e a desnegarei para obter um `bool` e continuar com outras comparações com elemento.</span><span class="sxs-lookup"><span data-stu-id="f3554-128">If an element-wise comparison is dynamic in a tuple equality, we use a dynamic invocation of the operator `false` and negate that to get a `bool` and continue with further element-wise comparisons.</span></span> 

<span data-ttu-id="f3554-129">Se uma comparação por elemento retornar algum outro tipo não bool em uma igualdade de tupla, haverá dois casos:</span><span class="sxs-lookup"><span data-stu-id="f3554-129">If an element-wise comparison returns some other non-bool type in a tuple equality, there are two cases:</span></span>
- <span data-ttu-id="f3554-130">Se o tipo não bool for convertido em `bool`, aplicamos essa conversão,</span><span class="sxs-lookup"><span data-stu-id="f3554-130">if the non-bool type converts to `bool`, we apply that conversion,</span></span>
- <span data-ttu-id="f3554-131">Se não houver tal conversão, mas o tipo tiver um operador `false`, usaremos isso e negarei o resultado.</span><span class="sxs-lookup"><span data-stu-id="f3554-131">if there is no such conversion, but the type has an operator `false`, we'll use that and negate the result.</span></span>

<span data-ttu-id="f3554-132">Em uma desigualdade de tupla, as mesmas regras se aplicam, exceto pelo fato de usarmos o operador `true` (sem negação) em vez da `false`do operador.</span><span class="sxs-lookup"><span data-stu-id="f3554-132">In a tuple inequality, the same rules apply except that we'll use the operator `true` (without negation) instead of the operator `false`.</span></span>

<span data-ttu-id="f3554-133">Essas regras são semelhantes às regras envolvidas para usar um tipo não bool em uma instrução `if` e alguns outros contextos existentes.</span><span class="sxs-lookup"><span data-stu-id="f3554-133">Those rules are similar to the rules involved for using a non-bool type in an `if` statement and some other existing contexts.</span></span>

## <a name="evaluation-order-and-special-cases"></a><span data-ttu-id="f3554-134">Ordem de avaliação e casos especiais</span><span class="sxs-lookup"><span data-stu-id="f3554-134">Evaluation order and special cases</span></span>
<span data-ttu-id="f3554-135">O valor do lado esquerdo é avaliado primeiro, depois o valor do lado direito e, em seguida, as comparações por elemento da esquerda para a direita (incluindo conversões e com a saída inicial com base em regras existentes para operadores condicionais e/ou).</span><span class="sxs-lookup"><span data-stu-id="f3554-135">The left-hand-side value is evaluated first, then the right-hand-side value, then the element-wise comparisons from left to right (including conversions, and with early exit based on existing rules for conditional AND/OR operators).</span></span>

<span data-ttu-id="f3554-136">Por exemplo, se houver uma conversão do tipo `A` para o tipo `B` e um método `(A, A) GetTuple()`, avaliar `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` significa:</span><span class="sxs-lookup"><span data-stu-id="f3554-136">For instance, if there is a conversion from type `A` to type `B` and a method `(A, A) GetTuple()`, evaluating `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` means:</span></span>
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- <span data-ttu-id="f3554-137">em seguida, as conversões e comparações e a lógica condicional do elemento são avaliadas (converta `new A(1)` no tipo `B`, compare-o com `new B(4)`e assim por diante).</span><span class="sxs-lookup"><span data-stu-id="f3554-137">then the element-wise conversions and comparisons and conditional logic is evaluated (convert `new A(1)` to type `B`, then compare it with `new B(4)`, and so on).</span></span>

### <a name="comparing-null-to-null"></a><span data-ttu-id="f3554-138">Comparando `null` a `null`</span><span class="sxs-lookup"><span data-stu-id="f3554-138">Comparing `null` to `null`</span></span>

<span data-ttu-id="f3554-139">Esse é um caso especial de comparações regulares, que são transferidas para comparações de tupla.</span><span class="sxs-lookup"><span data-stu-id="f3554-139">This is a special case from regular comparisons, that carries over to tuple comparisons.</span></span> <span data-ttu-id="f3554-140">A comparação de `null == null` é permitida e os literais de `null` não obtêm nenhum tipo.</span><span class="sxs-lookup"><span data-stu-id="f3554-140">The `null == null` comparison is allowed, and the `null` literals do not get any type.</span></span>
<span data-ttu-id="f3554-141">Na igualdade de tupla, isso significa que `(0, null) == (0, null)` também é permitido e os literais `null` e tupla não obtêm um tipo.</span><span class="sxs-lookup"><span data-stu-id="f3554-141">In tuple equality, this means, `(0, null) == (0, null)` is also allowed and the `null` and tuple literals don't get a type either.</span></span>

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a><span data-ttu-id="f3554-142">Comparando uma struct anulável com `null` sem `operator==`</span><span class="sxs-lookup"><span data-stu-id="f3554-142">Comparing a nullable struct to `null` without `operator==`</span></span>

<span data-ttu-id="f3554-143">Esse é outro caso especial de comparações regulares, que são transferidas para comparações de tupla.</span><span class="sxs-lookup"><span data-stu-id="f3554-143">This is another special case from regular comparisons, that carries over to tuple comparisons.</span></span>
<span data-ttu-id="f3554-144">Se você tiver um `struct S` sem `operator==`, a comparação de `(S?)x == null` será permitida e será interpretada como `((S?).x).HasValue`.</span><span class="sxs-lookup"><span data-stu-id="f3554-144">If you have a `struct S` without `operator==`, the `(S?)x == null` comparison is allowed, and it is interpreted as `((S?).x).HasValue`.</span></span>
<span data-ttu-id="f3554-145">Na igualdade de tupla, a mesma regra é aplicada, portanto `(0, (S?)x) == (0, null)` é permitido.</span><span class="sxs-lookup"><span data-stu-id="f3554-145">In tuple equality, the same rule is applied, so `(0, (S?)x) == (0, null)` is allowed.</span></span>

## <a name="compatibility"></a><span data-ttu-id="f3554-146">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="f3554-146">Compatibility</span></span>

<span data-ttu-id="f3554-147">Se alguém escreveu seus próprios tipos de `ValueTuple` com uma implementação do operador de comparação, ele teria sido previamente selecionado pela resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="f3554-147">If someone wrote their own `ValueTuple` types with  an implementation of the comparison operator, it would have previously been picked up by overload resolution.</span></span> <span data-ttu-id="f3554-148">Mas como o novo caso de tupla vem antes da resolução de sobrecarga, tratamos desse caso com a comparação de tupla em vez de depender da comparação definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="f3554-148">But since the new tuple case comes before overload resolution, we would handle this case with tuple comparison instead of relying on the user-defined comparison.</span></span>

----

<span data-ttu-id="f3554-149">Relaciona-se aos [operadores de teste relacional e de tipo](../../spec/expressions.md#relational-and-type-testing-operators) relacionados a [#190](https://github.com/dotnet/csharplang/issues/190)</span><span class="sxs-lookup"><span data-stu-id="f3554-149">Relates to [relational and type testing operators](../../spec/expressions.md#relational-and-type-testing-operators) Relates to [#190](https://github.com/dotnet/csharplang/issues/190)</span></span>