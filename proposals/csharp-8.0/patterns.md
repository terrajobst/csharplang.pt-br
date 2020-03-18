---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485059"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="3e9df-101">Correspondência de padrão recursivo</span><span class="sxs-lookup"><span data-stu-id="3e9df-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="3e9df-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="3e9df-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3e9df-103">As extensões de correspondência C# de padrões para habilitar muitos dos benefícios de tipos de dados algébricas e correspondência de padrões de linguagens funcionais, mas de forma que se integre perfeitamente com a sensação da linguagem subjacente.</span><span class="sxs-lookup"><span data-stu-id="3e9df-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="3e9df-104">Os elementos dessa abordagem são inspirados por recursos relacionados nas linguagens [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Correspondência de padrão extensível por meio de uma linguagem leve") de programação e [escalabilidade](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Objetos correspondentes com padrões, página 273").</span><span class="sxs-lookup"><span data-stu-id="3e9df-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3e9df-105">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="3e9df-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="3e9df-106">Expressão is</span><span class="sxs-lookup"><span data-stu-id="3e9df-106">Is Expression</span></span>

<span data-ttu-id="3e9df-107">O operador `is` é estendido para testar uma expressão em relação a um *padrão*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="3e9df-108">Essa forma de *relational_expression* é além dos formulários existentes na C# especificação.</span><span class="sxs-lookup"><span data-stu-id="3e9df-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="3e9df-109">É um erro de tempo de compilação se a *relational_expression* à esquerda do token de `is` não designa um valor ou não tem um tipo.</span><span class="sxs-lookup"><span data-stu-id="3e9df-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="3e9df-110">Cada *identificador* do padrão apresenta uma nova variável local que é *definitivamente atribuída* depois que o operador de `is` é `true` (ou seja, *definitivamente atribuído quando verdadeiro*).</span><span class="sxs-lookup"><span data-stu-id="3e9df-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="3e9df-111">Observação: há tecnicamente uma ambiguidade entre o *tipo* em um `is-expression` e *constant_pattern*, que pode ser uma análise válida de um identificador qualificado.</span><span class="sxs-lookup"><span data-stu-id="3e9df-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="3e9df-112">Tentamos associá-lo como um tipo de compatibilidade com as versões anteriores do idioma; somente se isso falhar, resolveremos como fazemos uma expressão em outros contextos, para a primeira coisa encontrada (que deve ser uma constante ou um tipo).</span><span class="sxs-lookup"><span data-stu-id="3e9df-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="3e9df-113">Essa ambiguidade só está presente no lado direito de uma expressão de `is`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="3e9df-114">Padrões</span><span class="sxs-lookup"><span data-stu-id="3e9df-114">Patterns</span></span>

