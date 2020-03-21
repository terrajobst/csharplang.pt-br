---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108358"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="6387a-101">Expressões de `new` de tipo de destino</span><span class="sxs-lookup"><span data-stu-id="6387a-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="6387a-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="6387a-102">[x] Proposed</span></span>
* <span data-ttu-id="6387a-103">[x] [protótipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="6387a-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="6387a-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="6387a-104">[ ] Implementation</span></span>
* <span data-ttu-id="6387a-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="6387a-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="6387a-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="6387a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6387a-107">Não exigir especificação de tipo para construtores quando o tipo é conhecido.</span><span class="sxs-lookup"><span data-stu-id="6387a-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="6387a-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="6387a-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="6387a-109">Permitir inicialização de campo sem duplicar o tipo.</span><span class="sxs-lookup"><span data-stu-id="6387a-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="6387a-110">Permitir a omissão do tipo quando ele puder ser inferido do uso.</span><span class="sxs-lookup"><span data-stu-id="6387a-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="6387a-111">Crie uma instância de um objeto sem soletrar o tipo.</span><span class="sxs-lookup"><span data-stu-id="6387a-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="6387a-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="6387a-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="6387a-113">A sintaxe de *object_creation_expression* seria modificada para tornar o *tipo* opcional quando parênteses estiverem presentes.</span><span class="sxs-lookup"><span data-stu-id="6387a-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="6387a-114">Isso é necessário para resolver a ambiguidade com *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="6387a-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="6387a-115">Um `new` com tipo de destino é conversível para qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="6387a-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="6387a-116">Como resultado, ele não contribui para a resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="6387a-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="6387a-117">Isso é principalmente para evitar alterações significativas imprevisíveis.</span><span class="sxs-lookup"><span data-stu-id="6387a-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="6387a-118">A lista de argumentos e as expressões de inicializador serão associadas depois que o tipo for determinado.</span><span class="sxs-lookup"><span data-stu-id="6387a-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="6387a-119">O tipo da expressão seria inferido do tipo de destino que seria necessário para ser um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="6387a-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="6387a-120">**Qualquer tipo de struct** (incluindo tipos de tupla)</span><span class="sxs-lookup"><span data-stu-id="6387a-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="6387a-121">**Qualquer tipo de referência** (incluindo tipos delegados)</span><span class="sxs-lookup"><span data-stu-id="6387a-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="6387a-122">**Qualquer parâmetro de tipo** com um construtor ou uma restrição de `struct`</span><span class="sxs-lookup"><span data-stu-id="6387a-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="6387a-123">com as seguintes exceções:</span><span class="sxs-lookup"><span data-stu-id="6387a-123">with the following exceptions:</span></span>

- <span data-ttu-id="6387a-124">**Tipos de enumeração:** nem todos os tipos enum contêm a constante zero, portanto, deve ser desejável usar o membro enum explícito.</span><span class="sxs-lookup"><span data-stu-id="6387a-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="6387a-125">**Tipos de interface:** esse é um recurso de nicho e deve ser preferível mencionar explicitamente o tipo.</span><span class="sxs-lookup"><span data-stu-id="6387a-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="6387a-126">**Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.</span><span class="sxs-lookup"><span data-stu-id="6387a-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="6387a-127">**dinâmico:** não permitimos `new dynamic()`, portanto, não permitimos `new()` com `dynamic` como um tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="6387a-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="6387a-128">Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="6387a-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="6387a-129">Quando o tipo de destino for um tipo de valor anulável, o `new` de tipo de destino será convertido para o tipo subjacente em vez do tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="6387a-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="6387a-130">**Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="6387a-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="6387a-131">As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct).</span><span class="sxs-lookup"><span data-stu-id="6387a-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="6387a-132">Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.</span><span class="sxs-lookup"><span data-stu-id="6387a-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
