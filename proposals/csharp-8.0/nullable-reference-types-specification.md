---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485206"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="c62f3-101">Especificação de tipos de referência anulável</span><span class="sxs-lookup"><span data-stu-id="c62f3-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="c62f3-102">Este é um trabalho em andamento-várias partes estão ausentes ou incompletas.</span><span class="sxs-lookup"><span data-stu-id="c62f3-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="c62f3-103">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="c62f3-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="c62f3-104">Tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="c62f3-104">Nullable reference types</span></span>

<span data-ttu-id="c62f3-105">Os tipos de referência anuláveis têm a mesma sintaxe `T?` que a forma abreviada de tipos de valores anuláveis, mas não têm um formato longo correspondente.</span><span class="sxs-lookup"><span data-stu-id="c62f3-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="c62f3-106">Para os fins da especificação, a produção de `nullable_type` atual é renomeada para `nullable_value_type`e uma `nullable_reference_type` produção é adicionada:</span><span class="sxs-lookup"><span data-stu-id="c62f3-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="c62f3-107">O `non_nullable_reference_type` em uma `nullable_reference_type` deve ser um tipo de referência não anulável (classe, interface, delegado ou matriz) ou um parâmetro de tipo restrito a ser um tipo de referência não anulável (por meio da restrição de `class` ou uma classe diferente de `object`).</span><span class="sxs-lookup"><span data-stu-id="c62f3-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="c62f3-108">Tipos de referência anuláveis não podem ocorrer nas seguintes posições:</span><span class="sxs-lookup"><span data-stu-id="c62f3-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="c62f3-109">como uma classe base ou interface</span><span class="sxs-lookup"><span data-stu-id="c62f3-109">as a base class or interface</span></span>
- <span data-ttu-id="c62f3-110">como receptor de um `member_access`</span><span class="sxs-lookup"><span data-stu-id="c62f3-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="c62f3-111">como o `type` em um `object_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="c62f3-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="c62f3-112">como o `delegate_type` em um `delegate_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="c62f3-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="c62f3-113">como o `type` em um `is_expression`, um `catch_clause` ou um `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="c62f3-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="c62f3-114">como o `interface` em um nome de membro de interface totalmente qualificado</span><span class="sxs-lookup"><span data-stu-id="c62f3-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="c62f3-115">Um aviso é fornecido em um `nullable_reference_type` em que o contexto de anotação anulável está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="c62f3-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="c62f3-116">Restrição de classe anulável</span><span class="sxs-lookup"><span data-stu-id="c62f3-116">Nullable class constraint</span></span>

<span data-ttu-id="c62f3-117">A restrição `class` tem um equivalente anulável `class?`:</span><span class="sxs-lookup"><span data-stu-id="c62f3-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="c62f3-118">O operador NULL-tolerante</span><span class="sxs-lookup"><span data-stu-id="c62f3-118">The null-forgiving operator</span></span>

<span data-ttu-id="c62f3-119">O operador post-Fix `!` é chamado de operador NULL-tolerante.</span><span class="sxs-lookup"><span data-stu-id="c62f3-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="c62f3-120">O `primary_expression` deve ser de um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="c62f3-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="c62f3-121">O operador de `!` de sufixo não tem efeito de tempo de execução-ele é avaliado como o resultado da expressão subjacente.</span><span class="sxs-lookup"><span data-stu-id="c62f3-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="c62f3-122">Sua única função é alterar o estado nulo da expressão e limitar os avisos fornecidos em seu uso.</span><span class="sxs-lookup"><span data-stu-id="c62f3-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="c62f3-123">variáveis de local digitadas implicitamente de tipo nulo</span><span class="sxs-lookup"><span data-stu-id="c62f3-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="c62f3-124">`var` infere um tipo anotado para tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="c62f3-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="c62f3-125">Por exemplo, em `var s = "";` a `var` é inferida como `string?`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="c62f3-126">Diretivas de compilador anuláveis</span><span class="sxs-lookup"><span data-stu-id="c62f3-126">Nullable compiler directives</span></span>

<span data-ttu-id="c62f3-127">`#nullable` diretivas controlam os contextos de anotação e de aviso que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="c62f3-128">`#pragma warning` diretivas são expandidas para permitir a alteração do contexto de aviso anulável e para permitir que avisos individuais sejam habilitados em mesmo quando estão desabilitados por padrão:</span><span class="sxs-lookup"><span data-stu-id="c62f3-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="c62f3-129">Observe que a nova forma de `pragma_warning_body` usa `nullable_action`, não `warning_action`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="c62f3-130">Contextos que permitem valor nulo</span><span class="sxs-lookup"><span data-stu-id="c62f3-130">Nullable contexts</span></span>

