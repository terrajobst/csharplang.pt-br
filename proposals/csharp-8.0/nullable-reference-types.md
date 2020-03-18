---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484611"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="f4881-101">Tipos de referência anuláveis emC#</span><span class="sxs-lookup"><span data-stu-id="f4881-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="f4881-102">O objetivo desse recurso é:</span><span class="sxs-lookup"><span data-stu-id="f4881-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="f4881-103">Permitir que os desenvolvedores expressem se uma variável, um parâmetro ou um resultado de um tipo de referência deve ser nulo ou não.</span><span class="sxs-lookup"><span data-stu-id="f4881-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="f4881-104">Forneça avisos quando tais variáveis, parâmetros e resultados não forem usados de acordo com essa intenção.</span><span class="sxs-lookup"><span data-stu-id="f4881-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="f4881-105">Expressão de intenção</span><span class="sxs-lookup"><span data-stu-id="f4881-105">Expression of intent</span></span>

<span data-ttu-id="f4881-106">O idioma já contém a sintaxe `T?` para tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="f4881-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="f4881-107">É simples estender essa sintaxe para tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="f4881-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="f4881-108">Supõe-se que a intenção de um tipo de referência não adornado `T` seja não nulo.</span><span class="sxs-lookup"><span data-stu-id="f4881-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="f4881-109">Verificação de referências anuláveis</span><span class="sxs-lookup"><span data-stu-id="f4881-109">Checking of nullable references</span></span>

<span data-ttu-id="f4881-110">Uma análise de fluxo acompanha variáveis de referência anuláveis.</span><span class="sxs-lookup"><span data-stu-id="f4881-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="f4881-111">Quando a análise considera que ela não seria nula (por exemplo, após uma verificação ou uma atribuição), seu valor será considerado uma referência não nula.</span><span class="sxs-lookup"><span data-stu-id="f4881-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="f4881-112">Uma referência anulável também pode ser tratada explicitamente como não nula com o operador de `x!` de sufixo (o operador "dammit"), para quando a análise de fluxo não puder estabelecer uma situação não nula que o desenvolvedor saiba.</span><span class="sxs-lookup"><span data-stu-id="f4881-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="f4881-113">Caso contrário, um aviso será fornecido se uma referência anulável for desreferenciada ou for convertida em um tipo não nulo.</span><span class="sxs-lookup"><span data-stu-id="f4881-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="f4881-114">Um aviso é fornecido ao converter de `S[]` para `T?[]` e de `S?[]` para `T[]`.</span><span class="sxs-lookup"><span data-stu-id="f4881-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="f4881-115">Um aviso é fornecido ao converter de `C<S>` para `C<T?>` exceto quando o parâmetro de tipo é covariant (`out`) e ao converter de `C<S?>` em `C<T>`, exceto quando o parâmetro de tipo é contravariant (`in`).</span><span class="sxs-lookup"><span data-stu-id="f4881-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="f4881-116">Um aviso será fornecido em `C<T?>` se o parâmetro de tipo tiver restrições não nulas.</span><span class="sxs-lookup"><span data-stu-id="f4881-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="f4881-117">Verificando referências não nulas</span><span class="sxs-lookup"><span data-stu-id="f4881-117">Checking of non-null references</span></span>

<span data-ttu-id="f4881-118">Um aviso será fornecido se um literal nulo for atribuído a uma variável não nula ou passado como um parâmetro não nulo.</span><span class="sxs-lookup"><span data-stu-id="f4881-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="f4881-119">Um aviso também será fornecido se um construtor não inicializar explicitamente os campos de referência não nulos.</span><span class="sxs-lookup"><span data-stu-id="f4881-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="f4881-120">Não podemos rastrear adequadamente que todos os elementos de uma matriz de referências não nulas são inicializados.</span><span class="sxs-lookup"><span data-stu-id="f4881-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="f4881-121">No entanto, poderíamos emitir um aviso se nenhum elemento de uma matriz recém-criada for atribuído antes de a matriz ser lida ou passada.</span><span class="sxs-lookup"><span data-stu-id="f4881-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="f4881-122">Isso pode lidar com o caso comum sem muito ruído.</span><span class="sxs-lookup"><span data-stu-id="f4881-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="f4881-123">Precisamos decidir se `default(T)` gera um aviso ou é simplesmente tratado como sendo do tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="f4881-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="f4881-124">Representação de metadados</span><span class="sxs-lookup"><span data-stu-id="f4881-124">Metadata representation</span></span>

