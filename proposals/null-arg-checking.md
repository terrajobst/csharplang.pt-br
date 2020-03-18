---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484548"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="a3c0e-101">Verificação de argumento nulo simplificado</span><span class="sxs-lookup"><span data-stu-id="a3c0e-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="a3c0e-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="a3c0e-102">Summary</span></span>
<span data-ttu-id="a3c0e-103">Esta proposta fornece uma sintaxe simplificada para validar argumentos de método não é `null` e gerar `ArgumentNullException` adequadamente.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="a3c0e-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="a3c0e-104">Motivation</span></span>
<span data-ttu-id="a3c0e-105">O trabalho de design de tipos de referência anuláveis nos fez examinar o código necessário para `null` validação de argumento.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="a3c0e-106">Considerando que o NRT não afeta a execução de código, os desenvolvedores ainda devem adicionar `if (arg is null) throw` código de placa mais para os projetos que são totalmente `null` limpos.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="a3c0e-107">Isso nos forneceu o desejo de explorar uma sintaxe mínima para o argumento `null` validação no idioma.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="a3c0e-108">Embora seja esperado que essa `null` sintaxe de validação de parâmetro seja emparelhada com frequência com NRT, a proposta é totalmente independente dela.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="a3c0e-109">A sintaxe pode ser usada independentemente das diretivas de `#nullable`.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a3c0e-110">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="a3c0e-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="a3c0e-111">Sintaxe de parâmetro de validação nula</span><span class="sxs-lookup"><span data-stu-id="a3c0e-111">Null validation parameter syntax</span></span>
<span data-ttu-id="a3c0e-112">O operador Bang, `!`, pode ser posicionado após um nome de parâmetro em uma lista de parâmetros e isso C# fará com que o compilador emita o código de verificação de `null` padrão para esse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="a3c0e-113">Isso é conhecido como `null` sintaxe de parâmetro de validação.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="a3c0e-114">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="a3c0e-115">Será convertido em:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="a3c0e-116">A verificação `null` gerada ocorrerá antes de qualquer código de desenvolvedor criado no método.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="a3c0e-117">Quando vários parâmetros contiverem o operador `!`, as verificações ocorrerão na mesma ordem em que os parâmetros são declarados.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="a3c0e-118">A verificação será especificamente para a igualdade de referência para `null`, ela não invoca `==` nem quaisquer operadores definidos pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="a3c0e-119">Isso também significa que o operador de `!` só pode ser adicionado a parâmetros cujo tipo pode ser testado para igualdade em relação a `null`.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="a3c0e-120">Isso significa que ele não pode ser usado em um parâmetro cujo tipo é conhecido como um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="a3c0e-121">No caso de um construtor, a validação de `null` ocorrerá antes de qualquer outro código no construtor.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="a3c0e-122">Isso inclui:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-122">That includes:</span></span> 

