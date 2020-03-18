---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484968"
---
# <a name="primary-constructors"></a><span data-ttu-id="b1d23-101">Construtores primários</span><span class="sxs-lookup"><span data-stu-id="b1d23-101">Primary constructors</span></span>

* <span data-ttu-id="b1d23-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="b1d23-102">[x] Proposed</span></span>
* <span data-ttu-id="b1d23-103">[] Protótipo: não iniciado</span><span class="sxs-lookup"><span data-stu-id="b1d23-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="b1d23-104">[] Implementação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="b1d23-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="b1d23-105">[] Especificação: não iniciada</span><span class="sxs-lookup"><span data-stu-id="b1d23-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="b1d23-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="b1d23-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b1d23-107">As classes podem ter uma lista de parâmetros e, quando elas fazem, sua especificação de classe base pode ter uma lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="b1d23-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="b1d23-108">Os parâmetros do construtor primário estão no escopo durante a declaração de classe e, se forem capturados por um membro de função ou função anônima, eles serão armazenados como campos particulares na classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="b1d23-109">Motivação</span><span class="sxs-lookup"><span data-stu-id="b1d23-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b1d23-110">É comum ter muitos códigos clichês no código de inicialização do programa.</span><span class="sxs-lookup"><span data-stu-id="b1d23-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="b1d23-111">No caso geral, um dado `x` de dados é mencionado muitas vezes:</span><span class="sxs-lookup"><span data-stu-id="b1d23-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="b1d23-112">Como um campo privado `_x`</span><span class="sxs-lookup"><span data-stu-id="b1d23-112">As a private field `_x`</span></span>
- <span data-ttu-id="b1d23-113">Como um parâmetro `x` a um construtor</span><span class="sxs-lookup"><span data-stu-id="b1d23-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="b1d23-114">Em uma atribuição `_x = x;` do campo do parâmetro no Construtor</span><span class="sxs-lookup"><span data-stu-id="b1d23-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="b1d23-115">Como uma propriedade `X`</span><span class="sxs-lookup"><span data-stu-id="b1d23-115">As a property `X`</span></span>
- <span data-ttu-id="b1d23-116">No setter de propriedade `x = value;`</span><span class="sxs-lookup"><span data-stu-id="b1d23-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="b1d23-117">No `return x;` de getter de propriedade</span><span class="sxs-lookup"><span data-stu-id="b1d23-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="b1d23-118">Para propriedades que não exigem validação ou computação, o tédio pode ser reduzido usando propriedades automáticas, gerando assim a necessidade de declarar um campo de apoio explícito para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="b1d23-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="b1d23-119">Mas se sua propriedade exigir qualquer tipo de lógica além do que uma propriedade automática fornece, a melhor é a que você faz.</span><span class="sxs-lookup"><span data-stu-id="b1d23-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="b1d23-120">Em vez disso, os construtores primários reduzem a sobrecarga colocando argumentos de construtor diretamente no escopo em toda a classe, novamente dispensandondo a necessidade de declarar explicitamente um campo de apoio.</span><span class="sxs-lookup"><span data-stu-id="b1d23-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="b1d23-121">Portanto, o exemplo acima se tornaria:</span><span class="sxs-lookup"><span data-stu-id="b1d23-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="b1d23-122">Neste exemplo, o Construtor principal reduz o número de entidades nomeadas para `x` de três para duas, dispensando o `_x` campo de backup.</span><span class="sxs-lookup"><span data-stu-id="b1d23-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="b1d23-123">Ele remove duas das três declarações de membro (mantendo apenas a declaração de propriedade em si) e reduz o número total de menção de `x`/`_x`/`X` de oito a cinco.</span><span class="sxs-lookup"><span data-stu-id="b1d23-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="b1d23-124">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="b1d23-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b1d23-125">As classes podem ter uma lista de parâmetros:</span><span class="sxs-lookup"><span data-stu-id="b1d23-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="b1d23-126">A lista de parâmetros faz com que um construtor seja declarado implicitamente para a classe, com a mesma acessibilidade que a própria classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="b1d23-127">Os parâmetros do construtor primário estão no escopo ao longo do corpo da classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="b1d23-128">Se eles forem capturados por um membro de função ou função anônima, eles serão armazenados como campos particulares na classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="b1d23-129">Se eles forem usados apenas durante a inicialização, eles não serão armazenados no objeto.</span><span class="sxs-lookup"><span data-stu-id="b1d23-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="b1d23-130">Se uma classe com um construtor primário tiver uma especificação de classe base, ela poderá ter uma lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="b1d23-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="b1d23-131">Isso serve como a lista de argumentos para um inicializador de `base(...)` do construtor declarado implicitamente.</span><span class="sxs-lookup"><span data-stu-id="b1d23-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="b1d23-132">Se nenhuma lista de argumentos for fornecida, será assumida uma prevazia.</span><span class="sxs-lookup"><span data-stu-id="b1d23-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="b1d23-133">A classe também pode ter construtores definidos explicitamente, mas todos eles precisam usar um inicializador de `this(...)`.</span><span class="sxs-lookup"><span data-stu-id="b1d23-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="b1d23-134">Isso garante que o construtor primário é sempre chamado quando uma nova instância é construída.</span><span class="sxs-lookup"><span data-stu-id="b1d23-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="b1d23-135">Todos os inicializadores no corpo da classe se tornarão atribuições no Construtor gerado.</span><span class="sxs-lookup"><span data-stu-id="b1d23-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="b1d23-136">Isso significa que, ao contrário de outras classes, inicializadores serão executados *depois* que o construtor base tiver sido invocado, não antes.</span><span class="sxs-lookup"><span data-stu-id="b1d23-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="b1d23-137">Além disso, a classe gerada conterá atribuições para inicializar quaisquer campos privados que foram gerados para armazenar parâmetros de Construtor primários que foram capturados por membros.</span><span class="sxs-lookup"><span data-stu-id="b1d23-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="b1d23-138">Esses membros são reescritos para usar o campo particular em vez do parâmetro de maneira semelhante a fechamentos para expressões lambda.</span><span class="sxs-lookup"><span data-stu-id="b1d23-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="b1d23-139">Os campos primários gerados são inicializados primeiro e, em seguida, as atribuições geradas pelo inicializador são executadas na ordem de aparência na classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="b1d23-140">Para o exemplo acima, o efeito da declaração de classe é como se fosse reescrito da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b1d23-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="b1d23-141">A captura tem restrições semelhantes à captura de variáveis locais por expressões lambda.</span><span class="sxs-lookup"><span data-stu-id="b1d23-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="b1d23-142">Por exemplo, parâmetros de `ref` e `out` são permitidos em construtores primários, mas não podem ser capturados para meus corpos de membros.</span><span class="sxs-lookup"><span data-stu-id="b1d23-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="b1d23-143">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="b1d23-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b1d23-144">Em ordem aproximada de significância.</span><span class="sxs-lookup"><span data-stu-id="b1d23-144">In rough order of significance.</span></span>

