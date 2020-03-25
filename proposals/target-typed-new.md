---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217197"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="b9d31-101">Expressões de `new` de tipo de destino</span><span class="sxs-lookup"><span data-stu-id="b9d31-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="b9d31-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="b9d31-102">[x] Proposed</span></span>
* <span data-ttu-id="b9d31-103">[x] [protótipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="b9d31-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="b9d31-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="b9d31-104">[ ] Implementation</span></span>
* <span data-ttu-id="b9d31-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="b9d31-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="b9d31-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="b9d31-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b9d31-107">Não exigir especificação de tipo para construtores quando o tipo é conhecido.</span><span class="sxs-lookup"><span data-stu-id="b9d31-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="b9d31-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="b9d31-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b9d31-109">Permitir inicialização de campo sem duplicar o tipo.</span><span class="sxs-lookup"><span data-stu-id="b9d31-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="b9d31-110">Permitir a omissão do tipo quando ele puder ser inferido do uso.</span><span class="sxs-lookup"><span data-stu-id="b9d31-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="b9d31-111">Crie uma instância de um objeto sem soletrar o tipo.</span><span class="sxs-lookup"><span data-stu-id="b9d31-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="b9d31-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="b9d31-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b9d31-113">A sintaxe de *object_creation_expression* seria modificada para tornar o *tipo* opcional quando parênteses estiverem presentes.</span><span class="sxs-lookup"><span data-stu-id="b9d31-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="b9d31-114">Isso é necessário para resolver a ambiguidade com *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="b9d31-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="b9d31-115">Um `new` com tipo de destino é conversível para qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="b9d31-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="b9d31-116">Como resultado, ele não contribui para a resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="b9d31-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="b9d31-117">Isso é principalmente para evitar alterações significativas imprevisíveis.</span><span class="sxs-lookup"><span data-stu-id="b9d31-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="b9d31-118">A lista de argumentos e as expressões de inicializador serão associadas depois que o tipo for determinado.</span><span class="sxs-lookup"><span data-stu-id="b9d31-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="b9d31-119">O tipo da expressão seria inferido do tipo de destino que seria necessário para ser um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="b9d31-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="b9d31-120">**Qualquer tipo de struct** (incluindo tipos de tupla)</span><span class="sxs-lookup"><span data-stu-id="b9d31-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="b9d31-121">**Qualquer tipo de referência** (incluindo tipos delegados)</span><span class="sxs-lookup"><span data-stu-id="b9d31-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="b9d31-122">**Qualquer parâmetro de tipo** com um construtor ou uma restrição de `struct`</span><span class="sxs-lookup"><span data-stu-id="b9d31-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="b9d31-123">com as seguintes exceções:</span><span class="sxs-lookup"><span data-stu-id="b9d31-123">with the following exceptions:</span></span>

- <span data-ttu-id="b9d31-124">**Tipos de enumeração:** nem todos os tipos enum contêm a constante zero, portanto, deve ser desejável usar o membro enum explícito.</span><span class="sxs-lookup"><span data-stu-id="b9d31-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="b9d31-125">**Tipos de interface:** esse é um recurso de nicho e deve ser preferível mencionar explicitamente o tipo.</span><span class="sxs-lookup"><span data-stu-id="b9d31-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="b9d31-126">**Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.</span><span class="sxs-lookup"><span data-stu-id="b9d31-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="b9d31-127">**dinâmico:** não permitimos `new dynamic()`, portanto, não permitimos `new()` com `dynamic` como um tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="b9d31-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="b9d31-128">Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="b9d31-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="b9d31-129">Quando o tipo de destino for um tipo de valor anulável, o `new` de tipo de destino será convertido para o tipo subjacente em vez do tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="b9d31-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="b9d31-130">**Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="b9d31-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="b9d31-131">As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct).</span><span class="sxs-lookup"><span data-stu-id="b9d31-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="b9d31-132">Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.</span><span class="sxs-lookup"><span data-stu-id="b9d31-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="b9d31-133">Diversos</span><span class="sxs-lookup"><span data-stu-id="b9d31-133">Miscellaneous</span></span>

<span data-ttu-id="b9d31-134">`throw new()` não é permitido.</span><span class="sxs-lookup"><span data-stu-id="b9d31-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="b9d31-135">O `new` de tipo de destino não é permitido com operadores binários.</span><span class="sxs-lookup"><span data-stu-id="b9d31-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="b9d31-136">Não é permitido quando não há tipo para destino: operadores unários, coleção de um `foreach`, em uma `using`, em uma deconstrução, em uma expressão `await`, como uma propriedade de tipo anônimo (`new { Prop = new() }`), em uma instrução `lock`, em uma `sizeof`, em uma instrução `fixed`, em um acesso de membro (`new().field`), em uma operação expedida dinamicamente (`someDynamic.Method(new())`), em uma consulta LINQ, como o operando do operador `is`, como operando esquerdo do operador `??` ,  ...</span><span class="sxs-lookup"><span data-stu-id="b9d31-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="b9d31-137">Ele também não é permitido como um `ref`.</span><span class="sxs-lookup"><span data-stu-id="b9d31-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b9d31-138">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="b9d31-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b9d31-139">Houve algumas preocupações com o tipo de destino `new` criar novas categorias de alterações significativas, mas já temos isso com `null` e `default`, e isso não foi um problema significativo.</span><span class="sxs-lookup"><span data-stu-id="b9d31-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b9d31-140">Alternativas</span><span class="sxs-lookup"><span data-stu-id="b9d31-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b9d31-141">A maioria das reclamações sobre os tipos muito longos para duplicar na inicialização de campo é sobre os *argumentos de tipo* , não o tipo em si, poderíamos inferir apenas argumentos de tipo como `new Dictionary(...)` (ou semelhantes) e inferir argumentos de tipo localmente a partir de argumentos ou o inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="b9d31-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="b9d31-142">Perguntas</span><span class="sxs-lookup"><span data-stu-id="b9d31-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="b9d31-143">Devemos proibir usos em árvores de expressão?</span><span class="sxs-lookup"><span data-stu-id="b9d31-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="b9d31-144">foi</span><span class="sxs-lookup"><span data-stu-id="b9d31-144">(no)</span></span>
- <span data-ttu-id="b9d31-145">Como o recurso interage com `dynamic` argumentos?</span><span class="sxs-lookup"><span data-stu-id="b9d31-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="b9d31-146">(sem tratamento especial)</span><span class="sxs-lookup"><span data-stu-id="b9d31-146">(no special treatment)</span></span>
- <span data-ttu-id="b9d31-147">Como o IntelliSense deve funcionar com `new()`?</span><span class="sxs-lookup"><span data-stu-id="b9d31-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="b9d31-148">(somente quando há um único tipo de destino)</span><span class="sxs-lookup"><span data-stu-id="b9d31-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b9d31-149">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="b9d31-149">Design meetings</span></span>

- [<span data-ttu-id="b9d31-150">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="b9d31-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="b9d31-151">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="b9d31-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="b9d31-152">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="b9d31-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="b9d31-153">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="b9d31-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="b9d31-154">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="b9d31-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
