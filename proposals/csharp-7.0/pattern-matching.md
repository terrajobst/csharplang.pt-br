---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485164"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="f2d77-101">Correspondência de padrões C# para 7</span><span class="sxs-lookup"><span data-stu-id="f2d77-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="f2d77-102">As extensões de correspondência C# de padrões para habilitar muitos dos benefícios de tipos de dados algébricas e correspondência de padrões de linguagens funcionais, mas de forma que se integre perfeitamente com a sensação da linguagem subjacente.</span><span class="sxs-lookup"><span data-stu-id="f2d77-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="f2d77-103">Os recursos básicos são: [tipos de registro](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), que são tipos cujo significado semântico é descrito pela forma dos dados; e correspondência de padrões, que é uma nova forma de expressão que permite a decomposição de vários níveis extremamente conciso desses tipos de dados.</span><span class="sxs-lookup"><span data-stu-id="f2d77-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="f2d77-104">Os elementos dessa abordagem são inspirados por recursos relacionados nas linguagens [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Correspondência de padrão extensível por meio de uma linguagem leve") de programação e [escalabilidade](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Objetos correspondentes com padrões").</span><span class="sxs-lookup"><span data-stu-id="f2d77-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="f2d77-105">Expressão is</span><span class="sxs-lookup"><span data-stu-id="f2d77-105">Is expression</span></span>

<span data-ttu-id="f2d77-106">O operador `is` é estendido para testar uma expressão em relação a um *padrão*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="f2d77-107">Essa forma de *relational_expression* é além dos formulários existentes na C# especificação.</span><span class="sxs-lookup"><span data-stu-id="f2d77-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="f2d77-108">É um erro de tempo de compilação se a *relational_expression* à esquerda do token de `is` não designa um valor ou não tem um tipo.</span><span class="sxs-lookup"><span data-stu-id="f2d77-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="f2d77-109">Cada *identificador* do padrão apresenta uma nova variável local que é *definitivamente atribuída* depois que o operador de `is` é `true` (ou seja, *definitivamente atribuído quando verdadeiro*).</span><span class="sxs-lookup"><span data-stu-id="f2d77-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="f2d77-110">Observação: há tecnicamente uma ambiguidade entre o *tipo* em um `is-expression` e *constant_pattern*, que pode ser uma análise válida de um identificador qualificado.</span><span class="sxs-lookup"><span data-stu-id="f2d77-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="f2d77-111">Tentamos associá-lo como um tipo de compatibilidade com as versões anteriores do idioma; somente se isso falhar, resolveremos como fazemos em outros contextos, para a primeira coisa encontrada (que deve ser uma constante ou um tipo).</span><span class="sxs-lookup"><span data-stu-id="f2d77-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="f2d77-112">Essa ambiguidade só está presente no lado direito de uma expressão de `is`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="f2d77-113">Padrões</span><span class="sxs-lookup"><span data-stu-id="f2d77-113">Patterns</span></span>

<span data-ttu-id="f2d77-114">Padrões são usados no operador de `is` e em uma *switch_statement* para expressar a forma de dados em que os dados de entrada devem ser comparados.</span><span class="sxs-lookup"><span data-stu-id="f2d77-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="f2d77-115">Os padrões podem ser recursivos para que as partes dos dados possam ser correspondidas em subpadrões.</span><span class="sxs-lookup"><span data-stu-id="f2d77-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="f2d77-116">Observação: há tecnicamente uma ambiguidade entre o *tipo* em um `is-expression` e *constant_pattern*, que pode ser uma análise válida de um identificador qualificado.</span><span class="sxs-lookup"><span data-stu-id="f2d77-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="f2d77-117">Tentamos associá-lo como um tipo de compatibilidade com as versões anteriores do idioma; somente se isso falhar, resolveremos como fazemos em outros contextos, para a primeira coisa encontrada (que deve ser uma constante ou um tipo).</span><span class="sxs-lookup"><span data-stu-id="f2d77-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="f2d77-118">Essa ambiguidade só está presente no lado direito de uma expressão de `is`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="f2d77-119">Padrão de declaração</span><span class="sxs-lookup"><span data-stu-id="f2d77-119">Declaration pattern</span></span>