* <span data-ttu-id="b1d23-145">A proposta usa a sintaxe que também foi proposta para registros posicionais.</span><span class="sxs-lookup"><span data-stu-id="b1d23-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="b1d23-146">Se quisermos ambos os recursos, será necessária alguma acomodação.</span><span class="sxs-lookup"><span data-stu-id="b1d23-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="b1d23-147">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="b1d23-147">E.g.</span></span> <span data-ttu-id="b1d23-148">um modificador de `data` em registros foi proposto.</span><span class="sxs-lookup"><span data-stu-id="b1d23-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="b1d23-149">O tamanho de alocação de objetos construídos é menos óbvio, pois o compilador determina se deve alocar um campo para um parâmetro de construtor primário com base no texto completo da classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="b1d23-150">Esse risco é semelhante à captura implícita de variáveis por expressões lambda.</span><span class="sxs-lookup"><span data-stu-id="b1d23-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="b1d23-151">Uma tentação comum (ou padrão acidental) pode ser capturar o parâmetro "mesmo" em vários níveis de herança, já que ele passa a cadeia de construtor, em vez de explicitamente alocado a ele um campo protegido na classe base, levando a alocações duplicadas para os mesmos dados em objetos.</span><span class="sxs-lookup"><span data-stu-id="b1d23-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="b1d23-152">Isso é muito semelhante ao risco atual de substituir Propriedades automáticas por propriedades automáticas.</span><span class="sxs-lookup"><span data-stu-id="b1d23-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="b1d23-153">Conforme proposto acima, não há lugar para lógica adicional que normalmente pode ser expressa em corpos de construtor.</span><span class="sxs-lookup"><span data-stu-id="b1d23-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="b1d23-154">A extensão "principais corpos do Construtor" abaixo aborda isso.</span><span class="sxs-lookup"><span data-stu-id="b1d23-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="b1d23-155">Como proposto, a semântica da ordem de execução é sutilmente diferente de com construtores comuns.</span><span class="sxs-lookup"><span data-stu-id="b1d23-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="b1d23-156">Isso pode ser remediado, mas com o custo de algumas das propostas de extensão (notadamente "corpos do construtor primário").</span><span class="sxs-lookup"><span data-stu-id="b1d23-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="b1d23-157">A proposta só funciona quando um único Construtor pode ser designado como primário.</span><span class="sxs-lookup"><span data-stu-id="b1d23-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="b1d23-158">Não há como ter uma acessibilidade separada da classe e o Construtor principal.</span><span class="sxs-lookup"><span data-stu-id="b1d23-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="b1d23-159">Por exemplo, em situações em que todos os construtores públicos são delegados para um construtor "Build-it-all" privado que seria necessário.</span><span class="sxs-lookup"><span data-stu-id="b1d23-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="b1d23-160">Se necessário, a sintaxe poderá ser proposta posteriormente.</span><span class="sxs-lookup"><span data-stu-id="b1d23-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="b1d23-161">Alternativas</span><span class="sxs-lookup"><span data-stu-id="b1d23-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b1d23-162">Os registros posicionais completos podem ser uma alternativa ou podem coexistir com construtores primários, dependendo das especificidades.</span><span class="sxs-lookup"><span data-stu-id="b1d23-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="b1d23-163">Eles permitiriam *mais* abreviação em um número *menor* de cenários.</span><span class="sxs-lookup"><span data-stu-id="b1d23-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="b1d23-164">Ambos são potencialmente úteis, mas ter ambos pode ser um exagero, a menos que eles possam ser perfeitamente integrados entre si.</span><span class="sxs-lookup"><span data-stu-id="b1d23-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="b1d23-165">Extensões possíveis</span><span class="sxs-lookup"><span data-stu-id="b1d23-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="b1d23-166">Essas são variações ou adições à proposta principal que podem ser consideradas em conjunto, ou em um estágio posterior, se considerado útil.</span><span class="sxs-lookup"><span data-stu-id="b1d23-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="b1d23-167">Corpos do construtor primário</span><span class="sxs-lookup"><span data-stu-id="b1d23-167">Primary constructor bodies</span></span>

