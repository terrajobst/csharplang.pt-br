---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484534"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="c6935-101">Expressões de `new` de tipo de destino</span><span class="sxs-lookup"><span data-stu-id="c6935-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="c6935-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="c6935-102">[x] Proposed</span></span>
* <span data-ttu-id="c6935-103">[x] [protótipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="c6935-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="c6935-104">[] Implementação</span><span class="sxs-lookup"><span data-stu-id="c6935-104">[ ] Implementation</span></span>
* <span data-ttu-id="c6935-105">[] Especificação</span><span class="sxs-lookup"><span data-stu-id="c6935-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="c6935-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="c6935-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c6935-107">Não exigir especificação de tipo para construtores quando o tipo é conhecido.</span><span class="sxs-lookup"><span data-stu-id="c6935-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="c6935-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="c6935-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c6935-109">Permitir inicialização de campo sem duplicar o tipo.</span><span class="sxs-lookup"><span data-stu-id="c6935-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="c6935-110">Permitir a omissão do tipo quando ele puder ser inferido do uso.</span><span class="sxs-lookup"><span data-stu-id="c6935-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="c6935-111">Crie uma instância de um objeto sem soletrar o tipo.</span><span class="sxs-lookup"><span data-stu-id="c6935-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="c6935-112">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="c6935-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c6935-113">A sintaxe de *object_creation_expression* seria modificada para tornar o *tipo* opcional quando parênteses estiverem presentes.</span><span class="sxs-lookup"><span data-stu-id="c6935-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="c6935-114">Isso é necessário para resolver a ambiguidade com *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="c6935-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="c6935-115">Um `new` com tipo de destino é conversível para qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="c6935-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="c6935-116">Como resultado, ele não contribui para a resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="c6935-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="c6935-117">Isso é principalmente para evitar alterações significativas imprevisíveis.</span><span class="sxs-lookup"><span data-stu-id="c6935-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="c6935-118">A lista de argumentos e as expressões de inicializador serão associadas depois que o tipo for determinado.</span><span class="sxs-lookup"><span data-stu-id="c6935-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="c6935-119">O tipo da expressão seria inferido do tipo de destino que seria necessário para ser um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="c6935-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="c6935-120">**Qualquer tipo de struct**</span><span class="sxs-lookup"><span data-stu-id="c6935-120">**Any struct type**</span></span>
- <span data-ttu-id="c6935-121">**Qualquer tipo de referência**</span><span class="sxs-lookup"><span data-stu-id="c6935-121">**Any reference type**</span></span>
- <span data-ttu-id="c6935-122">**Qualquer parâmetro de tipo** com um construtor ou uma restrição de `struct`</span><span class="sxs-lookup"><span data-stu-id="c6935-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="c6935-123">com as seguintes exceções:</span><span class="sxs-lookup"><span data-stu-id="c6935-123">with the following exceptions:</span></span>

- <span data-ttu-id="c6935-124">**Tipos de enumeração:** nem todos os tipos enum contêm a constante zero, portanto, deve ser desejável usar o membro enum explícito.</span><span class="sxs-lookup"><span data-stu-id="c6935-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="c6935-125">**Tipos de interface:** esse é um recurso de nicho e deve ser preferível mencionar explicitamente o tipo.</span><span class="sxs-lookup"><span data-stu-id="c6935-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="c6935-126">**Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.</span><span class="sxs-lookup"><span data-stu-id="c6935-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="c6935-127">**Construtor padrão de struct**: isso regra todos os tipos primitivos e a maioria dos tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="c6935-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="c6935-128">Se você quisesse usar o valor padrão desses tipos, poderia escrever `default` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c6935-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="c6935-129">Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="c6935-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="c6935-130">**Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="c6935-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="c6935-131">As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct).</span><span class="sxs-lookup"><span data-stu-id="c6935-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="c6935-132">Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.</span><span class="sxs-lookup"><span data-stu-id="c6935-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="c6935-133">**Problema aberto:** devemos permitir `throw new()` com `Exception` como o tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="c6935-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="c6935-134">Temos `throw null` hoje, mas não `throw default` (embora ele tenha o mesmo efeito).</span><span class="sxs-lookup"><span data-stu-id="c6935-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="c6935-135">Por outro lado, `throw new()` pode ser realmente útil como uma abreviação para `throw new Exception(...)`.</span><span class="sxs-lookup"><span data-stu-id="c6935-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="c6935-136">Observe que ele já é permitido pela especificação atual.</span><span class="sxs-lookup"><span data-stu-id="c6935-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="c6935-137">`Exception` é um tipo de referência, e a especificação para a instrução Throw indica que a expressão é convertida em `Exception`.</span><span class="sxs-lookup"><span data-stu-id="c6935-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="c6935-138">**Problema aberto:** devemos permitir usos de um `new` de tipo de destino com operadores aritméticos e de comparação definidos pelo usuário?</span><span class="sxs-lookup"><span data-stu-id="c6935-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="c6935-139">Para comparação, o `default` dá suporte apenas aos operadores de igualdade (definido pelo usuário e interno).</span><span class="sxs-lookup"><span data-stu-id="c6935-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="c6935-140">Isso fará sentido dar suporte a outros operadores para `new()` também?</span><span class="sxs-lookup"><span data-stu-id="c6935-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="c6935-141">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="c6935-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c6935-142">None.</span><span class="sxs-lookup"><span data-stu-id="c6935-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c6935-143">Alternativas</span><span class="sxs-lookup"><span data-stu-id="c6935-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c6935-144">A maioria das reclamações sobre os tipos muito longos para duplicar na inicialização de campo é sobre os *argumentos de tipo* , não o tipo em si, poderíamos inferir apenas argumentos de tipo como `new Dictionary(...)` (ou semelhantes) e inferir argumentos de tipo localmente a partir de argumentos ou o inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="c6935-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="c6935-145">Perguntas</span><span class="sxs-lookup"><span data-stu-id="c6935-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="c6935-146">Devemos proibir usos em árvores de expressão?</span><span class="sxs-lookup"><span data-stu-id="c6935-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="c6935-147">foi</span><span class="sxs-lookup"><span data-stu-id="c6935-147">(no)</span></span>
- <span data-ttu-id="c6935-148">Como o recurso interage com `dynamic` argumentos?</span><span class="sxs-lookup"><span data-stu-id="c6935-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="c6935-149">(sem tratamento especial)</span><span class="sxs-lookup"><span data-stu-id="c6935-149">(no special treatment)</span></span>
- <span data-ttu-id="c6935-150">Como o IntelliSense deve funcionar com `new()`?</span><span class="sxs-lookup"><span data-stu-id="c6935-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="c6935-151">(somente quando há um único tipo de destino)</span><span class="sxs-lookup"><span data-stu-id="c6935-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="c6935-152">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="c6935-152">Design meetings</span></span>

- [<span data-ttu-id="c6935-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="c6935-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
