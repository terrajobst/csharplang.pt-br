---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484485"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="25695-101">Suprimir emissão de `localsinit` sinalizador.</span><span class="sxs-lookup"><span data-stu-id="25695-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="25695-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="25695-102">[x] Proposed</span></span>
* <span data-ttu-id="25695-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="25695-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="25695-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="25695-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="25695-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="25695-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="25695-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="25695-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="25695-107">Permitir supressão de emissão de `localsinit` sinalizador por meio do atributo `SkipLocalsInitAttribute`.</span><span class="sxs-lookup"><span data-stu-id="25695-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="25695-108">Motivação</span><span class="sxs-lookup"><span data-stu-id="25695-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="25695-109">Tela de fundo</span><span class="sxs-lookup"><span data-stu-id="25695-109">Background</span></span>
<span data-ttu-id="25695-110">Por variáveis locais de especificação CLR que não contêm referências não são inicializadas para um valor específico pela VM/JIT.</span><span class="sxs-lookup"><span data-stu-id="25695-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="25695-111">A leitura dessas variáveis sem inicialização é de tipo seguro, mas, caso contrário, o comportamento é indefinido e específico da implementação.</span><span class="sxs-lookup"><span data-stu-id="25695-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="25695-112">Normalmente, os locais não inicializados contêm quaisquer valores que foram deixados na memória que agora estão ocupados pelo quadro de pilha.</span><span class="sxs-lookup"><span data-stu-id="25695-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="25695-113">Isso poderia levar a um comportamento não determinístico e difícil reproduzir bugs.</span><span class="sxs-lookup"><span data-stu-id="25695-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="25695-114">Há duas maneiras de "atribuir" uma variável local:</span><span class="sxs-lookup"><span data-stu-id="25695-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="25695-115">ao armazenar um valor ou</span><span class="sxs-lookup"><span data-stu-id="25695-115">by storing a value or</span></span> 
- <span data-ttu-id="25695-116">ao especificar `localsinit` sinalizador que força tudo que está alocado para o pool de memória local ser inicializado com zero Observação: isso inclui variáveis locais e `stackalloc` dados.</span><span class="sxs-lookup"><span data-stu-id="25695-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="25695-117">O uso de dados não inicializados é desencorajado e não é permitido no código verificável.</span><span class="sxs-lookup"><span data-stu-id="25695-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="25695-118">Embora seja possível provar que, por meio da análise de fluxo, é permitido que o algoritmo de verificação seja conservador e simplesmente exija que `localsinit` esteja definido.</span><span class="sxs-lookup"><span data-stu-id="25695-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="25695-119">Historicamente C# , o compilador emite `localsinit` sinalizador em todos os métodos que declaram locais.</span><span class="sxs-lookup"><span data-stu-id="25695-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="25695-120">Embora C# o empregue uma análise de atribuição definitiva, que é mais estrita do que a especificaçãoC# CLR exigida (também precisa considerar o escopo de locais), não é estritamente garantido que o código resultante seria formalmente verificável:</span><span class="sxs-lookup"><span data-stu-id="25695-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="25695-121">O CLR C# e as regras podem não concordar se a passagem de um argumento local as `out` é uma `use`.</span><span class="sxs-lookup"><span data-stu-id="25695-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="25695-122">O CLR C# e as regras podem não concordar com o tratamento de ramificações condicionais quando as condições são conhecidas (propagação constante).</span><span class="sxs-lookup"><span data-stu-id="25695-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="25695-123">O CLR também poderia simplesmente exigir `localinits`, já que isso é permitido.</span><span class="sxs-lookup"><span data-stu-id="25695-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="25695-124">Problema</span><span class="sxs-lookup"><span data-stu-id="25695-124">Problem</span></span>
<span data-ttu-id="25695-125">No aplicativo de alto desempenho, o custo da inicialização zero forçada pode ser perceptível.</span><span class="sxs-lookup"><span data-stu-id="25695-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="25695-126">É particularmente perceptível quando `stackalloc` é usado.</span><span class="sxs-lookup"><span data-stu-id="25695-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="25695-127">Em alguns casos, o JIT pode Elide inicialização zero inicial de locais individuais quando tal inicialização é "eliminada" por atribuições subsequentes.</span><span class="sxs-lookup"><span data-stu-id="25695-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="25695-128">Nem todos os JITs fazem isso e essa otimização tem limites.</span><span class="sxs-lookup"><span data-stu-id="25695-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="25695-129">Ele não ajuda com `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="25695-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="25695-130">Para ilustrar que o problema é real, há um bug conhecido em que um método que não contém nenhum local de `IL` não teria `localsinit` sinalizador.</span><span class="sxs-lookup"><span data-stu-id="25695-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="25695-131">O bug já está sendo explorado pelos usuários ao colocar `stackalloc` em tais métodos, intencionalmente, para evitar os custos de inicialização.</span><span class="sxs-lookup"><span data-stu-id="25695-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="25695-132">Isso é apesar do fato de que a ausência de `IL` locais é uma métrica instável e pode variar dependendo das alterações na estratégia de codegen.</span><span class="sxs-lookup"><span data-stu-id="25695-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="25695-133">O bug deve ser corrigido e os usuários devem ter uma maneira mais documentada e confiável de suprimir o sinalizador.</span><span class="sxs-lookup"><span data-stu-id="25695-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="25695-134">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="25695-134">Detailed design</span></span>

<span data-ttu-id="25695-135">Permitir a especificação de `System.Runtime.CompilerServices.SkipLocalsInitAttribute` como uma maneira de informar ao compilador para não emitir `localsinit` sinalizador.</span><span class="sxs-lookup"><span data-stu-id="25695-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="25695-136">O resultado final disso será que os locais podem não ser inicializados com zero pelo JIT, que está na maioria dos casos inobservados no C#.</span><span class="sxs-lookup"><span data-stu-id="25695-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="25695-137">Além disso `stackalloc` dados não serão inicializados com zero.</span><span class="sxs-lookup"><span data-stu-id="25695-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="25695-138">Isso é definitivamente observável, mas também é o cenário mais motivador.</span><span class="sxs-lookup"><span data-stu-id="25695-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="25695-139">Os destinos de atributo permitidos e reconhecidos são: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span><span class="sxs-lookup"><span data-stu-id="25695-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="25695-140">No entanto, o compilador não exigirá que esse atributo seja definido com os destinos listados, nem irá se preocupar em qual assembly o atributo está definido.</span><span class="sxs-lookup"><span data-stu-id="25695-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="25695-141">Quando o atributo é especificado em um contêiner (`class`, `module`, que contém o método para um método aninhado,...), o sinalizador afeta todos os métodos contidos no contêiner.</span><span class="sxs-lookup"><span data-stu-id="25695-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="25695-142">Os métodos sintetizados "herdam" o sinalizador do contêiner/proprietário lógico.</span><span class="sxs-lookup"><span data-stu-id="25695-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="25695-143">O sinalizador afeta apenas a estratégia CodeGen para corpos de métodos reais.</span><span class="sxs-lookup"><span data-stu-id="25695-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="25695-144">I.E.</span><span class="sxs-lookup"><span data-stu-id="25695-144">I.E.</span></span> <span data-ttu-id="25695-145">o sinalizador não tem efeito sobre métodos abstratos e não é propagado para substituir/implementar métodos.</span><span class="sxs-lookup"><span data-stu-id="25695-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="25695-146">Isso é explicitamente um **_recurso de compilador_** e **_não um recurso de linguagem_** .</span><span class="sxs-lookup"><span data-stu-id="25695-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="25695-147">Da mesma forma que as opções de linha de comando do compilador, o recurso controla os detalhes de implementação de uma estratégia CodeGen específica C# e não precisa ser exigido pela especificação.</span><span class="sxs-lookup"><span data-stu-id="25695-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="25695-148">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="25695-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="25695-149">Os compiladores antigos/outros podem não honrar o atributo.</span><span class="sxs-lookup"><span data-stu-id="25695-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="25695-150">Ignorar o atributo é um comportamento compatível.</span><span class="sxs-lookup"><span data-stu-id="25695-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="25695-151">Pode resultar apenas em uma ligeira queda de desempenho.</span><span class="sxs-lookup"><span data-stu-id="25695-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="25695-152">O código sem `localinits` sinalizador pode disparar falhas de verificação.</span><span class="sxs-lookup"><span data-stu-id="25695-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="25695-153">Os usuários que solicitam esse recurso geralmente não são preocupados com a capacidade de verificação.</span><span class="sxs-lookup"><span data-stu-id="25695-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="25695-154">A aplicação do atributo em níveis mais altos do que um método individual tem efeito não local, que é observável quando `stackalloc` é usado.</span><span class="sxs-lookup"><span data-stu-id="25695-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="25695-155">Ainda assim, esse é o cenário mais solicitado.</span><span class="sxs-lookup"><span data-stu-id="25695-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="25695-156">Alternativas</span><span class="sxs-lookup"><span data-stu-id="25695-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="25695-157">Omita `localinits` sinalizar quando o método for declarado no contexto de `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="25695-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="25695-158">Isso poderia causar uma alteração de comportamento silencioso e perigoso de determinístico para não determinístico em um caso de `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="25695-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="25695-159">Omita `localinits` sinalizar sempre.</span><span class="sxs-lookup"><span data-stu-id="25695-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="25695-160">Ainda pior do que acima.</span><span class="sxs-lookup"><span data-stu-id="25695-160">Even worse than above.</span></span>

- <span data-ttu-id="25695-161">Omita `localinits` sinalizador, a menos que `stackalloc` seja usado no corpo do método.</span><span class="sxs-lookup"><span data-stu-id="25695-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="25695-162">Não aborda o cenário mais solicitado e pode desativar o código não verificável sem a opção de revertê-lo de volta.</span><span class="sxs-lookup"><span data-stu-id="25695-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="25695-163">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="25695-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="25695-164">O atributo deve ser realmente emitido para os metadados?</span><span class="sxs-lookup"><span data-stu-id="25695-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="25695-165">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="25695-165">Design meetings</span></span>

<span data-ttu-id="25695-166">Nenhum ainda.</span><span class="sxs-lookup"><span data-stu-id="25695-166">None yet.</span></span> 