<span data-ttu-id="f4881-125">Adornos de nulidade devem ser representados em metadados como atributos.</span><span class="sxs-lookup"><span data-stu-id="f4881-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="f4881-126">Isso significa que os compiladores de nível inferior irão ignorá-los.</span><span class="sxs-lookup"><span data-stu-id="f4881-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="f4881-127">Precisamos decidir se apenas anotações anuláveis estão incluídas, ou também há alguma indicação de se não nulo foi "ligado" no assembly.</span><span class="sxs-lookup"><span data-stu-id="f4881-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="f4881-128">Genéricos</span><span class="sxs-lookup"><span data-stu-id="f4881-128">Generics</span></span>

<span data-ttu-id="f4881-129">Se um parâmetro de tipo `T` tiver restrições não anuláveis, ele será tratado como não anulável dentro de seu escopo.</span><span class="sxs-lookup"><span data-stu-id="f4881-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="f4881-130">Se um parâmetro de tipo for irrestrito ou tiver apenas restrições anuláveis, a situação será um pouco mais complexa: isso significa que o argumento de tipo correspondente pode *ser anulável ou não anulável.*</span><span class="sxs-lookup"><span data-stu-id="f4881-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="f4881-131">A coisa segura a fazer nessa situação é tratar o *parâmetro de tipo como anulável* e não anulável, fornecendo avisos quando um deles for violado.</span><span class="sxs-lookup"><span data-stu-id="f4881-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="f4881-132">Vale a pena considerar se as restrições de referência anuláveis explícitas devem ser permitidas.</span><span class="sxs-lookup"><span data-stu-id="f4881-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="f4881-133">No entanto, observe que não podemos evitar ter tipos de referência anuláveis *implicitamente* restrições em determinados casos (restrições herdadas).</span><span class="sxs-lookup"><span data-stu-id="f4881-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="f4881-134">A restrição `class` é não nula.</span><span class="sxs-lookup"><span data-stu-id="f4881-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="f4881-135">Podemos considerar se `class?` deve ser uma restrição anulável válida, indicando "tipo de referência anulável".</span><span class="sxs-lookup"><span data-stu-id="f4881-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="f4881-136">Inferência de tipos</span><span class="sxs-lookup"><span data-stu-id="f4881-136">Type inference</span></span>

<span data-ttu-id="f4881-137">Na inferência de tipos, se um tipo de contribuição for um tipo de referência anulável, o tipo resultante deverá ser anulável.</span><span class="sxs-lookup"><span data-stu-id="f4881-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="f4881-138">Em outras palavras, a nulidade é propagada.</span><span class="sxs-lookup"><span data-stu-id="f4881-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="f4881-139">Devemos considerar se o literal `null` como uma expressão participante deve contribuir com a nulidade.</span><span class="sxs-lookup"><span data-stu-id="f4881-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="f4881-140">Não hoje: para tipos de valor, ele leva a um erro, ao passo que, para tipos de referência, o NULL converte com êxito o tipo Plain.</span><span class="sxs-lookup"><span data-stu-id="f4881-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="f4881-141">Alterações da falha</span><span class="sxs-lookup"><span data-stu-id="f4881-141">Breaking changes</span></span>

<span data-ttu-id="f4881-142">Avisos não nulos são uma alteração significativa de quebra no código existente e devem ser acompanhados por um mecanismo de aceitação.</span><span class="sxs-lookup"><span data-stu-id="f4881-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="f4881-143">Menos óbvio, os avisos de tipos anuláveis (conforme descrito acima) são uma alteração significativa no código existente em determinados cenários em que a nulidade é implícita:</span><span class="sxs-lookup"><span data-stu-id="f4881-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="f4881-144">Os parâmetros de tipo irrestrito serão tratados como anuláveis implicitamente, portanto, atribuí-los a `object` ou acessar, por exemplo, `ToString` produzirão avisos.</span><span class="sxs-lookup"><span data-stu-id="f4881-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="f4881-145">se a inferência de tipos inferir a nulidade de `null` expressões, o código existente, às vezes, resultará em tipos anuláveis em vez de não anuláveis, o que pode levar a novos avisos.</span><span class="sxs-lookup"><span data-stu-id="f4881-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="f4881-146">Portanto, os avisos anuláveis também precisam ser opcionais</span><span class="sxs-lookup"><span data-stu-id="f4881-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="f4881-147">Por fim, adicionar anotações a uma API existente será uma alteração significativa para os usuários que optaram por avisos, quando eles atualizarem a biblioteca.</span><span class="sxs-lookup"><span data-stu-id="f4881-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="f4881-148">Isso também merece a capacidade de aceitar ou sair. "Desejo as correções de bugs, mas não estou pronto para lidar com suas novas anotações"</span><span class="sxs-lookup"><span data-stu-id="f4881-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="f4881-149">Em resumo, você precisa ser capaz de aceitar/sair de:</span><span class="sxs-lookup"><span data-stu-id="f4881-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="f4881-150">Avisos anuláveis</span><span class="sxs-lookup"><span data-stu-id="f4881-150">Nullable warnings</span></span>
* <span data-ttu-id="f4881-151">Avisos não nulos</span><span class="sxs-lookup"><span data-stu-id="f4881-151">Non-null warnings</span></span>
* <span data-ttu-id="f4881-152">Avisos de anotações em outros arquivos</span><span class="sxs-lookup"><span data-stu-id="f4881-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="f4881-153">A granularidade da aceitação sugere um modelo como o analisador, em que faixas de código pode aceitar e cancelar com pragmas e níveis de severidade podem ser escolhidos pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="f4881-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="f4881-154">Além disso, as opções por biblioteca ("ignorar as anotações de JSON.NET até que eu esteja pronto para lidar com a saída") podem ser expressas em código como atributos.</span><span class="sxs-lookup"><span data-stu-id="f4881-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="f4881-155">O design da experiência de aceitação/transição é crucial para o sucesso e a utilidade desse recurso.</span><span class="sxs-lookup"><span data-stu-id="f4881-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="f4881-156">Precisamos garantir que:</span><span class="sxs-lookup"><span data-stu-id="f4881-156">We need to make sure that:</span></span>