<span data-ttu-id="c62f3-131">Cada linha de código-fonte tem um *contexto de anotação anulável* e um *contexto de aviso anulável*.</span><span class="sxs-lookup"><span data-stu-id="c62f3-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="c62f3-132">Eles controlam se as anotações anuláveis têm efeito e se são fornecidos avisos de nulidade.</span><span class="sxs-lookup"><span data-stu-id="c62f3-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="c62f3-133">O contexto de anotação de uma determinada linha está *desabilitado* ou *habilitado*.</span><span class="sxs-lookup"><span data-stu-id="c62f3-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="c62f3-134">O contexto de aviso de uma determinada linha está *desabilitado* ou *habilitado*.</span><span class="sxs-lookup"><span data-stu-id="c62f3-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="c62f3-135">Ambos os contextos podem ser especificados no nível do projeto (fora C# do código-fonte) ou em qualquer lugar dentro de um arquivo de origem por meio de diretivas de pré-processador `#nullable`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="c62f3-136">Se nenhuma configuração de nível de projeto for fornecida, o padrão será para os dois contextos a serem *desabilitados*.</span><span class="sxs-lookup"><span data-stu-id="c62f3-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="c62f3-137">A diretiva `#nullable` controla os contextos de anotação e de aviso dentro do texto de origem e tem precedência sobre as configurações de nível de projeto.</span><span class="sxs-lookup"><span data-stu-id="c62f3-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="c62f3-138">Uma diretiva define os contextos que ele controla para linhas de código subsequentes, até que outra diretiva a substitua ou até o final do arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="c62f3-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="c62f3-139">O efeito das diretivas é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c62f3-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="c62f3-140">`#nullable disable`: define a anotação anulável e contextos de aviso como *desabilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="c62f3-141">`#nullable enable`: define a anotação anulável e contextos de aviso como *habilitados*</span><span class="sxs-lookup"><span data-stu-id="c62f3-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="c62f3-142">`#nullable restore`: restaura os contextos de anotação e de aviso anuláveis para as configurações do projeto</span><span class="sxs-lookup"><span data-stu-id="c62f3-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="c62f3-143">`#nullable disable annotations`: define o contexto de anotação anulável como *desabilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="c62f3-144">`#nullable enable annotations`: define o contexto de anotação anulável como *habilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="c62f3-145">`#nullable restore annotations`: restaura o contexto de anotação anulável para as configurações do projeto</span><span class="sxs-lookup"><span data-stu-id="c62f3-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="c62f3-146">`#nullable disable warnings`: define o contexto de aviso anulável como *desabilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="c62f3-147">`#nullable enable warnings`: define o contexto de aviso anulável como *habilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="c62f3-148">`#nullable restore warnings`: restaura o contexto de aviso anulável para as configurações do projeto</span><span class="sxs-lookup"><span data-stu-id="c62f3-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="c62f3-149">Possibilidade de nulidade de tipos</span><span class="sxs-lookup"><span data-stu-id="c62f3-149">Nullability of types</span></span>

<span data-ttu-id="c62f3-150">Um determinado tipo pode ter um dos quatro nullabilities: *alheios*, *nonnullble*, *Nullable* e *Unknown*.</span><span class="sxs-lookup"><span data-stu-id="c62f3-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="c62f3-151">Tipos não *nulos* e *desconhecidos* podem causar avisos se um valor `null` potencial for atribuído a eles.</span><span class="sxs-lookup"><span data-stu-id="c62f3-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="c62f3-152">Os tipos *alheios* e *Nullable* , no entanto, são "*nulo-atribuíveis*" e podem ter `null` valores atribuídos a eles sem avisos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="c62f3-153">Tipos *alheios* e *nonnull* podem ser desreferenciados ou atribuídos sem avisos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="c62f3-154">Os valores de tipos *anuláveis* e *desconhecidos* , no entanto, são "*nulos*" e podem causar avisos quando desreferenciados ou atribuídos sem a verificação nula adequada.</span><span class="sxs-lookup"><span data-stu-id="c62f3-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="c62f3-155">O *estado nulo padrão* de um tipo de rendimento nulo é "Talvez nulo".</span><span class="sxs-lookup"><span data-stu-id="c62f3-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="c62f3-156">O estado nulo padrão de um tipo não nulo de rendimento é "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="c62f3-157">O tipo e o contexto de anotação anulável que ele ocorre em determinar sua nulidade:</span><span class="sxs-lookup"><span data-stu-id="c62f3-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="c62f3-158">Um tipo de valor não nulo `S` é sempre não *nulo*</span><span class="sxs-lookup"><span data-stu-id="c62f3-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="c62f3-159">Um tipo de valor anulável `S?` é sempre *anulável*</span><span class="sxs-lookup"><span data-stu-id="c62f3-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="c62f3-160">Um tipo de referência não anotado `C` em um contexto de anotação *desabilitado* é *alheios*</span><span class="sxs-lookup"><span data-stu-id="c62f3-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="c62f3-161">Um tipo de referência não anotado `C` em um contexto de anotação *habilitado* não é *nulo*</span><span class="sxs-lookup"><span data-stu-id="c62f3-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="c62f3-162">Um tipo de referência anulável `C?` é *anulável* (mas um aviso pode ser produzido em um contexto de anotação *desabilitado* )</span><span class="sxs-lookup"><span data-stu-id="c62f3-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="c62f3-163">Além disso, os parâmetros de tipo levam suas restrições em conta:</span><span class="sxs-lookup"><span data-stu-id="c62f3-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="c62f3-164">Um parâmetro de tipo `T` em que todas as restrições (se houver) são tipos de rendimento nulo (*anuláveis* e *desconhecidos*) ou a restrição de `class?` é *desconhecida*</span><span class="sxs-lookup"><span data-stu-id="c62f3-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="c62f3-165">Um parâmetro de tipo `T` em que pelo menos uma restrição é *alheios* ou não *nulo* ou uma das restrições `struct` ou `class` é</span><span class="sxs-lookup"><span data-stu-id="c62f3-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="c62f3-166">*alheios* em um contexto de anotação *desabilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="c62f3-167">Não *nulo* em um contexto de anotação *habilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="c62f3-168">Um parâmetro de tipo anulável `T?` em que pelo menos uma das restrições de `T`é *alheios* ou não *nulo* , ou uma das restrições `struct` ou `class`, é</span><span class="sxs-lookup"><span data-stu-id="c62f3-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="c62f3-169">*permite valor nulo* em um contexto de anotação *desabilitado* (mas um aviso é produzido)</span><span class="sxs-lookup"><span data-stu-id="c62f3-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="c62f3-170">*permite valor nulo* em um contexto de anotação *habilitado*</span><span class="sxs-lookup"><span data-stu-id="c62f3-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="c62f3-171">Para um parâmetro de tipo `T`, `T?` só será permitido se `T` for conhecido como um tipo de valor ou se for conhecido como um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="c62f3-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="c62f3-172">Alheios vs não nulo</span><span class="sxs-lookup"><span data-stu-id="c62f3-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="c62f3-173">Um `type` é considerado em um determinado contexto de anotação quando o último token do tipo está dentro desse contexto.</span><span class="sxs-lookup"><span data-stu-id="c62f3-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="c62f3-174">Se um determinado tipo de referência `C` no código-fonte é interpretado como alheios ou não nulo depende do contexto de anotação desse código-fonte.</span><span class="sxs-lookup"><span data-stu-id="c62f3-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="c62f3-175">Mas, uma vez estabelecida, ele é considerado parte desse tipo e "viaja com ele", por exemplo, durante a substituição de argumentos de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="c62f3-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="c62f3-176">É como se houver uma anotação como `?` no tipo, mas invisível.</span><span class="sxs-lookup"><span data-stu-id="c62f3-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="c62f3-177">Restrições</span><span class="sxs-lookup"><span data-stu-id="c62f3-177">Constraints</span></span>

<span data-ttu-id="c62f3-178">Tipos de referência anuláveis podem ser usados como restrições genéricas.</span><span class="sxs-lookup"><span data-stu-id="c62f3-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="c62f3-179">Além disso, `object` agora é válido como uma restrição explícita.</span><span class="sxs-lookup"><span data-stu-id="c62f3-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="c62f3-180">A ausência de uma restrição agora é equivalente a uma restrição de `object?` (em vez de `object`), mas (ao contrário de `object` antes) `object?` não é proibida como uma restrição explícita.</span><span class="sxs-lookup"><span data-stu-id="c62f3-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="c62f3-181">`class?` é uma nova restrição que indica "tipo de referência possivelmente anulável", enquanto `class` denota "tipo de referência não nula".</span><span class="sxs-lookup"><span data-stu-id="c62f3-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="c62f3-182">A nulidade de um argumento de tipo ou de uma restrição não afeta se o tipo satisfaz a restrição, exceto onde esse já é o caso hoje (os tipos de valores anuláveis não satisfazem a restrição de `struct`).</span><span class="sxs-lookup"><span data-stu-id="c62f3-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="c62f3-183">No entanto, se o argumento de tipo não atender aos requisitos de nulidade da restrição, um aviso poderá ser dado.</span><span class="sxs-lookup"><span data-stu-id="c62f3-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="c62f3-184">Estado nulo e acompanhamento nulo</span><span class="sxs-lookup"><span data-stu-id="c62f3-184">Null state and null tracking</span></span>

<span data-ttu-id="c62f3-185">Cada expressão em um local de origem específico tem um *estado nulo*, que indica se acredita potencialmente ser possível avaliar como NULL.</span><span class="sxs-lookup"><span data-stu-id="c62f3-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="c62f3-186">O estado nulo é "NOT NULL" ou "Talvez NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="c62f3-187">O estado NULL é usado para determinar se um aviso deve ser fornecido sobre conversões e desreferências sem segurança nula.</span><span class="sxs-lookup"><span data-stu-id="c62f3-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="c62f3-188">Acompanhamento nulo para variáveis</span><span class="sxs-lookup"><span data-stu-id="c62f3-188">Null tracking for variables</span></span>

<span data-ttu-id="c62f3-189">Para determinadas expressões que denotam variáveis ou propriedades, o estado NULL é rastreado entre ocorrências, com base nas atribuições para elas, nos testes executados neles e no fluxo de controle entre elas.</span><span class="sxs-lookup"><span data-stu-id="c62f3-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="c62f3-190">Isso é semelhante a como a atribuição definitiva é controlada para variáveis.</span><span class="sxs-lookup"><span data-stu-id="c62f3-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="c62f3-191">As expressões rastreadas são aquelas do seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="c62f3-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="c62f3-192">Onde os identificadores denotam campos ou propriedades.</span><span class="sxs-lookup"><span data-stu-id="c62f3-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="c62f3-193">***Descrever as transições de estado nulos semelhantes à atribuição definitiva***</span><span class="sxs-lookup"><span data-stu-id="c62f3-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="c62f3-194">Estado nulo para expressões</span><span class="sxs-lookup"><span data-stu-id="c62f3-194">Null state for expressions</span></span>

<span data-ttu-id="c62f3-195">O estado nulo de uma expressão é derivado de seu formulário e tipo e do estado nulo das variáveis envolvidas.</span><span class="sxs-lookup"><span data-stu-id="c62f3-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="c62f3-196">{1&gt;Literais&lt;1}</span><span class="sxs-lookup"><span data-stu-id="c62f3-196">Literals</span></span>

<span data-ttu-id="c62f3-197">O estado nulo de um literal de `null` é "Talvez nulo".</span><span class="sxs-lookup"><span data-stu-id="c62f3-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="c62f3-198">O estado nulo de um literal `default` que está sendo convertido em um tipo que é conhecido não como um tipo de valor não nulo é "Talvez nulo".</span><span class="sxs-lookup"><span data-stu-id="c62f3-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="c62f3-199">O estado nulo de qualquer outro literal é "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="c62f3-200">Nomes simples</span><span class="sxs-lookup"><span data-stu-id="c62f3-200">Simple names</span></span>

<span data-ttu-id="c62f3-201">Se uma `simple_name` não for classificada como um valor, seu estado nulo será "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="c62f3-202">Caso contrário, é uma expressão rastreada e seu estado nulo é seu estado nulo rastreado neste local de origem.</span><span class="sxs-lookup"><span data-stu-id="c62f3-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="c62f3-203">Acesso de membros</span><span class="sxs-lookup"><span data-stu-id="c62f3-203">Member access</span></span>

<span data-ttu-id="c62f3-204">Se uma `member_access` não for classificada como um valor, seu estado nulo será "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="c62f3-205">Caso contrário, se for uma expressão rastreada, seu estado nulo será seu estado NULL rastreado nesse local de origem.</span><span class="sxs-lookup"><span data-stu-id="c62f3-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="c62f3-206">Caso contrário, seu estado nulo será o estado nulo padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="c62f3-207">Expressões de invocação</span><span class="sxs-lookup"><span data-stu-id="c62f3-207">Invocation expressions</span></span>

<span data-ttu-id="c62f3-208">Se um `invocation_expression` invocar um membro declarado com um ou mais atributos para um comportamento nulo especial, o estado NULL será determinado por esses atributos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="c62f3-209">Caso contrário, o estado nulo da expressão será o estado nulo padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="c62f3-210">Acesso ao elemento</span><span class="sxs-lookup"><span data-stu-id="c62f3-210">Element access</span></span>

<span data-ttu-id="c62f3-211">Se uma `element_access` invocar um indexador declarado com um ou mais atributos para um comportamento nulo especial, o estado NULL será determinado por esses atributos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="c62f3-212">Caso contrário, o estado nulo da expressão será o estado nulo padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="c62f3-213">Acesso de base</span><span class="sxs-lookup"><span data-stu-id="c62f3-213">Base access</span></span>

<span data-ttu-id="c62f3-214">Se `B` denota o tipo base do tipo delimitador, `base.I` tem o mesmo estado nulo que `((B)this).I` e `base[E]` tem o mesmo estado nulo que `((B)this)[E]`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="c62f3-215">Expressões padrão</span><span class="sxs-lookup"><span data-stu-id="c62f3-215">Default expressions</span></span>

<span data-ttu-id="c62f3-216">`default(T)` tem o estado nulo "não nulo" se `T` é conhecido como um tipo de valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="c62f3-217">Caso contrário, ele tem o estado nulo "Talvez nulo".</span><span class="sxs-lookup"><span data-stu-id="c62f3-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="c62f3-218">Expressões condicionais nulas</span><span class="sxs-lookup"><span data-stu-id="c62f3-218">Null-conditional expressions</span></span>

<span data-ttu-id="c62f3-219">Um `null_conditional_expression` tem o estado nulo "Talvez nulo".</span><span class="sxs-lookup"><span data-stu-id="c62f3-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="c62f3-220">Expressões de conversão</span><span class="sxs-lookup"><span data-stu-id="c62f3-220">Cast expressions</span></span>

<span data-ttu-id="c62f3-221">Se uma expressão de conversão `(T)E` invocar uma conversão definida pelo usuário, o estado nulo da expressão será o estado nulo padrão para seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="c62f3-222">Caso contrário, se `T` for nula, produzindo (*anulável* ou *desconhecido*), o estado NULL será "Talvez NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="c62f3-223">Caso contrário, o estado nulo será o mesmo que o estado nulo de `E`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="c62f3-224">Expressões Await</span><span class="sxs-lookup"><span data-stu-id="c62f3-224">Await expressions</span></span>

<span data-ttu-id="c62f3-225">O estado nulo de `await E` é o estado nulo padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="c62f3-226">O operador de `as`</span><span class="sxs-lookup"><span data-stu-id="c62f3-226">The `as` operator</span></span>

<span data-ttu-id="c62f3-227">Uma expressão de `as` tem o estado nulo "Talvez NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="c62f3-228">O operador de União nula</span><span class="sxs-lookup"><span data-stu-id="c62f3-228">The null-coalescing operator</span></span>

<span data-ttu-id="c62f3-229">`E1 ?? E2` tem o mesmo estado nulo que `E2`</span><span class="sxs-lookup"><span data-stu-id="c62f3-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="c62f3-230">O operador condicional</span><span class="sxs-lookup"><span data-stu-id="c62f3-230">The conditional operator</span></span>

<span data-ttu-id="c62f3-231">O estado nulo de `E1 ? E2 : E3` será "NOT NULL" se o estado nulo de `E2` e `E3` forem "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="c62f3-232">Caso contrário, será "Talvez NULL".</span><span class="sxs-lookup"><span data-stu-id="c62f3-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="c62f3-233">Expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="c62f3-233">Query expressions</span></span>

<span data-ttu-id="c62f3-234">O estado nulo de uma expressão de consulta é o estado nulo padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="c62f3-235">Operadores de atribuição</span><span class="sxs-lookup"><span data-stu-id="c62f3-235">Assignment operators</span></span>

<span data-ttu-id="c62f3-236">`E1 = E2` e `E1 op= E2` têm o mesmo estado nulo que `E2` depois que qualquer conversões implícita tiver sido aplicada.</span><span class="sxs-lookup"><span data-stu-id="c62f3-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="c62f3-237">Operadores unários e binários</span><span class="sxs-lookup"><span data-stu-id="c62f3-237">Unary and binary operators</span></span>

<span data-ttu-id="c62f3-238">Se um operador unário ou binário invocar um operador definido pelo usuário que é declarado com um ou mais atributos para um comportamento nulo especial, o estado NULL será determinado por esses atributos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="c62f3-239">Caso contrário, o estado nulo da expressão será o estado nulo padrão de seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="c62f3-240">***Algo especial a fazer para `+` binários sobre cadeias de caracteres e delegados?***</span><span class="sxs-lookup"><span data-stu-id="c62f3-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="c62f3-241">Expressões que propagam o estado nulo</span><span class="sxs-lookup"><span data-stu-id="c62f3-241">Expressions that propagate null state</span></span>

<span data-ttu-id="c62f3-242">`(E)`, `checked(E)` e `unchecked(E)` têm o mesmo estado nulo que `E`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="c62f3-243">Expressões que nunca são nulas</span><span class="sxs-lookup"><span data-stu-id="c62f3-243">Expressions that are never null</span></span>

<span data-ttu-id="c62f3-244">O estado nulo dos formulários de expressão a seguir é sempre "NOT NULL":</span><span class="sxs-lookup"><span data-stu-id="c62f3-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="c62f3-245">`this` acesso</span><span class="sxs-lookup"><span data-stu-id="c62f3-245">`this` access</span></span>
- <span data-ttu-id="c62f3-246">cadeias de caracteres interpoladas</span><span class="sxs-lookup"><span data-stu-id="c62f3-246">interpolated strings</span></span>
- <span data-ttu-id="c62f3-247">expressões de `new` (objeto, delegado, objeto anônimo e expressões de criação de matriz)</span><span class="sxs-lookup"><span data-stu-id="c62f3-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="c62f3-248">Expressões `typeof`</span><span class="sxs-lookup"><span data-stu-id="c62f3-248">`typeof` expressions</span></span>
- <span data-ttu-id="c62f3-249">Expressões `nameof`</span><span class="sxs-lookup"><span data-stu-id="c62f3-249">`nameof` expressions</span></span>
- <span data-ttu-id="c62f3-250">funções anônimas (métodos anônimos e expressões lambda)</span><span class="sxs-lookup"><span data-stu-id="c62f3-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="c62f3-251">expressões tolerante nulas</span><span class="sxs-lookup"><span data-stu-id="c62f3-251">null-forgiving expressions</span></span>
- <span data-ttu-id="c62f3-252">Expressões `is`</span><span class="sxs-lookup"><span data-stu-id="c62f3-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="c62f3-253">Inferência de tipos</span><span class="sxs-lookup"><span data-stu-id="c62f3-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="c62f3-254">Inferência de tipos para `var`</span><span class="sxs-lookup"><span data-stu-id="c62f3-254">Type inference for `var`</span></span>

<span data-ttu-id="c62f3-255">O tipo inferido para variáveis locais declaradas com `var` é informado pelo Estado nulo da expressão de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c62f3-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="c62f3-256">Se o tipo de `E` for um tipo de referência anulável `C?` e o estado nulo de `E` for "NOT NULL", o tipo inferido para `x` será `C`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="c62f3-257">Caso contrário, o tipo inferido é o tipo de `E`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="c62f3-258">A nulidade do tipo inferido para `x` é determinada conforme descrito acima, com base no contexto de anotação do `var`, assim como se o tipo tivesse sido fornecido explicitamente nessa posição.</span><span class="sxs-lookup"><span data-stu-id="c62f3-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="c62f3-259">Inferência de tipos para `var?`</span><span class="sxs-lookup"><span data-stu-id="c62f3-259">Type inference for `var?`</span></span>

<span data-ttu-id="c62f3-260">O tipo inferido para variáveis locais declaradas com `var?` é independente do estado nulo da expressão de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c62f3-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="c62f3-261">Se o tipo `T` de `E` for um tipo de valor anulável ou um tipo de referência anulável, o tipo inferido para `x` será `T`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="c62f3-262">Caso contrário, se `T` for um tipo de valor não nulo `S` o tipo inferido será `S?`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="c62f3-263">Caso contrário, se `T` for um tipo de referência não nulo `C` o tipo inferido será `C?`.</span><span class="sxs-lookup"><span data-stu-id="c62f3-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="c62f3-264">Caso contrário, a declaração é inválida.</span><span class="sxs-lookup"><span data-stu-id="c62f3-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="c62f3-265">A nulidade do tipo inferido para `x` é sempre *anulável*.</span><span class="sxs-lookup"><span data-stu-id="c62f3-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="c62f3-266">Inferência de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="c62f3-266">Generic type inference</span></span>

<span data-ttu-id="c62f3-267">A inferência de tipo genérico é aprimorada para ajudar a decidir se os tipos de referência inferidos devem ser anuláveis ou não.</span><span class="sxs-lookup"><span data-stu-id="c62f3-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="c62f3-268">Esse é um melhor esforço e não em si gera avisos, mas pode levar a avisos anuláveis quando os tipos inferidos da sobrecarga selecionada são aplicados aos argumentos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="c62f3-269">A inferência de tipos não depende do contexto de anotação dos tipos de entrada.</span><span class="sxs-lookup"><span data-stu-id="c62f3-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="c62f3-270">Em vez disso, um `type` é inferido que adquire seu próprio contexto de anotação de onde "teria sido" se ele tivesse sido expresso explicitamente.</span><span class="sxs-lookup"><span data-stu-id="c62f3-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="c62f3-271">Isso sublinha a função da inferência de tipos como uma conveniência para o que você poderia ter escrito por conta própria.</span><span class="sxs-lookup"><span data-stu-id="c62f3-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="c62f3-272">Mais precisamente, o contexto de anotação para um argumento de tipo inferido é o contexto do token que teria sido seguido pela lista de parâmetros de tipo `<...>`, tinha um; ou seja, o nome do método genérico que está sendo chamado.</span><span class="sxs-lookup"><span data-stu-id="c62f3-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="c62f3-273">Para expressões de consulta que são traduzidas para essas chamadas, o contexto é obtido da palavra-chave contextual inicial da cláusula de consulta da qual a chamada é gerada.</span><span class="sxs-lookup"><span data-stu-id="c62f3-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="c62f3-274">A primeira fase</span><span class="sxs-lookup"><span data-stu-id="c62f3-274">The first phase</span></span>

<span data-ttu-id="c62f3-275">Os tipos de referência anuláveis fluem para os limites das expressões iniciais, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="c62f3-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="c62f3-276">Além disso, dois novos tipos de limites, ou seja, `null` e `default` são introduzidos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="c62f3-277">Sua finalidade é realizar ocorrências de `null` ou `default` nas expressões de entrada, o que pode fazer com que um tipo inferido seja anulável, mesmo quando não fosse.</span><span class="sxs-lookup"><span data-stu-id="c62f3-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="c62f3-278">Isso funciona mesmo para tipos de *valores* anuláveis, que são aprimorados para pegar a "nulidade" no processo de inferência.</span><span class="sxs-lookup"><span data-stu-id="c62f3-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="c62f3-279">A determinação dos limites a serem adicionados na primeira fase é aprimorada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c62f3-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="c62f3-280">Se um argumento `Ei` tiver um tipo de referência, o tipo `U` usado para a inferência dependerá do estado nulo de `Ei`, bem como seu tipo declarado:</span><span class="sxs-lookup"><span data-stu-id="c62f3-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="c62f3-281">Se o tipo declarado for um tipo de referência não nulo `U0` ou um tipo de referência anulável `U0?`</span><span class="sxs-lookup"><span data-stu-id="c62f3-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="c62f3-282">Se o estado nulo de `Ei` for "NOT NULL", `U` será `U0`</span><span class="sxs-lookup"><span data-stu-id="c62f3-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="c62f3-283">Se o estado nulo de `Ei` for "Talvez nulo", `U` será `U0?`</span><span class="sxs-lookup"><span data-stu-id="c62f3-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="c62f3-284">Caso contrário, se `Ei` tiver um tipo declarado, `U` será esse tipo</span><span class="sxs-lookup"><span data-stu-id="c62f3-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="c62f3-285">Caso contrário, se `Ei` for `null`, `U` será o limite especial `null`</span><span class="sxs-lookup"><span data-stu-id="c62f3-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="c62f3-286">Caso contrário, se `Ei` for `default`, `U` será o limite especial `default`</span><span class="sxs-lookup"><span data-stu-id="c62f3-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="c62f3-287">Caso contrário, nenhuma inferência será feita.</span><span class="sxs-lookup"><span data-stu-id="c62f3-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="c62f3-288">Inferências exatas, de limite superior e de limite inferior</span><span class="sxs-lookup"><span data-stu-id="c62f3-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="c62f3-289">Em inferências *do* tipo `U` *ao* tipo `V`, se `V` for um tipo de referência anulável `V0?`, `V0` será usado em vez de `V` nas cláusulas a seguir.</span><span class="sxs-lookup"><span data-stu-id="c62f3-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="c62f3-290">Se `V` for uma das variáveis de tipo não fixas, `U` será adicionado como um limite exato, superior ou inferior como antes</span><span class="sxs-lookup"><span data-stu-id="c62f3-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="c62f3-291">Caso contrário, se `U` for `null` ou `default`, nenhuma inferência será feita</span><span class="sxs-lookup"><span data-stu-id="c62f3-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="c62f3-292">Caso contrário, se `U` for um tipo de referência anulável `U0?`, `U0` será usado em vez de `U` nas cláusulas subsequentes.</span><span class="sxs-lookup"><span data-stu-id="c62f3-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="c62f3-293">A essência é que a nulidade que se refere diretamente a uma das variáveis de tipo não fixos é preservada em seus limites.</span><span class="sxs-lookup"><span data-stu-id="c62f3-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="c62f3-294">Por outro lado, para as inferências que recursivamente os tipos de origem e destino, a nulidade é ignorada.</span><span class="sxs-lookup"><span data-stu-id="c62f3-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="c62f3-295">Pode ou não corresponder, mas se não for, um aviso será emitido mais tarde se a sobrecarga for escolhida e aplicada.</span><span class="sxs-lookup"><span data-stu-id="c62f3-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="c62f3-296">Resolver</span><span class="sxs-lookup"><span data-stu-id="c62f3-296">Fixing</span></span>

<span data-ttu-id="c62f3-297">Atualmente, a especificação não faz um bom trabalho descrevendo o que acontece quando vários limites são conversíveis de identidade entre si, mas são diferentes.</span><span class="sxs-lookup"><span data-stu-id="c62f3-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="c62f3-298">Isso pode ocorrer entre `object` e `dynamic`, entre os tipos de tupla que diferem apenas em nomes de elementos, entre os tipos construídos e agora também entre `C` e `C?` para tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="c62f3-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="c62f3-299">Além disso, precisamos propagar "nulidade" das expressões de entrada para o tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="c62f3-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="c62f3-300">Para lidar com isso, adicionamos mais fases para corrigir, que agora é:</span><span class="sxs-lookup"><span data-stu-id="c62f3-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="c62f3-301">Reunir todos os tipos em todos os limites como candidatos, removendo `?` de todos os que são tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="c62f3-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="c62f3-302">Elimine os candidatos com base nos requisitos de limites exatos, inferiores e superiores (mantendo os limites de `null` e `default`)</span><span class="sxs-lookup"><span data-stu-id="c62f3-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="c62f3-303">Elimine os candidatos que não têm uma conversão implícita para todos os outros candidatos</span><span class="sxs-lookup"><span data-stu-id="c62f3-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="c62f3-304">Se todos os candidatos restantes não tiverem conversões de identidade entre si, a inferência de tipos falhará</span><span class="sxs-lookup"><span data-stu-id="c62f3-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="c62f3-305">*Mescle* os candidatos restantes conforme descrito abaixo</span><span class="sxs-lookup"><span data-stu-id="c62f3-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="c62f3-306">Se o candidato resultante for um tipo de referência ou um tipo de valor não nulo e *todos* os limites exatos ou *qualquer* um dos limites inferiores forem tipos de valores anuláveis, tipos de referência anuláveis, `null` ou `default`, `?` será adicionado ao candidato resultante, tornando-o um tipo de valor anulável ou tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="c62f3-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="c62f3-307">A *mesclagem* é descrita entre dois tipos candidatos.</span><span class="sxs-lookup"><span data-stu-id="c62f3-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="c62f3-308">Ele é transitivo e comutador, portanto, os candidatos podem ser mesclados em qualquer ordem com o mesmo resultado final.</span><span class="sxs-lookup"><span data-stu-id="c62f3-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="c62f3-309">Ele será indefinido se os dois tipos candidatos não forem conversíveis de identidade entre si.</span><span class="sxs-lookup"><span data-stu-id="c62f3-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="c62f3-310">A função *Merge* usa dois tipos candidatos e uma direção ( *+* ou *-* ):</span><span class="sxs-lookup"><span data-stu-id="c62f3-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="c62f3-311">*Merge*(`T`, `T`, *d*) = t</span><span class="sxs-lookup"><span data-stu-id="c62f3-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="c62f3-312">*Merge*(`S`, `T?`, *+* ) = *mesclagem*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="c62f3-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="c62f3-313">*Merge*(`S`, `T?`, *-* ) = *mesclagem*(`S?`, `T`, *-* ) = *Merge*(`S`, `T`, *-* )</span><span class="sxs-lookup"><span data-stu-id="c62f3-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="c62f3-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*mesclagem*(`S1`, `T1`, *D1*)`,...,`*mesclagem*(`Sn`, `Tn`, *DN*)`>`, *em que*</span><span class="sxs-lookup"><span data-stu-id="c62f3-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="c62f3-315">`di` =  *+* se o parâmetro do `i`' th type de `C<...>` for Covariance</span><span class="sxs-lookup"><span data-stu-id="c62f3-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="c62f3-316">`di` =  *-* se o parâmetro do `i`' th type de `C<...>` for Compensate ou inconstante</span><span class="sxs-lookup"><span data-stu-id="c62f3-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="c62f3-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*mesclagem*(`S1`, `T1`, *D1*)`,...,`*mesclagem*(`Sn`, `Tn`, *DN*)`>`, *em que*</span><span class="sxs-lookup"><span data-stu-id="c62f3-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="c62f3-318">`di` =  *-* se o parâmetro do `i`' th type de `C<...>` for Covariance</span><span class="sxs-lookup"><span data-stu-id="c62f3-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="c62f3-319">`di` =  *+* se o parâmetro do `i`' th type de `C<...>` for Compensate ou inconstante</span><span class="sxs-lookup"><span data-stu-id="c62f3-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="c62f3-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*mesclar*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *em que*</span><span class="sxs-lookup"><span data-stu-id="c62f3-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="c62f3-321">`ni` estiver ausente se `si` e `ti` forem diferentes ou se ambos estiverem ausentes</span><span class="sxs-lookup"><span data-stu-id="c62f3-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="c62f3-322">`ni` `si` se `si` e `ti` forem iguais</span><span class="sxs-lookup"><span data-stu-id="c62f3-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="c62f3-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="c62f3-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="c62f3-324">Warnings</span><span class="sxs-lookup"><span data-stu-id="c62f3-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="c62f3-325">Possível atribuição nula</span><span class="sxs-lookup"><span data-stu-id="c62f3-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="c62f3-326">Desreferência de nulo potencial</span><span class="sxs-lookup"><span data-stu-id="c62f3-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="c62f3-327">Incompatibilidade de nulidade de restrição</span><span class="sxs-lookup"><span data-stu-id="c62f3-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="c62f3-328">Tipos anuláveis no contexto de anotação desabilitado</span><span class="sxs-lookup"><span data-stu-id="c62f3-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="c62f3-329">Atributos para comportamento nulo especial</span><span class="sxs-lookup"><span data-stu-id="c62f3-329">Attributes for special null behavior</span></span>