<span data-ttu-id="3e9df-115">Os padrões são usados no operador de *is_pattern* , em uma *switch_statement*e em uma *switch_expression* para expressar a forma de dados em que os dados de entrada (que chamamos de valor de entrada) devem ser comparados.</span><span class="sxs-lookup"><span data-stu-id="3e9df-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="3e9df-116">Os padrões podem ser recursivos para que as partes dos dados possam ser correspondidas em subpadrões.</span><span class="sxs-lookup"><span data-stu-id="3e9df-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a><span data-ttu-id="3e9df-117">Padrão de declaração</span><span class="sxs-lookup"><span data-stu-id="3e9df-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="3e9df-118">O *declaration_pattern* testa se uma expressão é de um determinado tipo e a converte para esse tipo se o teste for bem sucedido.</span><span class="sxs-lookup"><span data-stu-id="3e9df-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="3e9df-119">Isso pode introduzir uma variável local do tipo fornecido nomeado pelo identificador fornecido, se a designação for uma *single_variable_designation*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="3e9df-120">Essa variável local é *definitivamente atribuída* quando o resultado da operação de correspondência de padrões é `true`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="3e9df-121">A semântica de tempo de execução dessa expressão é que ela testa o tipo de tempo de execução do operando de *relational_expression* à esquerda em relação ao *tipo* no padrão.</span><span class="sxs-lookup"><span data-stu-id="3e9df-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="3e9df-122">Se for desse tipo de tempo de execução (ou algum subtipo) e não `null`, o resultado da `is operator` será `true`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="3e9df-123">Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3e9df-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="3e9df-124">Um valor de tipo estático `E` é considerado *compatível* com um tipo `T` se existir uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão de unboxing de `E` para `T`ou se um desses tipos for um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="3e9df-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="3e9df-125">É um erro de tempo de compilação se uma entrada do tipo `E` não for *compatível* com o padrão com o *tipo* em um padrão de tipo com o qual ele é correspondido.</span><span class="sxs-lookup"><span data-stu-id="3e9df-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="3e9df-126">O padrão de tipo é útil para executar testes de tipo de tempo de execução de tipos de referência e substitui o idioma</span><span class="sxs-lookup"><span data-stu-id="3e9df-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="3e9df-127">Com um pouco mais conciso</span><span class="sxs-lookup"><span data-stu-id="3e9df-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="3e9df-128">Erro se o *tipo* for um tipo de valor anulável.</span><span class="sxs-lookup"><span data-stu-id="3e9df-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="3e9df-129">O padrão de tipo pode ser usado para testar valores de tipos anuláveis: um valor do tipo `Nullable<T>` (ou um `T`em caixa) corresponde a um padrão de tipo `T2 id` se o valor for não nulo e o tipo de `T2` for `T`ou algum tipo ou interface base de `T`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="3e9df-130">Por exemplo, no fragmento de código</span><span class="sxs-lookup"><span data-stu-id="3e9df-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="3e9df-131">A condição da instrução de `if` é `true` em tempo de execução e a variável `v` contém o valor `3` do tipo `int` dentro do bloco.</span><span class="sxs-lookup"><span data-stu-id="3e9df-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="3e9df-132">Depois que o bloco, a variável `v` está no escopo, mas não é atribuída definitivamente.</span><span class="sxs-lookup"><span data-stu-id="3e9df-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="3e9df-133">Padrão de constante</span><span class="sxs-lookup"><span data-stu-id="3e9df-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="3e9df-134">Um padrão constante testa o valor de uma expressão em relação a um valor constante.</span><span class="sxs-lookup"><span data-stu-id="3e9df-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="3e9df-135">A constante pode ser qualquer expressão constante, como um literal, o nome de uma variável de `const` declarada ou uma constante de enumeração.</span><span class="sxs-lookup"><span data-stu-id="3e9df-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="3e9df-136">Quando o valor de entrada não é um tipo aberto, a expressão constante é convertida implicitamente no tipo da expressão correspondente; Se o tipo do valor de entrada não for *compatível* com o padrão com o tipo da expressão constante, a operação de correspondência de padrões será um erro.</span><span class="sxs-lookup"><span data-stu-id="3e9df-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="3e9df-137">O padrão *c* é considerado compatível com o valor de entrada convertido *e* , se `object.Equals(c, e)` retornar `true`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="3e9df-138">Esperamos ver `e is null` como a maneira mais comum de testar `null` no código recém gravado, pois não é possível invocar uma `operator==`definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="3e9df-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="3e9df-139">Padrão de var</span><span class="sxs-lookup"><span data-stu-id="3e9df-139">Var Pattern</span></span>

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

<span data-ttu-id="3e9df-140">Se a *designação* for uma *simple_designation*, uma expressão *e* corresponder ao padrão.</span><span class="sxs-lookup"><span data-stu-id="3e9df-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="3e9df-141">Em outras palavras, uma correspondência a um *padrão var* sempre é realizada com um *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="3e9df-142">Se o *simple_designation* for um *single_variable_designation*, o valor de *e* será associado a uma variável local introduzida recentemente.</span><span class="sxs-lookup"><span data-stu-id="3e9df-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="3e9df-143">O tipo da variável local é o tipo estático de *e*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="3e9df-144">Se a *designação* for uma *tuple_designation*, o padrão será equivalente a uma *positional_pattern* do formulário `(var` *designação*,... `)` onde os s de *design*são aqueles encontrados na *tuple_designation*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="3e9df-145">Por exemplo, o padrão `var (x, (y, z))` é equivalente a `(var x, (var y, var z))`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="3e9df-146">Erro se o nome `var` for associado a um tipo.</span><span class="sxs-lookup"><span data-stu-id="3e9df-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="3e9df-147">Descartar padrão</span><span class="sxs-lookup"><span data-stu-id="3e9df-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="3e9df-148">Uma expressão *e* corresponde ao padrão `_` sempre.</span><span class="sxs-lookup"><span data-stu-id="3e9df-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="3e9df-149">Em outras palavras, cada expressão corresponde ao padrão de descarte.</span><span class="sxs-lookup"><span data-stu-id="3e9df-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="3e9df-150">Um padrão de descarte não pode ser usado como o padrão de um *is_pattern_expression*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="3e9df-151">Padrão posicional</span><span class="sxs-lookup"><span data-stu-id="3e9df-151">Positional Pattern</span></span>

