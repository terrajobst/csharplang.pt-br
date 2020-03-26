---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281964"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="304af-101">Expressões de `new` de tipo de destino</span><span class="sxs-lookup"><span data-stu-id="304af-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="304af-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="304af-102">[x] Proposed</span></span>
* <span data-ttu-id="304af-103">[x] [protótipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="304af-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="304af-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="304af-104">[ ] Implementation</span></span>
* <span data-ttu-id="304af-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="304af-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="304af-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="304af-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="304af-107">Não exigir especificação de tipo para construtores quando o tipo é conhecido.</span><span class="sxs-lookup"><span data-stu-id="304af-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="304af-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="304af-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="304af-109">Permitir inicialização de campo sem duplicar o tipo.</span><span class="sxs-lookup"><span data-stu-id="304af-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="304af-110">Permitir a omissão do tipo quando ele puder ser inferido do uso.</span><span class="sxs-lookup"><span data-stu-id="304af-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="304af-111">Crie uma instância de um objeto sem soletrar o tipo.</span><span class="sxs-lookup"><span data-stu-id="304af-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="304af-112">Especificação</span><span class="sxs-lookup"><span data-stu-id="304af-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="304af-113">Um novo formulário sintático, *target_typed_new* do *object_creation_expression* é aceito, no qual o *tipo* é opcional.</span><span class="sxs-lookup"><span data-stu-id="304af-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="304af-114">Uma expressão de *target_typed_new* não tem um tipo.</span><span class="sxs-lookup"><span data-stu-id="304af-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="304af-115">No entanto, há uma nova *conversão de criação de objeto* que é uma conversão implícita de expressão, que existe de um *target_typed_new* a cada tipo.</span><span class="sxs-lookup"><span data-stu-id="304af-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="304af-116">Dado um tipo de destino `T`, o tipo `T0` é o tipo subjacente de `T`se `T` for uma instância de `System.Nullable`.</span><span class="sxs-lookup"><span data-stu-id="304af-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="304af-117">Caso contrário `T0` é `T`.</span><span class="sxs-lookup"><span data-stu-id="304af-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="304af-118">O significado de uma expressão de *target_typed_new* que é convertida no tipo `T` é igual ao significado de uma *object_creation_expression* correspondente que especifica `T0` como o tipo.</span><span class="sxs-lookup"><span data-stu-id="304af-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="304af-119">É um erro de tempo de compilação se um *target_typed_new* for usado como um operando de um operador unário ou binário, ou se for usado onde ele não está sujeito a uma *conversão de criação de objeto*.</span><span class="sxs-lookup"><span data-stu-id="304af-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="304af-120">**Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="304af-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="304af-121">As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct).</span><span class="sxs-lookup"><span data-stu-id="304af-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="304af-122">Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.</span><span class="sxs-lookup"><span data-stu-id="304af-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="304af-123">Diversos</span><span class="sxs-lookup"><span data-stu-id="304af-123">Miscellaneous</span></span>

<span data-ttu-id="304af-124">As seguintes são as consequências da especificação:</span><span class="sxs-lookup"><span data-stu-id="304af-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="304af-125">`throw new()` é permitido (o tipo de destino é `System.Exception`)</span><span class="sxs-lookup"><span data-stu-id="304af-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="304af-126">O `new` de tipo de destino não é permitido com operadores binários.</span><span class="sxs-lookup"><span data-stu-id="304af-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="304af-127">Não é permitido quando não há tipo para destino: operadores unários, coleção de um `foreach`, em uma `using`, em uma deconstrução, em uma expressão `await`, como uma propriedade de tipo anônimo (`new { Prop = new() }`), em uma instrução `lock`, em uma `sizeof`, em uma instrução `fixed`, em um acesso de membro (`new().field`), em uma operação expedida dinamicamente (`someDynamic.Method(new())`), em uma consulta LINQ, como o operando do operador `is`, como operando esquerdo do operador `??` ,  ...</span><span class="sxs-lookup"><span data-stu-id="304af-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="304af-128">Ele também não é permitido como um `ref`.</span><span class="sxs-lookup"><span data-stu-id="304af-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="304af-129">Os tipos de tipos a seguir não são permitidos como destinos da conversão</span><span class="sxs-lookup"><span data-stu-id="304af-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="304af-130">**Tipos de enumeração:** `new()` funcionarão (como `new Enum()` funciona para fornecer o valor padrão), mas `new(1)` não funcionará, pois os tipos de enumeração não têm um construtor.</span><span class="sxs-lookup"><span data-stu-id="304af-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="304af-131">**Tipos de interface:** Isso funcionaria da mesma forma que a expressão de criação correspondente para tipos COM.</span><span class="sxs-lookup"><span data-stu-id="304af-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="304af-132">**Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.</span><span class="sxs-lookup"><span data-stu-id="304af-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="304af-133">**dinâmico:** não permitimos `new dynamic()`, portanto, não permitimos `new()` com `dynamic` como um tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="304af-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="304af-134">**tuplas:** Eles têm o mesmo significado de uma criação de objeto usando o tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="304af-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="304af-135">Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="304af-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="304af-136">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="304af-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="304af-137">Houve algumas preocupações com o tipo de destino `new` criar novas categorias de alterações significativas, mas já temos isso com `null` e `default`, e isso não foi um problema significativo.</span><span class="sxs-lookup"><span data-stu-id="304af-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="304af-138">Alternativas</span><span class="sxs-lookup"><span data-stu-id="304af-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="304af-139">A maioria das reclamações sobre os tipos muito longos para duplicar na inicialização de campo é sobre os *argumentos de tipo* , não o tipo em si, poderíamos inferir apenas argumentos de tipo como `new Dictionary(...)` (ou semelhantes) e inferir argumentos de tipo localmente a partir de argumentos ou o inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="304af-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="304af-140">Perguntas</span><span class="sxs-lookup"><span data-stu-id="304af-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="304af-141">Devemos proibir usos em árvores de expressão?</span><span class="sxs-lookup"><span data-stu-id="304af-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="304af-142">foi</span><span class="sxs-lookup"><span data-stu-id="304af-142">(no)</span></span>
- <span data-ttu-id="304af-143">Como o recurso interage com `dynamic` argumentos?</span><span class="sxs-lookup"><span data-stu-id="304af-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="304af-144">(sem tratamento especial)</span><span class="sxs-lookup"><span data-stu-id="304af-144">(no special treatment)</span></span>
- <span data-ttu-id="304af-145">Como o IntelliSense deve funcionar com `new()`?</span><span class="sxs-lookup"><span data-stu-id="304af-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="304af-146">(somente quando há um único tipo de destino)</span><span class="sxs-lookup"><span data-stu-id="304af-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="304af-147">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="304af-147">Design meetings</span></span>

- [<span data-ttu-id="304af-148">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="304af-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="304af-149">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="304af-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="304af-150">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="304af-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="304af-151">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="304af-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="304af-152">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="304af-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="304af-153">LDM-2020-03-25</span><span class="sxs-lookup"><span data-stu-id="304af-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