<span data-ttu-id="f2d77-120">O *declaration_pattern* testa se uma expressão é de um determinado tipo e a converte para esse tipo se o teste for bem sucedido.</span><span class="sxs-lookup"><span data-stu-id="f2d77-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="f2d77-121">Se o *simple_designation* for um identificador, ele apresentará uma variável local do tipo fornecido nomeado pelo identificador fornecido.</span><span class="sxs-lookup"><span data-stu-id="f2d77-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="f2d77-122">Essa variável local é *definitivamente atribuída* quando o resultado da operação de correspondência de padrões é true.</span><span class="sxs-lookup"><span data-stu-id="f2d77-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="f2d77-123">A semântica de tempo de execução dessa expressão é que ela testa o tipo de tempo de execução do operando de *relational_expression* à esquerda em relação ao *tipo* no padrão.</span><span class="sxs-lookup"><span data-stu-id="f2d77-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="f2d77-124">Se for desse tipo de tempo de execução (ou algum subtipo), o resultado da `is operator` será `true`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="f2d77-125">Ele declara uma nova variável local denominada pelo *identificador* que recebe o valor do operando à esquerda quando o resultado é `true`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="f2d77-126">Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="f2d77-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="f2d77-127">Um valor de tipo estático `E` é chamado de *padrão compatível* com o tipo `T` se houver uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão unboxing de `E` para `T`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="f2d77-128">É um erro de tempo de compilação se uma expressão do tipo `E` não for compatível com o tipo em um padrão de tipo com o qual ele é correspondido.</span><span class="sxs-lookup"><span data-stu-id="f2d77-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="f2d77-129">Observação: [em C# 7,1, estendemos isso](../csharp-7.1/generics-pattern-match.md) para permitir uma operação de correspondência de padrões se o tipo de entrada ou o tipo `T` for um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="f2d77-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="f2d77-130">Este parágrafo é substituído pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="f2d77-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="f2d77-131">Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="f2d77-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="f2d77-132">Um valor de tipo estático `E` é chamado de *padrão compatível* com o tipo `T` se houver uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão unboxing de `E` para `T`**ou se `E` ou `T` for um tipo aberto**.</span><span class="sxs-lookup"><span data-stu-id="f2d77-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="f2d77-133">É um erro de tempo de compilação se uma expressão do tipo `E` não for compatível com o tipo em um padrão de tipo com o qual ele é correspondido.</span><span class="sxs-lookup"><span data-stu-id="f2d77-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="f2d77-134">O padrão de declaração é útil para executar testes de tipo de tempo de execução de tipos de referência e substitui o idioma</span><span class="sxs-lookup"><span data-stu-id="f2d77-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="f2d77-135">Com um pouco mais conciso</span><span class="sxs-lookup"><span data-stu-id="f2d77-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="f2d77-136">Erro se o *tipo* for um tipo de valor anulável.</span><span class="sxs-lookup"><span data-stu-id="f2d77-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="f2d77-137">O padrão de declaração pode ser usado para testar valores de tipos anuláveis: um valor do tipo `Nullable<T>` (ou um `T`em caixa) corresponde a um padrão de tipo `T2 id` se o valor for não nulo e o tipo de `T2` for `T`ou algum tipo ou interface base de `T`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="f2d77-138">Por exemplo, no fragmento de código</span><span class="sxs-lookup"><span data-stu-id="f2d77-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="f2d77-139">A condição da instrução de `if` é `true` em tempo de execução e a variável `v` contém o valor `3` do tipo `int` dentro do bloco.</span><span class="sxs-lookup"><span data-stu-id="f2d77-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="f2d77-140">Padrão de constante</span><span class="sxs-lookup"><span data-stu-id="f2d77-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="f2d77-141">Um padrão constante testa o valor de uma expressão em relação a um valor constante.</span><span class="sxs-lookup"><span data-stu-id="f2d77-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="f2d77-142">A constante pode ser qualquer expressão constante, como um literal, o nome de uma variável de `const` declarada ou uma constante de enumeração ou uma expressão de `typeof`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="f2d77-143">Se e *e* *c* forem de tipos integrais, o padrão será considerado correspondido se o resultado da `e == c` de expressão for `true`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="f2d77-144">Caso contrário, o padrão será considerado correspondente se `object.Equals(e, c)` retornar `true`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="f2d77-145">Nesse caso, é um erro de tempo de compilação se o tipo estático de *e* não for *compatível* com o tipo da constante.</span><span class="sxs-lookup"><span data-stu-id="f2d77-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="f2d77-146">Padrão de var</span><span class="sxs-lookup"><span data-stu-id="f2d77-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="f2d77-147">Uma expressão *e* corresponde a uma *var_pattern* sempre.</span><span class="sxs-lookup"><span data-stu-id="f2d77-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="f2d77-148">Em outras palavras, uma correspondência a um *padrão de var* sempre é realizada com sucesso.</span><span class="sxs-lookup"><span data-stu-id="f2d77-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="f2d77-149">Se o *simple_designation* for um identificador, em tempo de execução o valor de *e* será associado a uma variável local introduzida recentemente.</span><span class="sxs-lookup"><span data-stu-id="f2d77-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="f2d77-150">O tipo da variável local é o tipo estático de *e*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="f2d77-151">Erro se o nome `var` for associado a um tipo.</span><span class="sxs-lookup"><span data-stu-id="f2d77-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="f2d77-152">Instrução switch</span><span class="sxs-lookup"><span data-stu-id="f2d77-152">Switch statement</span></span>

<span data-ttu-id="f2d77-153">A instrução `switch` é estendida para selecionar para execução o primeiro bloco com um padrão associado que corresponda à *expressão de comutador*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="f2d77-154">A ordem na qual os padrões são correspondidos não está definida.</span><span class="sxs-lookup"><span data-stu-id="f2d77-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="f2d77-155">Um compilador tem permissão para corresponder padrões fora de ordem e reutilizar os resultados de padrões já correspondidos para calcular o resultado da correspondência de outros padrões.</span><span class="sxs-lookup"><span data-stu-id="f2d77-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="f2d77-156">Se uma *proteção de caso* estiver presente, sua expressão será do tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="f2d77-157">Ele é avaliado como uma condição adicional que deve ser satisfeita para que o caso seja considerado satisfeito.</span><span class="sxs-lookup"><span data-stu-id="f2d77-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="f2d77-158">Erro se um *switch_label* não puder ter nenhum efeito no tempo de execução porque seu padrão é incorporadas por casos anteriores.</span><span class="sxs-lookup"><span data-stu-id="f2d77-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="f2d77-159">[TODO: devemos ser mais precisos sobre as técnicas que o compilador precisa usar para alcançar esse julgamento.]</span><span class="sxs-lookup"><span data-stu-id="f2d77-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="f2d77-160">Uma variável de padrão declarada em um *switch_label* é definitivamente atribuída em seu bloco de caso se e somente se esse bloco de caso contiver precisamente um *switch_label*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="f2d77-161">[TODO: devemos especificar quando um *bloco de alternância* pode ser acessado.]</span><span class="sxs-lookup"><span data-stu-id="f2d77-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="f2d77-162">Escopo de variáveis de padrão</span><span class="sxs-lookup"><span data-stu-id="f2d77-162">Scope of pattern variables</span></span>

<span data-ttu-id="f2d77-163">O escopo de uma variável declarada em um padrão é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f2d77-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="f2d77-164">Se o padrão for um rótulo de caso, o escopo da variável será o *bloco de caso*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="f2d77-165">Caso contrário, a variável é declarada em uma expressão *is_pattern* , e seu escopo é baseado na construção que está delimitando imediatamente a expressão que contém a expressão *is_pattern* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f2d77-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="f2d77-166">Se a expressão estiver em uma lambda Expression-apto para, seu escopo será o corpo do lambda.</span><span class="sxs-lookup"><span data-stu-id="f2d77-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="f2d77-167">Se a expressão estiver em um método ou Propriedade apto para de expressão, seu escopo será o corpo do método ou da propriedade.</span><span class="sxs-lookup"><span data-stu-id="f2d77-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="f2d77-168">Se a expressão estiver em uma cláusula `when` de uma cláusula `catch`, seu escopo será a cláusula `catch`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="f2d77-169">Se a expressão estiver em um *iteration_statement*, seu escopo será apenas essa instrução.</span><span class="sxs-lookup"><span data-stu-id="f2d77-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="f2d77-170">Caso contrário, se a expressão estiver em algum outro formulário de instrução, seu escopo será o escopo que contém a instrução.</span><span class="sxs-lookup"><span data-stu-id="f2d77-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="f2d77-171">Com a finalidade de determinar o escopo, um *embedded_statement* é considerado em seu próprio escopo.</span><span class="sxs-lookup"><span data-stu-id="f2d77-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="f2d77-172">Por exemplo, a gramática para uma *if_statement* é</span><span class="sxs-lookup"><span data-stu-id="f2d77-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="f2d77-173">Portanto, se a instrução controlada de um *if_statement* declarar uma variável de padrão, seu escopo será restrito a esse *embedded_statement*:</span><span class="sxs-lookup"><span data-stu-id="f2d77-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="f2d77-174">Nesse caso, o escopo de `z` é a instrução inserida `M(y is var z);`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="f2d77-175">Outros casos são erros por outros motivos (por exemplo, no valor padrão de um parâmetro ou em um atributo, ambos são um erro porque esses contextos exigem uma expressão constante).</span><span class="sxs-lookup"><span data-stu-id="f2d77-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="f2d77-176">[Em C# 7,3, adicionamos os seguintes contextos](../csharp-7.3/expression-variables-in-initializers.md) nos quais uma variável de padrão pode ser declarada:</span><span class="sxs-lookup"><span data-stu-id="f2d77-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="f2d77-177">Se a expressão estiver em um *inicializador de Construtor*, seu escopo será o *inicializador de Construtor* e o corpo do construtor.</span><span class="sxs-lookup"><span data-stu-id="f2d77-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="f2d77-178">Se a expressão estiver em um inicializador de campo, seu escopo será o *equals_value_clause* em que ele aparece.</span><span class="sxs-lookup"><span data-stu-id="f2d77-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="f2d77-179">Se a expressão estiver em uma cláusula de consulta que é especificada para ser convertida no corpo de um lambda, seu escopo será apenas essa expressão.</span><span class="sxs-lookup"><span data-stu-id="f2d77-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="f2d77-180">Alterações na desambiguidade sintática</span><span class="sxs-lookup"><span data-stu-id="f2d77-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="f2d77-181">Há situações que envolvem genéricos em que C# a gramática é ambígua e a especificação da linguagem indica como resolver essas ambiguidades:</span><span class="sxs-lookup"><span data-stu-id="f2d77-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="f2d77-182">ambiguidades de gramática 7.6.5.2</span><span class="sxs-lookup"><span data-stu-id="f2d77-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="f2d77-183">As produções para *nome simples* (§ 7.6.3) e *acesso de membro* (§ 7.6.5) podem dar aumento às ambiguidades na gramática para expressões.</span><span class="sxs-lookup"><span data-stu-id="f2d77-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="f2d77-184">Por exemplo, a instrução:</span><span class="sxs-lookup"><span data-stu-id="f2d77-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="f2d77-185">pode ser interpretado como uma chamada para `F` com dois argumentos, `G < A` e `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="f2d77-186">Como alternativa, ele pode ser interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.</span><span class="sxs-lookup"><span data-stu-id="f2d77-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="f2d77-187">Se uma sequência de tokens puder ser analisada (no contexto) como um *nome simples* (§ 7.6.3), *acesso de membro* (§ 7.6.5) ou *acesso de membro de ponteiro* (§ 18.5.2) terminando com uma *lista de argumentos de tipo* (§ 4.4.1), o token imediatamente após o `>` token de fechamento é examinado.</span><span class="sxs-lookup"><span data-stu-id="f2d77-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="f2d77-188">Se for um de</span><span class="sxs-lookup"><span data-stu-id="f2d77-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="f2d77-189">em seguida, a *lista de argumentos de tipo* é mantida como parte do *nome simples*, *acesso de membro* ou ponteiro-acesso de *membro* e qualquer outra análise possível da sequência de tokens é descartada.</span><span class="sxs-lookup"><span data-stu-id="f2d77-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="f2d77-190">Caso contrário, *a lista de argumentos de tipo* não é considerada como parte do *nome simples*, *acesso de membro* ou > ponteiro- *acesso de membro*, mesmo se não houver outra análise possível da sequência de tokens.</span><span class="sxs-lookup"><span data-stu-id="f2d77-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="f2d77-191">Observe que essas regras não são aplicadas ao analisar uma *lista de argumentos de tipo* em um *namespace-ou-Type-name* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="f2d77-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="f2d77-192">A instrução</span><span class="sxs-lookup"><span data-stu-id="f2d77-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="f2d77-193">de acordo com essa regra, será interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.</span><span class="sxs-lookup"><span data-stu-id="f2d77-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="f2d77-194">As instruções</span><span class="sxs-lookup"><span data-stu-id="f2d77-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="f2d77-195">cada um será interpretado como uma chamada para `F` com dois argumentos.</span><span class="sxs-lookup"><span data-stu-id="f2d77-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="f2d77-196">A instrução</span><span class="sxs-lookup"><span data-stu-id="f2d77-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="f2d77-197">será interpretado como um operador menor que, maior que operador, e operador de adição unário, como se a instrução tivesse sido gravada `x = (F < A) > (+y)`, em vez de como um *nome simples* com uma *lista de argumentos de tipo* seguido por um operador Binary Plus.</span><span class="sxs-lookup"><span data-stu-id="f2d77-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="f2d77-198">Na instrução</span><span class="sxs-lookup"><span data-stu-id="f2d77-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="f2d77-199">os tokens `C<T>` são interpretados como um *namespace-ou-Type-Name* com uma *lista de argumentos de tipo*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="f2d77-200">Há várias alterações introduzidas no C# 7 que tornam essas regras de Desambigüidade não mais suficientes para lidar com a complexidade da linguagem.</span><span class="sxs-lookup"><span data-stu-id="f2d77-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="f2d77-201">Declarações de variável out</span><span class="sxs-lookup"><span data-stu-id="f2d77-201">Out variable declarations</span></span>

<span data-ttu-id="f2d77-202">Agora é possível declarar uma variável em um argumento out:</span><span class="sxs-lookup"><span data-stu-id="f2d77-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="f2d77-203">No entanto, o tipo pode ser genérico:</span><span class="sxs-lookup"><span data-stu-id="f2d77-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="f2d77-204">Como a gramática de idioma do argumento usa *expression*, esse contexto está sujeito à regra de desambiguidade.</span><span class="sxs-lookup"><span data-stu-id="f2d77-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="f2d77-205">Nesse caso, o `>` de fechamento é seguido por um *identificador*, que não é um dos tokens que permite que ele seja tratado como uma *lista de argumentos de tipo*.</span><span class="sxs-lookup"><span data-stu-id="f2d77-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="f2d77-206">Portanto, propondo para **Adicionar o *identificador* ao conjunto de tokens que dispara a Desambigüidade para uma lista de *argumentos de tipo*.**</span><span class="sxs-lookup"><span data-stu-id="f2d77-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="f2d77-207">Declarações de tuplas e de desconstrução</span><span class="sxs-lookup"><span data-stu-id="f2d77-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="f2d77-208">Um literal de tupla é executado exatamente com o mesmo problema.</span><span class="sxs-lookup"><span data-stu-id="f2d77-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="f2d77-209">Considere a expressão de tupla</span><span class="sxs-lookup"><span data-stu-id="f2d77-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="f2d77-210">Nas 6 regras C# antigas para analisar uma lista de argumentos, isso seria analisado como uma tupla com quatro elementos, começando com `A < B` como o primeiro.</span><span class="sxs-lookup"><span data-stu-id="f2d77-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="f2d77-211">No entanto, quando isso aparece à esquerda de uma desconstrução, queremos a desambiguidade disparada pelo token do *identificador* , conforme descrito acima:</span><span class="sxs-lookup"><span data-stu-id="f2d77-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="f2d77-212">Essa é uma declaração de desconstrução que declara duas variáveis, a primeira que é do tipo `A<B,C>` e nomeada `D`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="f2d77-213">Em outras palavras, o literal de tupla contém duas expressões, cada uma delas como uma expressão de declaração.</span><span class="sxs-lookup"><span data-stu-id="f2d77-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="f2d77-214">Para simplificar a especificação e o compilador, eu propondo que esse literal de tupla seja analisado como uma tupla de dois elementos onde quer que ele apareça (seja ou não exibido no lado esquerdo de uma atribuição).</span><span class="sxs-lookup"><span data-stu-id="f2d77-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="f2d77-215">Isso seria um resultado natural da Desambigüidade descrita na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="f2d77-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="f2d77-216">Correspondência de padrões</span><span class="sxs-lookup"><span data-stu-id="f2d77-216">Pattern-matching</span></span>

<span data-ttu-id="f2d77-217">A correspondência de padrões introduz um novo contexto em que surge a ambiguidade do tipo de expressão.</span><span class="sxs-lookup"><span data-stu-id="f2d77-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="f2d77-218">Anteriormente, o lado direito de um operador de `is` era um tipo.</span><span class="sxs-lookup"><span data-stu-id="f2d77-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="f2d77-219">Agora ele pode ser um tipo ou uma expressão e, se for um tipo, pode ser seguido por um identificador.</span><span class="sxs-lookup"><span data-stu-id="f2d77-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="f2d77-220">Isso pode, tecnicamente, alterar o significado do código existente:</span><span class="sxs-lookup"><span data-stu-id="f2d77-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="f2d77-221">Isso pode ser analisado sob as regras do C# 6 como</span><span class="sxs-lookup"><span data-stu-id="f2d77-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="f2d77-222">Mas, sob as regras do C# 7 (com a Inambiguidade proposta acima) seria analisado como</span><span class="sxs-lookup"><span data-stu-id="f2d77-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="f2d77-223">que declara uma variável `B` do tipo `T<A>`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="f2d77-224">Felizmente, os compiladores nativo e Roslyn têm um bug no qual eles fornecem um erro de sintaxe no código C# 6.</span><span class="sxs-lookup"><span data-stu-id="f2d77-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="f2d77-225">Portanto, essa alteração significativa em particular não é uma preocupação.</span><span class="sxs-lookup"><span data-stu-id="f2d77-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="f2d77-226">Correspondência de padrões introduz tokens adicionais que devem orientar a resolução de ambiguidade para selecionar um tipo.</span><span class="sxs-lookup"><span data-stu-id="f2d77-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="f2d77-227">Os seguintes exemplos de código C# 6 válido serão desfeitos sem regras de Desambigüidade adicionais:</span><span class="sxs-lookup"><span data-stu-id="f2d77-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="f2d77-228">Alteração proposta na regra de desambiguidade</span><span class="sxs-lookup"><span data-stu-id="f2d77-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="f2d77-229">Eu propondo para revisar a especificação para alterar a lista de tokens de ambiguidade de</span><span class="sxs-lookup"><span data-stu-id="f2d77-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="f2d77-230">até</span><span class="sxs-lookup"><span data-stu-id="f2d77-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="f2d77-231">E, em determinados contextos, tratamos o *identificador* como um token de ambiguidade.</span><span class="sxs-lookup"><span data-stu-id="f2d77-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="f2d77-232">Esses contextos são onde a sequência de tokens que estão sendo desambiguados é imediatamente precedida por uma das palavras-chave `is`, `case`ou `out`ou surge ao analisar o primeiro elemento de um literal de tupla (nesse caso, os tokens são precedidos por `(` ou `:` e o identificador é seguido por um `,`) ou um elemento subsequente de um literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="f2d77-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="f2d77-233">Regra de desambiguidade modificada</span><span class="sxs-lookup"><span data-stu-id="f2d77-233">Modified disambiguation rule</span></span>

<span data-ttu-id="f2d77-234">A regra de desambiguidade revisada seria algo assim</span><span class="sxs-lookup"><span data-stu-id="f2d77-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="f2d77-235">Se uma sequência de tokens puder ser analisada (no contexto) como um *nome simples* (§ 7.6.3), *acesso de membro* (§ 7.6.5) ou *acesso de membro de ponteiro* (§ 18.5.2) terminando com uma *lista de argumentos de tipo* (§ 4.4.1), o token imediatamente após o `>` token de fechamento é examinado, para ver se ele está</span><span class="sxs-lookup"><span data-stu-id="f2d77-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="f2d77-236">Um dos `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span><span class="sxs-lookup"><span data-stu-id="f2d77-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="f2d77-237">Um dos operadores relacionais `<  >  <=  >=  is as`; or</span><span class="sxs-lookup"><span data-stu-id="f2d77-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="f2d77-238">Uma palavra-chave de consulta contextual que aparece dentro de uma expressão de consulta; or</span><span class="sxs-lookup"><span data-stu-id="f2d77-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="f2d77-239">Em determinados contextos, tratamos o *identificador* como um token de ambiguidade.</span><span class="sxs-lookup"><span data-stu-id="f2d77-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="f2d77-240">Esses contextos são onde a sequência de tokens que estão sendo desambiguados é imediatamente precedida por uma das palavras-chave `is`, `case` ou `out`ou surge ao analisar o primeiro elemento de um literal de tupla (nesse caso, os tokens são precedidos por `(` ou `:` e o identificador é seguido por um `,`) ou um elemento subsequente de um literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="f2d77-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="f2d77-241">Se o seguinte token estiver entre essa lista ou um identificador nesse contexto, a lista de argumentos de *tipo* será mantida como parte do *nome simples*, *acesso de membro* ou *ponteiro-acesso de membro* e qualquer outra análise possível da sequência de tokens será descartada.</span><span class="sxs-lookup"><span data-stu-id="f2d77-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="f2d77-242">Caso contrário, *a lista de argumentos de tipo* não será considerada como parte do *nome simples*, *acesso de membro* ou acesso de *membro de ponteiro*, mesmo que não haja nenhuma outra análise possível da sequência de tokens.</span><span class="sxs-lookup"><span data-stu-id="f2d77-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="f2d77-243">Observe que essas regras não são aplicadas ao analisar uma *lista de argumentos de tipo* em um *namespace-ou-Type-name* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="f2d77-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="f2d77-244">Alterações recentes devido a esta proposta</span><span class="sxs-lookup"><span data-stu-id="f2d77-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="f2d77-245">Nenhuma alteração significativa é conhecida devido a essa regra de Desambigüidade proposta.</span><span class="sxs-lookup"><span data-stu-id="f2d77-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="f2d77-246">Exemplos interessantes</span><span class="sxs-lookup"><span data-stu-id="f2d77-246">Interesting examples</span></span>

<span data-ttu-id="f2d77-247">Aqui estão alguns resultados interessantes dessas regras de Desambigüidade:</span><span class="sxs-lookup"><span data-stu-id="f2d77-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="f2d77-248">A expressão `(A < B, C > D)` é uma tupla com dois elementos, cada uma delas.</span><span class="sxs-lookup"><span data-stu-id="f2d77-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="f2d77-249">A expressão `(A<B,C> D, E)` é uma tupla com dois elementos, a primeira é uma expressão de declaração.</span><span class="sxs-lookup"><span data-stu-id="f2d77-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="f2d77-250">O `M(A < B, C > D, E)` de invocação tem três argumentos.</span><span class="sxs-lookup"><span data-stu-id="f2d77-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="f2d77-251">O `M(out A<B,C> D, E)` de invocação tem dois argumentos, o primeiro deles é uma declaração de `out`.</span><span class="sxs-lookup"><span data-stu-id="f2d77-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="f2d77-252">A expressão `e is A<B> C` usa uma expressão de declaração.</span><span class="sxs-lookup"><span data-stu-id="f2d77-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="f2d77-253">O rótulo case `case A<B> C:` usa uma expressão de declaração.</span><span class="sxs-lookup"><span data-stu-id="f2d77-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="f2d77-254">Alguns exemplos de correspondência de padrões</span><span class="sxs-lookup"><span data-stu-id="f2d77-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="f2d77-255">É-como</span><span class="sxs-lookup"><span data-stu-id="f2d77-255">Is-As</span></span>

<span data-ttu-id="f2d77-256">Podemos substituir o idioma</span><span class="sxs-lookup"><span data-stu-id="f2d77-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="f2d77-257">Com um pouco mais conciso e direto</span><span class="sxs-lookup"><span data-stu-id="f2d77-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="f2d77-258">Testando anulável</span><span class="sxs-lookup"><span data-stu-id="f2d77-258">Testing nullable</span></span>

<span data-ttu-id="f2d77-259">Podemos substituir o idioma</span><span class="sxs-lookup"><span data-stu-id="f2d77-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="f2d77-260">Com um pouco mais conciso e direto</span><span class="sxs-lookup"><span data-stu-id="f2d77-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="f2d77-261">Simplificação aritmética</span><span class="sxs-lookup"><span data-stu-id="f2d77-261">Arithmetic simplification</span></span>

<span data-ttu-id="f2d77-262">Suponha que definimos um conjunto de tipos recursivos para representar expressões (por uma proposta separada):</span><span class="sxs-lookup"><span data-stu-id="f2d77-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="f2d77-263">Agora, podemos definir uma função para computar a derivada (não reduzida) de uma expressão:</span><span class="sxs-lookup"><span data-stu-id="f2d77-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="f2d77-264">Um simplificador de expressão demonstra padrões posicionais:</span><span class="sxs-lookup"><span data-stu-id="f2d77-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