<span data-ttu-id="3e9df-152">Um padrão posicional verifica se o valor de entrada não é `null`, invoca um método `Deconstruct` apropriado e executa uma correspondência de padrão adicional nos valores resultantes.</span><span class="sxs-lookup"><span data-stu-id="3e9df-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="3e9df-153">Ele também dá suporte a uma sintaxe de padrão semelhante a tupla (sem o tipo que está sendo fornecido) quando o tipo do valor de entrada é o mesmo que o tipo que contém `Deconstruct`, ou se o tipo do valor de entrada é um tipo de tupla ou se o tipo do valor de entrada é `object` ou `ITuple` e o tipo de tempo de execução da expressão implementa `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

<span data-ttu-id="3e9df-154">Se o *tipo* for omitido, levaremos para ser o tipo estático do valor de entrada.</span><span class="sxs-lookup"><span data-stu-id="3e9df-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="3e9df-155">Dada uma correspondência de um valor de entrada ao *tipo* de padrão `(` *subpattern_list* `)`, um método é selecionado pesquisando em *tipo* para declarações acessíveis de `Deconstruct` e selecionando um entre eles usando as mesmas regras que para a declaração de desconstrução.</span><span class="sxs-lookup"><span data-stu-id="3e9df-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="3e9df-156">Erro se um *positional_pattern* omitir o tipo, tiver um único *subpadrão* sem um *identificador*, não tiver nenhum *property_subpattern* e não tiver *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="3e9df-157">Essa ambiguidade é desambiguada entre um *constant_pattern* que está entre parênteses e um *positional_pattern*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="3e9df-158">Para extrair os valores para corresponder aos padrões na lista,</span><span class="sxs-lookup"><span data-stu-id="3e9df-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="3e9df-159">Se o *tipo* foi omitido e o tipo do valor de entrada é um tipo de tupla, o número de subpadrões é necessário para ser o mesmo que a cardinalidade da tupla.</span><span class="sxs-lookup"><span data-stu-id="3e9df-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="3e9df-160">Cada elemento de tupla é correspondido em relação ao *subpadrão*correspondente e a correspondência é realizada com sucesso se todas elas forem bem sucedidos.</span><span class="sxs-lookup"><span data-stu-id="3e9df-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="3e9df-161">Se qualquer *subpadrão* tiver um *identificador*, isso deverá nomear um elemento de tupla na posição correspondente no tipo de tupla.</span><span class="sxs-lookup"><span data-stu-id="3e9df-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="3e9df-162">Caso contrário, se um `Deconstruct` adequado existir como um membro do *tipo*, será um erro de tempo de compilação se o tipo do valor de entrada não for compatível com o *padrão* com o *tipo*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="3e9df-163">Em tempo de execução, o valor de entrada é testado em relação ao *tipo*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="3e9df-164">Se isso falhar, a correspondência de padrão posicional falhará.</span><span class="sxs-lookup"><span data-stu-id="3e9df-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="3e9df-165">Se tiver sucesso, o valor de entrada será convertido nesse tipo e `Deconstruct` será invocado com novas variáveis geradas pelo compilador para receber os parâmetros de `out`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="3e9df-166">Cada valor recebido é correspondido em relação ao *subpadrão*correspondente e a correspondência é realizada com êxito se todas elas forem bem sucedidos.</span><span class="sxs-lookup"><span data-stu-id="3e9df-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="3e9df-167">Se qualquer *subpadrão* tiver um *identificador*, isso deverá nomear um parâmetro na posição correspondente de `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="3e9df-168">Caso contrário, se o *tipo* foi omitido, e o valor de entrada for do tipo `object` ou `ITuple` ou algum tipo que possa ser convertido em `ITuple` por uma conversão de referência implícita e nenhum *identificador* aparecer entre os subpadrãos, então corresponderei usando `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="3e9df-169">Caso contrário, o padrão é um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3e9df-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="3e9df-170">A ordem na qual os subpadrãos são correspondidos em tempo de execução não é especificado e uma correspondência com falha pode não tentar corresponder a todos os subpadrãos.</span><span class="sxs-lookup"><span data-stu-id="3e9df-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="3e9df-171">{1&gt;Exemplo&lt;1}</span><span class="sxs-lookup"><span data-stu-id="3e9df-171">Example</span></span>

<span data-ttu-id="3e9df-172">Este exemplo usa muitos dos recursos descritos nesta especificação</span><span class="sxs-lookup"><span data-stu-id="3e9df-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="3e9df-173">Padrão de propriedade</span><span class="sxs-lookup"><span data-stu-id="3e9df-173">Property Pattern</span></span>