<span data-ttu-id="b1d23-168">Os construtores em si geralmente contêm lógica de validação de parâmetro ou outro código de inicialização não trivial que não pode ser expresso como inicializadores.</span><span class="sxs-lookup"><span data-stu-id="b1d23-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="b1d23-169">Construtores primários poderiam ser estendidos para permitir que blocos de instrução apareçam diretamente no corpo da classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="b1d23-170">Essas instruções seriam inseridas no Construtor gerado no ponto em que aparecem entre as atribuições de inicialização e, portanto, seriam executadas intercaladas com inicializadores.</span><span class="sxs-lookup"><span data-stu-id="b1d23-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="b1d23-171">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b1d23-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="b1d23-172">Campos de inicializador e funções de inicializador</span><span class="sxs-lookup"><span data-stu-id="b1d23-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="b1d23-173">Em uma classe com um construtor principal, podemos considerar declarações de campo e de método sem que os modificadores de acessibilidade sejam mais como variáveis locais e funções locais:</span><span class="sxs-lookup"><span data-stu-id="b1d23-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="b1d23-174">Assim como os parâmetros de Construtor primários, os "campos de inicializador" só seriam capturados em um campo privado real se fossem usados em membros de função.</span><span class="sxs-lookup"><span data-stu-id="b1d23-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="b1d23-175">As "funções do inicializador" só seriam consideradas para capturar os parâmetros do construtor primário e os campos de inicializador se eles eram usados em outros membros da função.</span><span class="sxs-lookup"><span data-stu-id="b1d23-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="b1d23-176">Se não forem capturadas, elas poderão ser geradas de maneira mais ideal, como as funções locais.</span><span class="sxs-lookup"><span data-stu-id="b1d23-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="b1d23-177">Assim como os parâmetros de Construtor primários, eles não estarão disponíveis por meio de acesso de membro, mas apenas como um nome simples.</span><span class="sxs-lookup"><span data-stu-id="b1d23-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="b1d23-178">Isso pode ser usado para variáveis temporárias e funções auxiliares que só são relevantes para a inicialização:</span><span class="sxs-lookup"><span data-stu-id="b1d23-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="b1d23-179">Isso pode ser muito sutil, especialmente porque a ausência de modificadores de acessibilidade em outro lugar significa apenas `private`.</span><span class="sxs-lookup"><span data-stu-id="b1d23-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="b1d23-180">Instruções do inicializador</span><span class="sxs-lookup"><span data-stu-id="b1d23-180">Initializer statements</span></span>