* <span data-ttu-id="f4881-157">Os usuários podem adotar a verificação de nulidade gradualmente, como desejam</span><span class="sxs-lookup"><span data-stu-id="f4881-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="f4881-158">Os autores de biblioteca podem adicionar anotações de nulidade sem medo de quebrar clientes</span><span class="sxs-lookup"><span data-stu-id="f4881-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="f4881-159">Apesar desses, não há uma noção de "pesadelo de configuração"</span><span class="sxs-lookup"><span data-stu-id="f4881-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="f4881-160">Ajustes</span><span class="sxs-lookup"><span data-stu-id="f4881-160">Tweaks</span></span>

<span data-ttu-id="f4881-161">Poderíamos considerar não usar as anotações de `?` em locais, mas apenas observando se elas são usadas de acordo com o que é atribuído a elas.</span><span class="sxs-lookup"><span data-stu-id="f4881-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="f4881-162">Eu não prefiro isso; Acho que devemos permitir que as pessoas expressem suas intenções.</span><span class="sxs-lookup"><span data-stu-id="f4881-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="f4881-163">Poderíamos considerar um `T! x` abreviado em parâmetros, que gera automaticamente uma verificação nula em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="f4881-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="f4881-164">Determinados padrões em tipos genéricos, como `FirstOrDefault` ou `TryGet`, têm um comportamento um pouco estranho com argumentos de tipo não anuláveis, pois eles implicitamente geram valores padrão em determinadas situações.</span><span class="sxs-lookup"><span data-stu-id="f4881-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="f4881-165">Poderíamos tentar nuancer o sistema de tipos para acomodá-lo melhor.</span><span class="sxs-lookup"><span data-stu-id="f4881-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="f4881-166">Por exemplo, poderíamos permitir `?` em parâmetros de tipo irrestrito, embora o argumento de tipo já possa ser anulável.</span><span class="sxs-lookup"><span data-stu-id="f4881-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="f4881-167">Acho que vale a pena, e isso leva à estranhaidade relacionada à interação com tipos de *valor* anulável.</span><span class="sxs-lookup"><span data-stu-id="f4881-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="f4881-168">Tipos de valor anuláveis</span><span class="sxs-lookup"><span data-stu-id="f4881-168">Nullable value types</span></span>

<span data-ttu-id="f4881-169">Poderíamos considerar a adoção de algumas das semânticas acima para tipos de valor anuláveis também.</span><span class="sxs-lookup"><span data-stu-id="f4881-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="f4881-170">Já mencionamos inferência de tipos, onde poderíamos inferir `int?` de `(7, null)`, em vez de apenas dar um erro.</span><span class="sxs-lookup"><span data-stu-id="f4881-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="f4881-171">Outra oportunidade é aplicar a análise de fluxo a tipos de valor anuláveis.</span><span class="sxs-lookup"><span data-stu-id="f4881-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="f4881-172">Quando eles são considerados não nulos, podemos realmente permitir o uso como o tipo não anulável de determinadas maneiras (por exemplo, acesso de membro).</span><span class="sxs-lookup"><span data-stu-id="f4881-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="f4881-173">Precisamos apenas ter cuidado para que as coisas que você *já* possa fazer em um tipo de valor anulável sejam preferenciais, para fins de compatibilidade de volta.</span><span class="sxs-lookup"><span data-stu-id="f4881-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