<span data-ttu-id="3e9df-174">Um padrão de propriedade verifica se o valor de entrada não é `null` e corresponde recursivamente valores extraídos pelo uso de propriedades ou campos acessíveis.</span><span class="sxs-lookup"><span data-stu-id="3e9df-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="3e9df-175">Erro se qualquer _subpadrão_ de um _property_pattern_ não contiver um _identificador_ (ele deve ser do segundo formulário, que tem um _identificador_).</span><span class="sxs-lookup"><span data-stu-id="3e9df-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="3e9df-176">Uma vírgula à direita após o último subpadrão é opcional.</span><span class="sxs-lookup"><span data-stu-id="3e9df-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="3e9df-177">Observe que um padrão de verificação nula sai de um padrão de propriedade trivial.</span><span class="sxs-lookup"><span data-stu-id="3e9df-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="3e9df-178">Para verificar se a cadeia de caracteres `s` é não nula, você pode escrever qualquer um dos seguintes formulários</span><span class="sxs-lookup"><span data-stu-id="3e9df-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="3e9df-179">Dada uma correspondência de uma expressão *e* ao *tipo* de padrão `{` *property_pattern_list* `}`, será um erro de tempo de compilação se a expressão *e* não for *compatível* com o padrão com o tipo *t* designado por *tipo*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="3e9df-180">Se o tipo estiver ausente, levaremos para ser o tipo estático de *e*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="3e9df-181">Se o *identificador* estiver presente, ele declara uma variável de padrão *do tipo Type.*</span><span class="sxs-lookup"><span data-stu-id="3e9df-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="3e9df-182">Cada um dos identificadores que aparecem no lado esquerdo de seu *property_pattern_list* deve designar uma propriedade legível ou um campo de *T*acessível. Se o *simple_designation* do *property_pattern* estiver presente, ele definirá uma variável de padrão do tipo *T*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="3e9df-183">Em tempo de execução, a expressão é testada em relação a *T*. Se isso falhar, a correspondência de padrão de propriedade falhará e o resultado será `false`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="3e9df-184">Se tiver sucesso, cada campo ou propriedade de *property_subpattern* será lido e seu valor corresponderá ao seu padrão correspondente.</span><span class="sxs-lookup"><span data-stu-id="3e9df-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="3e9df-185">O resultado da correspondência inteira será `false` apenas se o resultado de qualquer um deles for `false`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="3e9df-186">A ordem na qual os subpadrãos são correspondidos não é especificada e uma correspondência com falha pode não corresponder a todos os subpadrãos em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="3e9df-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="3e9df-187">Se a correspondência for realizada com sucesso e a *simple_designation* da *property_pattern* for uma *single_variable_designation*, ela definirá uma variável do tipo *T* que recebe o valor correspondente.</span><span class="sxs-lookup"><span data-stu-id="3e9df-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="3e9df-188">Observação: o padrão de propriedade pode ser usado para correspondência de padrões com tipos anônimos.</span><span class="sxs-lookup"><span data-stu-id="3e9df-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="3e9df-189">{1&gt;Exemplo&lt;1}</span><span class="sxs-lookup"><span data-stu-id="3e9df-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="3e9df-190">Expressão switch</span><span class="sxs-lookup"><span data-stu-id="3e9df-190">Switch Expression</span></span>

<span data-ttu-id="3e9df-191">Um *switch_expression* é adicionado para dar suporte à semântica semelhante a `switch`para um contexto de expressão.</span><span class="sxs-lookup"><span data-stu-id="3e9df-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="3e9df-192">A C# sintaxe da linguagem é aumentada com as seguintes produções sintáticas:</span><span class="sxs-lookup"><span data-stu-id="3e9df-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