<span data-ttu-id="b1d23-181">Uma combinação de radicais das extensões acima, seria simplesmente permitir instruções diretamente no corpo da classe.</span><span class="sxs-lookup"><span data-stu-id="b1d23-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="b1d23-182">Essas instruções são exatamente como os corpos de Construtor intercalados propostos acima, exceto que não precisam estar entre `{ }`.</span><span class="sxs-lookup"><span data-stu-id="b1d23-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="b1d23-183">Para que isso seja suficientemente útil, as variáveis "locais" e as funções auxiliares também precisariam ser expressas no nível superior da classe, da maneira explorada na extensão "campos de inicializador e funções de inicializador" acima.</span><span class="sxs-lookup"><span data-stu-id="b1d23-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="b1d23-184">Acesso de membros</span><span class="sxs-lookup"><span data-stu-id="b1d23-184">Member access</span></span>

<span data-ttu-id="b1d23-185">A proposta principal trata os parâmetros do construtor primário como parâmetros que só podem ser chamados de nomes simples.</span><span class="sxs-lookup"><span data-stu-id="b1d23-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="b1d23-186">Uma alternativa é permitir que eles sejam referenciados como se fossem campos particulares, ou seja, com acesso de membro, *mesmo* que eles não sejam gerados como campos.</span><span class="sxs-lookup"><span data-stu-id="b1d23-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="b1d23-187">Isso permitiria que eles fossem referenciados como `this.x` quando sombreados por variáveis locais e acessados de uma instância diferente como `other.x`.</span><span class="sxs-lookup"><span data-stu-id="b1d23-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="b1d23-188">Se aplicado à extensão "campos de inicializador e funções de inicializador", isso também reduziria o grau para o qual eles eram diferentes dos membros privados comuns.</span><span class="sxs-lookup"><span data-stu-id="b1d23-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="b1d23-189">A única diferença seria que o compilador é livre para Elide-los do objeto, se usado apenas durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="b1d23-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