- <span data-ttu-id="a3c0e-123">Encadeamento com outros construtores com `this` ou `base`</span><span class="sxs-lookup"><span data-stu-id="a3c0e-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="a3c0e-124">Inicializadores de campo que ocorrem implicitamente no Construtor</span><span class="sxs-lookup"><span data-stu-id="a3c0e-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="a3c0e-125">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="a3c0e-126">Será traduzido aproximadamente para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="a3c0e-127">Observação: esse não é um C# código legal, mas sim apenas uma aproximação do que a implementação faz.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="a3c0e-128">A sintaxe do parâmetro de validação `null` também será válida em listas de parâmetros lambda.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="a3c0e-129">Isso é válido mesmo na sintaxe de parâmetro único que não tem parênteses.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="a3c0e-130">A sintaxe também é válida em parâmetros para métodos iteradores.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="a3c0e-131">Ao contrário de outro código no iterador, a validação de `null` ocorrerá quando o método do iterador for invocado, não quando o enumerador subjacente for movimentado.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="a3c0e-132">Isso é verdadeiro para iteradores tradicionais ou `async`s.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="a3c0e-133">O operador `!` só pode ser usado para listas de parâmetros que têm um corpo de método associado.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="a3c0e-134">Isso significa que ele não pode ser usado em um método `abstract`, `interface`, `delegate` ou `partial` definição de método.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="a3c0e-135">A extensão é nula</span><span class="sxs-lookup"><span data-stu-id="a3c0e-135">Extending is null</span></span>
<span data-ttu-id="a3c0e-136">Os tipos para os quais a expressão `is null` é válida serão estendidos para incluir parâmetros de tipo irrestrito.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="a3c0e-137">Isso permitirá que ele preencha a intenção de verificar `null` em todos os tipos que uma verificação de `null` é válida.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="a3c0e-138">Especificamente, esses são tipos que não são definitivamente conhecidos como tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="a3c0e-139">Por exemplo, parâmetros de tipo que são restritos a `struct` não podem ser usados com essa sintaxe.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="a3c0e-140">O comportamento de `is null` em um parâmetro de tipo será o mesmo que `== null` hoje.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="a3c0e-141">Nos casos em que o parâmetro de tipo é instanciado como um tipo de valor, o código será avaliado como `false`.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="a3c0e-142">Para casos em que é um tipo de referência, o código fará uma verificação de `is null` adequada.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="a3c0e-143">Interseção com tipos de referência anuláveis</span><span class="sxs-lookup"><span data-stu-id="a3c0e-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="a3c0e-144">Qualquer parâmetro que tenha um operador de `!` aplicado ao nome será iniciado com o estado anulável não `null`.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="a3c0e-145">Isso é verdadeiro mesmo que o tipo do parâmetro em si seja potencialmente `null`.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="a3c0e-146">Isso pode ocorrer com um tipo anulável explicitamente, como digamos `string?`ou com um parâmetro de tipo irrestrito.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="a3c0e-147">Quando uma sintaxe de `!` em parâmetros é combinada com um tipo explicitamente anulável no parâmetro, um aviso será emitido pelo compilador:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="a3c0e-148">Problemas em aberto</span><span class="sxs-lookup"><span data-stu-id="a3c0e-148">Open Issues</span></span>
<span data-ttu-id="a3c0e-149">Nenhum</span><span class="sxs-lookup"><span data-stu-id="a3c0e-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="a3c0e-150">Considerações</span><span class="sxs-lookup"><span data-stu-id="a3c0e-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="a3c0e-151">{1&gt;Construtores&lt;1}</span><span class="sxs-lookup"><span data-stu-id="a3c0e-151">Constructors</span></span>
<span data-ttu-id="a3c0e-152">A geração de código para construtores significa que há uma alteração de comportamento pequena, mas observável, ao mover da validação de `null` padrão hoje e a `null` sintaxe de parâmetro de validação (`!`).</span><span class="sxs-lookup"><span data-stu-id="a3c0e-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="a3c0e-153">A `null` verificação na validação padrão ocorre depois de ambos os inicializadores de campo e de qualquer `base` ou `this` chamadas.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="a3c0e-154">Isso significa que um desenvolvedor não pode necessariamente migrar 100% de sua validação de `null` para a nova sintaxe.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="a3c0e-155">Os construtores exigem, pelo menos, alguma inspeção.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="a3c0e-156">Após a discussão, foi decidido que isso é muito improvável de causar problemas significativos de adoção.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="a3c0e-157">É mais lógico que a `null` verificação seja executada antes de qualquer lógica no construtor.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="a3c0e-158">Pode revisitar se forem descobertos problemas de compatibilidade significativos.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="a3c0e-159">Aviso ao misturar?</span><span class="sxs-lookup"><span data-stu-id="a3c0e-159">Warning when mixing ?</span></span> <span data-ttu-id="a3c0e-160">e!</span><span class="sxs-lookup"><span data-stu-id="a3c0e-160">and !</span></span>
<span data-ttu-id="a3c0e-161">Houve uma discussão longa sobre se um aviso deve ou não ser emitido quando a sintaxe de `!` é aplicada a um parâmetro que é explicitamente digitado para um tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="a3c0e-162">Na superfície, ela parece uma declaração não-atestada pelo desenvolvedor, mas há casos em que as hierarquias de tipos poderiam forçar os desenvolvedores nessa situação.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="a3c0e-163">Considere a seguinte hierarquia de classe em uma série de assemblies (supondo que todos sejam compilados com `null` verificação habilitada):</span><span class="sxs-lookup"><span data-stu-id="a3c0e-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="a3c0e-164">Aqui, o autor de `C3` decidiu adicionar `null` validação ao parâmetro `o`.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="a3c0e-165">Isso está totalmente em linha com o objetivo do recurso ser usado.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="a3c0e-166">Agora imagine em uma data posterior o autor de Assembly2 decide adicionar a seguinte substituição:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="a3c0e-167">Isso é permitido por tipos de referência anuláveis, pois é legal tornar o contrato mais flexível para posições de entrada.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="a3c0e-168">O recurso NRT em geral permite co/contravariância razoáveis em parâmetro/retorno de nulidade.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="a3c0e-169">No entanto, o idioma faz a verificação de co/contravariância com base na substituição mais específica, não na declaração original.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="a3c0e-170">Isso significa que o autor de Assembly3 receberá um aviso sobre o tipo de `o` não correspondente e precisará alterar a assinatura para o seguinte para eliminá-la:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="a3c0e-171">Neste ponto, o autor do Assembly3 tem algumas opções:</span><span class="sxs-lookup"><span data-stu-id="a3c0e-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="a3c0e-172">Eles podem aceitar/suprimir o aviso sobre `object?` e `object` incompatibilidade.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="a3c0e-173">Eles podem aceitar/suprimir o aviso sobre `object?` e `!` incompatibilidade.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="a3c0e-174">Eles podem apenas remover a verificação de validação de `null` (excluir `!` e fazer a verificação explícita)</span><span class="sxs-lookup"><span data-stu-id="a3c0e-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="a3c0e-175">Esse é um cenário real, mas, por enquanto, a ideia é avançar com o aviso.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="a3c0e-176">Se o aviso ocorrer com mais frequência do que antecipamos, podemos removê-lo mais tarde (o inverso não é verdadeiro).</span><span class="sxs-lookup"><span data-stu-id="a3c0e-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="a3c0e-177">Argumentos de setter de propriedade implícita</span><span class="sxs-lookup"><span data-stu-id="a3c0e-177">Implicit property setter arguments</span></span>
<span data-ttu-id="a3c0e-178">O argumento `value` de um parâmetro é implícito e não aparece em nenhuma lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="a3c0e-179">Isso significa que ele não pode ser um destino desse recurso.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="a3c0e-180">A sintaxe setter de propriedade pode ser estendida para incluir uma lista de parâmetros para permitir que o operador de `!` seja aplicado.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="a3c0e-181">Mas isso recorta a ideia desse recurso, tornando `null` a validação mais simples.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="a3c0e-182">Assim, o argumento de `value` implícito simplesmente não funcionará com esse recurso.</span><span class="sxs-lookup"><span data-stu-id="a3c0e-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="a3c0e-183">Considerações futuras</span><span class="sxs-lookup"><span data-stu-id="a3c0e-183">Future Considerations</span></span>
<span data-ttu-id="a3c0e-184">Nenhum</span><span class="sxs-lookup"><span data-stu-id="a3c0e-184">None</span></span>