<span data-ttu-id="3e9df-193">O *switch_expression* não é permitido como um *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="3e9df-194">Estamos pensando em relaxar isso em uma revisão futura.</span><span class="sxs-lookup"><span data-stu-id="3e9df-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="3e9df-195">O tipo de *switch_expression* é o [*melhor tipo comum*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) de expressões que aparecem à direita dos tokens de `=>` do *switch_expression_arm*s se esse tipo existir e a expressão em todos os ARM da expressão switch puder ser convertida implicitamente nesse tipo.</span><span class="sxs-lookup"><span data-stu-id="3e9df-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="3e9df-196">Além disso, adicionamos uma nova *conversão de expressão de comutador*, que é uma conversão implícita predefinida de uma expressão de switch para cada tipo `T` para a qual existe uma conversão implícita da expressão de cada arm para `T`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="3e9df-197">Ocorrerá um erro se algum padrão de *switch_expression_arm*não puder afetar o resultado, pois algum padrão e proteção anteriores sempre corresponderá.</span><span class="sxs-lookup"><span data-stu-id="3e9df-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="3e9df-198">Uma expressão de switch é considerada *exaustiva* se algum ARM da expressão switch tratar cada valor de sua entrada.</span><span class="sxs-lookup"><span data-stu-id="3e9df-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="3e9df-199">O compilador deverá produzir um aviso se uma expressão de comutador não for *exaustiva*.</span><span class="sxs-lookup"><span data-stu-id="3e9df-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="3e9df-200">Em tempo de execução, o resultado da *switch_expression* é o valor da *expressão* da primeira *switch_expression_arm* para a qual a expressão no lado esquerdo da *switch_expression* corresponde ao padrão do *switch_expression_arm*e para o qual a *case_guard* do *switch_expression_arm*, se presente, é avaliada como `true`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="3e9df-201">Se não houver tal *switch_expression_arm*, o *switch_expression* lançará uma instância da exceção `System.Runtime.CompilerServices.SwitchExpressionException`.</span><span class="sxs-lookup"><span data-stu-id="3e9df-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="3e9df-202">Parênteses opcional ao alternar para um literal de tupla</span><span class="sxs-lookup"><span data-stu-id="3e9df-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="3e9df-203">Para alternar para um literal de tupla usando o *switch_statement*, você precisa escrever o que parece ser parênteses redundante</span><span class="sxs-lookup"><span data-stu-id="3e9df-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="3e9df-204">Para permitir</span><span class="sxs-lookup"><span data-stu-id="3e9df-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="3e9df-205">os parênteses da instrução switch são opcionais quando a expressão que está sendo ativada é um literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="3e9df-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="3e9df-206">Ordem de avaliação na correspondência de padrões</span><span class="sxs-lookup"><span data-stu-id="3e9df-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="3e9df-207">Dar a flexibilidade do compilador ao reordenar as operações executadas durante a correspondência de padrões pode permitir a flexibilidade que pode ser usada para melhorar a eficiência da correspondência de padrões.</span><span class="sxs-lookup"><span data-stu-id="3e9df-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="3e9df-208">O requisito (não imposto) seria que as propriedades acessadas em um padrão, e os métodos desconstruir, precisam ser "puras" (com efeito colateral gratuito, idempotente, etc.).</span><span class="sxs-lookup"><span data-stu-id="3e9df-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="3e9df-209">Isso não significa que adicionaremos pureza como um conceito de linguagem, apenas que permitiremos a flexibilidade do compilador em operações de reordenação.</span><span class="sxs-lookup"><span data-stu-id="3e9df-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="3e9df-210">**Resolução 2018-04-04 LDM**: confirmada: o compilador tem permissão para reordenar chamadas para `Deconstruct`, acessos de propriedade e invocações de métodos no `ITuple`e pode assumir que os valores retornados são os mesmos de várias chamadas.</span><span class="sxs-lookup"><span data-stu-id="3e9df-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="3e9df-211">O compilador não deve invocar funções que não afetem o resultado, e teremos muito cuidado antes de fazer qualquer alteração na ordem de avaliação gerada pelo compilador no futuro.</span><span class="sxs-lookup"><span data-stu-id="3e9df-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="3e9df-212">Algumas otimizações possíveis</span><span class="sxs-lookup"><span data-stu-id="3e9df-212">Some Possible Optimizations</span></span>

<span data-ttu-id="3e9df-213">A compilação da correspondência de padrões pode tirar proveito de partes comuns de padrões.</span><span class="sxs-lookup"><span data-stu-id="3e9df-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="3e9df-214">Por exemplo, se o teste de tipo de nível superior de dois padrões sucessivos em um *switch_statement* for o mesmo tipo, o código gerado poderá ignorar o teste de tipo para o segundo padrão.</span><span class="sxs-lookup"><span data-stu-id="3e9df-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="3e9df-215">Quando alguns dos padrões são inteiros ou cadeias de caracteres, o compilador pode gerar o mesmo tipo de código que ele gera para uma instrução switch em versões anteriores do idioma.</span><span class="sxs-lookup"><span data-stu-id="3e9df-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="3e9df-216">Para obter mais informações sobre esses tipos de otimizações, consulte [[Scott e Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Quando fazer a correspondência da heurística de compilação é importante?").</span><span class="sxs-lookup"><span data-stu-id="3e9df-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
