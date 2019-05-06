---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229489"
---
# <a name="classes"></a><span data-ttu-id="86af4-101">Classes</span><span class="sxs-lookup"><span data-stu-id="86af4-101">Classes</span></span>

<span data-ttu-id="86af4-102">Uma classe é uma estrutura de dados que pode conter dados membros (campos e constantes), membros da função (métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e tipos aninhados.</span><span class="sxs-lookup"><span data-stu-id="86af4-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="86af4-103">Tipos de classe dá suporte à herança, um mecanismo pelo qual uma classe derivada pode estender e especializar uma classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="86af4-104">Declarações de classe</span><span class="sxs-lookup"><span data-stu-id="86af4-104">Class declarations</span></span>

<span data-ttu-id="86af4-105">Um *class_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="86af4-106">Um *class_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *class_modifier*s ([modificadores de classe](classes.md#class-modifiers)), seguido por um recurso opcional `partial` modificador, seguido da palavra-chave `class` e uma *identificador* que nomeia a classe, seguida por um opcional *type_parameter_list* ([parâmetros de tipo](classes.md#type-parameters)), seguido por um recurso opcional *class_base* especificação ([da Base de dados de classe especificação](classes.md#class-base-specification)), seguido por um conjunto opcional de *type_parameter_constraints_clause*s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), seguido por um *class_body*  ([Classe corpo](classes.md#class-body)), opcionalmente seguido por um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="86af4-107">Uma declaração de classe não pode fornecer *type_parameter_constraints_clause*s, a menos que ele também fornece um *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="86af4-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="86af4-108">Uma declaração de classe que fornece um *type_parameter_list* é um ***declaração de classe genérica***.</span><span class="sxs-lookup"><span data-stu-id="86af4-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="86af4-109">Além disso, qualquer classe aninhada dentro de uma declaração de classe genérica ou uma declaração de struct genérico é em si uma declaração de classe genérica, já que os parâmetros de tipo para o tipo de conteúdo devem ser fornecidos para criar um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="86af4-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="86af4-110">Modificadores de classe</span><span class="sxs-lookup"><span data-stu-id="86af4-110">Class modifiers</span></span>

<span data-ttu-id="86af4-111">Um *class_declaration* opcionalmente pode incluir uma sequência de modificadores de classe:</span><span class="sxs-lookup"><span data-stu-id="86af4-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="86af4-112">Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="86af4-113">O `new` modificador é permitido em classes aninhadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="86af4-114">Especifica que a classe oculta um membro herdado com o mesmo nome, conforme descrito em [o novo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="86af4-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="86af4-115">É um erro de tempo de compilação para o `new` modificador para aparecer em uma declaração de classe não é uma declaração de classe aninhada.</span><span class="sxs-lookup"><span data-stu-id="86af4-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="86af4-116">O `public`, `protected`, `internal`, e `private` modificadores de acessibilidade da classe de controle.</span><span class="sxs-lookup"><span data-stu-id="86af4-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="86af4-117">Dependendo do contexto no qual ocorre a declaração de classe, alguns desses modificadores não podem ter permissão ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="86af4-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="86af4-118">O `abstract`, `sealed` e `static` modificadores são discutidos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="86af4-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="86af4-119">Classes abstratas</span><span class="sxs-lookup"><span data-stu-id="86af4-119">Abstract classes</span></span>

<span data-ttu-id="86af4-120">O `abstract` modificador é usado para indicar que uma classe é incompleta e que ele é destinado a ser usado apenas como uma classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="86af4-121">Uma classe abstrata é diferente de uma classe não abstrata das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="86af4-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="86af4-122">Uma classe abstrata não pode ser instanciada diretamente, e é um erro de tempo de compilação para usar o `new` operador em uma classe abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="86af4-123">Embora seja possível ter variáveis e valores cujos tipos de tempo de compilação são abstratos, tais variáveis e valores serão necessariamente ser `null` ou contém referências às instâncias de classes não abstratas derivadas dos tipos abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="86af4-124">Uma classe abstrata é permitida (mas não obrigatório) para conter membros abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="86af4-125">Uma classe abstrata não pode ser sealed.</span><span class="sxs-lookup"><span data-stu-id="86af4-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="86af4-126">Quando uma classe não abstrata é derivada de uma classe abstrata, a classe não abstrata deve incluir implementações reais de todos os membros abstratos herdados, substituindo, portanto, esses membros abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="86af4-127">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="86af4-128">a classe abstrata `A` apresenta um método abstrato `F`.</span><span class="sxs-lookup"><span data-stu-id="86af4-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="86af4-129">Classe `B` apresenta um método adicional `G`, mas já que ele não fornece uma implementação de `F`, `B` também deve ser declarada como abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="86af4-130">Classe `C` substituições `F` e fornece uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="86af4-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="86af4-131">Como não há nenhum membro abstrato na `C`, `C` é permitido (mas não obrigatório) a ser não abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="86af4-132">Classes lacradas</span><span class="sxs-lookup"><span data-stu-id="86af4-132">Sealed classes</span></span>

<span data-ttu-id="86af4-133">O `sealed` modificador é usado para evitar a derivação de uma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="86af4-134">Ocorrerá um erro de tempo de compilação se uma classe selada for especificada como a classe base de outra classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="86af4-135">Uma classe selada não pode ser também uma classe abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="86af4-136">O `sealed` modificador é usado principalmente para evitar derivação não intencional, mas ele também permite que determinadas otimizações de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86af4-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="86af4-137">Em particular, como uma classe selada é conhecida nunca ter quaisquer classes derivadas, é possível transformar as invocações de membro de função virtual em instâncias de classe sealed em chamadas não virtuais.</span><span class="sxs-lookup"><span data-stu-id="86af4-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="86af4-138">Classes estáticas</span><span class="sxs-lookup"><span data-stu-id="86af4-138">Static classes</span></span>

<span data-ttu-id="86af4-139">O `static` modificador é usado para marcar a classe que está sendo declarada como um ***classe estática***.</span><span class="sxs-lookup"><span data-stu-id="86af4-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="86af4-140">Uma classe estática não pode ser instanciada, não pode ser usada como um tipo e pode conter apenas membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="86af4-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="86af4-141">Apenas uma classe estática pode conter declarações de métodos de extensão ([métodos de extensão](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="86af4-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="86af4-142">Uma declaração de classe estática está sujeito às seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="86af4-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="86af4-143">Uma classe estática não pode incluir um `sealed` ou `abstract` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="86af4-144">No entanto, observe que uma vez que uma classe estática não pode ser instanciada ou derivada, ele se comporta como se ele foi bloqueado e abstrato.</span><span class="sxs-lookup"><span data-stu-id="86af4-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="86af4-145">Uma classe estática não pode incluir um *class_base* especificação ([classe base especificação](classes.md#class-base-specification)) e não é possível especificar explicitamente uma classe base ou uma lista de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="86af4-146">Uma classe estática herda implicitamente de tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="86af4-147">Uma classe estática pode conter apenas membros estáticos ([membros estáticos e de instância](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="86af4-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="86af4-148">Observe que as constantes e tipos aninhados são classificados como membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="86af4-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="86af4-149">Uma classe estática não pode ter membros com `protected` ou `protected internal` declarado de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="86af4-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="86af4-150">Ele é um erro de tempo de compilação para violar qualquer uma dessas restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="86af4-151">Uma classe estática não tem nenhum construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-151">A static class has no instance constructors.</span></span> <span data-ttu-id="86af4-152">Não é possível declarar um construtor de instância em uma classe estática e nenhum construtor de instância padrão ([1&gt;construtores padrão](classes.md#default-constructors)) é fornecido para uma classe estática.</span><span class="sxs-lookup"><span data-stu-id="86af4-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="86af4-153">Os membros de uma classe estática não são automaticamente estáticos, e as declarações de membro devem incluir explicitamente um `static` modificador (exceto constantes e tipos aninhados).</span><span class="sxs-lookup"><span data-stu-id="86af4-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="86af4-154">Quando uma classe é aninhada dentro de uma classe estática de externa, a classe aninhada não é uma classe estática, a menos que ele inclua explicitamente um `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="86af4-155">__Referenciar tipos de classe estática__</span><span class="sxs-lookup"><span data-stu-id="86af4-155">__Referencing static class types__</span></span>

<span data-ttu-id="86af4-156">Um *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) tem permissão para fazer referência a uma classe estática se</span><span class="sxs-lookup"><span data-stu-id="86af4-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="86af4-157">O *namespace_or_type_name* é o `T` em uma *namespace_or_type_name* do formulário `T.I`, ou</span><span class="sxs-lookup"><span data-stu-id="86af4-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="86af4-158">O *namespace_or_type_name* é o `T` em uma *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) do formulário `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="86af4-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="86af4-159">Um *primary_expression* ([membros de função](expressions.md#function-members)) tem permissão para fazer referência a uma classe estática se</span><span class="sxs-lookup"><span data-stu-id="86af4-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="86af4-160">O *primary_expression* é o `E` em uma *member_access* ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) do formulário `E.I`.</span><span class="sxs-lookup"><span data-stu-id="86af4-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="86af4-161">Em qualquer outro contexto é um erro de tempo de compilação para fazer referência a uma classe estática.</span><span class="sxs-lookup"><span data-stu-id="86af4-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="86af4-162">Por exemplo, é um erro para uma classe estática para ser usado como uma classe base, um tipo constituinte ([tipos aninhados](classes.md#nested-types)) de um membro, um argumento de tipo genérico ou uma restrição de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="86af4-163">Da mesma forma, uma classe estática não pode ser usada em um tipo de matriz, um tipo de ponteiro, uma `new` expressão, uma expressão de conversão, uma `is` expressão, um `as` expressão, um `sizeof` expressão ou uma expressão de valor padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="86af4-164">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="86af4-164">Partial modifier</span></span>

<span data-ttu-id="86af4-165">O `partial` modificador é usado para indicar que este *class_declaration* é uma declaração de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="86af4-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="86af4-166">Combinam várias declarações de tipo parcial com o mesmo nome dentro de uma declaração de namespace ou tipo delimitador à declaração de um tipo de formulário, seguindo as regras especificadas na [tipos parciais](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="86af4-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="86af4-167">Tendo a declaração de uma classe distribuída em segmentos separados do texto do programa pode ser útil se esses segmentos são produzidos ou mantidos em diferentes contextos.</span><span class="sxs-lookup"><span data-stu-id="86af4-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="86af4-168">Por exemplo, uma parte de uma declaração de classe pode ser gerada, de máquina, enquanto o outro é criado manualmente.</span><span class="sxs-lookup"><span data-stu-id="86af4-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="86af4-169">Separação textual dos dois impede que atualizações por um entrem em conflito com as atualizações por outros.</span><span class="sxs-lookup"><span data-stu-id="86af4-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="86af4-170">Parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="86af4-170">Type parameters</span></span>

<span data-ttu-id="86af4-171">Um parâmetro de tipo é um identificador simples que denota um espaço reservado para um tipo de argumento fornecido para criar um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="86af4-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="86af4-172">Um parâmetro de tipo é um espaço reservado formal para um tipo que será fornecido posteriormente.</span><span class="sxs-lookup"><span data-stu-id="86af4-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="86af4-173">Por outro lado, um argumento de tipo ([argumentos de tipo](types.md#type-arguments)) é o tipo real que é substituído para o parâmetro de tipo quando um tipo construído é criado.</span><span class="sxs-lookup"><span data-stu-id="86af4-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="86af4-174">Cada parâmetro de tipo em uma declaração de classe define um nome no espaço de declaração ([declarações](basic-concepts.md#declarations)) dessa classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="86af4-175">Portanto, ele não pode ter o mesmo nome que outro parâmetro de tipo ou um membro declarado na classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="86af4-176">Um parâmetro de tipo não pode ter o mesmo nome que o próprio tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="86af4-177">Especificação de classe base</span><span class="sxs-lookup"><span data-stu-id="86af4-177">Class base specification</span></span>

<span data-ttu-id="86af4-178">Uma declaração de classe pode incluir um *class_base* especificação, que define a classe base direta da classe e as interfaces ([Interfaces](interfaces.md)) diretamente implementado pela classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="86af4-179">A classe base especificada em uma declaração de classe pode ser um tipo de classe construído ([construído tipos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="86af4-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="86af4-180">Uma classe base não pode ser um parâmetro de tipo por conta própria, embora isso pode envolver os parâmetros de tipo que estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="86af4-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="86af4-181">Classes base</span><span class="sxs-lookup"><span data-stu-id="86af4-181">Base classes</span></span>

<span data-ttu-id="86af4-182">Quando um *class_type* está incluído na *class_base*, ele especifica a classe base direta da classe que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="86af4-183">Se uma declaração de classe não tiver nenhuma *class_base*, ou se o *class_base* listas apenas tipos de interface, a classe base direta é assumida ser `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="86af4-184">Uma classe herda os membros de sua classe base direta, conforme descrito em [herança](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="86af4-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="86af4-185">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="86af4-186">classe `A` deve ser a classe base direta do `B`, e `B` deve ser derivado de `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="86af4-187">Uma vez que `A` faz não explicitamente especificar uma classe base direta, sua classe base direta é implicitamente `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="86af4-188">Para um tipo de classe construída, se uma classe base for especificada na declaração de classe genérica, a classe base do tipo construído é obtida pela substituição, para cada *type_parameter* na declaração de classe base, correspondente *type_argument* do tipo construído.</span><span class="sxs-lookup"><span data-stu-id="86af4-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="86af4-189">Dadas as declarações de classe genérica</span><span class="sxs-lookup"><span data-stu-id="86af4-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="86af4-190">a classe base do tipo construído `G<int>` seria `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="86af4-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="86af4-191">A classe base direta de um tipo de classe deve ser pelo menos tão acessíveis quanto o próprio tipo de classe ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="86af4-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="86af4-192">Por exemplo, é um erro de tempo de compilação para um `public` classe para derivar de uma `private` ou `internal` classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="86af4-193">A classe base direta de um tipo de classe não deve ser qualquer um dos seguintes tipos: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="86af4-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="86af4-194">Além disso, uma declaração de classe genérica não é possível usar `System.Attribute` como uma classe base direta ou indireta.</span><span class="sxs-lookup"><span data-stu-id="86af4-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="86af4-195">Ao determinar o significado da especificação de classe base direta `A` de uma classe `B`, a classe base direta de `B` temporariamente será considerado como `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="86af4-196">Intuitivamente, isso garante que o significado de uma especificação de classe base não pode recursivamente depender de si mesma.</span><span class="sxs-lookup"><span data-stu-id="86af4-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="86af4-197">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="86af4-198">é um erro desde na especificação de classe base `A<C.B>` a classe base direta do `C` é considerado `object`e, portanto, (pelas regras da [nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) `C` não é considerado como ter um membro `B`.</span><span class="sxs-lookup"><span data-stu-id="86af4-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="86af4-199">As classes de base de um tipo de classe são a classe base direta e suas classes base.</span><span class="sxs-lookup"><span data-stu-id="86af4-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="86af4-200">Em outras palavras, o conjunto de classes base é o fechamento transitivo da relação de classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="86af4-201">Referindo-se ao exemplo anterior, as classes base `B` estão `A` e `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="86af4-202">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="86af4-203">as classes base `D<int>` estão `C<int[]>`, `B<IComparable<int[]>>`, `A`, e `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="86af4-204">Com exceção de classe `object`, cada tipo de classe tem exatamente uma classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="86af4-205">O `object` classe não tem nenhuma classe base direta e é a classe base definitiva de todas as outras classes.</span><span class="sxs-lookup"><span data-stu-id="86af4-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="86af4-206">Quando uma classe `B` deriva de uma classe `A`, ele é um erro de tempo de compilação para `A` depender `B`.</span><span class="sxs-lookup"><span data-stu-id="86af4-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="86af4-207">Uma classe ***depende diretamente*** sua classe base direta (se houver) e ***depende diretamente*** a classe dentro do qual ele está aninhado imediatamente (se houver).</span><span class="sxs-lookup"><span data-stu-id="86af4-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="86af4-208">Dada esta definição, o conjunto completo de classes dos quais depende de uma classe é o fechamento reflexivo e transitivo do ***depende diretamente*** relação.</span><span class="sxs-lookup"><span data-stu-id="86af4-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="86af4-209">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="86af4-210">está incorreto porque a classe depende em si.</span><span class="sxs-lookup"><span data-stu-id="86af4-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="86af4-211">Da mesma forma, o exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="86af4-212">é um erro porque as classes circularmente dependem em si.</span><span class="sxs-lookup"><span data-stu-id="86af4-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="86af4-213">Por fim, o exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="86af4-214">resulta em um erro de tempo de compilação porque `A` depende `B.C` (sua classe base direta), que depende `B` (sua classe delimitadora), que depende circularmente `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="86af4-215">Observe que uma classe não depende das classes que estão aninhadas dentro dele.</span><span class="sxs-lookup"><span data-stu-id="86af4-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="86af4-216">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="86af4-217">`B` depende `A` (porque `A` é sua classe base direta e sua classe delimitadora), mas `A` não depende `B` (como `B` não é uma classe base nem uma classe delimitadora de `A` ).</span><span class="sxs-lookup"><span data-stu-id="86af4-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="86af4-218">Portanto, o exemplo é válido.</span><span class="sxs-lookup"><span data-stu-id="86af4-218">Thus, the example is valid.</span></span>

<span data-ttu-id="86af4-219">Não é possível derivar de um `sealed` classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="86af4-220">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="86af4-221">classe `B` é um erro porque ele tenta derivar a `sealed` classe `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="86af4-222">Implementações de interfaces</span><span class="sxs-lookup"><span data-stu-id="86af4-222">Interface implementations</span></span>

<span data-ttu-id="86af4-223">Um *class_base* especificação pode incluir uma lista dos tipos de interface, na qual caso a classe deve implementar diretamente os tipos de interface fornecido.</span><span class="sxs-lookup"><span data-stu-id="86af4-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="86af4-224">Implementações de interface são discutidas mais detalhadamente em [implementações de Interface](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="86af4-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="86af4-225">Restrições de parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="86af4-225">Type parameter constraints</span></span>

<span data-ttu-id="86af4-226">Declarações de tipo e método genéricas, opcionalmente, podem especificar restrições de parâmetro de tipo, incluindo *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="86af4-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="86af4-227">Cada *type_parameter_constraints_clause* consiste o token `where`, seguido do nome de um parâmetro de tipo, seguido por dois-pontos e a lista de restrições para parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="86af4-228">Pode haver no máximo um `where` cláusula para cada parâmetro de tipo e o `where` cláusulas podem ser listadas em qualquer ordem.</span><span class="sxs-lookup"><span data-stu-id="86af4-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="86af4-229">Como o `get` e `set` tokens em um acessador de propriedade, o `where` token não é uma palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="86af4-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="86af4-230">A lista de restrições fornecido em uma `where` cláusula pode incluir qualquer um dos componentes a seguir, nesta ordem: uma única restrição primária, uma ou mais restrições secundárias e a restrição de construtor, `new()`.</span><span class="sxs-lookup"><span data-stu-id="86af4-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="86af4-231">Uma restrição primária pode ser um tipo de classe ou o ***restrição de tipo de referência*** `class` ou o ***restrição de tipo de valor*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="86af4-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="86af4-232">Uma restrição secundária pode ser um *type_parameter* ou *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="86af4-233">A restrição de tipo de referência Especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="86af4-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="86af4-234">Essa restrição atendem a todos os tipos de classe, tipos de interface, tipos de delegados, tipos de matriz e os parâmetros de tipo conhecidos por ser um tipo de referência (conforme definido abaixo).</span><span class="sxs-lookup"><span data-stu-id="86af4-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="86af4-235">A restrição de tipo de valor Especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de valor não anulável.</span><span class="sxs-lookup"><span data-stu-id="86af4-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="86af4-236">Todos os tipos de struct não anuláveis, tipos enum e parâmetros de tipo com a restrição de tipo de valor satisfazem essa restrição.</span><span class="sxs-lookup"><span data-stu-id="86af4-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="86af4-237">Observe que, embora classificado como um tipo de valor, um tipo anulável ([tipos anuláveis](types.md#nullable-types)) não satisfaz a restrição de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="86af4-238">Um parâmetro de tipo com a restrição de tipo de valor não pode ter também o *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="86af4-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="86af4-239">Tipos de ponteiro nunca são permitidos para alguns argumentos de tipo e não são considerados para satisfazer qualquer referência tipo ou valor restrições de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="86af4-240">Se uma restrição é um tipo de classe, um tipo de interface ou um parâmetro de tipo, esse tipo Especifica um mínimo "tipo base" que deve oferecer suporte a cada argumento de tipo usado para esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="86af4-241">Sempre que um tipo construído ou método genérico for usado, o argumento de tipo é verificado em relação às restrições no parâmetro de tipo em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="86af4-242">O argumento de tipo fornecido deve satisfazer as condições descritas [atendendo às restrições de](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="86af4-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="86af4-243">Um *class_type* restrição deve satisfazer as regras a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="86af4-244">O tipo deve ser um tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-244">The type must be a class type.</span></span>
*  <span data-ttu-id="86af4-245">O tipo não deve ser `sealed`.</span><span class="sxs-lookup"><span data-stu-id="86af4-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="86af4-246">O tipo não deve ser um dos seguintes tipos: `System.Array`, `System.Delegate`, `System.Enum`, ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="86af4-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="86af4-247">O tipo não deve ser `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-247">The type must not be `object`.</span></span> <span data-ttu-id="86af4-248">Como todos os tipos derivam `object`, tal restrição não terá efeito algum se ele eram permitido.</span><span class="sxs-lookup"><span data-stu-id="86af4-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="86af4-249">No máximo uma restrição para um parâmetro de tipo determinado pode ser um tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="86af4-250">Um tipo especificado como uma *interface_type* restrição deve satisfazer as regras a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="86af4-251">O tipo deve ser um tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="86af4-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="86af4-252">Um tipo não deve ser especificado mais de uma vez em um determinado `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="86af4-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="86af4-253">Em ambos os casos, a restrição pode envolver qualquer um dos parâmetros de tipo da declaração de método como parte de um tipo construído ou tipo associado e pode envolver o tipo que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="86af4-254">Qualquer tipo de classe ou interface especificado como uma restrição de parâmetro de tipo deve ser pelo menos tão acessível ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)) como o tipo genérico ou método que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="86af4-255">Um tipo especificado como uma *type_parameter* restrição deve satisfazer as regras a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="86af4-256">O tipo deve ser um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="86af4-257">Um tipo não deve ser especificado mais de uma vez em um determinado `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="86af4-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="86af4-258">Além disso não deve haver nenhum ciclo no gráfico de dependência de parâmetros de tipo, em que a dependência é uma relação transitiva definida por:</span><span class="sxs-lookup"><span data-stu-id="86af4-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="86af4-259">Se um parâmetro de tipo `T` é usado como uma restrição de parâmetro de tipo `S` , em seguida, `S` ***depende*** `T`.</span><span class="sxs-lookup"><span data-stu-id="86af4-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="86af4-260">Se um parâmetro de tipo `S` depende de um parâmetro de tipo `T` e `T` depende de um parâmetro de tipo `U` , em seguida, `S` ***depende*** `U`.</span><span class="sxs-lookup"><span data-stu-id="86af4-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="86af4-261">Dada essa relação, é um erro de tempo de compilação para um parâmetro de tipo depender de si mesma (direta ou indiretamente).</span><span class="sxs-lookup"><span data-stu-id="86af4-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="86af4-262">Todas as restrições devem ser consistentes entre os parâmetros de tipo dependente.</span><span class="sxs-lookup"><span data-stu-id="86af4-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="86af4-263">Se o parâmetro de tipo `S` depende do parâmetro de tipo `T` então:</span><span class="sxs-lookup"><span data-stu-id="86af4-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="86af4-264">`T` não deve ter a restrição de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="86af4-265">Caso contrário, `T` efetivamente o está lacrado `S` seria forçado a ter o mesmo tipo que `T`, eliminando a necessidade de dois parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="86af4-266">Se `S` tem a restrição de tipo de valor, em seguida, `T` não deve ter uma *class_type* restrição.</span><span class="sxs-lookup"><span data-stu-id="86af4-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="86af4-267">Se `S` tem uma *class_type* restrição `A` e `T` tem uma *class_type* restrição `B` e em seguida, deve haver uma conversão de identidade ou implícita fazer referência a conversão de `A` à `B` ou uma conversão de referência implícita da `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="86af4-268">Se `S` também depende do parâmetro de tipo `U` e `U` tem um *class_type* restrição `A` e `T` tem um *class_type* restrição `B` e em seguida, deve haver uma conversão de identidade ou uma conversão de referência implícita da `A` ao `B` ou uma conversão de referência implícita da `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="86af4-269">Ele é válido para `S` tenha a restrição de tipo de valor e `T` ter restrição de tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="86af4-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="86af4-270">Efetivamente, isso limita `T` para os tipos `System.Object`, `System.ValueType`, `System.Enum`e qualquer tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="86af4-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="86af4-271">Se o `where` cláusula para um parâmetro de tipo inclui uma restrição de construtor (que tem o formato `new()`), é possível usar o `new` operador para criar instâncias do tipo ([deexpressõesdecriaçãodoobjeto](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="86af4-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="86af4-272">Qualquer tipo de argumento usado para um parâmetro de tipo com uma restrição de construtor deve ter um construtor sem parâmetros público (Este construtor implicitamente existe para qualquer tipo de valor) ou ser um parâmetro de tipo com a restrição de tipo de valor ou a restrição de construtor (consulte [Restrições de parâmetro de tipo](classes.md#type-parameter-constraints) para obter detalhes).</span><span class="sxs-lookup"><span data-stu-id="86af4-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="86af4-273">A seguir estão exemplos de restrições:</span><span class="sxs-lookup"><span data-stu-id="86af4-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="86af4-274">O exemplo a seguir está em erro, porque ele faz com que uma circularidade no grafo de dependência dos parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="86af4-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="86af4-275">Os exemplos a seguir ilustram situações inválidas adicionais:</span><span class="sxs-lookup"><span data-stu-id="86af4-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="86af4-276">O ***efetivo classe base*** de um parâmetro de tipo `T` é definido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86af4-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="86af4-277">Se `T` não tem nenhuma restrição primária ou restrições de parâmetro de tipo, sua classe base eficaz é `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="86af4-278">Se `T` tem a restrição de tipo de valor, sua classe base eficaz é `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="86af4-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="86af4-279">Se `T` tem uma *class_type* restrição `C` , mas nenhum *type_parameter* restrições, sua classe base eficaz é `C`.</span><span class="sxs-lookup"><span data-stu-id="86af4-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="86af4-280">Se `T` não tem nenhum *class_type* restrição, mas tem um ou mais *type_parameter* restrições, sua classe base efetiva é o tipo mais contido ([eliminada conversão operadores](conversions.md#lifted-conversion-operators)) no conjunto de classes base efetivos de seu *type_parameter* restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="86af4-281">As regras de consistência se existe um tipo mais contido.</span><span class="sxs-lookup"><span data-stu-id="86af4-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="86af4-282">Se `T` tem um *class_type* restrição e um ou mais *type_parameter* restrições, sua classe base efetiva é o tipo mais contido ([eliminada conversão operadores](conversions.md#lifted-conversion-operators)) no conjunto que consiste o *class_type* restrição de `T` e as classes base efetivas de seu *type_parameter* restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="86af4-283">As regras de consistência se existe um tipo mais contido.</span><span class="sxs-lookup"><span data-stu-id="86af4-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="86af4-284">Se `T` tem a restrição de tipo de referência, mas não *class_type* restrições, sua classe base eficaz é `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="86af4-285">Com a finalidade dessas regras, se T tem uma restrição `V` que é um *value_type*, em vez disso, use o tipo de base mais específico `V` que é um *class_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="86af4-286">Jamais pode acontecer em uma restrição explicitamente especificada, mas pode ocorrer quando as restrições de um método genérico são implicitamente herdadas por uma declaração de método de substituição ou uma implementação explícita de um método de interface.</span><span class="sxs-lookup"><span data-stu-id="86af4-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="86af4-287">Essas regras garantem que a classe base eficaz é sempre uma *class_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="86af4-288">O ***conjunto da interface efetivo*** de um parâmetro de tipo `T` é definido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86af4-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="86af4-289">Se `T` não tem nenhum *secondary_constraints*, seu conjunto de interface efetiva está vazio.</span><span class="sxs-lookup"><span data-stu-id="86af4-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="86af4-290">Se `T` tem *interface_type* restrições, mas não *type_parameter* restrições, seu conjunto de interface eficaz é seu conjunto de *interface_type* restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="86af4-291">Se `T` não tem nenhum *interface_type* restrições, mas tem *type_parameter* restrições, seu conjunto de interface eficaz é a união dos conjuntos interface efetivo de seu *type_ parâmetro* restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="86af4-292">Se `T` tem ambos *interface_type* restrições e *type_parameter* restrições, seu conjunto de interface eficaz é a união de seu conjunto de *interface_type* restrições e os conjuntos de interface efetivo de seu *type_parameter* restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="86af4-293">É um parâmetro de tipo ***conhecido por ser um tipo de referência*** se ele tem a restrição de tipo de referência ou não é de sua classe base efetivo `object` ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="86af4-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="86af4-294">Valores de um tipo de parâmetro de tipo restrita podem ser usados para acessar os membros de instância implicados pelas restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="86af4-295">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="86af4-296">os métodos de `IPrintable` pode ser chamado diretamente nas `x` porque `T` é restrito para implementar sempre `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="86af4-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="86af4-297">Corpo da classe</span><span class="sxs-lookup"><span data-stu-id="86af4-297">Class body</span></span>

<span data-ttu-id="86af4-298">O *class_body* de uma classe define os membros dessa classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="86af4-299">Tipos parciais</span><span class="sxs-lookup"><span data-stu-id="86af4-299">Partial types</span></span>

<span data-ttu-id="86af4-300">Uma declaração de tipo pode ser dividida entre vários ***declarações de tipo parcial***.</span><span class="sxs-lookup"><span data-stu-id="86af4-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="86af4-301">A declaração de tipo é construída de suas partes, seguindo as regras nesta seção, momento em que ele será tratado como uma única declaração durante o restante do processamento de tempo de compilação e tempo de execução do programa.</span><span class="sxs-lookup"><span data-stu-id="86af4-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="86af4-302">Um *class_declaration*, *struct_declaration* ou *interface_declaration* representa uma declaração de tipo parcial se ela incluir um `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="86af4-303">`partial` não é uma palavra-chave e atua apenas como um modificador se ela aparecer imediatamente antes de uma das palavras-chave `class`, `struct` ou `interface` em uma declaração de tipo, ou antes do tipo `void` em uma declaração de método.</span><span class="sxs-lookup"><span data-stu-id="86af4-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="86af4-304">Em outros contextos, ele pode ser usado como um identificador normal.</span><span class="sxs-lookup"><span data-stu-id="86af4-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="86af4-305">Cada parte de uma declaração de tipo parcial deve incluir um `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="86af4-306">Ele deve ter o mesmo nome e ser declarado no mesmo namespace ou tipo de declaração como as outras partes.</span><span class="sxs-lookup"><span data-stu-id="86af4-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="86af4-307">O `partial` modificador indica que as partes adicionais da declaração de tipo podem existir em outro lugar, mas a existência de tais partes adicionais não é um requisito; ele é válido para um tipo com uma única declaração incluir o `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="86af4-308">Todas as partes de um tipo parcial devem ser compiladas juntos, de modo que as partes podem ser mescladas em tempo de compilação em uma única declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="86af4-309">Especificamente, tipos parciais não permitem tipos já compilados seja estendida.</span><span class="sxs-lookup"><span data-stu-id="86af4-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="86af4-310">Tipos aninhados podem ser declarados em várias partes, usando o `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="86af4-311">Normalmente, o tipo recipiente é declarado usando `partial` como bem e cada parte do tipo aninhado é declarado em uma parte diferente do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="86af4-312">O `partial` modificador não é permitido em declarações de enum ou delegado.</span><span class="sxs-lookup"><span data-stu-id="86af4-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="86af4-313">Atributos</span><span class="sxs-lookup"><span data-stu-id="86af4-313">Attributes</span></span>

<span data-ttu-id="86af4-314">Os atributos de um tipo parcial são determinados pela combinação, em uma ordem não especificada, os atributos de cada uma das partes.</span><span class="sxs-lookup"><span data-stu-id="86af4-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="86af4-315">Se um atributo é colocado em várias partes, é equivalente a especificar o atributo várias vezes no tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="86af4-316">Por exemplo, as duas partes:</span><span class="sxs-lookup"><span data-stu-id="86af4-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="86af4-317">são equivalentes a uma declaração, como:</span><span class="sxs-lookup"><span data-stu-id="86af4-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="86af4-318">Combinam atributos em parâmetros de tipo de maneira semelhante.</span><span class="sxs-lookup"><span data-stu-id="86af4-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="86af4-319">Modificadores</span><span class="sxs-lookup"><span data-stu-id="86af4-319">Modifiers</span></span>

<span data-ttu-id="86af4-320">Quando uma declaração de tipo parcial inclui uma especificação de acessibilidade (o `public`, `protected`, `internal`, e `private` modificadores) ele deve concordar com todas as outras partes que incluem uma especificação de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="86af4-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="86af4-321">Se nenhuma parte de um tipo parcial inclui uma especificação de acessibilidade, o tipo é determinado a acessibilidade padrão apropriado ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="86af4-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="86af4-322">Se um ou mais declarações parciais de um tipo aninhado incluem um `new` modificador, nenhum aviso é relatado se o tipo aninhado oculta um membro herdado ([ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="86af4-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="86af4-323">Se um ou mais declarações parciais de uma classe incluem uma `abstract` modificador, a classe será considerada abstrata ([classes abstratas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="86af4-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="86af4-324">Caso contrário, a classe é considerada não-abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="86af4-325">Se um ou mais declarações parciais de uma classe incluem uma `sealed` modificador, a classe será considerada lacrado ([lacrados classes](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="86af4-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="86af4-326">Caso contrário, a classe é considerada sem lacre.</span><span class="sxs-lookup"><span data-stu-id="86af4-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="86af4-327">Observe que uma classe não pode ser abstract e sealed.</span><span class="sxs-lookup"><span data-stu-id="86af4-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="86af4-328">Quando o `unsafe` modificador é usado em uma declaração de tipo parcial, somente essa parte é considerada um contexto inseguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="86af4-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="86af4-329">Parâmetros de tipo e restrições</span><span class="sxs-lookup"><span data-stu-id="86af4-329">Type parameters and constraints</span></span>

<span data-ttu-id="86af4-330">Se um tipo genérico é declarado em várias partes, cada parte deve declarar os parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="86af4-331">Cada parte deve ter o mesmo número de parâmetros de tipo e o mesmo nome para cada parâmetro de tipo, em ordem.</span><span class="sxs-lookup"><span data-stu-id="86af4-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="86af4-332">Quando uma declaração de tipo genérico parcial inclui restrições (`where` cláusulas), as restrições devem concordar com todas as outras partes que incluem restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="86af4-333">Especificamente, cada parte que inclui as restrições deve ter restrições para o mesmo conjunto de parâmetros de tipo, e para cada parâmetro de tipo dos conjuntos de primário, secundário e restrições de construtor devem ser equivalente.</span><span class="sxs-lookup"><span data-stu-id="86af4-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="86af4-334">Dois conjuntos de restrições são equivalentes se eles contiverem os mesmos membros.</span><span class="sxs-lookup"><span data-stu-id="86af4-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="86af4-335">Se nenhuma parte de um tipo genérico parcial especifica restrições de parâmetro de tipo, o tipo de parâmetros são considerados sem restrição.</span><span class="sxs-lookup"><span data-stu-id="86af4-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="86af4-336">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="86af4-337">é correto porque as partes que incluem restrições (os dois primeiros) efetivamente especificam o mesmo conjunto de primário, secundário e restrições de construtor para o mesmo conjunto de parâmetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="86af4-338">Classe base</span><span class="sxs-lookup"><span data-stu-id="86af4-338">Base class</span></span>

<span data-ttu-id="86af4-339">Quando uma declaração de classe parcial inclui uma especificação de classe base, ele deve concordar com todas as outras partes que incluem uma especificação de classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="86af4-340">Se nenhuma parte de uma classe parcial inclui uma especificação de classe base, torna-se a classe base `System.Object` ([classes Base](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="86af4-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="86af4-341">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="86af4-341">Base interfaces</span></span>

<span data-ttu-id="86af4-342">O conjunto de interfaces base para um tipo declarado em várias partes é a união das interfaces base especificada em cada parte.</span><span class="sxs-lookup"><span data-stu-id="86af4-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="86af4-343">Uma interface de base específica somente pode ser nomeada de uma vez em cada parte, mas é permitido para várias partes nomear as mesmas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="86af4-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="86af4-344">Deve haver apenas uma implementação dos membros de uma determinada interface base.</span><span class="sxs-lookup"><span data-stu-id="86af4-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="86af4-345">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="86af4-346">o conjunto de interfaces base para a classe `C` está `IA`, `IB`, e `IC`.</span><span class="sxs-lookup"><span data-stu-id="86af4-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="86af4-347">Normalmente, cada parte fornece uma implementação de interface (s) declarado na parte em questão; No entanto, isso não é um requisito.</span><span class="sxs-lookup"><span data-stu-id="86af4-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="86af4-348">Uma parte pode fornecer a implementação de uma interface declarada em uma parte diferente:</span><span class="sxs-lookup"><span data-stu-id="86af4-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="86af4-349">Membros</span><span class="sxs-lookup"><span data-stu-id="86af4-349">Members</span></span>

<span data-ttu-id="86af4-350">Com exceção dos métodos parciais ([métodos parciais](classes.md#partial-methods)), o conjunto de membros de um tipo declarado em várias partes é simplesmente a união de conjunto de membros declarados em cada parte.</span><span class="sxs-lookup"><span data-stu-id="86af4-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="86af4-351">Os corpos de todas as partes da declaração do tipo compartilham o mesmo espaço de declaração ([declarações](basic-concepts.md#declarations)) e o escopo de cada membro ([escopos](basic-concepts.md#scopes)) estende a órgãos de desenvolvimento de todas as partes.</span><span class="sxs-lookup"><span data-stu-id="86af4-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="86af4-352">O domínio de acessibilidade de qualquer membro sempre inclui todas as partes do tipo delimitador; um `private` membro declarado em uma parte está livremente acessível de outra parte.</span><span class="sxs-lookup"><span data-stu-id="86af4-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="86af4-353">Ele é um erro de tempo de compilação para declarar o mesmo membro em mais de uma parte do tipo, a menos que esse membro é um tipo com o `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="86af4-354">A ordenação dos membros dentro de um tipo é raramente significativo ao código C#, mas pode ser significativo ao fazer interface com outras linguagens e ambientes.</span><span class="sxs-lookup"><span data-stu-id="86af4-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="86af4-355">Nesses casos, a ordenação dos membros dentro de um tipo declarado em várias partes é indefinido.</span><span class="sxs-lookup"><span data-stu-id="86af4-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="86af4-356">Métodos parciais</span><span class="sxs-lookup"><span data-stu-id="86af4-356">Partial methods</span></span>

<span data-ttu-id="86af4-357">Métodos parciais podem ser definidos em uma parte de uma declaração de tipo e implementados em outro.</span><span class="sxs-lookup"><span data-stu-id="86af4-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="86af4-358">A implementação é opcional. Se nenhuma parte implementa o método parcial, a declaração de método parcial e todas as chamadas para ele são removidas da declaração de tipo resultante da combinação das partes.</span><span class="sxs-lookup"><span data-stu-id="86af4-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="86af4-359">Métodos parciais não é possível definir os modificadores de acesso, mas são implicitamente `private`.</span><span class="sxs-lookup"><span data-stu-id="86af4-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="86af4-360">Tipo de retorno deve ser `void`, e seus parâmetros não podem ter o `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="86af4-361">O identificador `partial` é reconhecido como uma palavra-chave especial em uma declaração de método somente se ela aparecer imediatamente antes o `void` tipo; caso contrário, ele pode ser usado como um identificador normal.</span><span class="sxs-lookup"><span data-stu-id="86af4-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="86af4-362">Um método parcial não pode implementar métodos de interface explicitamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="86af4-363">Há dois tipos de declaração de método parcial: Se o corpo da declaração do método é um ponto e vírgula, a declaração é considerada um ***definindo a declaração de método parcial***.</span><span class="sxs-lookup"><span data-stu-id="86af4-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="86af4-364">Se o corpo é fornecido como uma *bloco*, a declaração deve ser um ***implementar a declaração de método parcial***.</span><span class="sxs-lookup"><span data-stu-id="86af4-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="86af4-365">Entre as partes de uma declaração de tipo pode haver apenas uma definição de declaração de método parcial com uma determinada assinatura e pode haver apenas uma implementação de declaração de método parcial com uma determinada assinatura.</span><span class="sxs-lookup"><span data-stu-id="86af4-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="86af4-366">Se uma declaração de método parcial de implementação é fornecida, uma definição de declaração de método parcial correspondente deve existir e as declarações devem corresponder como especificado no seguinte:</span><span class="sxs-lookup"><span data-stu-id="86af4-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="86af4-367">As declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem), nome do método, o número de parâmetros de tipo e o número de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="86af4-368">Parâmetros correspondentes nas declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem) e os mesmos tipos (módulo diferenças nos nomes de parâmetro de tipo).</span><span class="sxs-lookup"><span data-stu-id="86af4-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="86af4-369">Parâmetros de tipo correspondente nas declarações devem ter as mesmas restrições (módulo diferenças nos nomes de parâmetro de tipo).</span><span class="sxs-lookup"><span data-stu-id="86af4-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="86af4-370">Uma declaração de método parcial de implementação pode aparecer na mesma parte da declaração de método parcial de definição correspondente.</span><span class="sxs-lookup"><span data-stu-id="86af4-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="86af4-371">Um método definição parcial participa da resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="86af4-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="86af4-372">Portanto, se uma declaração de implementação é fornecida, expressões de invocação podem resolver para invocações de método parcial.</span><span class="sxs-lookup"><span data-stu-id="86af4-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="86af4-373">Como um método parcial sempre retorna `void`, tais expressões de invocação sempre será instruções de expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="86af4-374">Além disso, como um método parcial é implicitamente `private`, tais instruções sempre ocorrerá dentro de uma das partes da declaração de tipo dentro do qual o método parcial é declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="86af4-375">Se nenhuma parte de uma declaração de tipo parcial contém uma declaração de implementação para um determinado método parcial, qualquer instrução de expressão invocá-lo é simplesmente removida da declaração de tipo combinado.</span><span class="sxs-lookup"><span data-stu-id="86af4-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="86af4-376">Portanto, a expressão de invocação, incluindo todas as expressões constituintes, não tem nenhum efeito em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86af4-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="86af4-377">O método parcial em si também é removido e não será um membro da declaração de tipo combinado.</span><span class="sxs-lookup"><span data-stu-id="86af4-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="86af4-378">Se existir uma declaração de implementação para um determinado método parcial, as invocações de métodos parciais são mantidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="86af4-379">O método parcial dá origem a uma declaração de método, semelhante à declaração de método parcial de implementação, exceto os seguintes:</span><span class="sxs-lookup"><span data-stu-id="86af4-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="86af4-380">O `partial` modificador não está incluído</span><span class="sxs-lookup"><span data-stu-id="86af4-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="86af4-381">Os atributos na declaração de método resultantes são os atributos combinados da definição e a declaração de método parcial de implementação em ordem não especificada.</span><span class="sxs-lookup"><span data-stu-id="86af4-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="86af4-382">Duplicatas não são removidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="86af4-383">Os atributos nos parâmetros de declaração de método resultantes são os atributos combinados dos parâmetros correspondentes da definição e a declaração de método parcial de implementação em ordem não especificada.</span><span class="sxs-lookup"><span data-stu-id="86af4-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="86af4-384">Duplicatas não são removidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-384">Duplicates are not removed.</span></span>

<span data-ttu-id="86af4-385">Se uma declaração de definição, mas não uma declaração de implementação é fornecido para um método parcial M, as seguintes restrições se aplicam:</span><span class="sxs-lookup"><span data-stu-id="86af4-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="86af4-386">É um erro de tempo de compilação para criar um delegado de método ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="86af4-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="86af4-387">É um erro de tempo de compilação para fazer referência a `M` dentro de uma função anônima que é convertida em um tipo de árvore de expressão ([avaliação das conversões de função anônima para tipos de árvore de expressão](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="86af4-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="86af4-388">Expressões que ocorrem como parte de uma invocação de `M` não afetam o estado de atribuição definitiva ([atribuição definitiva](variables.md#definite-assignment)), que potencialmente podem levar a erros de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="86af4-389">`M` não pode ser o ponto de entrada para um aplicativo ([inicialização do aplicativo](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="86af4-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="86af4-390">Métodos parciais são úteis para permitir que uma parte de uma declaração de tipo para personalizar o comportamento da outra parte, por exemplo, aquele que é gerado por uma ferramenta.</span><span class="sxs-lookup"><span data-stu-id="86af4-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="86af4-391">Considere a seguinte declaração de classe parcial:</span><span class="sxs-lookup"><span data-stu-id="86af4-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="86af4-392">Se essa classe será compilada sem outras partes, as declarações de método parcial a definição e suas invocações serão removidas e a declaração de classe combinado resultante será equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="86af4-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="86af4-393">Suponha que outra parte for fornecido, no entanto, que fornece declarações de implementação dos métodos parciais:</span><span class="sxs-lookup"><span data-stu-id="86af4-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="86af4-394">Em seguida, a declaração de classe combinado resultante será equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="86af4-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="86af4-395">Associação de nome</span><span class="sxs-lookup"><span data-stu-id="86af4-395">Name binding</span></span>

<span data-ttu-id="86af4-396">Embora cada parte de um tipo extensível deve ser declarado no mesmo namespace, as partes normalmente são gravadas dentro de declarações de namespace diferente.</span><span class="sxs-lookup"><span data-stu-id="86af4-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="86af4-397">Assim, diferentes `using` diretivas ([diretivas Using](namespaces.md#using-directives)) podem estar presentes para cada parte.</span><span class="sxs-lookup"><span data-stu-id="86af4-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="86af4-398">Ao interpretar os nomes simples ([inferência](expressions.md#type-inference)) dentro de uma parte, apenas o `using` diretivas das ções do namespace delimitador essa parte são consideradas.</span><span class="sxs-lookup"><span data-stu-id="86af4-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="86af4-399">Isso pode resultar no mesmo identificador de ter diferentes significados em partes diferentes:</span><span class="sxs-lookup"><span data-stu-id="86af4-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="86af4-400">Membros de classe</span><span class="sxs-lookup"><span data-stu-id="86af4-400">Class members</span></span>

<span data-ttu-id="86af4-401">Os membros de uma classe consistem em membros introduzidos por seus *class_member_declaration*s e os membros herdados da classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="86af4-402">Os membros de um tipo de classe são divididos nas seguintes categorias:</span><span class="sxs-lookup"><span data-stu-id="86af4-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="86af4-403">Constantes, que representam valores constantes associados com a classe ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="86af4-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="86af4-404">Campos, que são as variáveis da classe ([campos](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="86af4-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="86af4-405">Métodos que implementam o cálculos e ações que podem ser executadas pela classe ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="86af4-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="86af4-406">Propriedades que definem as características de chamada e as ações associadas à leitura e gravação a essas características ([propriedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="86af4-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="86af4-407">Eventos, que definem as notificações que podem ser geradas pela classe ([eventos](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="86af4-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="86af4-408">Indexadores, que permitem que instâncias da classe a serem indexados da mesma forma (sintaticamente) como matrizes ([indexadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="86af4-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="86af4-409">Operadores, que definem os operadores de expressão que podem ser aplicados a instâncias da classe ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="86af4-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="86af4-410">Construtores, que implementam as ações necessárias para inicializar instâncias da classe de instância ([construtores de instância](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="86af4-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="86af4-411">Destruidores, que implementam as ações a serem executadas antes de instâncias da classe serem descartadas permanentemente ([destruidores](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="86af4-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="86af4-412">Construtores estáticos, que implementam as ações necessárias para inicializar a classe em si ([construtores estáticos](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="86af4-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="86af4-413">Tipos que representam os tipos que são locais para a classe ([tipos aninhados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="86af4-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="86af4-414">Os membros que podem conter código executável são conhecidos coletivamente como o *membros de função* do tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="86af4-415">Os membros da função de um tipo de classe são os métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos desse tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="86af4-416">Um *class_declaration* cria um novo espaço de declaração ([declarações](basic-concepts.md#declarations)) e o *class_member_declaration*s imediatamente contido pelo *classe _declaration* introduzir novos membros nesse espaço de declaração.</span><span class="sxs-lookup"><span data-stu-id="86af4-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="86af4-417">As seguintes regras se aplicam a *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="86af4-418">Construtores de instância, destruidores e construtores estáticos devem ter o mesmo nome que a classe delimitadora.</span><span class="sxs-lookup"><span data-stu-id="86af4-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="86af4-419">Todos os outros membros devem ter nomes que diferem do nome da classe delimitadora imediatamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="86af4-420">O nome de uma constante, campo, propriedade, evento ou tipo deve ser diferente dos nomes de todos os outros membros declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="86af4-421">O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="86af4-422">Além disso, a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe não podem ter assinaturas que diferem apenas por `ref` e `out`.</span><span class="sxs-lookup"><span data-stu-id="86af4-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="86af4-423">A assinatura de um construtor de instância deve ser diferente das assinaturas de todos os outros construtores de instância declarados na mesma classe e dois construtores declarados na mesma classe não podem ter assinaturas que diferem somente por `ref` e `out`.</span><span class="sxs-lookup"><span data-stu-id="86af4-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="86af4-424">A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="86af4-425">A assinatura de um operador deve ser diferente das assinaturas de todos os outros operadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="86af4-426">Os membros herdados de um tipo de classe ([herança](classes.md#inheritance)) não fazem parte do espaço da declaração de uma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="86af4-427">Portanto, uma classe derivada tem permissão para declarar um membro com o mesmo nome ou assinatura como um membro herdado (que na verdade oculta o membro herdado).</span><span class="sxs-lookup"><span data-stu-id="86af4-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="86af4-428">O tipo de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-428">The instance type</span></span>

<span data-ttu-id="86af4-429">Cada declaração de classe tem um tipo de limite associado ([associado e não associados a tipos](types.md#bound-and-unbound-types)), o ***tipo de instância***.</span><span class="sxs-lookup"><span data-stu-id="86af4-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="86af4-430">Para uma declaração de classe genérica, o tipo de instância é formado pela criação de um tipo construído ([construído tipos](types.md#constructed-types)) da declaração de tipo, com cada um do tipo fornecido argumentos que são correspondentes parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="86af4-431">Como o tipo de instância usa os parâmetros de tipo, ele só pode ser usado em que os parâmetros de tipo estão no escopo; ou seja, dentro da declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="86af4-432">O tipo de instância é o tipo de `this` para código escrito na declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="86af4-433">Para classes não genéricas, o tipo de instância é simplesmente a classe declarada.</span><span class="sxs-lookup"><span data-stu-id="86af4-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="86af4-434">O exemplo a seguir mostra várias declarações de classe, juntamente com seus tipos de instância:</span><span class="sxs-lookup"><span data-stu-id="86af4-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="86af4-435">Membros de tipos construídos</span><span class="sxs-lookup"><span data-stu-id="86af4-435">Members of constructed types</span></span>

<span data-ttu-id="86af4-436">Os membros não herdado de um tipo construído são obtidos pela substituição, para cada *type_parameter* na declaração de membro, correspondente *type_argument* do tipo construído.</span><span class="sxs-lookup"><span data-stu-id="86af4-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="86af4-437">O processo de substituição se baseia o significado semântico de declarações de tipo e não é substituição simplesmente textual.</span><span class="sxs-lookup"><span data-stu-id="86af4-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="86af4-438">Por exemplo, dada a declaração de classe genérica</span><span class="sxs-lookup"><span data-stu-id="86af4-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="86af4-439">o tipo construído `Gen<int[],IComparable<string>>` tem os seguintes membros:</span><span class="sxs-lookup"><span data-stu-id="86af4-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="86af4-440">O tipo do membro `a` na declaração de classe genérica `Gen` é "matriz bidimensional de `T`", de modo que o tipo do membro `a` no tipo construído acima é "matriz bidimensional de matriz unidimensional de `int`", ou `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="86af4-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="86af4-441">Dentro de membros da função de instância, o tipo de `this` é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da declaração do recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="86af4-442">Todos os membros de uma classe genérica podem usar parâmetros de tipo de qualquer classe delimitadora, diretamente ou como parte de um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="86af4-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="86af4-443">Quando um determinado fechada tipo construído ([aberto e fechado tipos](types.md#open-and-closed-types)) é usado em tempo de execução, cada uso de um parâmetro de tipo é substituído com o argumento de tipo real fornecido para o tipo construído.</span><span class="sxs-lookup"><span data-stu-id="86af4-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="86af4-444">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="86af4-445">Herança</span><span class="sxs-lookup"><span data-stu-id="86af4-445">Inheritance</span></span>

<span data-ttu-id="86af4-446">Uma classe ***herda*** os membros de seu tipo de classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="86af4-447">Herança significa que uma classe contém implicitamente todos os membros de seu tipo de classe base direta, exceto para os construtores de instância, destruidores e construtores estáticos da classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="86af4-448">Alguns aspectos importantes de herança são:</span><span class="sxs-lookup"><span data-stu-id="86af4-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="86af4-449">A herança é transitiva.</span><span class="sxs-lookup"><span data-stu-id="86af4-449">Inheritance is transitive.</span></span> <span data-ttu-id="86af4-450">Se `C` deriva `B`, e `B` é derivado de `A`, em seguida, `C` herda os membros declarados na `B` , bem como os membros declarados na `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="86af4-451">Uma classe derivada estende sua classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="86af4-452">Uma classe derivada pode adicionar novos membros aos que ela herda, mas ela não pode remover a definição de um membro herdado.</span><span class="sxs-lookup"><span data-stu-id="86af4-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="86af4-453">Construtores de instância, destruidores e construtores estáticos não são herdados, mas são todos os outros membros, independentemente de sua acessibilidade declarada ([acesso de membro](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="86af4-454">No entanto, dependendo de sua acessibilidade declarada, os membros herdados podem não estar acessíveis em uma classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86af4-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="86af4-455">Uma classe derivada pode ***ocultar*** ([ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance)) membros herdados, declarando novos membros com o mesmo nome ou assinatura.</span><span class="sxs-lookup"><span data-stu-id="86af4-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="86af4-456">Observe entretanto que ocultar um membro herdado não remove esse membro — ele apenas tornará esse membro inacessível diretamente por meio da classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86af4-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="86af4-457">Uma instância de uma classe contém um conjunto de todos os campos de instância declarada na classe e suas classes base e uma conversão implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe a partir de um tipo de classe derivada para qualquer um dos seus tipos de classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="86af4-458">Portanto, uma referência a uma instância de uma classe derivada pode ser tratada como uma referência a uma instância de qualquer uma das suas classes base.</span><span class="sxs-lookup"><span data-stu-id="86af4-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="86af4-459">Uma classe pode declarar indexadores, propriedades e métodos virtuais, e as classes derivadas podem substituir a implementação desses membros de função.</span><span class="sxs-lookup"><span data-stu-id="86af4-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="86af4-460">Isso permite que classes apresentam comportamento polimórfico, no qual as ações executadas por uma invocação de membro de função varia de acordo com o tipo de tempo de execução da instância por meio do qual esse membro da função é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="86af4-461">O membro herdado de um tipo de classe construída são os membros do tipo de classe base imediata ([classes Base](classes.md#base-classes)), que é encontrado, substituindo os argumentos de tipo do tipo construído para cada ocorrência do tipo correspondente parâmetros de *class_base* especificação.</span><span class="sxs-lookup"><span data-stu-id="86af4-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="86af4-462">Esses membros, por sua vez, são transformados pelo substituindo, para cada *type_parameter* na declaração de membro, correspondente *type_argument* dos *class_base* especificação.</span><span class="sxs-lookup"><span data-stu-id="86af4-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="86af4-463">No exemplo acima, o tipo construído `D<int>` tem um membro não herdado `public int G(string s)` obtidas substituindo o argumento de tipo `int` para o parâmetro de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="86af4-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="86af4-464">`D<int>` também tem um membro herdado da declaração de classe `B`.</span><span class="sxs-lookup"><span data-stu-id="86af4-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="86af4-465">Esse membro herdado é determinado pelo primeiro determinando o tipo de classe base `B<int[]>` dos `D<int>` substituindo `int` para `T` na especificação de classe base `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="86af4-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="86af4-466">Em seguida, como um argumento de tipo para `B`, `int[]` substituirá `U` na `public U F(long index)`, produzindo o membro herdado `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="86af4-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="86af4-467">O modificador new</span><span class="sxs-lookup"><span data-stu-id="86af4-467">The new modifier</span></span>

<span data-ttu-id="86af4-468">Um *class_member_declaration* tem permissão para declarar um membro com o mesmo nome ou assinatura como um membro herdado.</span><span class="sxs-lookup"><span data-stu-id="86af4-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="86af4-469">Quando isso ocorrer, o membro da classe derivada é considerado ***ocultar*** membro da classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="86af4-470">Ocultar um membro herdado não é considerado um erro, mas ele faz com que o compilador emita um aviso.</span><span class="sxs-lookup"><span data-stu-id="86af4-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="86af4-471">Para suprimir o aviso, a declaração do membro da classe derivada pode incluir um `new` modificador para indicar que o membro derivado destina-se para ocultar o membro base.</span><span class="sxs-lookup"><span data-stu-id="86af4-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="86af4-472">Este tópico é discutido mais detalhadamente em [ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="86af4-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="86af4-473">Se um `new` modificador é incluído em uma declaração que não oculta um membro herdado, um aviso para esse efeito é emitido.</span><span class="sxs-lookup"><span data-stu-id="86af4-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="86af4-474">Esse aviso é suprimido, removendo o `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="86af4-475">Modificadores de acesso</span><span class="sxs-lookup"><span data-stu-id="86af4-475">Access modifiers</span></span>

<span data-ttu-id="86af4-476">Um *class_member_declaration* pode ter qualquer um dos cinco tipos possíveis de acessibilidade declarada ([declarado acessibilidade](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , ou `private`.</span><span class="sxs-lookup"><span data-stu-id="86af4-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="86af4-477">Exceto para o `protected internal` combinação, é um erro de tempo de compilação para especificar mais de um modificador de acesso.</span><span class="sxs-lookup"><span data-stu-id="86af4-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="86af4-478">Quando um *class_member_declaration* não inclui nenhum modificador de acesso, `private` será assumido.</span><span class="sxs-lookup"><span data-stu-id="86af4-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="86af4-479">Tipos constituintes</span><span class="sxs-lookup"><span data-stu-id="86af4-479">Constituent types</span></span>

<span data-ttu-id="86af4-480">Tipos que são usados na declaração de membro são chamados os tipos de membros daquele membro.</span><span class="sxs-lookup"><span data-stu-id="86af4-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="86af4-481">Os tipos possíveis constituintes são o tipo de uma constante, campo, propriedade, indexador ou evento, o tipo de retorno de um método ou operador e os tipos de parâmetro de um método, indexador, operador ou Construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="86af4-482">Os tipos constituintes de um membro devem ser pelo menos tão acessíveis quanto o próprio membro ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="86af4-483">Membros estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-483">Static and instance members</span></span>

<span data-ttu-id="86af4-484">Membros de uma classe são ***membros estáticos*** ou ***membros de instância***.</span><span class="sxs-lookup"><span data-stu-id="86af4-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="86af4-485">Em termos gerais, é útil pensar em membros estáticos como pertencente a tipos de classes e membros de instância como pertencentes a objetos (instâncias de tipos de classe).</span><span class="sxs-lookup"><span data-stu-id="86af4-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="86af4-486">Quando uma declaração de campo, método, propriedade, evento, operador ou construtor inclui um `static` modificador, ele declara um membro estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="86af4-487">Além disso, uma declaração de constante ou um tipo declara implicitamente um membro estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="86af4-488">Membros estáticos têm as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="86af4-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="86af4-489">Quando um membro estático `M` é referenciada em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, `E` deve indicar um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="86af4-490">É um erro de tempo de compilação para `E` para denotar uma instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="86af4-491">Um campo estático identifica exatamente um local de armazenamento para serem compartilhados por todas as instâncias de um tipo de classe fechado.</span><span class="sxs-lookup"><span data-stu-id="86af4-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="86af4-492">Não importa quantas instâncias de um tipo de classe fechados são criadas, há sempre apenas uma cópia de um campo estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="86af4-493">Um membro de função estática (método, propriedade, evento, operador ou construtor) não funciona em uma instância específica e é um erro de tempo de compilação para se referir a `this` em tal um membro da função.</span><span class="sxs-lookup"><span data-stu-id="86af4-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="86af4-494">Quando uma declaração de campo, método, propriedade, evento, indexador, construtor ou destruidor não incluir um `static` modificador, ele declara um membro de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="86af4-495">(Um membro de instância é chamado, às vezes, um membro não estático.) Membros de instância têm as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="86af4-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="86af4-496">Quando um membro de instância `M` é referenciada em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, `E` preciso marcar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="86af4-497">É um erro de tempo de associação para `E` para denotar um tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="86af4-498">Cada instância de uma classe contém um conjunto separado de todos os campos de instância da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="86af4-499">Um membro de função de instância (método, propriedade, indexador, construtor de instância ou destruidor) opera em uma determinada instância da classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="86af4-500">O exemplo a seguir ilustra as regras para acessar estáticas e membros de instância:</span><span class="sxs-lookup"><span data-stu-id="86af4-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="86af4-501">O `F` método mostra que em um membro da função de instância, uma *simple_name* ([nomes simples](expressions.md#simple-names)) pode ser usado para acessar membros de instância e membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="86af4-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="86af4-502">O `G` método mostra que em um membro da função estática, é um erro de tempo de compilação para acessar um membro de instância por meio de um *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="86af4-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="86af4-503">O `Main` método mostra que em um *member_access* ([acesso de membro](expressions.md#member-access)), membros de instância devem ser acessados por meio de instâncias e os membros estáticos devem ser acessados por meio de tipos.</span><span class="sxs-lookup"><span data-stu-id="86af4-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="86af4-504">Tipos aninhados</span><span class="sxs-lookup"><span data-stu-id="86af4-504">Nested types</span></span>

<span data-ttu-id="86af4-505">Um tipo declarado dentro de uma declaração de classe ou struct é chamado de um ***tipo aninhado***.</span><span class="sxs-lookup"><span data-stu-id="86af4-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="86af4-506">Um tipo que é declarado dentro de uma unidade de compilação ou namespace é chamado de um ***tipo não aninhado***.</span><span class="sxs-lookup"><span data-stu-id="86af4-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="86af4-507">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="86af4-508">classe `B` é um tipo aninhado porque é declarado na classe `A`e a classe `A` é um tipo não aninhado porque é declarado dentro de uma unidade de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="86af4-509">Nome totalmente qualificado</span><span class="sxs-lookup"><span data-stu-id="86af4-509">Fully qualified name</span></span>

<span data-ttu-id="86af4-510">O nome totalmente qualificado ([nomes totalmente qualificados](basic-concepts.md#fully-qualified-names)) para um tipo aninhado é `S.N` onde `S` é o nome totalmente qualificado do tipo no qual tipo `N` é declarada.</span><span class="sxs-lookup"><span data-stu-id="86af4-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="86af4-511">Acessibilidade declarada</span><span class="sxs-lookup"><span data-stu-id="86af4-511">Declared accessibility</span></span>

<span data-ttu-id="86af4-512">Tipos aninhados não podem ter `public` ou `internal` declarado acessibilidade e ter `internal` declarado acessibilidade por padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="86af4-513">Tipos aninhados podem ter essas formas de acessibilidade declarada também, além de uma ou mais formas adicionais de acessibilidade declarada, dependendo se o tipo recipiente é uma classe ou struct:</span><span class="sxs-lookup"><span data-stu-id="86af4-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="86af4-514">Um tipo aninhado que é declarado em uma classe pode ter qualquer um dos cinco formas de acessibilidade declarada (`public`, `protected internal`, `protected`, `internal`, ou `private`) e, como outros membros de classe, o padrão é `private` declarado acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="86af4-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="86af4-515">Um tipo aninhado que é declarado em um struct pode ter qualquer um dos três formas de acessibilidade declarada (`public`, `internal`, ou `private`) e, como outros membros de struct, o padrão é `private` declarado de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="86af4-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="86af4-516">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="86af4-517">declara uma classe particular aninhada `Node`.</span><span class="sxs-lookup"><span data-stu-id="86af4-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="86af4-518">Ocultando</span><span class="sxs-lookup"><span data-stu-id="86af4-518">Hiding</span></span>

<span data-ttu-id="86af4-519">Um tipo aninhado pode ocultar ([ocultar nomes](basic-concepts.md#name-hiding)) um membro base.</span><span class="sxs-lookup"><span data-stu-id="86af4-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="86af4-520">O `new` modificador é permitido em declarações de tipo aninhado, de modo que a ocultação de pode ser expressos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="86af4-521">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="86af4-522">mostra uma classe aninhada `M` que oculta o método `M` definidos no `Base`.</span><span class="sxs-lookup"><span data-stu-id="86af4-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="86af4-523">Esse acesso</span><span class="sxs-lookup"><span data-stu-id="86af4-523">this access</span></span>

<span data-ttu-id="86af4-524">Um tipo aninhado e seu tipo recipiente não têm uma relação especial em relação ao *this_access* ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="86af4-525">Especificamente, `this` dentro de um tipo aninhado não pode ser usado para fazer referência a membros de instância do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="86af4-526">Em casos onde um tipo aninhado precisa acessar os membros de instância de seu tipo recipiente, o acesso pode ser fornecido, fornecendo o `this` para a instância do tipo recipiente, como um argumento de construtor para o tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="86af4-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="86af4-527">O exemplo a seguir</span><span class="sxs-lookup"><span data-stu-id="86af4-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="86af4-528">mostra detalhes desta técnica.</span><span class="sxs-lookup"><span data-stu-id="86af4-528">shows this technique.</span></span> <span data-ttu-id="86af4-529">Uma instância do `C` cria uma instância do `Nested` e passa seu próprio `this` ao `Nested`do construtor para fornecer acesso subsequente ao `C`do membros da instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="86af4-530">Acesso a membros particulares e protegidos do tipo recipiente</span><span class="sxs-lookup"><span data-stu-id="86af4-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="86af4-531">Um tipo aninhado tem acesso a todos os membros que são acessíveis ao seu tipo recipiente, incluindo os membros do tipo recipiente que tenham `private` e `protected` declarado de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="86af4-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="86af4-532">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="86af4-533">mostra uma classe `C` que contém uma classe aninhada `Nested`.</span><span class="sxs-lookup"><span data-stu-id="86af4-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="86af4-534">Dentro de `Nested`, o método `G` chama o método estático `F` definidos no `C`, e `F` privada declarou acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="86af4-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="86af4-535">Um tipo aninhado também pode acessar membros protegidos definidos em um tipo base do seu tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="86af4-536">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="86af4-537">a classe aninhada `Derived.Nested` acessa o método protegido `F` definidos no `Derived`da classe base `Base`, por chamada por meio de uma instância de `Derived`.</span><span class="sxs-lookup"><span data-stu-id="86af4-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="86af4-538">Tipos aninhados nas classes genéricas</span><span class="sxs-lookup"><span data-stu-id="86af4-538">Nested types in generic classes</span></span>

<span data-ttu-id="86af4-539">Uma declaração de classe genérica pode conter declarações de tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="86af4-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="86af4-540">Os parâmetros de tipo da classe delimitadora podem ser usados dentro dos tipos aninhados.</span><span class="sxs-lookup"><span data-stu-id="86af4-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="86af4-541">Uma declaração de tipo aninhado pode conter parâmetros de tipo adicionais que se aplicam somente ao tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="86af4-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="86af4-542">Cada declaração de tipo contida dentro de uma declaração de classe genérica é implicitamente uma declaração de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="86af4-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="86af4-543">Quando escrever uma referência a um tipo aninhado dentro de um tipo genérico, tipo construído recipiente, incluindo seus argumentos de tipo deve ser nomeado.</span><span class="sxs-lookup"><span data-stu-id="86af4-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="86af4-544">No entanto, de dentro da classe externa, o tipo aninhado pode ser usado sem qualificação; o tipo de instância da classe externa pode ser usado implicitamente ao construir o tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="86af4-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="86af4-545">O exemplo a seguir mostra três maneiras diferentes de corretas para se referir a um tipo construído criado a partir de `Inner`; as duas primeiras são equivalentes:</span><span class="sxs-lookup"><span data-stu-id="86af4-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="86af4-546">Embora seja ruim programação estilo, um parâmetro de tipo em um tipo aninhado pode ocultar um membro ou parâmetro declarado no tipo externo do tipo:</span><span class="sxs-lookup"><span data-stu-id="86af4-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="86af4-547">Nomes de membro reservado</span><span class="sxs-lookup"><span data-stu-id="86af4-547">Reserved member names</span></span>

<span data-ttu-id="86af4-548">Para facilitar a C# tempo de execução implementação subjacente, para cada declaração de membro de origem é uma propriedade, evento ou indexador, a implementação deverá reservar duas assinaturas de método com base no tipo de declaração do membro, seu nome e seu tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="86af4-549">Ele é um erro de tempo de compilação para um programa declarar um membro cuja assinatura coincide com uma destas opções reservadas assinaturas, mesmo se a implementação de tempo de execução subjacente não faz uso dessas reservas.</span><span class="sxs-lookup"><span data-stu-id="86af4-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="86af4-550">Os nomes reservados não introduzem declarações, portanto, eles não participam de pesquisa de membro.</span><span class="sxs-lookup"><span data-stu-id="86af4-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="86af4-551">No entanto, uma declaração associada ao método reservado assinaturas participem da herança ([herança](classes.md#inheritance)) e pode ser ocultada com o `new` modificador ([o novo modificador](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="86af4-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="86af4-552">A reserva desses nomes atende a três propósitos:</span><span class="sxs-lookup"><span data-stu-id="86af4-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="86af4-553">Para permitir que a implementação subjacente usar um identificador comum como um nome de método para obter ou definir o acesso para o recurso de linguagem C#.</span><span class="sxs-lookup"><span data-stu-id="86af4-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="86af4-554">Para permitir que outras linguagens interoperar usando um identificador comum como um nome de método para obter ou definir o acesso para o recurso de linguagem C#.</span><span class="sxs-lookup"><span data-stu-id="86af4-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="86af4-555">Para ajudar a garantir que o código-fonte aceitos por um compilador de conformidade é aceito por outro, fazendo as especificidades do membro reservado nomes consistentes em todas as implementações do C#.</span><span class="sxs-lookup"><span data-stu-id="86af4-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="86af4-556">A declaração de um destruidor ([destruidores](classes.md#destructors)) também faz com que uma assinatura a ser reservado ([nomes de membro reservados para os destruidores](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="86af4-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="86af4-557">Nomes de membro para propriedades reservados</span><span class="sxs-lookup"><span data-stu-id="86af4-557">Member names reserved for properties</span></span>

<span data-ttu-id="86af4-558">Para uma propriedade `P` ([Properties](classes.md#properties)) do tipo `T`, as assinaturas a seguir são reservadas:</span><span class="sxs-lookup"><span data-stu-id="86af4-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="86af4-559">Ambas as assinaturas são reservadas, mesmo se a propriedade é somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="86af4-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="86af4-560">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="86af4-561">uma classe `A` define uma propriedade somente leitura `P`, assim, reservando assinaturas para `get_P` e `set_P` métodos.</span><span class="sxs-lookup"><span data-stu-id="86af4-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="86af4-562">Uma classe `B` deriva `A` e oculta a ambas as assinaturas reservadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="86af4-563">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="86af4-564">Nomes de membro reservados para eventos</span><span class="sxs-lookup"><span data-stu-id="86af4-564">Member names reserved for events</span></span>

<span data-ttu-id="86af4-565">Para um evento `E` ([eventos](classes.md#events)) de tipo de delegado `T`, as assinaturas a seguir são reservadas:</span><span class="sxs-lookup"><span data-stu-id="86af4-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="86af4-566">Nomes de membro reservados para indexadores</span><span class="sxs-lookup"><span data-stu-id="86af4-566">Member names reserved for indexers</span></span>

<span data-ttu-id="86af4-567">Para um indexador ([indexadores](classes.md#indexers)) do tipo `T` com a lista de parâmetros `L`, as assinaturas a seguir são reservadas:</span><span class="sxs-lookup"><span data-stu-id="86af4-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="86af4-568">Ambas as assinaturas são reservadas, mesmo que o indexador é somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="86af4-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="86af4-569">Além do nome do membro `Item` é reservado.</span><span class="sxs-lookup"><span data-stu-id="86af4-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="86af4-570">Nomes de membro reservados para os destruidores</span><span class="sxs-lookup"><span data-stu-id="86af4-570">Member names reserved for destructors</span></span>

<span data-ttu-id="86af4-571">Para uma classe que contém um destruidor ([destruidores](classes.md#destructors)), a assinatura a seguir é reservada:</span><span class="sxs-lookup"><span data-stu-id="86af4-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="86af4-572">Constantes</span><span class="sxs-lookup"><span data-stu-id="86af4-572">Constants</span></span>

<span data-ttu-id="86af4-573">Um ***constante*** é um membro da classe que representa um valor constante: um valor que pode ser calculado em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="86af4-574">Um *constant_declaration* introduz uma ou mais constantes de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="86af4-575">Um *constant_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um `new` modificador ([o novo modificador](classes.md#the-new-modifier)), e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="86af4-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="86af4-576">Os atributos e modificadores que se aplicam a todos os membros declarados pela *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="86af4-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="86af4-577">Mesmo que as constantes são consideradas membros estáticos, um *constant_declaration* não exige nem permite que um `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="86af4-578">É um erro para o mesmo modificador aparecer várias vezes em uma declaração de constante.</span><span class="sxs-lookup"><span data-stu-id="86af4-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="86af4-579">O *tipo* de uma *constant_declaration* Especifica o tipo dos membros apresentados pela declaração.</span><span class="sxs-lookup"><span data-stu-id="86af4-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="86af4-580">O tipo é seguido por uma lista de *constant_declarator*s, cada uma delas apresenta um novo membro.</span><span class="sxs-lookup"><span data-stu-id="86af4-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="86af4-581">Um *constant_declarator* consiste em um *identificador* que nomeia o membro, seguido por um "`=`" token, seguido por um *constant_expression* ([ Expressões de constante](expressions.md#constant-expressions)) que fornece o valor do membro.</span><span class="sxs-lookup"><span data-stu-id="86af4-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="86af4-582">O *tipo* especificado em uma constante de declaração deve ser `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, uma *enum_type*, ou um *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="86af4-583">Cada *constant_expression* deve produzir um valor do tipo de destino ou de um tipo que pode ser convertido para o tipo de destino por uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="86af4-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="86af4-584">O *tipo* de uma constante deve ser pelo menos tão acessível quanto a própria constante ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-585">O valor de uma constante é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="86af4-586">Uma constante em si pode participar em uma *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="86af4-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="86af4-587">Portanto, uma constante pode ser usada em qualquer construção que requer uma *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="86af4-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="86af4-588">Exemplos de tais construções `case` rótulos `goto case` instruções, `enum` declarações de membros, atributos e outras declarações de constante.</span><span class="sxs-lookup"><span data-stu-id="86af4-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="86af4-589">Conforme descrito em [expressões constantes](expressions.md#constant-expressions), um *constant_expression* é uma expressão que pode ser completamente avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="86af4-590">Como o único modo para criar um valor não nulo de uma *reference_type* diferente de `string` é aplicar o `new` operador e desde o `new` operador não é permitido em um *constant_ expressão*, o único valor possível para constantes de *reference_type*s diferente `string` é `null`.</span><span class="sxs-lookup"><span data-stu-id="86af4-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="86af4-591">Quando um nome simbólico para um valor constante for desejado, mas quando o tipo de valor não é permitido em uma declaração de constante, ou quando o valor não pode ser calculado em tempo de compilação por um *constant_expression*, um `readonly` (de campo [Campos somente leitura](classes.md#readonly-fields)) pode ser usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="86af4-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="86af4-592">Uma declaração de constante que declara várias constantes é equivalente a várias declarações de constantes únicos com o mesmo atributos, modificadores e tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="86af4-593">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="86af4-594">equivale a</span><span class="sxs-lookup"><span data-stu-id="86af4-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="86af4-595">Constantes são permitidas para dependem outras constantes dentro do mesmo programa, desde que as dependências não são de natureza circular.</span><span class="sxs-lookup"><span data-stu-id="86af4-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="86af4-596">O compilador organiza automaticamente para avaliar as declarações de constante na ordem apropriada.</span><span class="sxs-lookup"><span data-stu-id="86af4-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="86af4-597">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="86af4-598">o compilador primeiro avalia `A.Y`, em seguida, avalia `B.Z`e finalmente avalia `A.X`, produzindo os valores `10`, `11`, e `12`.</span><span class="sxs-lookup"><span data-stu-id="86af4-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="86af4-599">Declarações de constante podem depender de constantes de outros programas, mas essas dependências só são possíveis em uma única direção.</span><span class="sxs-lookup"><span data-stu-id="86af4-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="86af4-600">Referindo-se ao exemplo acima, se `A` e `B` foram declarados em programas separados, seria possível `A.X` depender `B.Z`, mas `B.Z` poderia, em seguida, não simultaneamente dependem `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="86af4-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="86af4-601">Campos</span><span class="sxs-lookup"><span data-stu-id="86af4-601">Fields</span></span>

<span data-ttu-id="86af4-602">Um ***campo*** é um membro que representa uma variável associada a um objeto ou classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="86af4-603">Um *field_declaration* apresenta um ou mais campos de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="86af4-604">Um *field_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um `new` modificador ([o novo modificador](classes.md#the-new-modifier)), um uma combinação válida de quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) e uma `static` modificador ([campos estáticos e de instância](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="86af4-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="86af4-605">Além disso, uma *field_declaration* pode incluir uma `readonly` modificador ([campos somente leitura](classes.md#readonly-fields)) ou um `volatile` modificador ([campos voláteis](classes.md#volatile-fields)), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="86af4-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="86af4-606">Os atributos e modificadores que se aplicam a todos os membros declarados pela *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="86af4-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="86af4-607">É um erro para o mesmo modificador aparecer várias vezes em uma declaração de campo.</span><span class="sxs-lookup"><span data-stu-id="86af4-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="86af4-608">O *tipo* de uma *field_declaration* Especifica o tipo dos membros apresentados pela declaração.</span><span class="sxs-lookup"><span data-stu-id="86af4-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="86af4-609">O tipo é seguido por uma lista de *variable_declarator*s, cada uma delas apresenta um novo membro.</span><span class="sxs-lookup"><span data-stu-id="86af4-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="86af4-610">Um *variable_declarator* consiste em um *identificador* que nomeia esse membro, opcionalmente seguido por um "`=`" token e uma *variable_initializer* ([ Inicializadores de variável](classes.md#variable-initializers)) que fornece o valor inicial desse membro.</span><span class="sxs-lookup"><span data-stu-id="86af4-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="86af4-611">O *tipo* de um campo deve ser pelo menos tão acessíveis quanto o próprio campo ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-612">O valor de um campo é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="86af4-613">O valor de um campo não readonly é modificado usando um *atribuição* ([operadores de atribuição](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="86af4-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="86af4-614">O valor de um campo não readonly pode ser obtida e modificados usando incremento de sufixo e operadores de decremento ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators)) e o incremento de prefixo e operadores de decremento ([prefixo operadores de incremento e decremento](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="86af4-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="86af4-615">Uma declaração de campo que declara vários campos é equivalente a várias declarações de campos únicos com o mesmo atributos, modificadores e tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="86af4-616">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="86af4-617">equivale a</span><span class="sxs-lookup"><span data-stu-id="86af4-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="86af4-618">Campos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-618">Static and instance fields</span></span>

<span data-ttu-id="86af4-619">Quando uma declaração de campo inclui um `static` modificador, os campos apresentados pela declaração estão ***campos estáticos***.</span><span class="sxs-lookup"><span data-stu-id="86af4-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="86af4-620">Quando nenhum `static` modificador estiver presente, os campos apresentados pela declaração estão ***campos de instância***.</span><span class="sxs-lookup"><span data-stu-id="86af4-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="86af4-621">Campos estáticos e campos de instância são dois dos vários tipos de variáveis ([variáveis](variables.md)) comportados pelo C# e às vezes eles são denominados ***variáveis estáticas*** e ***variáveis de instância*** , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="86af4-622">Um campo estático não é parte de uma instância específica; em vez disso, ele é compartilhado entre todas as instâncias de um tipo fechado ([aberto e fechado tipos](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="86af4-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="86af4-623">Não importa quantas instâncias de um tipo de classe fechados são criadas, há sempre apenas uma cópia de um campo estático para o domínio de aplicativo associado.</span><span class="sxs-lookup"><span data-stu-id="86af4-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="86af4-624">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="86af4-625">Um campo de instância pertence a uma instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="86af4-626">Especificamente, cada instância de uma classe contém um conjunto separado de todos os campos de instância dessa classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="86af4-627">Quando um campo é referenciado em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é um campo estático, `E` deve indicar um tipo que contém `M` e se `M` é um campo de instância, E preciso marcar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="86af4-628">As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="86af4-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="86af4-629">Campos somente leitura</span><span class="sxs-lookup"><span data-stu-id="86af4-629">Readonly fields</span></span>

<span data-ttu-id="86af4-630">Quando um *field_declaration* inclui um `readonly` modificador, os campos apresentados pela declaração são ***campos somente leitura***.</span><span class="sxs-lookup"><span data-stu-id="86af4-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="86af4-631">As atribuições diretas aos campos somente leitura só podem ocorrer como parte da declaração ou em um construtor de instância ou o construtor estático na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="86af4-632">(Um campo somente leitura pode ser atribuído a várias vezes nesses contextos.) Especificamente, as atribuições diretas para um `readonly` campo são permitidos apenas nos seguintes contextos:</span><span class="sxs-lookup"><span data-stu-id="86af4-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="86af4-633">No *variable_declarator* que apresenta o campo (incluindo um *variable_initializer* na declaração).</span><span class="sxs-lookup"><span data-stu-id="86af4-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="86af4-634">Para um campo de instância, nos construtores de instância da classe que contém a declaração de campo; para um campo estático, no construtor estático da classe que contém a declaração do campo.</span><span class="sxs-lookup"><span data-stu-id="86af4-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="86af4-635">Eles também são os únicos contextos em que ele é válido para passar uma `readonly` campo como um `out` ou `ref` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86af4-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="86af4-636">A tentativa de atribuir a um `readonly` campo ou passá-lo como um `out` ou `ref` parâmetro em qualquer outro contexto é um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="86af4-637">Usando campos somente leitura estático para constantes</span><span class="sxs-lookup"><span data-stu-id="86af4-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="86af4-638">Um `static readonly` campo é útil quando um nome simbólico para um valor constante é desejado, mas quando o tipo do valor não é permitido em um `const` declaração, ou quando o valor não pode ser calculado em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="86af4-639">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="86af4-640">o `Black`, `White`, `Red`, `Green`, e `Blue` membros não podem ser declarados como `const` membros porque seus valores não podem ser computados em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="86af4-641">No entanto, declará-las `static readonly` em vez disso, tem muito o mesmo efeito.</span><span class="sxs-lookup"><span data-stu-id="86af4-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="86af4-642">Controle de versão de campos somente leitura estático e constantes</span><span class="sxs-lookup"><span data-stu-id="86af4-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="86af4-643">Constantes e campos somente leitura têm semântica de controle de versão de binários diferentes.</span><span class="sxs-lookup"><span data-stu-id="86af4-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="86af4-644">Quando uma expressão fizer referência a uma constante, o valor da constante é obtido em tempo de compilação, mas quando uma expressão fizer referência a um campo somente leitura, o valor do campo não é obtido até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86af4-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="86af4-645">Considere um aplicativo que consiste em dois programas separados:</span><span class="sxs-lookup"><span data-stu-id="86af4-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="86af4-646">O `Program1` e `Program2` namespaces denotam dois programas que são compilados separadamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="86af4-647">Porque `Program1.Utils.X` é declarado como um campo somente leitura estático, a saída de valor a `Console.WriteLine` instrução não é conhecida no tempo de compilação, mas em vez disso, é obtida em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86af4-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="86af4-648">Portanto, se o valor de `X` é alterada e `Program1` é recompilado, o `Console.WriteLine` instrução produzirá o novo valor, mesmo se `Program2` não é recompilado.</span><span class="sxs-lookup"><span data-stu-id="86af4-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="86af4-649">No entanto, tinha `X` foi uma constante, o valor da `X` seria obtido no momento `Program2` foi compilado e não seriam afetadas pelas alterações nos `Program1` até que `Program2` é recompilado.</span><span class="sxs-lookup"><span data-stu-id="86af4-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="86af4-650">Campos voláteis</span><span class="sxs-lookup"><span data-stu-id="86af4-650">Volatile fields</span></span>

<span data-ttu-id="86af4-651">Quando um *field_declaration* inclui um `volatile` são do modificador, os campos apresentados pela declaração ***campos voláteis***.</span><span class="sxs-lookup"><span data-stu-id="86af4-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="86af4-652">Para campos de não-volátil, técnicas de otimização que reorganizar as instruções podem levar a resultados inesperados e imprevisíveis em programas multithread que acessam campos sem sincronização como a fornecida pelo *lock_statement*  ([a instrução lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="86af4-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="86af4-653">Essas otimizações podem ser executadas pelo compilador, o sistema de tempo de execução ou hardware.</span><span class="sxs-lookup"><span data-stu-id="86af4-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="86af4-654">Para os campos voláteis, essas otimizações de reordenação são restritas:</span><span class="sxs-lookup"><span data-stu-id="86af4-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="86af4-655">Uma leitura de um campo volátil é chamada um ***volátil de leitura***.</span><span class="sxs-lookup"><span data-stu-id="86af4-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="86af4-656">Uma leitura volátil tem "semântica de aquisição"; ou seja, é garantido que ele ocorrer antes de todas as referências a memória que ocorrem depois na sequência da instrução.</span><span class="sxs-lookup"><span data-stu-id="86af4-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="86af4-657">Uma gravação de um campo volátil é chamada de um ***escrita volátil***.</span><span class="sxs-lookup"><span data-stu-id="86af4-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="86af4-658">Uma escrita volátil tem "versão semântica"; ou seja, é garantido que ele ocorrer após todas as referências de memória antes da instrução de gravação na sequência da instrução.</span><span class="sxs-lookup"><span data-stu-id="86af4-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="86af4-659">Essas restrições garantem que todos os threads observem as gravações voláteis executadas por qualquer outro thread na ordem em que elas foram realizadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="86af4-660">Uma implementação em conformidade não é necessário para fornecer uma ordenação total único de gravações voláteis como visto no todos os threads de execução.</span><span class="sxs-lookup"><span data-stu-id="86af4-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="86af4-661">O tipo de um campo volátil deve ser um destes procedimentos:</span><span class="sxs-lookup"><span data-stu-id="86af4-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="86af4-662">Um *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-662">A *reference_type*.</span></span>
*  <span data-ttu-id="86af4-663">O tipo `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, ou` System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="86af4-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="86af4-664">Uma *enum_type* tem um tipo de base de enumeração de `byte`, `sbyte`, `short`, `ushort`, `int`, ou `uint`.</span><span class="sxs-lookup"><span data-stu-id="86af4-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="86af4-665">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="86af4-666">produz a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="86af4-667">Neste exemplo, o método `Main` inicia um novo thread que executa o método `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="86af4-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="86af4-668">Este método armazena um valor em um campo não volátil denominado `result`, em seguida, armazena `true` no campo volátil `finished`.</span><span class="sxs-lookup"><span data-stu-id="86af4-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="86af4-669">O thread principal aguarda o campo `finished` seja definida como `true`, em seguida, lê o campo `result`.</span><span class="sxs-lookup"><span data-stu-id="86af4-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="86af4-670">Uma vez que `finished` tiver sido declarado `volatile`, o thread principal deve ler o valor `143` do campo `result`.</span><span class="sxs-lookup"><span data-stu-id="86af4-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="86af4-671">Se o campo `finished` não tivessem sido declarados `volatile`, será permitido para o repositório para `result` fique visível para o thread principal após o armazenamento para `finished`e, portanto, para o thread principal ler o valor `0` do campo `result`.</span><span class="sxs-lookup"><span data-stu-id="86af4-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="86af4-672">Declarando `finished` como um `volatile` campo impede que qualquer inconsistência tal.</span><span class="sxs-lookup"><span data-stu-id="86af4-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="86af4-673">Inicialização do campo</span><span class="sxs-lookup"><span data-stu-id="86af4-673">Field initialization</span></span>

<span data-ttu-id="86af4-674">O valor inicial de um campo, seja em um campo estático ou um campo de instância, é o valor padrão ([valores padrão](variables.md#default-values)) do tipo do campo.</span><span class="sxs-lookup"><span data-stu-id="86af4-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="86af4-675">Não é possível observar o valor de um campo antes dessa inicialização padrão tenha ocorrido e um campo, portanto, nunca é "não inicializado".</span><span class="sxs-lookup"><span data-stu-id="86af4-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="86af4-676">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="86af4-677">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="86af4-678">porque `b` e `i` são ambos inicializadas automaticamente para valores padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="86af4-679">Inicializadores de variável</span><span class="sxs-lookup"><span data-stu-id="86af4-679">Variable initializers</span></span>

<span data-ttu-id="86af4-680">Declarações de campo podem incluir *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="86af4-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="86af4-681">Para campos estáticos, inicializadores de variável correspondem às instruções de atribuição que são executadas durante a inicialização de classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="86af4-682">Por exemplo, campos de inicializadores de variável correspondem às instruções de atribuição que são executadas quando uma instância da classe é criada.</span><span class="sxs-lookup"><span data-stu-id="86af4-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="86af4-683">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="86af4-684">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="86af4-685">porque uma atribuição para `x` ocorre quando executam os inicializadores de campo estático e as atribuições a `i` e `s` ocorrer ao executar os inicializadores de campo de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="86af4-686">A inicialização de valor padrão descrita em [inicialização do campo](classes.md#field-initialization) ocorre para todos os campos, incluindo os campos que têm inicializadores de variável.</span><span class="sxs-lookup"><span data-stu-id="86af4-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="86af4-687">Assim, quando uma classe é inicializada, todos os campos estáticos nessa classe são inicializados pela primeira vez para seus valores padrão e, em seguida, os inicializadores de campo estático são executados na ordem textual.</span><span class="sxs-lookup"><span data-stu-id="86af4-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="86af4-688">Da mesma forma, quando uma instância de uma classe é criada, todos os campos de instância nessa instância são inicializados pela primeira vez para seus valores padrão e, em seguida, os inicializadores de campo de instância são executados na ordem textual.</span><span class="sxs-lookup"><span data-stu-id="86af4-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="86af4-689">É possível para campos estáticos com inicializadores de variável a ser observado em seu estado de valor padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="86af4-690">No entanto, isso é altamente desaconselhável como uma questão de estilo.</span><span class="sxs-lookup"><span data-stu-id="86af4-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="86af4-691">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="86af4-692">exibir esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="86af4-692">exhibits this behavior.</span></span> <span data-ttu-id="86af4-693">Apesar das definições circulares de um e b, o programa é válido.</span><span class="sxs-lookup"><span data-stu-id="86af4-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="86af4-694">Isso resulta na saída</span><span class="sxs-lookup"><span data-stu-id="86af4-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="86af4-695">porque os campos estáticos `a` e `b` são inicializadas como `0` (o valor padrão para `int`) antes de seus inicializadores são executados.</span><span class="sxs-lookup"><span data-stu-id="86af4-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="86af4-696">Quando o inicializador para `a` é executado, o valor da `b` for zero e, portanto `a` é inicializado como `1`.</span><span class="sxs-lookup"><span data-stu-id="86af4-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="86af4-697">Quando o inicializador para `b` é executado, o valor da `a` já está `1`e, portanto `b` é inicializado como `2`.</span><span class="sxs-lookup"><span data-stu-id="86af4-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="86af4-698">Inicialização do campo estático</span><span class="sxs-lookup"><span data-stu-id="86af4-698">Static field initialization</span></span>

<span data-ttu-id="86af4-699">Os inicializadores de variável de campo estático de uma classe correspondem a uma sequência de atribuições que são executados na ordem textual em que aparecem na declaração da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="86af4-700">Se um construtor estático ([construtores estáticos](classes.md#static-constructors)) existe na classe, a execução de inicializadores de campo estático ocorre imediatamente antes de executar esse construtor estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="86af4-701">Caso contrário, os inicializadores de campo estático são executados em um momento dependente da implementação antes do primeiro uso de um campo estático da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="86af4-702">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="86af4-703">pode produzir a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="86af4-704">ou a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="86af4-705">porque a execução de `X`do inicializador e `Y`do inicializador pode ocorrer em qualquer ordem; eles apenas são restritos a ocorrer antes das referências a esses campos.</span><span class="sxs-lookup"><span data-stu-id="86af4-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="86af4-706">No entanto, no exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="86af4-707">a saída deve ser:</span><span class="sxs-lookup"><span data-stu-id="86af4-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="86af4-708">porque as regras para quando executam os construtores estáticos (conforme definido em [construtores estáticos](classes.md#static-constructors)) fornecê-lo `B`do construtor estático (e, portanto, `B`do inicializadores de campo estático) deve ser executado antes `A`do construtor estático e inicializadores de campo.</span><span class="sxs-lookup"><span data-stu-id="86af4-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="86af4-709">Inicialização do campo de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-709">Instance field initialization</span></span>

<span data-ttu-id="86af4-710">Os inicializadores de variável de campo de instância de uma classe correspondem a uma sequência de atribuições que são executados imediatamente após a entrada para qualquer um dos construtores de instância ([inicializadores de construtor](classes.md#constructor-initializers)) dessa classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="86af4-711">Os inicializadores de variável são executados na ordem textual em que aparecem na declaração da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="86af4-712">O processo de criação e a inicialização da instância classe é descrito posteriormente em [construtores de instância](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="86af4-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="86af4-713">Um inicializador de variável para um campo de instância não pode referenciar a instância que está sendo criada.</span><span class="sxs-lookup"><span data-stu-id="86af4-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="86af4-714">Portanto, é um erro de tempo de compilação para fazer referência a `this` em um inicializador de variável, pois ele é um erro de tempo de compilação para um inicializador de variável fazer referência a qualquer membro de instância por meio de um *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="86af4-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="86af4-715">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="86af4-716">o inicializador de variável de `y` resulta em um erro de tempo de compilação, pois ela faz referência a um membro da instância que está sendo criado.</span><span class="sxs-lookup"><span data-stu-id="86af4-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="86af4-717">Métodos</span><span class="sxs-lookup"><span data-stu-id="86af4-717">Methods</span></span>

<span data-ttu-id="86af4-718">Um ***método*** é um membro que implementa um cálculo ou uma ação que pode ser executada por um objeto ou classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="86af4-719">Os métodos são declarados usando *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="86af4-720">Um *method_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos e de instância](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([Substituir métodos](classes.md#override-methods)), `sealed` ([lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([Métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="86af4-721">Uma declaração tem uma combinação válida dos modificadores se todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="86af4-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="86af4-722">A declaração inclui uma combinação válida de modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="86af4-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="86af4-723">A declaração não inclui o mesmo modificador várias vezes.</span><span class="sxs-lookup"><span data-stu-id="86af4-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="86af4-724">A declaração inclui no máximo um dos modificadores a seguir: `static`, `virtual`, e `override`.</span><span class="sxs-lookup"><span data-stu-id="86af4-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="86af4-725">A declaração inclui no máximo um dos modificadores a seguir: `new` e `override`.</span><span class="sxs-lookup"><span data-stu-id="86af4-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="86af4-726">Se a declaração inclui o `abstract` modificador e, em seguida, a declaração não inclui nenhum dos modificadores a seguir: `static`, `virtual`, `sealed` ou `extern`.</span><span class="sxs-lookup"><span data-stu-id="86af4-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="86af4-727">Se a declaração inclui o `private` modificador e, em seguida, a declaração não inclui nenhum dos modificadores a seguir: `virtual`, `override`, ou `abstract`.</span><span class="sxs-lookup"><span data-stu-id="86af4-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="86af4-728">Se a declaração inclui o `sealed` modificador e, em seguida, a declaração também inclui o `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="86af4-729">Se a declaração inclui o `partial` modificador, em seguida, ele não inclui nenhum dos modificadores a seguir: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, ou `extern`.</span><span class="sxs-lookup"><span data-stu-id="86af4-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="86af4-730">Um método que tem o `async` modificador é uma função assíncrona e segue as regras descritas em [funções assíncronas](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="86af4-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="86af4-731">O *return_type* de um método de declaração especifica o tipo de valor calculado e retornado pelo método.</span><span class="sxs-lookup"><span data-stu-id="86af4-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="86af4-732">O *return_type* é `void` se o método não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="86af4-733">Se a declaração inclui o `partial` modificador e, em seguida, o tipo de retorno deve ser `void`.</span><span class="sxs-lookup"><span data-stu-id="86af4-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="86af4-734">O *member_name* Especifica o nome do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="86af4-735">A menos que o método é uma implementação de membro de interface explícita ([implementações de membros de interface explícita](interfaces.md#explicit-interface-member-implementations)), o *member_name* é simplesmente uma *identificador*.</span><span class="sxs-lookup"><span data-stu-id="86af4-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="86af4-736">Para uma implementação de membro de interface explícita, o *member_name* consiste em um *interface_type* seguido por um "`.`" e uma *identificador*.</span><span class="sxs-lookup"><span data-stu-id="86af4-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="86af4-737">Opcional *type_parameter_list* Especifica os parâmetros de tipo do método ([parâmetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="86af4-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="86af4-738">Se um *type_parameter_list* for especificado o método é um ***método genérico***.</span><span class="sxs-lookup"><span data-stu-id="86af4-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="86af4-739">Se o método tem um `extern` modificador, uma *type_parameter_list* não pode ser especificado.</span><span class="sxs-lookup"><span data-stu-id="86af4-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="86af4-740">Opcional *formal_parameter_list* Especifica os parâmetros do método ([parâmetros de método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="86af4-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="86af4-741">Opcional *type_parameter_constraints_clause*s especificar restrições em parâmetros de tipo individuais ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) e só pode ser especificada se uma *type_parameter_ lista* também for fornecido, e o método não tem um `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="86af4-742">O *return_type* e cada um dos tipos referenciados nas *formal_parameter_list* de um método deve ser pelo menos tão acessíveis quanto o próprio método ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-743">O *method_body* é um vírgula, um ***corpo da instrução*** ou uma ***corpo da expressão***.</span><span class="sxs-lookup"><span data-stu-id="86af4-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="86af4-744">Um corpo de declaração consiste em uma *bloco*, que especifica as instruções para executar quando o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="86af4-745">Consiste em um corpo de expressão `=>` seguido por um *expressão* e um ponto e vírgula e denota uma única expressão para executar quando o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="86af4-746">Para `abstract` e `extern` métodos, o *method_body* consiste simplesmente em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="86af4-747">Para `partial` métodos de *method_body* pode consistir em um ponto e vírgula, um corpo do bloco ou um corpo de expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="86af4-748">Para todos os outros métodos, o *method_body* é um corpo do bloco ou um corpo de expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="86af4-749">Se o *method_body* consiste em um ponto e vírgula, em seguida, a declaração não pode incluir o `async` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="86af4-750">O nome, a lista de parâmetros de tipo e a lista de parâmetros formais de um método de definem a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="86af4-751">Especificamente, a assinatura de um método consiste em seu nome, o número de parâmetros de tipo e o número, modificadores e tipos de seus parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="86af4-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="86af4-752">Para esses fins, qualquer parâmetro de tipo do método que ocorre no tipo de um parâmetro formal é identificado, não por seu nome, mas, por sua posição ordinal na lista de argumentos de tipo do método. O tipo de retorno não é parte da assinatura do método, nem é os nomes dos parâmetros de tipo ou os parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="86af4-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="86af4-753">O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="86af4-754">Além disso, a assinatura de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe não podem ter assinaturas que diferem somente por `ref` e `out`.</span><span class="sxs-lookup"><span data-stu-id="86af4-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="86af4-755">O método *type_parameter*s estão no escopo em todo o *method_declaration*e pode ser usado para tipos de formulário em todo o escopo no *return_type*, *method_body*, e *type_parameter_constraints_clause*s, mas não no *atributos*.</span><span class="sxs-lookup"><span data-stu-id="86af4-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="86af4-756">Todos os parâmetros de tipo e parâmetros formais devem ter nomes diferentes.</span><span class="sxs-lookup"><span data-stu-id="86af4-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="86af4-757">Parâmetros de método</span><span class="sxs-lookup"><span data-stu-id="86af4-757">Method parameters</span></span>

<span data-ttu-id="86af4-758">Os parâmetros de um método, se houver, são declarados pelo método *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="86af4-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="86af4-759">A lista de parâmetros formal consiste em um ou mais parâmetros separados por vírgulas dos quais apenas a última pode ser um *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="86af4-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="86af4-760">Um *fixed_parameter* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um recurso opcional `ref`, `out` ou `this` modificador, um *tipo*, um *identificador* e um opcional *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="86af4-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="86af4-761">Cada *fixed_parameter* declara um parâmetro do tipo especificado com o nome fornecido.</span><span class="sxs-lookup"><span data-stu-id="86af4-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="86af4-762">O `this` modificador designa o método como um método de extensão e só é permitido no primeiro parâmetro de um método estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="86af4-763">Métodos de extensão são descritos na [métodos de extensão](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="86af4-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="86af4-764">Um *fixed_parameter* com um *default_argument* é conhecido como um ***parâmetro opcional***, enquanto uma *fixed_parameter* sem um *default_argument* é um ***parâmetro necessário***.</span><span class="sxs-lookup"><span data-stu-id="86af4-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="86af4-765">Um parâmetro obrigatório não pode aparecer após um parâmetro opcional em uma *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="86af4-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="86af4-766">Um `ref` ou `out` parâmetro não pode ter um *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="86af4-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="86af4-767">O *expressão* em um *default_argument* deve ser um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="86af4-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="86af4-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="86af4-768">a *constant_expression*</span></span>
*  <span data-ttu-id="86af4-769">uma expressão do formulário `new S()` onde `S` é um tipo de valor</span><span class="sxs-lookup"><span data-stu-id="86af4-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="86af4-770">uma expressão do formulário `default(S)` onde `S` é um tipo de valor</span><span class="sxs-lookup"><span data-stu-id="86af4-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="86af4-771">O *expressão* deve ser implicitamente conversível por uma identidade ou uma conversão que permitem valor nulo para o tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86af4-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="86af4-772">Se os parâmetros opcionais ocorrem em uma declaração de método parcial de implementação ([métodos parciais](classes.md#partial-methods)), uma implementação de membro de interface explícita ([implementações de membros de interface explícita](interfaces.md#explicit-interface-member-implementations)) ou em um declaração de indexador único parâmetro ([indexadores](classes.md#indexers)) o compilador deve fornecer um aviso, pois esses membros nunca podem ser invocados de forma que permite que os argumentos a serem omitidos.</span><span class="sxs-lookup"><span data-stu-id="86af4-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="86af4-773">Um *parameter_array* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um `params` modificador, um *array_type*, e um *identificador*.</span><span class="sxs-lookup"><span data-stu-id="86af4-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="86af4-774">Uma matriz de parâmetros declara um único parâmetro do tipo matriz fornecida com o nome fornecido.</span><span class="sxs-lookup"><span data-stu-id="86af4-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="86af4-775">O *array_type* de um parâmetro de matriz deve ser um tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="86af4-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="86af4-776">Em uma invocação de método, uma matriz de parâmetro permite que um único argumento do tipo de matriz especificada a ser especificado ou permite a zero ou mais argumentos do tipo de elemento de matriz sejam especificados.</span><span class="sxs-lookup"><span data-stu-id="86af4-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="86af4-777">Matrizes de parâmetros são descritos mais detalhadamente em [matrizes de parâmetro](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="86af4-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="86af4-778">Um *parameter_array* pode ocorrer após um parâmetro opcional, mas não pode ter um valor padrão – a omissão de argumentos para um *parameter_array* resultaria em vez disso, a criação de uma matriz vazia.</span><span class="sxs-lookup"><span data-stu-id="86af4-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="86af4-779">O exemplo a seguir ilustra os diferentes tipos de parâmetros:</span><span class="sxs-lookup"><span data-stu-id="86af4-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="86af4-780">No *formal_parameter_list* para `M`, `i` é um parâmetro ref necessária, `d` é um parâmetro de valor necessário `b`, `s`, `o` e `t` parâmetros de valor opcional e `a` é uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="86af4-781">Uma declaração de método cria um espaço de declaração separada para parâmetros, parâmetros de tipo e variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="86af4-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="86af4-782">Nomes são apresentados nesse espaço de declaração, a lista de parâmetros de tipo e a lista de parâmetros formais do método e por declarações de variável local na *bloco* do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="86af4-783">É um erro para dois membros de um espaço de declaração de método ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="86af4-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="86af4-784">É um erro para o espaço de declaração de método e o espaço de declaração de variável local de um espaço de declaração aninhadas para conter elementos com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="86af4-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="86af4-785">Uma invocação de método ([invocações de método](expressions.md#method-invocations)) cria uma cópia específica para essa invocação, de parâmetros formais e variáveis locais do método e lista de argumentos de invocação atribui valores ou referências de variável para o recém-criados parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="86af4-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="86af4-786">Dentro de *bloco* de um método, os parâmetros formais podem ser referenciados por seus identificadores na *simple_name* expressões ([nomes simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="86af4-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="86af4-787">Há quatro tipos de parâmetros formais:</span><span class="sxs-lookup"><span data-stu-id="86af4-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="86af4-788">Parâmetros de valor, que são declarados sem qualquer modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="86af4-789">Fazer referência a parâmetros, que são declarados com o `ref` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="86af4-790">Parâmetros de saída, que são declarados com o `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="86af4-791">Matrizes de parâmetros que são declarados com o `params` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="86af4-792">Conforme descrito em [assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading), o `ref` e `out` modificadores fazem parte da assinatura do método, mas o `params` modificador não é.</span><span class="sxs-lookup"><span data-stu-id="86af4-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="86af4-793">Parâmetros de valor</span><span class="sxs-lookup"><span data-stu-id="86af4-793">Value parameters</span></span>

<span data-ttu-id="86af4-794">Um parâmetro declarado com nenhum modificador é um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="86af4-795">Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento correspondente fornecido na invocação de método.</span><span class="sxs-lookup"><span data-stu-id="86af4-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="86af4-796">Quando um parâmetro formal é um parâmetro de valor, o argumento correspondente em uma invocação de método deve ser uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="86af4-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="86af4-797">Um método tem permissão para atribuir novos valores para um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="86af4-798">Essas atribuições afetam apenas o local de armazenamento local representado pelo parâmetro de valor — eles não têm efeito sobre o argumento real fornecido na invocação do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="86af4-799">Parâmetros de referência</span><span class="sxs-lookup"><span data-stu-id="86af4-799">Reference parameters</span></span>

<span data-ttu-id="86af4-800">Um parâmetro declarado com um `ref` modificador é um parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="86af4-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="86af4-801">Ao contrário de um parâmetro de valor, um parâmetro de referência não cria um novo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="86af4-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="86af4-802">Em vez disso, um parâmetro de referência representa o mesmo local de armazenamento como a variável fornecido como o argumento na invocação de método.</span><span class="sxs-lookup"><span data-stu-id="86af4-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="86af4-803">Quando um parâmetro formal é um parâmetro de referência, o argumento correspondente em uma invocação de método deve conter a palavra-chave `ref` seguido por um *variable_reference* ([precisas regras para determinar atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo tipo que o parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="86af4-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="86af4-804">Uma variável deve ser definitivamente atribuída antes que ele pode ser passado como um parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="86af4-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="86af4-805">Dentro de um método, um parâmetro de referência é sempre considerado atribuído definitivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="86af4-806">Um método declarado como um iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de referência.</span><span class="sxs-lookup"><span data-stu-id="86af4-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="86af4-807">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="86af4-808">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="86af4-809">Para a invocação dos `Swap` na `Main`, `x` representa `i` e `y` representa `j`.</span><span class="sxs-lookup"><span data-stu-id="86af4-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="86af4-810">Portanto, a invocação tem o efeito de troca os valores das `i` e `j`.</span><span class="sxs-lookup"><span data-stu-id="86af4-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="86af4-811">Em um método que usa parâmetros de referência, é possível que vários nomes representar o mesmo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="86af4-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="86af4-812">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="86af4-813">a invocação de `F` na `G` passa uma referência a `s` para ambos `a` e `b`.</span><span class="sxs-lookup"><span data-stu-id="86af4-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="86af4-814">Assim, para essa invocação, os nomes `s`, `a`, e `b` todas se referem ao mesmo local de armazenamento e as todas as atribuições de três modificar o campo de instância `s`.</span><span class="sxs-lookup"><span data-stu-id="86af4-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="86af4-815">Parâmetros de saída</span><span class="sxs-lookup"><span data-stu-id="86af4-815">Output parameters</span></span>

<span data-ttu-id="86af4-816">Um parâmetro declarado com um `out` modificador é um parâmetro de saída.</span><span class="sxs-lookup"><span data-stu-id="86af4-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="86af4-817">Semelhante a um parâmetro de referência, um parâmetro de saída não cria um novo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="86af4-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="86af4-818">Em vez disso, um parâmetro de saída representa o mesmo local de armazenamento como a variável fornecido como o argumento na invocação de método.</span><span class="sxs-lookup"><span data-stu-id="86af4-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="86af4-819">Quando um parâmetro formal é um parâmetro de saída, o argumento correspondente em uma invocação de método deve conter a palavra-chave `out` seguido por um *variable_reference* ([precisas regras para determinar atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo tipo que o parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="86af4-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="86af4-820">Uma variável não precisa ser definitivamente atribuída antes que ela pode ser passada como um parâmetro output, mas após uma invocação em que uma variável foi passada como um parâmetro de saída, a variável é considerada atribuída definitivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="86af4-821">Dentro de um método, assim como um local variável, um parâmetro de saída é inicialmente considerado não atribuídos e deve ser definitivamente atribuído antes que seu valor é usado.</span><span class="sxs-lookup"><span data-stu-id="86af4-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="86af4-822">Cada parâmetro de saída de um método deve ser definitivamente atribuído antes que o método retorna.</span><span class="sxs-lookup"><span data-stu-id="86af4-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="86af4-823">Um método declarado como um método parcial ([métodos parciais](classes.md#partial-methods)) ou um iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de saída.</span><span class="sxs-lookup"><span data-stu-id="86af4-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="86af4-824">Parâmetros de saída normalmente são usados em métodos que produzem vários valores de retorno.</span><span class="sxs-lookup"><span data-stu-id="86af4-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="86af4-825">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="86af4-826">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="86af4-827">Observe que o `dir` e `name` variáveis podem ser atribuídas antes de serem passados para `SplitPath`, e eles são considerados definitivamente atribuídos após a chamada.</span><span class="sxs-lookup"><span data-stu-id="86af4-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="86af4-828">Matrizes de parâmetros</span><span class="sxs-lookup"><span data-stu-id="86af4-828">Parameter arrays</span></span>

<span data-ttu-id="86af4-829">Um parâmetro declarado com um `params` modificador é uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="86af4-830">Se uma lista de parâmetros formal inclui uma matriz de parâmetros, ele deve ser o último parâmetro na lista e ele deve ser de um tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="86af4-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="86af4-831">Por exemplo, os tipos `string[]` e `string[][]` pode ser usado como o tipo de uma matriz de parâmetros, mas o tipo `string[,]` não pode.</span><span class="sxs-lookup"><span data-stu-id="86af4-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="86af4-832">Não é possível combinar as `params` modificador com os modificadores `ref` e `out`.</span><span class="sxs-lookup"><span data-stu-id="86af4-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="86af4-833">Uma matriz de parâmetro permite que os argumentos a ser especificada em uma das duas maneiras de uma invocação de método:</span><span class="sxs-lookup"><span data-stu-id="86af4-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="86af4-834">O argumento fornecido para uma matriz de parâmetros pode ser uma única expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86af4-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="86af4-835">Nesse caso, a matriz de parâmetro Age exatamente como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="86af4-836">Como alternativa, a invocação pode especificar zero ou mais argumentos para a matriz de parâmetros, onde cada argumento é uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="86af4-837">Nesse caso, a invocação cria uma instância do tipo de parâmetro de matriz com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecido e usa a instância de matriz recém-criado como o real argumento.</span><span class="sxs-lookup"><span data-stu-id="86af4-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="86af4-838">Exceto para permitir que um número variável de argumentos em uma invocação, uma matriz de parâmetros é precisamente equivalente a um parâmetro de valor ([parâmetros de valores](classes.md#value-parameters)) do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="86af4-839">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="86af4-840">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="86af4-841">A primeira invocação da `F` simplesmente passa a matriz `a` como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="86af4-842">A segunda chamada de `F` cria automaticamente os quatro elementos `int[]` com os valores do elemento fornecido e passa esse instância como um parâmetro de valor de matriz.</span><span class="sxs-lookup"><span data-stu-id="86af4-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="86af4-843">Da mesma forma, a terceira invocação de `F` cria um elemento de zero `int[]` e passa essa instância como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="86af4-844">As invocações de segunda e terceira são precisamente equivalentes a gravação:</span><span class="sxs-lookup"><span data-stu-id="86af4-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="86af4-845">Ao executar a resolução de sobrecarga, um método com uma matriz de parâmetros pode ser aplicável em sua forma normal ou em sua forma expandida ([membro da função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="86af4-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="86af4-846">A forma expandida de um método está disponível somente se o formulário normal do método não é aplicável e somente se um método aplicável com a mesma assinatura que a forma expandida já não está declarado no mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="86af4-847">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="86af4-848">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="86af4-849">No exemplo, duas das formas possíveis expandidas do método com uma matriz de parâmetros já estão incluídas na classe como métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="86af4-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="86af4-850">Esses formulários expandidos, portanto, são considerados não ao executar a resolução de sobrecarga e das invocações de método primeiro e terceiro, portanto, selecione os métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="86af4-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="86af4-851">Quando uma classe declara um método com uma matriz de parâmetros, não é incomum para incluir também algumas das formas expandidas como métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="86af4-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="86af4-852">Ao fazer isso, é possível evitar a alocação de uma matriz a instância que ocorre quando uma forma expandida de um método com uma matriz de parâmetros é invocada.</span><span class="sxs-lookup"><span data-stu-id="86af4-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="86af4-853">Quando o tipo de uma matriz de parâmetros é `object[]`, uma ambiguidade potencial surge entre o formulário normal do método e o formulário expended para um único `object` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86af4-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="86af4-854">O motivo para a ambiguidade é que um `object[]` é implicitamente conversível para o tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="86af4-855">A ambiguidade não apresenta nenhum problema, no entanto, uma vez que ele pode ser resolvido com a inserção de uma conversão se necessário.</span><span class="sxs-lookup"><span data-stu-id="86af4-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="86af4-856">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="86af4-857">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="86af4-858">Nas invocações primeiros e últimas do `F`, a forma normal de `F` é aplicável porque existe uma conversão implícita do tipo de argumento para o tipo de parâmetro (ambos são do tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="86af4-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="86af4-859">Portanto, a resolução de sobrecarga seleciona a forma normal de `F`, e o argumento é passado como um parâmetro de valor comum.</span><span class="sxs-lookup"><span data-stu-id="86af4-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="86af4-860">Em que as invocações de segunda e terceira, a forma normal de `F` não é aplicável porque não existe de nenhuma conversão implícita do tipo de argumento para o tipo de parâmetro (tipo `object` não pode ser implicitamente convertido no tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="86af4-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="86af4-861">No entanto, a forma expandida do `F` é aplicável, portanto, é selecionada por resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="86af4-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="86af4-862">Como resultado, um elemento `object[]` é criado por meio da chamada, e o único elemento da matriz é inicializado com o valor do argumento fornecido (que por si só é uma referência a um `object[]`).</span><span class="sxs-lookup"><span data-stu-id="86af4-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="86af4-863">Métodos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-863">Static and instance methods</span></span>

<span data-ttu-id="86af4-864">Quando uma declaração de método inclui um `static` modificador, que o método deve ser um método estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="86af4-865">Quando nenhum `static` modificador estiver presente, o método deve ser um método de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="86af4-866">Um método estático não funciona em uma instância específica e é um erro de tempo de compilação para se referir a `this` em um método estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="86af4-867">Um método de instância opera em uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="86af4-868">Quando um método é referenciado em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é um método estático, `E` deve indicar um tipo que contém `M`e se `M` é um método de instância `E` preciso marcar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="86af4-869">As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="86af4-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="86af4-870">Métodos virtuais</span><span class="sxs-lookup"><span data-stu-id="86af4-870">Virtual methods</span></span>

<span data-ttu-id="86af4-871">Quando uma declaração de método de instância inclui um `virtual` modificador, que o método deve ser um método virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="86af4-872">Quando nenhum `virtual` modificador estiver presente, o método deve ser um método não virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="86af4-873">A implementação de um método não virtual é invariável: A implementação é o mesmo se o método é invocado em uma instância da classe na qual ele é declarado ou uma instância de uma classe derivada.</span><span class="sxs-lookup"><span data-stu-id="86af4-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="86af4-874">Por outro lado, a implementação de um método virtual pode ser substituída por classes derivadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="86af4-875">O processo de substituição a implementação de um método virtual herdado é conhecido como ***substituindo*** método ([substituir métodos](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="86af4-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="86af4-876">Em uma invocação de método virtual, o ***tipo de tempo de execução*** da instância para o qual usa essa invocação ocorre determina a implementação real do método para invocar.</span><span class="sxs-lookup"><span data-stu-id="86af4-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="86af4-877">Em uma invocação de método não virtual, o ***tipo de tempo de compilação*** da instância é o fator determinante.</span><span class="sxs-lookup"><span data-stu-id="86af4-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="86af4-878">Em termos de precisos, quando um método denominado `N` é invocado com uma lista de argumentos `A` em uma instância com um tipo de tempo de compilação `C` e um tipo de tempo de execução `R` (onde `R` seja `C` ou uma classe derivada de `C`), a invocação é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86af4-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="86af4-879">Em primeiro lugar, a resolução de sobrecarga é aplicada a `C`, `N`, e `A`para selecionar um método específico `M` do conjunto de métodos declarados em e herdada por `C`.</span><span class="sxs-lookup"><span data-stu-id="86af4-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="86af4-880">Isso é descrito em [invocações de método](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="86af4-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="86af4-881">Então, se `M` é um método não virtual, `M` é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="86af4-882">Caso contrário, `M` é um método virtual e a implementação mais derivada de `M` com relação ao `R` é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="86af4-883">Para cada método virtual declarado em ou herdadas por uma classe, existe uma ***implementação derivada mais*** do método em relação a essa classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="86af4-884">A implementação mais derivada de um método virtual `M` em relação a uma classe `R` é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86af4-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="86af4-885">Se `R` contém o apresentando `virtual` declaração de `M`, em seguida, essa é a implementação mais derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="86af4-886">Caso contrário, se `R` contém uma `override` dos `M`, em seguida, essa é a implementação mais derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="86af4-887">Caso contrário, a maioria implementação derivada da `M` em relação às `R` é o mesmo que a implementação mais derivada de `M` em relação a classe base direta de `R`.</span><span class="sxs-lookup"><span data-stu-id="86af4-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="86af4-888">O exemplo a seguir ilustra as diferenças entre os métodos virtuais e não virtuais:</span><span class="sxs-lookup"><span data-stu-id="86af4-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="86af4-889">No exemplo, `A` apresenta um método não virtual `F` e um método virtual `G`.</span><span class="sxs-lookup"><span data-stu-id="86af4-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="86af4-890">A classe `B` apresenta um novo método não virtual `F`, assim, ocultando a herdado `F`e também substitui o método herdado `G`.</span><span class="sxs-lookup"><span data-stu-id="86af4-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="86af4-891">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="86af4-892">Observe que a instrução `a.G()` invoca `B.G`, e não `A.G`.</span><span class="sxs-lookup"><span data-stu-id="86af4-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="86af4-893">Isso ocorre porque o tempo de execução de tipo da instância (que é `B`), não o tipo de tempo de compilação da instância (que é `A`), determina a implementação real do método para invocar.</span><span class="sxs-lookup"><span data-stu-id="86af4-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="86af4-894">Porque os métodos podem ocultar métodos herdados, é possível que uma classe conter vários métodos virtuais com a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="86af4-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="86af4-895">Isso não apresenta um problema de ambiguidade, desde que todos, exceto o método mais derivado estão ocultos.</span><span class="sxs-lookup"><span data-stu-id="86af4-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="86af4-896">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="86af4-897">o `C` e `D` classes contêm dois métodos virtuais com a mesma assinatura: Aquele introduzido por `A` e um introduzido por `C`.</span><span class="sxs-lookup"><span data-stu-id="86af4-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="86af4-898">O método introduzido por `C` oculta o método herdado de `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="86af4-899">Assim, a declaração de substituição em `D` substitui o método introduzido pela `C`, e não é possível `D` substituir o método introduzido por `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="86af4-900">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="86af4-901">Observe que é possível invocar o método virtual oculto acessando uma instância de `D` por meio de um menor derivado tipo no qual o método não está oculto.</span><span class="sxs-lookup"><span data-stu-id="86af4-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="86af4-902">Substituir métodos</span><span class="sxs-lookup"><span data-stu-id="86af4-902">Override methods</span></span>

<span data-ttu-id="86af4-903">Quando uma declaração de método de instância inclui um `override` modificador, o método deve ser um ***substituir método***.</span><span class="sxs-lookup"><span data-stu-id="86af4-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="86af4-904">Um método de substituição substitui um método virtual herdado com a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="86af4-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="86af4-905">Enquanto uma declaração de método virtual apresenta um novo método, uma declaração de método de substituição restringe um método virtual herdado existente fornecendo uma nova implementação do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="86af4-906">O método substituído por um `override` declaração é conhecida como o ***substituída do método base***.</span><span class="sxs-lookup"><span data-stu-id="86af4-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="86af4-907">Para um método de substituição `M` declarado em uma classe `C`, o método base substituído é determinado examinando-se cada tipo de classe base de `C`, começando com o tipo de classe base direta de `C` e continuando com cada sucessivas tipo de classe base direta, até em um tipo de classe base pelo menos um método acessível é localizado que tem a mesma assinatura que `M` após a substituição de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="86af4-908">Para fins de localizar o método base substituído, um método é considerado acessível se estiver `public`, se ele estiver `protected`, se ele for `protected internal`, ou se ele for `internal` e declarado no mesmo programa como `C`.</span><span class="sxs-lookup"><span data-stu-id="86af4-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="86af4-909">Um erro de tempo de compilação ocorre, a menos que todos os itens a seguir são verdadeiras para uma declaração de substituição:</span><span class="sxs-lookup"><span data-stu-id="86af4-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="86af4-910">Um método base substituído pode estar localizado, conforme descrito acima.</span><span class="sxs-lookup"><span data-stu-id="86af4-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="86af4-911">Não há exatamente um tal método base substituído.</span><span class="sxs-lookup"><span data-stu-id="86af4-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="86af4-912">Essa restrição tem efeito somente se o tipo de classe base é um tipo construído em que a substituição de argumentos de tipo faz a assinatura dos dois métodos o mesmo.</span><span class="sxs-lookup"><span data-stu-id="86af4-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="86af4-913">O método base substituído é um virtual, abstrato ou substituir o método.</span><span class="sxs-lookup"><span data-stu-id="86af4-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="86af4-914">Em outras palavras, o método base substituído não pode ser estático ou não virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="86af4-915">O método base substituído não é um método lacrado.</span><span class="sxs-lookup"><span data-stu-id="86af4-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="86af4-916">O método de substituição e o método base substituído têm o mesmo tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="86af4-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="86af4-917">A declaração de substituição e o método base substituído têm a mesma acessibilidade declarada.</span><span class="sxs-lookup"><span data-stu-id="86af4-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="86af4-918">Em outras palavras, uma declaração de substituição não é possível alterar a acessibilidade do método virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="86af4-919">No entanto, se o método base substituído é protegido interno e é declarada em um assembly diferente do que o assembly que contém o método de substituição e em seguida, o método de substituição declarado acessibilidade deve ser protegida.</span><span class="sxs-lookup"><span data-stu-id="86af4-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="86af4-920">A declaração de substituição não especifica o tipo de parâmetro-restrições cláusulas.</span><span class="sxs-lookup"><span data-stu-id="86af4-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="86af4-921">Em vez disso, as restrições são herdadas do método base substituído.</span><span class="sxs-lookup"><span data-stu-id="86af4-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="86af4-922">Observe que as restrições que são parâmetros de tipo no método substituído podem ser substituídas por argumentos de tipo na restrição herdado.</span><span class="sxs-lookup"><span data-stu-id="86af4-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="86af4-923">Isso pode levar a restrições que não são válidas quando explicitamente especificado, como tipos de valor ou tipos lacrados.</span><span class="sxs-lookup"><span data-stu-id="86af4-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="86af4-924">O exemplo a seguir demonstra como as regras de substituição funcionam para classes genéricas:</span><span class="sxs-lookup"><span data-stu-id="86af4-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="86af4-925">Uma declaração de substituição pode acessar o método base substituído usando um *base_access* ([acesso de Base](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="86af4-926">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="86af4-927">o `base.PrintFields()` invocação `B` invoca o `PrintFields` método declarado em `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="86af4-928">Um *base_access* desabilita o mecanismo de invocação virtual e simplesmente trata o método base como um método não virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="86af4-929">Teve a invocação no `B` foi gravado `((A)this).PrintFields()`, seria recursivamente invocar o `PrintFields` método declarado em `B`, não aquela declarada no `A`, desde que `PrintFields` é virtual e o tipo de tempo de execução do `((A)this)` é `B`.</span><span class="sxs-lookup"><span data-stu-id="86af4-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="86af4-930">Apenas, incluindo um `override` can modificador um método substituem outro método.</span><span class="sxs-lookup"><span data-stu-id="86af4-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="86af4-931">Em todos os outros casos, um método com a mesma assinatura que um método herdado simplesmente oculta o método herdado.</span><span class="sxs-lookup"><span data-stu-id="86af4-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="86af4-932">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="86af4-933">o `F` método no `B` não inclui um `override` modificador e, portanto, não substitui o `F` método na `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="86af4-934">Em vez disso, o `F` método no `B` oculta o método na `A`, e um aviso é relatado como a declaração não inclui um `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="86af4-935">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="86af4-936">o `F` método no `B` oculta virtual `F` método herdado do `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="86af4-937">Desde o novo `F` na `B` tem acesso privado, seu escopo inclui apenas o corpo da classe `B` e não se estende para `C`.</span><span class="sxs-lookup"><span data-stu-id="86af4-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="86af4-938">Portanto, a declaração de `F` na `C` tem permissão para substituir o `F` herdado de `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="86af4-939">Métodos lacrados</span><span class="sxs-lookup"><span data-stu-id="86af4-939">Sealed methods</span></span>

<span data-ttu-id="86af4-940">Quando uma declaração de método de instância inclui um `sealed` modificador, que o método deve ser um ***lacrados método***.</span><span class="sxs-lookup"><span data-stu-id="86af4-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="86af4-941">Se uma declaração de método de instância inclui o `sealed` modificador, ele também deve incluir o `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="86af4-942">Usar o `sealed` modificador impede que uma classe derivada ainda mais, substituindo o método.</span><span class="sxs-lookup"><span data-stu-id="86af4-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="86af4-943">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="86af4-944">a classe `B` fornece dois métodos de substituição: uma `F` método que tem o `sealed` modificador e um `G` método que não tem.</span><span class="sxs-lookup"><span data-stu-id="86af4-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="86af4-945">`B`uso do lacrado `modifier` impede `C` substituam ainda mais `F`.</span><span class="sxs-lookup"><span data-stu-id="86af4-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="86af4-946">Métodos abstratos</span><span class="sxs-lookup"><span data-stu-id="86af4-946">Abstract methods</span></span>

<span data-ttu-id="86af4-947">Quando uma declaração de método de instância inclui um `abstract` modificador, que o método deve ser um ***método abstrato***.</span><span class="sxs-lookup"><span data-stu-id="86af4-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="86af4-948">Embora um método abstrato também é implicitamente um método virtual, ele não pode ter o modificador `virtual`.</span><span class="sxs-lookup"><span data-stu-id="86af4-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="86af4-949">Uma declaração de método abstrato apresenta um novo método virtual, mas não fornece uma implementação desse método.</span><span class="sxs-lookup"><span data-stu-id="86af4-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="86af4-950">Em vez disso, as classes derivadas não abstratas devem fornecer sua própria implementação, substituindo esse método.</span><span class="sxs-lookup"><span data-stu-id="86af4-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="86af4-951">Como um método abstrato não fornece nenhuma implementação real, o *method_body* de um método abstrato consiste apenas em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="86af4-952">Declarações de método abstrato são permitidas apenas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="86af4-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="86af4-953">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="86af4-954">o `Shape` classe define o conceito abstrato de um objeto de forma geométrica que pode pintar em si.</span><span class="sxs-lookup"><span data-stu-id="86af4-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="86af4-955">O `Paint` método é abstrato porque não há nenhuma implementação padrão significativos.</span><span class="sxs-lookup"><span data-stu-id="86af4-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="86af4-956">O `Ellipse` e `Box` classes são concretas `Shape` implementações.</span><span class="sxs-lookup"><span data-stu-id="86af4-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="86af4-957">Como essas classes são não-abstrata, eles são necessários para substituir o `Paint` método e fornecer uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="86af4-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="86af4-958">É um erro de tempo de compilação para um *base_access* ([acesso de Base](expressions.md#base-access)) para fazer referência a um método abstrato.</span><span class="sxs-lookup"><span data-stu-id="86af4-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="86af4-959">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="86af4-960">um erro de tempo de compilação é relatado para o `base.F()` invocação porque faz referência a um método abstrato.</span><span class="sxs-lookup"><span data-stu-id="86af4-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="86af4-961">Uma declaração de método abstrato tem permissão para substituir um método virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="86af4-962">Isso permite que uma classe abstrata forçar a nova implementação do método em classes derivadas e torna a implementação original do método não está disponível.</span><span class="sxs-lookup"><span data-stu-id="86af4-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="86af4-963">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="86af4-964">classe `A` declara um método virtual, a classe `B` substitui esse método com um método abstrato e classe `C` substitui o método abstrato para fornecer sua própria implementação.</span><span class="sxs-lookup"><span data-stu-id="86af4-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="86af4-965">Métodos externos</span><span class="sxs-lookup"><span data-stu-id="86af4-965">External methods</span></span>

<span data-ttu-id="86af4-966">Quando uma declaração de método inclui um `extern` modificador, que o método deve ser um ***método externo***.</span><span class="sxs-lookup"><span data-stu-id="86af4-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="86af4-967">Métodos externos são implementados externamente, normalmente usando um idioma diferente do C#.</span><span class="sxs-lookup"><span data-stu-id="86af4-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="86af4-968">Como uma declaração de método externo não fornece nenhuma implementação real, o *method_body* de um método externo consiste apenas em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="86af4-969">Um método externo não pode ser genérico.</span><span class="sxs-lookup"><span data-stu-id="86af4-969">An external method may not be generic.</span></span>

<span data-ttu-id="86af4-970">O `extern` modificador é normalmente usado em conjunto com um `DllImport` atributo ([interoperação com componentes COM e Win32](attributes.md#interoperation-with-com-and-win32-components)), permitindo que os métodos externos a ser implementada por DLLs (bibliotecas de vínculo dinâmico).</span><span class="sxs-lookup"><span data-stu-id="86af4-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="86af4-971">O ambiente de execução pode dar suporte a outros mecanismos, no qual as implementações de métodos externos podem ser fornecidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="86af4-972">Quando um método externo inclui um `DllImport` atributo, a declaração do método também deve incluir um `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="86af4-973">Este exemplo demonstra o uso do `extern` modificador e o `DllImport` atributo:</span><span class="sxs-lookup"><span data-stu-id="86af4-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="86af4-974">Métodos parciais (recapitulação)</span><span class="sxs-lookup"><span data-stu-id="86af4-974">Partial methods (recap)</span></span>

<span data-ttu-id="86af4-975">Quando uma declaração de método inclui um `partial` modificador, que o método deve ser um ***método parcial***.</span><span class="sxs-lookup"><span data-stu-id="86af4-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="86af4-976">Métodos parciais só podem ser declarados como membros de tipos parciais ([tipos parciais](classes.md#partial-types)) e estão sujeitos a várias restrições.</span><span class="sxs-lookup"><span data-stu-id="86af4-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="86af4-977">Métodos parciais são descritos na [métodos parciais](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="86af4-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="86af4-978">Métodos de extensão</span><span class="sxs-lookup"><span data-stu-id="86af4-978">Extension methods</span></span>

<span data-ttu-id="86af4-979">Quando o primeiro parâmetro de um método inclui o `this` modificador, que o método deve ser um ***método de extensão***.</span><span class="sxs-lookup"><span data-stu-id="86af4-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="86af4-980">Métodos de extensão só podem ser declarados em classes estáticas não genérico, não aninhadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="86af4-981">O primeiro parâmetro de um método de extensão não pode ter nenhum modificador diferente de `this`, e o tipo de parâmetro não pode ser um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="86af4-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="86af4-982">Este é um exemplo de uma classe estática que declara dois métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="86af4-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="86af4-983">Um método de extensão é um método estático normal.</span><span class="sxs-lookup"><span data-stu-id="86af4-983">An extension method is a regular static method.</span></span> <span data-ttu-id="86af4-984">Além disso, em que sua classe estática delimitador no escopo, um método de extensão pode ser invocado usando sintaxe de invocação de método de instância ([invocações de método de extensão](expressions.md#extension-method-invocations)), usando a expressão de receptor como o primeiro argumento.</span><span class="sxs-lookup"><span data-stu-id="86af4-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="86af4-985">O programa a seguir usa os métodos de extensão declarados acima:</span><span class="sxs-lookup"><span data-stu-id="86af4-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="86af4-986">O `Slice` método está disponível na `string[]`e o `ToInt32` método está disponível no `string`, porque eles foram declarados como métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="86af4-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="86af4-987">O significado do programa é o mesmo que as chamadas de método estático normal seguinte, usando:</span><span class="sxs-lookup"><span data-stu-id="86af4-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="86af4-988">Corpo do método</span><span class="sxs-lookup"><span data-stu-id="86af4-988">Method body</span></span>

<span data-ttu-id="86af4-989">O *method_body* de um método de declaração consiste em um corpo do bloco, um corpo de expressão ou um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="86af4-990">O ***tipo de resultado*** de um método é `void` se for o tipo de retorno `void`, ou se o método é assíncrono e o tipo de retorno é `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="86af4-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="86af4-991">Caso contrário, o tipo de resultado de um método não assíncrono é seu tipo de retorno e o tipo de resultado de um método assíncrono com o tipo de retorno `System.Threading.Tasks.Task<T>` é `T`.</span><span class="sxs-lookup"><span data-stu-id="86af4-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="86af4-992">Quando um método tem um `void` resultar de tipo e um corpo do bloco `return` instruções ([a instrução return](statements.md#the-return-statement)) no bloco não são permitidos para especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="86af4-993">Se a execução do bloco de um método void é concluída normalmente (ou seja, controlam fluxos de fora do corpo do método), que o método simplesmente retorna para seu chamador atual.</span><span class="sxs-lookup"><span data-stu-id="86af4-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="86af4-994">Quando um método tem um `void` resultado e um corpo de expressão, a expressão `E` deve ser um *statement_expression*, e o corpo é exatamente equivalente a um corpo do bloco do formulário `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="86af4-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="86af4-995">Quando um método tem um tipo de resultado não nulo e um bloco de corpo, cada `return` instrução do bloco deve especificar uma expressão que é implicitamente conversível para o tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="86af4-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="86af4-996">O ponto de extremidade de um corpo do bloco de um método que retorna o valor não deve ser acessível.</span><span class="sxs-lookup"><span data-stu-id="86af4-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="86af4-997">Em outras palavras, em um método de retorno de valor com um corpo do bloco, o controle não tem permissão para fluir para fora o final do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="86af4-998">Quando um método tem um tipo de resultado não nulo e um corpo de expressão, a expressão deve ser implicitamente conversível para o tipo de resultado e o corpo é exatamente equivalente a um corpo do bloco do formulário `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="86af4-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="86af4-999">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="86af4-1000">o valor de retorno `F` método resulta em um erro de tempo de compilação porque o controle pode fluir para fora o final do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="86af4-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="86af4-1001">O `G` e `H` métodos estão corretos, porque todos os possíveis caminhos de execução terminam com uma instrução return que especifica um valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86af4-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="86af4-1002">O `I` método está correto, pois seu corpo é equivalente a um bloco de instrução com apenas uma única instrução return nele.</span><span class="sxs-lookup"><span data-stu-id="86af4-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="86af4-1003">Sobrecarga de método</span><span class="sxs-lookup"><span data-stu-id="86af4-1003">Method overloading</span></span>

<span data-ttu-id="86af4-1004">As regras de resolução de sobrecarga de método são descritas em [inferência de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="86af4-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="86af4-1005">Propriedades</span><span class="sxs-lookup"><span data-stu-id="86af4-1005">Properties</span></span>

<span data-ttu-id="86af4-1006">Um ***propriedade*** é um membro que fornece acesso a uma característica de um objeto ou uma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="86af4-1007">Exemplos de propriedades incluem o comprimento de uma cadeia de caracteres, o tamanho de uma fonte, a legenda de uma janela, o nome de um cliente e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="86af4-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="86af4-1008">As propriedades são uma extensão natural dos campos — elas são denominadas membros com tipos associados e a sintaxe para acessar os campos e propriedades é o mesmo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="86af4-1009">No entanto, diferentemente dos campos, as propriedades não denotam locais de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="86af4-1010">Em vez disso, as propriedades têm ***acessadores*** que especificam as instruções a serem executadas quando os valores forem lidos ou gravados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="86af4-1011">Propriedades, portanto, fornecem um mecanismo para associar as ações com a leitura e gravação de atributos de um objeto; Além disso, eles permitem que esses atributos a ser computado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="86af4-1012">As propriedades são declaradas usando *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="86af4-1013">Um *property_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos e de instância](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([Substituir métodos](classes.md#override-methods)), `sealed` ([lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([Métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="86af4-1014">Declarações de propriedade estão sujeitos às mesmas regras de declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="86af4-1015">O *tipo* de uma propriedade de declaração especifica o tipo da propriedade introduzida pela declaração e o *member_name* Especifica o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="86af4-1016">A menos que a propriedade é uma implementação de membro de interface explícita, o *member_name* é simplesmente uma *identificador*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="86af4-1017">Para uma implementação de membro de interface explícita ([implementações de membros de interface explícita](interfaces.md#explicit-interface-member-implementations)), o *member_name* consiste em um *interface_type* seguido por um " `.`"e uma *identificador*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="86af4-1018">O *tipo* de uma propriedade deve ser pelo menos tão acessível quanto a própria propriedade ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-1019">Um *property_body* qualquer um pode consistir de uma ***corpo do acessador*** ou uma ***corpo da expressão***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="86af4-1020">Em um corpo de acessador *accessor_declarations*, que deve ser colocada em "`{`"e"`}`" tokens, declare acessadores ([acessadores](classes.md#accessors)) da propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="86af4-1021">Os acessadores de especificam as instruções executáveis associadas com a leitura e gravação de propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="86af4-1022">Um corpo de expressão consiste `=>` seguido por um *expressão* `E` e um ponto e vírgula é exatamente equivalente para o corpo da instrução `{ get { return E; } }`e apenas, portanto, pode ser usado para especificar apenas de getter propriedades em que o resultado do getter é determinado por uma única expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="86af4-1023">Um *property_initializer* só podem ser designados para uma propriedade implementada automaticamente ([implementadas automaticamente propriedades](classes.md#automatically-implemented-properties)) e faz com que a inicialização do campo subjacente de tais as propriedades com o valor fornecido pelo *expressão*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="86af4-1024">Embora a sintaxe para acessar uma propriedade é o mesmo que para um campo, uma propriedade não é classificada como uma variável.</span><span class="sxs-lookup"><span data-stu-id="86af4-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="86af4-1025">Portanto, não é possível passar uma propriedade como uma `ref` ou `out` argumento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="86af4-1026">Quando uma declaração de propriedade inclui um `extern` modificador, a propriedade deve ser um ***propriedade externa***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="86af4-1027">Como uma declaração de propriedade externa não fornece nenhuma implementação real, cada um dos seus *accessor_declarations* consiste em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="86af4-1028">Propriedades estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-1028">Static and instance properties</span></span>

<span data-ttu-id="86af4-1029">Quando uma declaração de propriedade inclui um `static` modificador, a propriedade deve ser um ***propriedade estática***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="86af4-1030">Quando nenhum `static` modificador estiver presente, a propriedade deve ser um ***propriedade da instância***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="86af4-1031">Uma propriedade estática não está associada uma instância específica e é um erro de tempo de compilação para se referir a `this` nos acessadores de uma propriedade estática.</span><span class="sxs-lookup"><span data-stu-id="86af4-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="86af4-1032">Uma propriedade de instância está associada uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores da propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="86af4-1033">Quando uma propriedade é referenciada em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é uma propriedade estática, `E` deve indicar um tipo que contém `M`e se `M` é uma propriedade de instância, E preciso marcar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="86af4-1034">As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="86af4-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="86af4-1035">Acessadores</span><span class="sxs-lookup"><span data-stu-id="86af4-1035">Accessors</span></span>

<span data-ttu-id="86af4-1036">O *accessor_declarations* de uma propriedade, especifique as instruções executáveis associadas com a leitura e gravação a essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="86af4-1037">As declarações de acessador consistem em uma *get_accessor_declaration*, um *set_accessor_declaration*, ou ambos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="86af4-1038">Cada declaração de acessador consiste o token `get` ou `set` seguido de um recurso opcional *accessor_modifier* e um *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="86af4-1039">O uso de *accessor_modifier*s é regido pelos seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="86af4-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="86af4-1040">Uma *accessor_modifier* não pode ser usado em uma interface ou em uma implementação de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="86af4-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="86af4-1041">Para uma propriedade ou indexador que não tem nenhum `override` modificador, uma *accessor_modifier* é permitido somente se a propriedade ou o indexador tiver tanto um `get` e `set` acessador e, em seguida, é permitida apenas em um desses acessadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="86af4-1042">Para uma propriedade ou indexador que inclui um `override` modificador, um acessador deve corresponder a *accessor_modifier*, se houver, do acessador que está sendo substituído.</span><span class="sxs-lookup"><span data-stu-id="86af4-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="86af4-1043">O *accessor_modifier* deve declarar a acessibilidade é estritamente mais restritiva que a acessibilidade declarada de propriedade ou indexador em si.</span><span class="sxs-lookup"><span data-stu-id="86af4-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="86af4-1044">Para ser preciso:</span><span class="sxs-lookup"><span data-stu-id="86af4-1044">To be precise:</span></span>
   * <span data-ttu-id="86af4-1045">Se a propriedade ou indexador tem uma acessibilidade declarada de `public`, o *accessor_modifier* pode ser `protected internal`, `internal`, `protected`, ou `private`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="86af4-1046">Se a propriedade ou indexador tem uma acessibilidade declarada de `protected internal`, o *accessor_modifier* pode ser `internal`, `protected`, ou `private`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="86af4-1047">Se a propriedade ou indexador tem uma acessibilidade declarada de `internal` ou `protected`, o *accessor_modifier* deve ser `private`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="86af4-1048">Se a propriedade ou indexador tem uma acessibilidade declarada de `private`, nenhum *accessor_modifier* pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="86af4-1049">Para `abstract` e `extern` propriedades, o *accessor_body* para cada acessador especificado é simplesmente um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="86af4-1050">Uma propriedade não-abstrata, não-externo pode ter cada *accessor_body* ser um ponto e vírgula, caso em que ele é um ***propriedade implementada automaticamente*** ([implementada automaticamente propriedades ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="86af4-1051">Uma propriedade implementada automaticamente deve ter pelo menos um acessador get.</span><span class="sxs-lookup"><span data-stu-id="86af4-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="86af4-1052">Para os acessadores de qualquer outra não-abstrata, não extern propriedade, o *accessor_body* é um *bloco* que especifica as instruções a serem executadas quando o acessador correspondente é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="86af4-1053">Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="86af4-1054">Exceto como o destino de uma atribuição, quando uma propriedade é referenciada em uma expressão, o `get` acessador da propriedade é invocado para calcular o valor da propriedade ([valores das expressões](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="86af4-1055">O corpo de uma `get` acessador deve obedecer às regras para retornar o valor métodos descritos [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="86af4-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="86af4-1056">Em particular, todos os `return` instruções no corpo de um `get` acessador deve especificar uma expressão que é implicitamente conversível para o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="86af4-1057">Além disso, o ponto de extremidade de um `get` acessador não pode estar acessível.</span><span class="sxs-lookup"><span data-stu-id="86af4-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="86af4-1058">Um `set` acessador corresponde a um método com um parâmetro de valor único do tipo de propriedade e um `void` tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="86af4-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="86af4-1059">O parâmetro implícito de um `set` acessador é sempre denominado `value`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="86af4-1060">Quando uma propriedade é referenciada como o destino de uma atribuição ([operadores de atribuição](expressions.md#assignment-operators)), ou como o operando da `++` ou `--` ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators), [ Incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)), o `set` acessador é invocado com um argumento (cujo valor é o do lado direito da atribuição ou o operando das `++` ou `--` operador) que fornece o novo valor ([atribuição simples](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="86af4-1061">O corpo de uma `set` acessador deve estar de acordo com as regras para `void` métodos descritos [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="86af4-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="86af4-1062">Em particular, `return` instruções de `set` corpo do acessador não são permitidos para especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="86af4-1063">Uma vez que um `set` acessador implicitamente tem um parâmetro chamado `value`, é um erro de tempo de compilação para uma declaração de constante ou variável local em um `set` acessador esse nome.</span><span class="sxs-lookup"><span data-stu-id="86af4-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="86af4-1064">Com base na presença ou ausência do `get` e `set` acessadores, uma propriedade é classificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86af4-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="86af4-1065">Uma propriedade que inclui tanto uma `get` acessador e um `set` acessador deve ser um ***leitura-gravação*** propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="86af4-1066">Uma propriedade que tem apenas um `get` acessador deve ser um ***somente leitura*** propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="86af4-1067">Ele é um erro de tempo de compilação para uma propriedade somente leitura ser o destino de uma atribuição.</span><span class="sxs-lookup"><span data-stu-id="86af4-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="86af4-1068">Uma propriedade que tem apenas um `set` acessador deve ser um ***somente gravação*** propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="86af4-1069">Exceto como o destino de uma atribuição, ele é um erro de tempo de compilação para fazer referência a uma propriedade somente gravação em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="86af4-1070">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="86af4-1071">o `Button` controle declara um público `Caption` propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="86af4-1072">O `get` acessador do `Caption` propriedade retorna a cadeia de caracteres armazenada em particular `caption` campo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="86af4-1073">O `set` acessador verifica se o novo valor é diferente do valor atual e, nesse caso, ele armazena o novo valor e redesenha o controle.</span><span class="sxs-lookup"><span data-stu-id="86af4-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="86af4-1074">Propriedades geralmente seguem o padrão mostrado acima: O `get` acessador simplesmente retorna um valor armazenado em um campo privado e o `set` acessador modifica esse campo particular e, em seguida, executa ações adicionais necessárias para atualizar totalmente o estado do objeto.</span><span class="sxs-lookup"><span data-stu-id="86af4-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="86af4-1075">Dada a `Button` classe acima, o seguinte é um exemplo de uso do `Caption` propriedade:</span><span class="sxs-lookup"><span data-stu-id="86af4-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="86af4-1076">Aqui, o `set` acessador é invocado, atribuindo um valor para a propriedade e o `get` acessador é invocado por fazer referência à propriedade em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="86af4-1077">O `get` e `set` acessadores de uma propriedade não são membros diferentes e não é possível declarar os acessadores de uma propriedade separadamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="86af4-1078">Como tal, não é possível que os dois acessadores de uma propriedade de leitura / gravação ter acessibilidade diferente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="86af4-1079">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="86af4-1080">não declara uma única propriedade de leitura / gravação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="86af4-1081">Em vez disso, ele declara duas propriedades com o mesmo nome, um somente leitura e somente gravação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="86af4-1082">Como dois membros declarados na mesma classe não podem ter o mesmo nome, o exemplo faz com que ocorra um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="86af4-1083">Quando uma classe derivada não declara uma propriedade com o mesmo nome como uma propriedade herdada, a propriedade derivada oculta a propriedade herdada com relação à leitura e gravação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="86af4-1084">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="86af4-1085">o `P` propriedade na `B` oculta a `P` propriedade no `A` em relação à leitura e gravação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="86af4-1086">Dessa forma, nas instruções</span><span class="sxs-lookup"><span data-stu-id="86af4-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="86af4-1087">a atribuição ao `b.P` causa um erro de tempo de compilação a ser relatado, desde o somente leitura `P` propriedade na `B` oculta a somente gravação `P` propriedade no `A`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="86af4-1088">No entanto, observe que uma conversão pode ser usada para acessar o oculto `P` propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="86af4-1089">Diferentemente dos campos públicos, propriedades fornecem uma separação entre o estado interno de um objeto e sua interface pública.</span><span class="sxs-lookup"><span data-stu-id="86af4-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="86af4-1090">Considere o exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="86af4-1091">Aqui, o `Label` classe usa dois `int` campos, `x` e `y`, para armazenar seu local.</span><span class="sxs-lookup"><span data-stu-id="86af4-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="86af4-1092">O local é publicamente exposta como uma `X` e uma `Y` propriedade e como um `Location` propriedade do tipo `Point`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="86af4-1093">Se, em uma versão futura do `Label`, ele se torna mais conveniente armazenar o local como um `Point` internamente, a alteração pode ser feita sem afetar a interface pública da classe:</span><span class="sxs-lookup"><span data-stu-id="86af4-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="86af4-1094">Tinha `x` e `y` em vez disso, foram `public readonly` campos, teria sido impossível fazer uma alteração para o `Label` classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="86af4-1095">Expõe estado por meio das propriedades não é necessariamente menos eficiente do que expor campos diretamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="86af4-1096">Em particular, quando uma propriedade é não virtual e contém apenas uma pequena quantidade de código, o ambiente de execução pode substituir chamadas para acessadores com o código real de acessadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="86af4-1097">Esse processo é conhecido como ***inlining***, e torna o acesso de propriedade tão eficiente quanto o acesso de campo, mas preserva a maior flexibilidade de propriedades.</span><span class="sxs-lookup"><span data-stu-id="86af4-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="86af4-1098">Desde a invocação de um `get` acessador é conceitualmente equivalente ao ler o valor de um campo, ele é considerado um estilo ruim de programação para `get` acessadores ter efeitos colaterais observáveis.</span><span class="sxs-lookup"><span data-stu-id="86af4-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="86af4-1099">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="86af4-1100">O valor da `Next` propriedade depende do número de vezes que a propriedade foi acessada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="86af4-1101">Assim, acessar a propriedade produz um efeito colateral observável, e a propriedade deve ser implementada como um método em vez disso.</span><span class="sxs-lookup"><span data-stu-id="86af4-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="86af4-1102">A convenção de "sem efeitos colaterais" para `get` acessadores não significa que `get` acessadores sempre devem ser escritos para simplesmente retornar valores armazenados nos campos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="86af4-1103">Na verdade, `get` acessadores geralmente calcular o valor de uma propriedade ao acessar vários campos ou invocar métodos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="86af4-1104">No entanto, projetado corretamente `get` acessador não executará nenhuma ação que causam alterações observáveis no estado do objeto.</span><span class="sxs-lookup"><span data-stu-id="86af4-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="86af4-1105">Propriedades podem ser usadas para atrasar a inicialização de um recurso até o momento em que ele é referenciado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="86af4-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="86af4-1106">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="86af4-1107">O `Console` classe contém três propriedades: `In`, `Out`, e `Error`, que representam a entrada padrão, saída e dispositivos de erro, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="86af4-1108">Ao expor esses membros como propriedades, o `Console` classe pode atrasar sua inicialização até que eles são realmente usados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="86af4-1109">Por exemplo, após a primeira referenciando o `Out` propriedade, como em</span><span class="sxs-lookup"><span data-stu-id="86af4-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="86af4-1110">subjacente `TextWriter` para o dispositivo de saída é criado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="86af4-1111">Porém, se o aplicativo não faz referência à `In` e `Error` propriedades, em seguida, não há objetos são criados para esses dispositivos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="86af4-1112">Propriedades implementadas automaticamente</span><span class="sxs-lookup"><span data-stu-id="86af4-1112">Automatically implemented properties</span></span>

<span data-ttu-id="86af4-1113">Uma propriedade implementada automaticamente (ou ***auto-propriedade*** de forma abreviada), é uma propriedade de não-externo não abstrata com corpos de acessador somente-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="86af4-1114">Propriedades automáticas devem ter um acessador get e, opcionalmente, podem ter um acessador set.</span><span class="sxs-lookup"><span data-stu-id="86af4-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="86af4-1115">Quando uma propriedade é especificada como uma propriedade implementada automaticamente, um campo oculto existente está automaticamente disponível para a propriedade e os acessadores são implementados para ler e gravar a esse campo existente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="86af4-1116">Se a propriedade automática não possui nenhum acessador set, o campo de suporte é considerado `readonly` ([campos somente leitura](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="86af4-1117">Assim como um `readonly` campo, uma propriedade de automática apenas de getter também pode ser atribuída a no corpo de um construtor da classe delimitadora.</span><span class="sxs-lookup"><span data-stu-id="86af4-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="86af4-1118">Essa atribuição atribui diretamente para o campo de suporte da propriedade somente leitura.</span><span class="sxs-lookup"><span data-stu-id="86af4-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="86af4-1119">Uma propriedade automática, opcionalmente, pode ter um *property_initializer*, que é aplicado diretamente ao campo de suporte como um *variable_initializer* ([inicializadores de variável](classes.md#variable-initializers)) .</span><span class="sxs-lookup"><span data-stu-id="86af4-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="86af4-1120">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="86af4-1121">é equivalente à declaração a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="86af4-1122">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="86af4-1123">é equivalente à declaração a seguir:</span><span class="sxs-lookup"><span data-stu-id="86af4-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="86af4-1124">Observe que as atribuições para o campo somente leitura são legais, porque eles ocorrem dentro do construtor.</span><span class="sxs-lookup"><span data-stu-id="86af4-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="86af4-1125">Acessibilidade</span><span class="sxs-lookup"><span data-stu-id="86af4-1125">Accessibility</span></span>

<span data-ttu-id="86af4-1126">Se tiver um acessador de um *accessor_modifier*, o domínio de acessibilidade ([domínios acessibilidade](basic-concepts.md#accessibility-domains)) do acessador é determinado usando a acessibilidade declarada do *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="86af4-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="86af4-1127">Se um acessador não tem um *accessor_modifier*, o domínio de acessibilidade do acessador é determinado com base a acessibilidade declarada de propriedade ou indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="86af4-1128">A presença de um *accessor_modifier* nunca afeta a pesquisa de membro ([operadores](expressions.md#operators)) ou resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="86af4-1129">Os modificadores de propriedade ou indexador sempre determinam qual propriedade ou indexador está vinculado, independentemente do contexto do acesso.</span><span class="sxs-lookup"><span data-stu-id="86af4-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="86af4-1130">Depois que uma determinada propriedade ou indexador tiver sido selecionado, os domínios de acessibilidade dos acessadores específicos envolvidos são usados para determinar se esse uso é válido:</span><span class="sxs-lookup"><span data-stu-id="86af4-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="86af4-1131">Se o uso é como um valor ([valores de expressões](expressions.md#values-of-expressions)), o `get` acessador deve existir e estar acessível.</span><span class="sxs-lookup"><span data-stu-id="86af4-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="86af4-1132">Se o uso é como o destino de uma atribuição simple ([atribuição simples](expressions.md#simple-assignment)), o `set` acessador deve existir e estar acessível.</span><span class="sxs-lookup"><span data-stu-id="86af4-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="86af4-1133">Se o uso é como o destino de atribuição composta ([atribuição composta](expressions.md#compound-assignment)), ou como o destino da `++` ou `--` operadores ([membros de função](expressions.md#function-members).9, [ Expressões de invocação](expressions.md#invocation-expressions)), ambos os `get` acessadores e o `set` acessador deve existir e estar acessível.</span><span class="sxs-lookup"><span data-stu-id="86af4-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="86af4-1134">No exemplo a seguir, a propriedade `A.Text` está oculto pela propriedade `B.Text`, mesmo em contextos em que apenas o `set` acessador é chamado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="86af4-1135">Por outro lado, a propriedade `B.Count` não é acessível a classe `M`, portanto, a propriedade acessível `A.Count` é usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="86af4-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="86af4-1136">Um acessador que é usado para implementar uma interface não pode ter um *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="86af4-1137">Se apenas um acessador é usado para implementar uma interface, o outro acessador pode ser declarado com um *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="86af4-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="86af4-1138">Acessadores de propriedade abstrata, substituição e virtuais, selado</span><span class="sxs-lookup"><span data-stu-id="86af4-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="86af4-1139">Um `virtual` declaração de propriedade que especifica que os acessadores da propriedade são virtuais.</span><span class="sxs-lookup"><span data-stu-id="86af4-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="86af4-1140">O `virtual` modificador se aplica a ambos os acessadores de uma propriedade de leitura / gravação — não é possível apenas um acessador de uma propriedade de leitura / gravação seja virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="86af4-1141">Um `abstract` declaração de propriedade especifica que os acessadores da propriedade são virtuais, mas não fornece uma implementação real dos acessadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="86af4-1142">Em vez disso, as classes derivadas não abstratas devem fornecer sua própria implementação para os acessadores, substituindo a propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="86af4-1143">Como um acessador de uma declaração de propriedade abstrata não fornece nenhuma implementação real, sua *accessor_body* consiste apenas em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="86af4-1144">Uma declaração de propriedade que inclui ambos os `abstract` e `override` modificadores Especifica que a propriedade é abstrata e substitui uma propriedade base.</span><span class="sxs-lookup"><span data-stu-id="86af4-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="86af4-1145">Os acessadores de tal propriedade também são abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="86af4-1146">Declarações de propriedade abstrata são permitidas apenas em classes abstratas ([classes abstratas](classes.md#abstract-classes)). Os acessadores de uma propriedade herdada de virtual podem ser substituídos em uma classe derivada incluindo uma declaração de propriedade que especifica um `override` diretiva.</span><span class="sxs-lookup"><span data-stu-id="86af4-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="86af4-1147">Isso é conhecido como um ***substituindo a declaração de propriedade***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="86af4-1148">Uma declaração de propriedade de substituição não declara uma nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="86af4-1149">Em vez disso, ele simplesmente é especialista as implementações de acessadores de uma propriedade virtual existente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="86af4-1150">Uma declaração de propriedade de substituição deve especificar o exato mesmo modificadores de acessibilidade, o tipo e o nome que a propriedade herdada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="86af4-1151">Se a propriedade herdada tiver apenas um único acessador (ou seja, se a propriedade herdada for somente leitura ou somente gravação), a propriedade de substituição deve incluir somente o acessador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="86af4-1152">Se a propriedade herdada inclui ambos os acessadores (ou seja, se a propriedade herdada for leitura / gravação), a propriedade de substituição pode incluir um único acessador ou ambos os acessadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="86af4-1153">Uma declaração de propriedade de substituição pode incluir o `sealed` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="86af4-1154">Uso desse modificador impede que uma classe derivada ainda mais substituindo a propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="86af4-1155">Os acessadores de uma propriedade lacrado também são lacrados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="86af4-1156">Exceto pelas diferenças na declaração e chamada de sintaxe, substituição sealed, virtual e acessadores abstratos se comporta exatamente como virtual, selada, substituição e métodos abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="86af4-1157">Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [substituir métodos](classes.md#override-methods), [lacrados métodos](classes.md#sealed-methods), e [métodos abstratos](classes.md#abstract-methods) se aplicam como se acessadores eram os métodos de um formulário correspondente:</span><span class="sxs-lookup"><span data-stu-id="86af4-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="86af4-1158">Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade e os modificadores mesmos que a propriedade recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="86af4-1159">Um `set` acessador corresponde a um método com um parâmetro de valor único de tipo de propriedade, um `void` retornar o tipo e os modificadores mesmos que a propriedade recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="86af4-1160">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="86af4-1161">`X` é uma propriedade somente leitura virtual, `Y` é uma propriedade de leitura / gravação virtual, e `Z` é uma propriedade de leitura / gravação abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="86af4-1162">Porque `Z` é abstrato, a classe continente `A` também deve ser declarada como abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="86af4-1163">Uma classe que deriva de `A` é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="86af4-1164">Aqui, as declarações das `X`, `Y`, e `Z` estão substituindo declarações de propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="86af4-1165">Cada declaração de propriedade corresponde exatamente os modificadores de acessibilidade, o tipo e o nome da propriedade herdada correspondente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="86af4-1166">O `get` acessador da `X` e o `set` acessador de `Y` usam o `base` palavra-chave para acessar os acessadores herdados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="86af4-1167">A declaração de `Z` substitui os dois acessadores abstratos — portanto, não há nenhum membro de função abstract pendentes no `B`, e `B` tem permissão para ser uma classe não abstrata.</span><span class="sxs-lookup"><span data-stu-id="86af4-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="86af4-1168">Quando uma propriedade é declarada como um `override`, qualquer acessadores substituídos devem estar acessíveis ao código de substituição.</span><span class="sxs-lookup"><span data-stu-id="86af4-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="86af4-1169">Além disso, a acessibilidade declarada da propriedade ou indexador em si e dos acessadores, deve corresponder do membro substituído e acessadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="86af4-1170">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="86af4-1171">Eventos</span><span class="sxs-lookup"><span data-stu-id="86af4-1171">Events</span></span>

<span data-ttu-id="86af4-1172">Uma ***evento*** é um membro que permite que um objeto ou classe para fornecer notificações.</span><span class="sxs-lookup"><span data-stu-id="86af4-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="86af4-1173">Os clientes podem anexar o código executável para eventos fornecendo ***manipuladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="86af4-1174">Eventos são declarados usando *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="86af4-1175">Uma *event_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos e de instância](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([Substituir métodos](classes.md#override-methods)), `sealed` ([lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([Métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="86af4-1176">Declarações de evento estão sujeitos às mesmas regras de declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="86af4-1177">O *tipo* de um evento declaração deve ser um *delegate_type* ([tipos de referência](types.md#reference-types)) e que *delegate_type* deve ter pelo menos como acessível quanto o próprio evento ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-1178">Uma declaração de evento pode incluir *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="86af4-1179">No entanto, se não existir, para não-externo e eventos de não-abstrata, o compilador fornece-os automaticamente ([eventos semelhantes a campo](classes.md#field-like-events)); para eventos de extern, os acessadores são fornecidos externamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="86af4-1180">Uma declaração de evento que omite *event_accessor_declarations* define um ou mais eventos — uma para cada um dos *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="86af4-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="86af4-1181">Os atributos e modificadores que se aplicam a todos os membros declarados, como uma *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="86af4-1182">É um erro de tempo de compilação para um *event_declaration* de incluir ambas as `abstract` modificador e delimitado por chaves *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="86af4-1183">Quando uma declaração de evento inclui um `extern` modificador, o evento deve ser um ***evento externo***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="86af4-1184">Como uma declaração de evento externo fornece nenhuma implementação real, ele é um erro para que ele inclua ambos os `extern` modificador e *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="86af4-1185">É um erro de tempo de compilação para um *variable_declarator* de uma declaração de evento com um `abstract` ou `external` modificador para incluir um *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="86af4-1186">Um evento pode ser usado como o operando esquerdo do `+=` e `-=` operadores ([atribuição de evento](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="86af4-1187">Esses operadores são usados, respectivamente, para anexar manipuladores de eventos ou remover manipuladores de eventos de um evento, e os modificadores de acesso do evento de controlam os contextos em que essas operações são permitidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="86af4-1188">Uma vez que `+=` e `-=` são as únicas operações que são permitidas em um evento fora do tipo que declara o evento, o código externo podem adicionar e remover manipuladores para um evento, mas não é possível de qualquer outra maneira de obter ou modificar a lista subjacente do evento manipuladores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="86af4-1189">Em uma operação do formulário `x += y` ou `x -= y`, quando `x` é um evento e a referência ocorre fora do tipo que contém a declaração de `x`, o resultado da operação tem tipo `void` (em vez de o tipo de `x`, com o valor de `x` após a atribuição).</span><span class="sxs-lookup"><span data-stu-id="86af4-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="86af4-1190">Essa regra proíbe código externo indiretamente examinando o delegado subjacente de um evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="86af4-1191">O exemplo a seguir mostra como os manipuladores de eventos são anexados a instâncias do `Button` classe:</span><span class="sxs-lookup"><span data-stu-id="86af4-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="86af4-1192">Aqui, o `LoginDialog` construtor de instância cria dois `Button` instâncias e anexa os manipuladores de eventos para o `Click` eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="86af4-1193">Eventos semelhantes a campo</span><span class="sxs-lookup"><span data-stu-id="86af4-1193">Field-like events</span></span>

<span data-ttu-id="86af4-1194">Dentro do texto do programa de classe ou struct que contém a declaração de um evento, determinados eventos podem ser usados como campos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="86af4-1195">Para ser usado dessa forma, um evento não deve ser `abstract` ou `extern`e não deve incluir explicitamente *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="86af4-1196">Esse evento pode ser usado em qualquer contexto que permite que um campo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="86af4-1197">O campo contém um delegado ([delegados](delegates.md)) que se refere à lista de manipuladores de eventos que foram adicionados ao evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="86af4-1198">Se nenhum manipulador de eventos tiver sido adicionado, o campo contém `null`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="86af4-1199">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="86af4-1200">`Click` é usado como um campo dentro de `Button` classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="86af4-1201">Como demonstrado no exemplo, o campo pode ser examinado, modificado e usado em expressões de invocação do delegado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="86af4-1202">O `OnClick` método na `Button` classe "gera" o `Click` eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="86af4-1203">A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento — assim, não há constructos de linguagem especial para gerar eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="86af4-1204">Observe que a invocação de delegado é precedida por uma verificação que garante que o delegado for não nulo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="86af4-1205">Fora da declaração do `Button` classe, o `Click` membro só pode ser usado no lado esquerdo das `+=` e `-=` operadores, como em</span><span class="sxs-lookup"><span data-stu-id="86af4-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="86af4-1206">que anexa um delegado à lista de invocação de `Click` evento, e</span><span class="sxs-lookup"><span data-stu-id="86af4-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="86af4-1207">remover um delegado da lista de invocação do `Click` eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="86af4-1208">Ao compilar um evento de campo, o compilador automaticamente cria o armazenamento para manter o delegado e cria acessadores para o evento que adicionar ou removem manipuladores de eventos para o campo de delegado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="86af4-1209">As operações de adição e remoção são thread-safe e pode (mas não é necessárias) ser feito durante a manutenção do bloqueio ([instrução lock](statements.md#the-lock-statement)) no objeto de recipiente para um evento de instância, ou o objeto de tipo ([anônimo expressões de criação do objeto](expressions.md#anonymous-object-creation-expressions)) para um evento estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="86af4-1210">Portanto, uma instância declaração de evento do formulário:</span><span class="sxs-lookup"><span data-stu-id="86af4-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="86af4-1211">será compilada para algo equivalente a:</span><span class="sxs-lookup"><span data-stu-id="86af4-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="86af4-1212">Dentro da classe `X`, faz referência às `Ev` no lado esquerdo das `+=` e `-=` operadores fazem com que a adicionar e remover acessadores a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="86af4-1213">Todas as outras referências a `Ev` são compilados para referenciar o campo oculto `__Ev` em vez disso ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="86af4-1214">O nome "`__Ev`" é arbitrário; o campo oculto pode ter qualquer nome ou sem nome em todos os.</span><span class="sxs-lookup"><span data-stu-id="86af4-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="86af4-1215">Acessadores de eventos</span><span class="sxs-lookup"><span data-stu-id="86af4-1215">Event accessors</span></span>

<span data-ttu-id="86af4-1216">Declarações de evento normalmente omitir *event_accessor_declarations*, como no `Button` exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="86af4-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="86af4-1217">Uma situação para fazer o tema envolve o caso em que o custo de armazenamento de um campo por evento não é aceitável.</span><span class="sxs-lookup"><span data-stu-id="86af4-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="86af4-1218">Nesses casos, uma classe pode incluir *event_accessor_declarations* e usar um mecanismo privado para armazenar a lista de manipuladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="86af4-1219">O *event_accessor_declarations* de um evento, especifique as instruções executáveis associadas ao adicionar e remover manipuladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="86af4-1220">As declarações de acessador consistem em uma *add_accessor_declaration* e uma *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="86af4-1221">Cada declaração de acessador consiste o token `add` ou `remove` seguido por um *bloco*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="86af4-1222">O *bloco* associado com um *add_accessor_declaration* Especifica as instruções para executar quando um manipulador de eventos é adicionado e o *bloco* associado com um *remove_accessor_declaration* Especifica as instruções para executar quando um manipulador de eventos é removido.</span><span class="sxs-lookup"><span data-stu-id="86af4-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="86af4-1223">Cada *add_accessor_declaration* e *remove_accessor_declaration* corresponde a um método com um parâmetro de valor único do tipo de evento e um `void` tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="86af4-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="86af4-1224">O parâmetro implícito de um acessador de evento é chamado `value`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="86af4-1225">Quando um evento é usado em uma atribuição de evento, o acessador de evento apropriado é usado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="86af4-1226">Especificamente, se for o operador de atribuição `+=` acessador adicionar é usado, e se o operador de atribuição é `-=` , o acessador de remoção será usado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="86af4-1227">Em ambos os casos, o operando à direita do operador de atribuição é usado como o argumento para o acessador do evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="86af4-1228">O bloco de um *add_accessor_declaration* ou um *remove_accessor_declaration* devem obedecer às regras para `void` métodos descritos [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="86af4-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="86af4-1229">Em particular, `return` instruções em tal um bloco não são permitidas para especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="86af4-1230">Uma vez que um acessador de evento implicitamente tem um parâmetro chamado `value`, ele é um erro de tempo de compilação para um local constante ou variável declarada em um acessador de evento esse nome.</span><span class="sxs-lookup"><span data-stu-id="86af4-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="86af4-1231">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="86af4-1232">o `Control` classe implementa um mecanismo de armazenamento interno para eventos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="86af4-1233">O `AddEventHandler` método associa um valor de delegado com uma chave, o `GetEventHandler` método retorna o delegado atualmente associado a uma chave e o `RemoveEventHandler` método Remove um delegado como um manipulador de eventos para o evento especificado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="86af4-1234">Presumivelmente, o mecanismo de armazenamento subjacente foi projetado, de modo que não há nenhum custo para associar um `null` delegar o valor com uma chave e, portanto, os eventos sem tratamento não consumam nenhum armazenamento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="86af4-1235">Eventos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-1235">Static and instance events</span></span>

<span data-ttu-id="86af4-1236">Quando uma declaração de evento inclui um `static` modificador, o evento deve ser um ***evento estático***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="86af4-1237">Quando nenhum `static` modificador estiver presente, o evento deve ser um ***eventos de instância***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="86af4-1238">Um evento estático não está associado uma instância específica e é um erro de tempo de compilação para se referir a `this` nos acessadores de um evento estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="86af4-1239">Um evento de instância está associado uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores de evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="86af4-1240">Quando um evento é referenciado em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é um evento estático, `E` deve indicar um tipo que contém `M`e se `M` é um evento de instância, E preciso marcar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="86af4-1241">As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="86af4-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="86af4-1242">Acessadores de evento abstrato, substituição e virtuais, selado</span><span class="sxs-lookup"><span data-stu-id="86af4-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="86af4-1243">Um `virtual` declaração de evento especifica que os acessadores de evento são virtuais.</span><span class="sxs-lookup"><span data-stu-id="86af4-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="86af4-1244">O `virtual` modificador se aplica a ambos os acessadores de um evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="86af4-1245">Um `abstract` declaração de evento especifica que os acessadores do evento são virtuais, mas não fornece uma implementação real dos acessadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="86af4-1246">Em vez disso, as classes derivadas não abstratas devem fornecer sua própria implementação para os acessadores, substituindo o evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="86af4-1247">Como uma declaração de evento abstrata não fornece nenhuma implementação real, ele não pode fornecer delimitada por chaves *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="86af4-1248">Uma declaração de evento que inclui ambos os `abstract` e `override` modificadores Especifica que o evento é abstrato e substitui um evento de base.</span><span class="sxs-lookup"><span data-stu-id="86af4-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="86af4-1249">Os acessadores de um evento também são abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="86af4-1250">Declarações de evento abstrato são permitidas apenas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="86af4-1251">Os acessadores de um evento virtual herdado podem ser substituídos em uma classe derivada incluindo uma declaração de evento que especifica um `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="86af4-1252">Isso é conhecido como um ***substituindo a declaração de evento***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="86af4-1253">Uma declaração de evento de substituição não declara um novo evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="86af4-1254">Em vez disso, ele simplesmente é especialista as implementações de acessadores de um evento virtual existente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="86af4-1255">Uma declaração de evento de substituição deve especificar o exato mesmo modificadores de acessibilidade, o tipo e o nome que o evento substituído.</span><span class="sxs-lookup"><span data-stu-id="86af4-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="86af4-1256">Uma declaração de evento de substituição pode incluir o `sealed` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="86af4-1257">Uso desse modificador impede que uma classe derivada ainda mais a substituição de evento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="86af4-1258">Os acessadores de um evento lacrado também são lacrados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="86af4-1259">É um erro de tempo de compilação para uma declaração de evento de substituição incluir um `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="86af4-1260">Exceto pelas diferenças na declaração e chamada de sintaxe, substituição sealed, virtual e acessadores abstratos se comporta exatamente como virtual, selada, substituição e métodos abstratos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="86af4-1261">Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [substituir métodos](classes.md#override-methods), [lacrados métodos](classes.md#sealed-methods), e [métodos abstratos](classes.md#abstract-methods) se aplicam como se acessadores eram os métodos de um formulário correspondente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="86af4-1262">Cada acessador corresponde a um método com um parâmetro de valor único de tipo de evento, um `void` retornar o tipo e os modificadores mesmos que o evento recipiente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="86af4-1263">Indexadores</span><span class="sxs-lookup"><span data-stu-id="86af4-1263">Indexers</span></span>

<span data-ttu-id="86af4-1264">Uma ***indexador*** é um membro que permite que um objeto a serem indexados da mesma forma como uma matriz.</span><span class="sxs-lookup"><span data-stu-id="86af4-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="86af4-1265">Indexadores são declarados usando *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="86af4-1266">Uma *indexer_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([substituir métodos](classes.md#override-methods) ), `sealed` ([Lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="86af4-1267">Declarações de indexador estão sujeitos às mesmas regras de declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores, com a exceção de que o modificador estático não é permitida em uma declaração do indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="86af4-1268">Os modificadores `virtual`, `override`, e `abstract` são mutuamente exclusivos, exceto em um caso.</span><span class="sxs-lookup"><span data-stu-id="86af4-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="86af4-1269">O `abstract` e `override` modificadores podem ser usados juntos para que um indexador abstrato pode substituir um virtual.</span><span class="sxs-lookup"><span data-stu-id="86af4-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="86af4-1270">O *tipo* de um indexador declaração especifica o tipo de elemento do indexador introduzido por declaração.</span><span class="sxs-lookup"><span data-stu-id="86af4-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="86af4-1271">A menos que o indexador é uma implementação de membro de interface explícita, o *tipo* é seguido da palavra-chave `this`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="86af4-1272">Para uma implementação de membro de interface explícita, o *tipo* é seguido por um *interface_type*, um "`.`" e a palavra-chave `this`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="86af4-1273">Ao contrário de outros membros, os indexadores não tem nomes definidos pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="86af4-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="86af4-1274">O *formal_parameter_list* Especifica os parâmetros do indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="86af4-1275">Lista de parâmetros formais de um indexador corresponde de um método ([parâmetros de método](classes.md#method-parameters)), exceto que deve ser especificado pelo menos um parâmetro e que o `ref` e `out` modificadores de parâmetro não são permitidos. .</span><span class="sxs-lookup"><span data-stu-id="86af4-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="86af4-1276">O *tipo* de um indexador e cada um dos tipos referenciados nas *formal_parameter_list* deve ser pelo menos tão acessíveis quanto o próprio indexador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-1277">Uma *indexer_body* qualquer um pode consistir de uma ***corpo do acessador*** ou uma ***corpo da expressão***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="86af4-1278">Em um corpo de acessador *accessor_declarations*, que deve ser colocada em "`{`"e"`}`" tokens, declare acessadores ([acessadores](classes.md#accessors)) da propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="86af4-1279">Os acessadores de especificam as instruções executáveis associadas com a leitura e gravação de propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="86af4-1280">Um corpo de expressão consiste em "`=>`" seguido por uma expressão `E` e um ponto e vírgula é exatamente equivalente para o corpo da instrução `{ get { return E; } }`e pode, portanto, apenas ser usado para especificar indexadores somente getter onde está o resultado do getter fornecido por uma única expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="86af4-1281">Embora a sintaxe para acessar um elemento do indexador é o mesmo que para um elemento de matriz, um elemento do indexador não é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="86af4-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="86af4-1282">Assim, não é possível passar um elemento do indexador como um `ref` ou `out` argumento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="86af4-1283">Lista de parâmetros formais de um indexador define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="86af4-1284">Especificamente, a assinatura de um indexador consiste em número e tipos de seus parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="86af4-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="86af4-1285">O tipo de elemento e os nomes dos parâmetros formais não fazem parte da assinatura do indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="86af4-1286">A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="86af4-1287">Indexadores e propriedades são muito semelhantes em conceito, mas diferem das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="86af4-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="86af4-1288">Uma propriedade é identificada por seu nome, enquanto um indexador é identificado por sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="86af4-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="86af4-1289">Uma propriedade é acessada por meio de um *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)), enquanto um indexador elemento é acessado por meio de um *element_access* ([acesso ao indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="86af4-1290">Uma propriedade pode ser um `static` membro, enquanto um indexador é sempre um membro de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="86af4-1291">Um `get` acessador de uma propriedade corresponde a um método sem parâmetros, enquanto um `get` acessador de um indexador corresponde a um método com a mesma lista de parâmetro formal que o indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="86af4-1292">Um `set` acessador de uma propriedade corresponde a um método com um parâmetro único chamado `value`, enquanto um `set` acessador de um indexador corresponde a um método com a mesma lista de parâmetro formal que o indexador, além de um parâmetro adicional chamado `value`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="86af4-1293">É um erro de tempo de compilação para um acessador de indexador declarar uma variável local com o mesmo nome como um parâmetro de indexador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="86af4-1294">Em uma declaração de propriedade de substituição, a propriedade herdada é acessada usando a sintaxe `base.P`, onde `P` é o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="86af4-1295">Em uma declaração de indexador de substituição, o indexador herdado é acessado usando a sintaxe `base[E]`, onde `E` é uma lista separada por vírgulas de expressões.</span><span class="sxs-lookup"><span data-stu-id="86af4-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="86af4-1296">Não há nenhum conceito de um "indexador implementado automaticamente".</span><span class="sxs-lookup"><span data-stu-id="86af4-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="86af4-1297">É um erro ter um indexador não-abstrata, não externa com acessadores de ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="86af4-1298">Além dessas diferenças, todas as regras definidas em [acessadores](classes.md#accessors) e [implementadas automaticamente propriedades](classes.md#automatically-implemented-properties) se aplicam a acessadores de indexador, bem como para acessadores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="86af4-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="86af4-1299">Quando uma declaração de indexador inclui um `extern` modificador, o indexador é considerado uma ***indexador externo***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="86af4-1300">Como uma declaração de indexador externo não fornece nenhuma implementação real, cada um dos seus *accessor_declarations* consiste em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="86af4-1301">O exemplo a seguir declara um `BitArray` classe que implementa um indexador para acessar os bits individuais na matriz de bits.</span><span class="sxs-lookup"><span data-stu-id="86af4-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="86af4-1302">Uma instância da `BitArray` classe consome significativamente menos memória do que um correspondente `bool[]` (já que cada valor do primeiro ocupa apenas um bit em vez do último de um byte), mas permite que as mesmas operações que um `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="86af4-1303">O seguinte `CountPrimes` classe usa um `BitArray` e o algoritmo de "sieve" clássico para calcular o número de números primos entre 1 e um máximo de determinado:</span><span class="sxs-lookup"><span data-stu-id="86af4-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="86af4-1304">Observe que a sintaxe para acessar os elementos do `BitArray` é exatamente a mesma usada para um `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="86af4-1305">O exemplo a seguir mostra uma classe de 26 \* 10 grade que tem um indexador com dois parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="86af4-1306">O primeiro parâmetro deve ser uma letra maiuscula ou letra minúscula no intervalo A-Z e a segunda é necessário para ser um número inteiro no intervalo de 0 a 9.</span><span class="sxs-lookup"><span data-stu-id="86af4-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="86af4-1307">Sobrecarga de indexador</span><span class="sxs-lookup"><span data-stu-id="86af4-1307">Indexer overloading</span></span>

<span data-ttu-id="86af4-1308">As regras de resolução de sobrecarga do indexador são descritas em [inferência de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="86af4-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="86af4-1309">Operadores</span><span class="sxs-lookup"><span data-stu-id="86af4-1309">Operators</span></span>

<span data-ttu-id="86af4-1310">Uma ***operador*** é um membro que define o significado de um operador de expressão que pode ser aplicado a instâncias da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="86af4-1311">Operadores são declarados usando *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="86af4-1312">Há três categorias de operadores sobrecarregáveis: Operadores unários ([operadores unários](classes.md#unary-operators)), operadores binários ([operadores binários](classes.md#binary-operators)) e os operadores de conversão ([operadores de conversão](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="86af4-1313">O *operator_body* é um vírgula, um ***corpo da instrução*** ou uma ***corpo da expressão***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="86af4-1314">Um corpo de declaração consiste em uma *bloco*, que especifica as instruções para executar quando o operador é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="86af4-1315">O *bloco* deve obedecer às regras para retornar o valor métodos descritos [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="86af4-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="86af4-1316">Consiste em um corpo de expressão `=>` seguido por uma expressão e um ponto e vírgula e denota uma única expressão para executar quando o operador é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="86af4-1317">Para `extern` operadores, o *operator_body* consiste simplesmente em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="86af4-1318">Para todos os outros operadores, o *operator_body* é um corpo do bloco ou um corpo de expressão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="86af4-1319">As seguintes regras se aplicam a todas as declarações de operador:</span><span class="sxs-lookup"><span data-stu-id="86af4-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="86af4-1320">Uma declaração do operador deve incluir um `public` e um `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="86af4-1321">O parâmetro (s) de um operador deve ser parâmetros de valor ([parâmetros de valores](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="86af4-1322">É um erro de tempo de compilação para uma declaração do operador especificar `ref` ou `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="86af4-1323">A assinatura de um operador ([operadores unários](classes.md#unary-operators), [operadores binários](classes.md#binary-operators), [operadores de conversão](classes.md#conversion-operators)) deve ser diferente das assinaturas de todos os outros operadores declarados no mesma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="86af4-1324">Todos os tipos referenciados em uma declaração do operador devem ser pelo menos tão acessíveis quanto o próprio operador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="86af4-1325">É um erro para o mesmo modificador aparecer várias vezes em uma declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="86af4-1326">Cada categoria de operador impõe restrições adicionais, conforme descrito nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="86af4-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="86af4-1327">Como outros membros, os operadores declarados em uma classe base são herdados por classes derivadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="86af4-1328">Como declarações de operador sempre exigem a classe ou struct no qual o operador é declarado para participar na assinatura do operador, não é possível que um operador declarado em uma classe derivada ocultar um operador declarado em uma classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="86af4-1329">Portanto, o `new` modificador nunca é necessária e, portanto, nunca permitido, em uma declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="86af4-1330">Informações adicionais sobre os operadores unários e binários podem ser encontradas no [operadores](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="86af4-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="86af4-1331">Informações adicionais sobre os operadores de conversão podem ser encontradas no [conversões definidas pelo usuário](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="86af4-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="86af4-1332">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="86af4-1332">Unary operators</span></span>

<span data-ttu-id="86af4-1333">As seguintes regras se aplicam às declarações de operador unário, onde `T` indica o tipo de instância da classe ou struct que contém a declaração do operador:</span><span class="sxs-lookup"><span data-stu-id="86af4-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="86af4-1334">Um unário `+`, `-`, `!`, ou `~` operador deve levar um único parâmetro do tipo `T` ou `T?` e pode retornar qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="86af4-1335">Um unário `++` ou `--` operador deve levar um único parâmetro do tipo `T` ou `T?` e deve retornar o mesmo tipo ou um tipo derivado dele.</span><span class="sxs-lookup"><span data-stu-id="86af4-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="86af4-1336">Um unário `true` ou `false` operador deve levar um único parâmetro do tipo `T` ou `T?` e deve retornar tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="86af4-1337">A assinatura de um operador unário consiste o token de operador (`+`, `-`, `!`, `~`, `++`, `--`, `true`, ou `false`) e o tipo do parâmetro formal único.</span><span class="sxs-lookup"><span data-stu-id="86af4-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="86af4-1338">O tipo de retorno não é parte de assinatura de um operador unário, nem é o nome do parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="86af4-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="86af4-1339">O `true` e `false` operadores unários exigem declaração por pares.</span><span class="sxs-lookup"><span data-stu-id="86af4-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="86af4-1340">Ocorrerá um erro de tempo de compilação se uma classe declara um destes operadores sem declarar também o outro.</span><span class="sxs-lookup"><span data-stu-id="86af4-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="86af4-1341">O `true` e `false` operadores são descritos mais detalhadamente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators) e [expressões Boolianas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="86af4-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="86af4-1342">O exemplo a seguir mostra uma implementação e uso subsequente de `operator ++` para uma classe de vetor de inteiro:</span><span class="sxs-lookup"><span data-stu-id="86af4-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="86af4-1343">Observe como o método do operador retorna o valor produzido, adicionando 1 ao operando, assim como o incremento de sufixo e operadores de decremento ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators)) e o incremento de prefixo e de decremento operadores ([incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="86af4-1344">Ao contrário em C++, esse método precisa não modificar o valor do operando diretamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="86af4-1345">Na verdade, modificando o valor do operando violar a semântica padrão de operador de incremento de sufixo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="86af4-1346">Operadores binários</span><span class="sxs-lookup"><span data-stu-id="86af4-1346">Binary operators</span></span>

<span data-ttu-id="86af4-1347">As seguintes regras se aplicam às declarações de operador binário, onde `T` indica o tipo de instância da classe ou struct que contém a declaração do operador:</span><span class="sxs-lookup"><span data-stu-id="86af4-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="86af4-1348">Um operador de deslocamento não binário deve levar dois parâmetros, pelo menos um dos quais deve ter o tipo `T` ou `T?`e pode retornar qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="86af4-1349">Um binário `<<` ou `>>` operador deve levar dois parâmetros, o primeiro deles deve ser do tipo `T` ou `T?` e o segundo dos quais deve ser do tipo `int` ou `int?`e pode retornar qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="86af4-1350">A assinatura de um operador binário consiste o token de operador (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, ou `<=`) e os tipos dos dois parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="86af4-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="86af4-1351">O tipo de retorno e os nomes dos parâmetros formais não fazem parte da assinatura de um operador binário.</span><span class="sxs-lookup"><span data-stu-id="86af4-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="86af4-1352">Determinados operadores binários requerem uma declaração por pares.</span><span class="sxs-lookup"><span data-stu-id="86af4-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="86af4-1353">Para cada declaração de qualquer operador de um par, deve haver uma declaração correspondente do operador do par.</span><span class="sxs-lookup"><span data-stu-id="86af4-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="86af4-1354">Duas declarações do operador correspondam quando tiverem o mesmo tipo de retorno e o mesmo tipo para cada parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86af4-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="86af4-1355">Os operadores a seguir exigem uma declaração por pares:</span><span class="sxs-lookup"><span data-stu-id="86af4-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="86af4-1356">`operator ==` e `operator !=`</span><span class="sxs-lookup"><span data-stu-id="86af4-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="86af4-1357">`operator >` e `operator <`</span><span class="sxs-lookup"><span data-stu-id="86af4-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="86af4-1358">`operator >=` e `operator <=`</span><span class="sxs-lookup"><span data-stu-id="86af4-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="86af4-1359">Operadores de conversão</span><span class="sxs-lookup"><span data-stu-id="86af4-1359">Conversion operators</span></span>

<span data-ttu-id="86af4-1360">Uma declaração do operador de conversão apresenta uma ***conversão definida pelo usuário*** ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)) que amplia as conversões implícitas e explícitas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="86af4-1361">Uma declaração do operador de conversão que inclui o `implicit` palavra-chave introduz uma conversão implícita definidas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="86af4-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="86af4-1362">Conversões implícitas podem ocorrer em uma variedade de situações, incluindo as invocações de função de membro, expressões de conversão e atribuições.</span><span class="sxs-lookup"><span data-stu-id="86af4-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="86af4-1363">Isso é descrito posteriormente em [conversões implícitas](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="86af4-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="86af4-1364">Uma declaração do operador de conversão que inclui o `explicit` palavra-chave introduz uma conversão explícita definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="86af4-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="86af4-1365">Conversões explícitas podem ocorrer em expressões de conversão e são descritas mais detalhadamente no [conversões explícitas](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="86af4-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="86af4-1366">Um operador de conversão converte de um tipo de fonte, indicado pelo tipo de parâmetro de operador de conversão, para um tipo de destino, indicado pelo tipo de retorno do operador de conversão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="86af4-1367">Para um tipo de origem especificado `S` e o tipo de destino `T`, se `S` ou `T` são tipos que permitem valor nulos, deixe `S0` e `T0` consulte seus tipos base, caso contrário `S0` e `T0` são igual a `S` e `T` , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="86af4-1368">Uma classe ou struct é permitida para declarar uma conversão de um tipo de fonte `S` para um tipo de destino `T` somente se todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="86af4-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="86af4-1369">`S0` e `T0` são de tipos diferentes.</span><span class="sxs-lookup"><span data-stu-id="86af4-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="86af4-1370">Tanto `S0` ou `T0` é o tipo de classe ou struct em que a declaração do operador ocorre.</span><span class="sxs-lookup"><span data-stu-id="86af4-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="86af4-1371">Nem `S0` nem `T0` é um *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="86af4-1372">Excluindo conversões definidas pelo usuário, uma conversão não existe na `S` à `T` ou do `T` para `S`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="86af4-1373">Para fins dessas regras, qualquer tipo de parâmetros associados `S` ou `T` são considerados tipos exclusivos que têm nenhuma relação de herança com outros tipos e quaisquer restrições no tipo desses parâmetros serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="86af4-1374">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="86af4-1375">a primeira operátoru duas é permitidas porque, para as finalidades [indexadores](classes.md#indexers).3, `T` e `int` e `string` respectivamente são considerados tipos exclusivos sem relação alguma com.</span><span class="sxs-lookup"><span data-stu-id="86af4-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="86af4-1376">No entanto, o terceiro operador é um erro, porque `C<T>` é a classe base de `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="86af4-1377">Na segunda regra que segue um operador de conversão deve converter para ou do tipo de classe ou estrutura na qual o operador é declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="86af4-1378">Por exemplo, é possível para um tipo de classe ou struct `C` para definir uma conversão de `C` para `int` e do `int` para `C`, mas não contra `int` para `bool`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="86af4-1379">Não é possível redefinir diretamente uma conversão predefinida.</span><span class="sxs-lookup"><span data-stu-id="86af4-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="86af4-1380">Assim, os operadores de conversão não são permitidos para converter de ou para o `object` porque já existem conversões implícitas e explícitas entre `object` e todos os outros tipos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="86af4-1381">Da mesma forma, nem a origem ou os tipos de destino de uma conversão podem ser um tipo base dos outros, uma vez que uma conversão, em seguida, seria já existe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="86af4-1382">No entanto, é possível declarar operadores em tipos genéricos que, para argumentos de tipo específico, especificam as conversões que já existem como conversões predefinidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="86af4-1383">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="86af4-1384">Quando digita `object` é especificado como um argumento de tipo para `T`, o segundo operador declara uma conversão que já existe (implícito e, portanto, também um valor explícito, existe conversão de qualquer tipo para `object`).</span><span class="sxs-lookup"><span data-stu-id="86af4-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="86af4-1385">Em casos em que uma conversão predefinida existe entre dois tipos, as conversões definidas pelo usuário entre esses tipos são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="86af4-1386">Especificamente:</span><span class="sxs-lookup"><span data-stu-id="86af4-1386">Specifically:</span></span>

*  <span data-ttu-id="86af4-1387">Se uma conversão implícita predefinida ([conversões implícitas](conversions.md#implicit-conversions)) do tipo `S` digitar `T`, todas as conversões definidas por usuário (implícitas ou explícitas) do `S` para `T` são ignorados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="86af4-1388">Se uma conversão explícita predefinida ([conversões explícitas](conversions.md#explicit-conversions)) do tipo `S` digitar `T`, nenhuma conversão explícita definida pelo usuário do `S` para `T` são ignorados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="86af4-1389">Além disso:</span><span class="sxs-lookup"><span data-stu-id="86af4-1389">Furthermore:</span></span>

<span data-ttu-id="86af4-1390">Se `T` é um tipo de interface, definido pelo usuário conversões implícitas de `S` para `T` são ignorados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="86af4-1391">Caso contrário, definido pelo usuário conversões implícitas de `S` para `T` ainda são consideradas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="86af4-1392">Para todos os tipos, mas `object`, os operadores são declarados pelo `Convertible<T>` tipo acima não entram em conflito com conversões predefinidas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="86af4-1393">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="86af4-1394">No entanto, para o tipo `object`, conversões predefinidas ocultar as conversões definidas pelo usuário em todos os casos, mas um:</span><span class="sxs-lookup"><span data-stu-id="86af4-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="86af4-1395">Conversões definidas pelo usuário não são permitidas para converter de ou para o *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="86af4-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="86af4-1396">Em particular, essa restrição garante que nenhum transformações definidas pelo usuário ocorram durante a conversão em um *interface_type*e que uma conversão para um *interface_type* terá êxito apenas se o objeto Na verdade, que está sendo convertido implementa especificado *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="86af4-1397">A assinatura de um operador de conversão consiste no tipo de origem e o tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="86af4-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="86af4-1398">(Observe que isso é o único formulário de membro para o qual o tipo de retorno participa na assinatura.) O `implicit` ou `explicit` classificação de um operador de conversão não é parte da assinatura do operador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="86af4-1399">Dessa forma, uma classe ou struct não pode declarar tanto uma `implicit` e um `explicit` operador de conversão com os mesmos tipos de origem e destino.</span><span class="sxs-lookup"><span data-stu-id="86af4-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="86af4-1400">Em geral, as conversões implícitas definidas pelo usuário devem ser criadas para nunca lançam exceções e perder informações.</span><span class="sxs-lookup"><span data-stu-id="86af4-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="86af4-1401">Se uma conversão definida pelo usuário pode gerar exceções (por exemplo, porque o argumento de origem está fora do intervalo) ou perda de informações (por exemplo, descartando os bits de ordem superior) e, em seguida, essa conversão deve ser definida como uma conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="86af4-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="86af4-1402">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="86af4-1403">a conversão de `Digit` à `byte` é implícito porque nunca gera exceções ou perde informações, mas a conversão de `byte` à `Digit` é explícito desde `Digit` pode representar apenas um subconjunto dos possíveis valores de um `byte`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="86af4-1404">Construtores de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-1404">Instance constructors</span></span>

<span data-ttu-id="86af4-1405">Um ***construtor de instância*** é um membro que implementa as ações necessárias para inicializar uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="86af4-1406">Construtores de instância são declaradas usando *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="86af4-1407">Um *constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)) e uma `extern` ([métodos externos](classes.md#external-methods)) modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="86af4-1408">Uma declaração de construtor não é permitida para incluir o modificador mesmo várias vezes.</span><span class="sxs-lookup"><span data-stu-id="86af4-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="86af4-1409">O *identificador* de uma *constructor_declarator* deve nomear a classe na qual o construtor de instância é declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="86af4-1410">Se nenhum outro nome for especificado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="86af4-1411">Opcional *formal_parameter_list* de uma instância do construtor está sujeito às mesmas regras que o *formal_parameter_list* de um método ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="86af4-1412">Lista de parâmetros formais define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um construtor de instância e controla o processo pelo qual resolução de sobrecarga ([inferência de tipo](expressions.md#type-inference)) seleciona um determinado Construtor de instância em uma invocação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="86af4-1413">Cada um dos tipos referenciados nos *formal_parameter_list* de uma instância de construtor deve ser pelo menos tão acessíveis quanto o próprio construtor ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="86af4-1414">Opcional *constructor_initializer* Especifica outro construtor de instância para invocar antes de executar as instruções fornecidas na *constructor_body* desse construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="86af4-1415">Isso é descrito posteriormente em [inicializadores de construtor](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="86af4-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="86af4-1416">Quando uma declaração de construtor inclui um `extern` modificador, o construtor deve ser um ***externo construtor***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="86af4-1417">Como uma declaração de construtor externo não fornece nenhuma implementação real, sua *constructor_body* consiste em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="86af4-1418">Para todos os outros construtores, o *constructor_body* consiste em um *bloco* que especifica as instruções para inicializar uma nova instância da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="86af4-1419">Isso corresponde exatamente a *bloco* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="86af4-1420">Construtores de instância não são herdadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="86af4-1421">Dessa forma, uma classe não tem nenhum construtor de instância que não sejam aqueles realmente declarados na classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="86af4-1422">Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão é fornecido automaticamente ([1&gt;construtores padrão](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="86af4-1423">Construtores de instância são invocados pelo *object_creation_expression*s ([expressões de criação do objeto](expressions.md#object-creation-expressions)) e por meio das *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="86af4-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="86af4-1424">Inicializadores de construtores</span><span class="sxs-lookup"><span data-stu-id="86af4-1424">Constructor initializers</span></span>

<span data-ttu-id="86af4-1425">Todos os construtores de instância (exceto aqueles para a classe `object`) implicitamente incluem uma invocação de outro construtor de instância imediatamente anterior a *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="86af4-1426">O construtor para invocar implicitamente é determinado pelo *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="86af4-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="86af4-1427">Um inicializador do construtor de instância do formulário `base(argument_list)` ou `base()` faz com que um construtor de instância da classe base direta a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="86af4-1428">Esse construtor está selecionado, usando *argument_list* se estiverem presentes e as regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="86af4-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="86af4-1429">O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessível contidos na classe base direta ou o construtor padrão ([1&gt;construtores padrão](classes.md#default-constructors)), se nenhum construtor de instância é declaradas no classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="86af4-1430">Se esse conjunto está vazio ou se um construtor de instância única melhor não pode ser identificado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="86af4-1431">Um inicializador do construtor de instância do formulário `this(argument-list)` ou `this()` faz com que um construtor de instância da classe em si a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="86af4-1432">O construtor está selecionado, usando *argument_list* se estiverem presentes e as regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="86af4-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="86af4-1433">O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessível declarados na classe em si.</span><span class="sxs-lookup"><span data-stu-id="86af4-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="86af4-1434">Se esse conjunto está vazio ou se um construtor de instância única melhor não pode ser identificado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="86af4-1435">Se uma declaração de construtor de instância inclui um inicializador de construtor que invoca o construtor em si, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="86af4-1436">Se um construtor de instância não tem nenhum inicializador de construtor, um inicializador de construtor do formulário `base()` é fornecido implicitamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="86af4-1437">Portanto, uma declaração de construtor de instância do formulário</span><span class="sxs-lookup"><span data-stu-id="86af4-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="86af4-1438">é exatamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="86af4-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="86af4-1439">O escopo dos parâmetros concedida pela *formal_parameter_list* de um construtor de instância declaração inclui o inicializador do construtor de declaração.</span><span class="sxs-lookup"><span data-stu-id="86af4-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="86af4-1440">Portanto, um inicializador de construtor tem permissão para acessar os parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="86af4-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="86af4-1441">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="86af4-1442">Um inicializador de construtor de instância não é possível acessar a instância que está sendo criada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="86af4-1443">Portanto, é um erro de tempo de compilação para fazer referência a `this` em uma expressão de argumento o inicializador do construtor, como é um erro de tempo de compilação para uma expressão de argumento fazer referência a qualquer membro de instância por meio de um *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="86af4-1444">Inicializadores de variável de instância</span><span class="sxs-lookup"><span data-stu-id="86af4-1444">Instance variable initializers</span></span>

<span data-ttu-id="86af4-1445">Quando um construtor de instância não tem nenhum inicializador de construtor, ou ele tem um inicializador de construtor do formulário `base(...)`, esse construtor implicitamente executa as inicializações especificadas pelo *variable_initializer*s de os campos de instância é declarado na sua classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="86af4-1446">Isso corresponde a uma sequência de atribuições que são executados imediatamente após a entrada para o construtor e antes da invocação do construtor de classe base direta implícita.</span><span class="sxs-lookup"><span data-stu-id="86af4-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="86af4-1447">Os inicializadores de variável são executados na ordem textual em que aparecem na declaração da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="86af4-1448">Execução do construtor</span><span class="sxs-lookup"><span data-stu-id="86af4-1448">Constructor execution</span></span>

<span data-ttu-id="86af4-1449">Inicializadores de variável são transformados em instruções de atribuição, e essas instruções de atribuição são executadas antes da invocação do construtor de instância de classe base.</span><span class="sxs-lookup"><span data-stu-id="86af4-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="86af4-1450">Essa ordenação garantirá que todos os campos de instância são inicializados por seus inicializadores de variável antes de quaisquer declarações que têm acesso a essa instância são executadas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="86af4-1451">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="86af4-1452">Quando `new B()` é usado para criar uma instância de `B`, a seguinte saída é produzida:</span><span class="sxs-lookup"><span data-stu-id="86af4-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="86af4-1453">O valor de `x` é 1, porque o inicializador de variável é executado antes que o construtor de instância de classe base seja invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="86af4-1454">No entanto, o valor de `y` é 0 (o valor padrão de um `int`) porque a atribuição ao `y` não é executada até depois que o construtor de classe base retorna.</span><span class="sxs-lookup"><span data-stu-id="86af4-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="86af4-1455">É útil pensar em inicializadores de construtor e inicializadores de variável de instância como instruções que são automaticamente inseridas antes do *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="86af4-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="86af4-1456">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="86af4-1457">contém várias inicializadores de variável; Ele também contém os inicializadores de construtor de ambos os formulários (`base` e `this`).</span><span class="sxs-lookup"><span data-stu-id="86af4-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="86af4-1458">O exemplo corresponde ao código mostrado abaixo, onde cada comentário indica uma instrução inserida automaticamente (a sintaxe usada para as invocações de construtor automaticamente inserido não é válido, mas serve apenas para ilustrar o mecanismo).</span><span class="sxs-lookup"><span data-stu-id="86af4-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="86af4-1459">Construtores padrão</span><span class="sxs-lookup"><span data-stu-id="86af4-1459">Default constructors</span></span>

<span data-ttu-id="86af4-1460">Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão é fornecido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="86af4-1461">Esse construtor padrão simplesmente invoca o construtor sem parâmetros da classe base direta.</span><span class="sxs-lookup"><span data-stu-id="86af4-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="86af4-1462">Se a classe é abstrata a acessibilidade declarada para o construtor padrão é protegida.</span><span class="sxs-lookup"><span data-stu-id="86af4-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="86af4-1463">Caso contrário, a acessibilidade declarada para o construtor padrão é pública.</span><span class="sxs-lookup"><span data-stu-id="86af4-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="86af4-1464">Portanto, o construtor padrão sempre é do formulário</span><span class="sxs-lookup"><span data-stu-id="86af4-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="86af4-1465">ou</span><span class="sxs-lookup"><span data-stu-id="86af4-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="86af4-1466">onde `C` é o nome da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="86af4-1467">Se a resolução de sobrecarga não puder determinar um melhor candidato exclusivo para o inicializador do construtor de classe base ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="86af4-1468">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="86af4-1469">um construtor padrão é fornecido porque a classe não contém nenhuma declaração de construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="86af4-1470">Portanto, o exemplo é precisamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="86af4-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="86af4-1471">Construtores particulares</span><span class="sxs-lookup"><span data-stu-id="86af4-1471">Private constructors</span></span>

<span data-ttu-id="86af4-1472">Quando uma classe `T` declara apenas construtores de instância privada, não é possível para as classes fora do texto de programa do `T` derivar `T` ou para criar diretamente instâncias do `T`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="86af4-1473">Assim, se uma classe contém apenas membros estáticos e não se destina a ser instanciado, adicionar um construtor de instância privada vazio impedirá instanciação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="86af4-1474">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="86af4-1475">O `Trig` classe grupos constantes e métodos relacionados, mas não se destina a ser instanciado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="86af4-1476">Portanto, ele declara um construtor de instância única de privado vazio.</span><span class="sxs-lookup"><span data-stu-id="86af4-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="86af4-1477">Construtor de pelo menos uma instância deve ser declarado para suprimir a geração automática de um construtor padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="86af4-1478">Parâmetros do construtor de instância opcional</span><span class="sxs-lookup"><span data-stu-id="86af4-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="86af4-1479">O `this(...)` formulário de inicializador de construtor é comumente usado em conjunto com sobrecarga para implementar os parâmetros do construtor de instância opcional.</span><span class="sxs-lookup"><span data-stu-id="86af4-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="86af4-1480">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="86af4-1481">os construtores de dois instância primeiro simplesmente fornecem os valores padrão para os argumentos ausentes.</span><span class="sxs-lookup"><span data-stu-id="86af4-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="86af4-1482">Ambos usam um `this(...)` inicializador de construtor para invocar o terceiro construtor de instância, o que realmente faz o trabalho de inicializar a nova instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="86af4-1483">O efeito é que um dos parâmetros do construtor opcional:</span><span class="sxs-lookup"><span data-stu-id="86af4-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="86af4-1484">Construtores estáticos</span><span class="sxs-lookup"><span data-stu-id="86af4-1484">Static constructors</span></span>

<span data-ttu-id="86af4-1485">Um ***construtor estático*** é um membro que implementa as ações necessárias para inicializar um tipo de classe fechado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="86af4-1486">Construtores estáticos são declarados usando *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="86af4-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="86af4-1487">Um *static_constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e um `extern` modificador ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="86af4-1488">O *identificador* de uma *static_constructor_declaration* deve nomear a classe na qual o construtor estático é declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="86af4-1489">Se nenhum outro nome for especificado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="86af4-1490">Quando uma declaração de construtor estático inclui um `extern` modificador, o construtor estático é considerado uma ***externo construtor estático***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="86af4-1491">Como uma declaração de construtor estático externo não fornece nenhuma implementação real, sua *static_constructor_body* consiste em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="86af4-1492">Para todas as outras declarações de construtor estático, o *static_constructor_body* consiste em um *bloco* que especifica as instruções para executar a fim de inicializar a classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="86af4-1493">Isso corresponde exatamente a *method_body* de um método estático com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="86af4-1494">Construtores estáticos não são herdados e não podem ser chamados diretamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="86af4-1495">O construtor estático para um tipo de classe fechado é executado no máximo uma vez em um determinado domínio de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="86af4-1496">A execução de um construtor estático é disparada pela primeira dos seguintes eventos ocorra dentro de um domínio de aplicativo:</span><span class="sxs-lookup"><span data-stu-id="86af4-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="86af4-1497">Uma instância do tipo de classe é criada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="86af4-1498">Qualquer um dos membros estáticos do tipo de classe são referenciados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="86af4-1499">Se uma classe contém o `Main` método ([inicialização do aplicativo](basic-concepts.md#application-startup)) em que a execução começa, o construtor estático para essa classe é executado antes de `Main` método é chamado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="86af4-1500">Para inicializar um novo tipo de classes fechado, primeiro um novo conjunto de campos estáticos ([campos estáticos e de instância](classes.md#static-and-instance-fields)) para esse tipo específico de fechado é criado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="86af4-1501">Cada um dos campos estáticos é inicializada com seu valor padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="86af4-1502">Em seguida, os inicializadores de campo estático ([inicialização do campo estático](classes.md#static-field-initialization)) são executadas para esses campos estáticos.</span><span class="sxs-lookup"><span data-stu-id="86af4-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="86af4-1503">Por fim, o construtor estático é executado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="86af4-1504">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="86af4-1505">deve produzir a saída:</span><span class="sxs-lookup"><span data-stu-id="86af4-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="86af4-1506">porque a execução de `A`do construtor estático é disparado pela chamada para `A.F`e a execução de `B`do construtor estático é disparado pela chamada para `B.F`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="86af4-1507">É possível criar dependências circulares que permitem que os campos estáticos com inicializadores de variável a ser observado em seu estado de valor padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="86af4-1508">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="86af4-1509">produz a saída</span><span class="sxs-lookup"><span data-stu-id="86af4-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="86af4-1510">Para executar o `Main` método, o sistema pela primeira vez é executado o inicializador para `B.Y`, antes de classe `B`do construtor estático.</span><span class="sxs-lookup"><span data-stu-id="86af4-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="86af4-1511">`Y`do inicializador faz com que `A`do construtor estático para ser executado porque o valor de `A.X` é referenciado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="86af4-1512">O construtor estático de `A` por sua vez prossegue para calcular o valor da `X`e fazer buscas caso o valor padrão de `Y`, que é zero.</span><span class="sxs-lookup"><span data-stu-id="86af4-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="86af4-1513">`A.X` Portanto, é inicializado como 1.</span><span class="sxs-lookup"><span data-stu-id="86af4-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="86af4-1514">O processo de execução `A`do construtor estático e inicializadores de campo estático, em seguida, for concluído, retornando para o cálculo do valor inicial do `Y`, cujo resultado se torna 2.</span><span class="sxs-lookup"><span data-stu-id="86af4-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="86af4-1515">Como o construtor estático é executado exatamente uma vez para cada fechado o tipo de classe construída, é um local conveniente para impor verificações de tempo de execução no parâmetro de tipo não podem ser verificados em tempo de compilação por meio de restrições ([parâmetro de tipo restrições de](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="86af4-1516">Por exemplo, o tipo a seguir usa um construtor estático para impor que o argumento de tipo é um enum:</span><span class="sxs-lookup"><span data-stu-id="86af4-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="86af4-1517">Destruidores</span><span class="sxs-lookup"><span data-stu-id="86af4-1517">Destructors</span></span>

<span data-ttu-id="86af4-1518">Um ***destruidor*** é um membro que implementa as ações necessárias para destruir uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="86af4-1519">Um destruidor for declarado usando um *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="86af4-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="86af4-1520">Um *destructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="86af4-1521">O *identificador* de uma *destructor_declaration* deve nomear a classe em que o destruidor for declarado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="86af4-1522">Se nenhum outro nome for especificado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86af4-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="86af4-1523">Quando uma declaração de destruidor inclui um `extern` modificador, o destruidor deve ser um ***destruidor externo***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="86af4-1524">Como uma declaração de destruidor externo não fornece nenhuma implementação real, sua *destructor_body* consiste em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="86af4-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="86af4-1525">Para todos os destruidores, o *destructor_body* consiste em um *bloco* que especifica as instruções a executar para destruir uma instância da classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="86af4-1526">Um *destructor_body* corresponde exatamente a *method_body* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="86af4-1527">Destruidores não são herdados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1527">Destructors are not inherited.</span></span> <span data-ttu-id="86af4-1528">Portanto, uma classe não possui nenhum destruidor diferente daquele que podem ser declarados nessa classe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="86af4-1529">Uma vez que um destruidor deve não ter nenhum parâmetro, não pode ser sobrecarregado, portanto, uma classe pode ter, no máximo, um destruidor.</span><span class="sxs-lookup"><span data-stu-id="86af4-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="86af4-1530">Os destruidores são invocados automaticamente e não podem ser invocados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="86af4-1531">Uma instância não estiver qualificada para destruição quando ele não é mais possível para qualquer código usar essa instância.</span><span class="sxs-lookup"><span data-stu-id="86af4-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="86af4-1532">Execução do destruidor para a instância pode ocorrer a qualquer momento depois que a instância se tornar qualificada para destruição.</span><span class="sxs-lookup"><span data-stu-id="86af4-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="86af4-1533">Quando uma instância é destruída, os destruidores na cadeia de herança da instância são chamados na ordem, do mais derivado para menos derivado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="86af4-1534">Um destruidor pode ser executado em qualquer thread.</span><span class="sxs-lookup"><span data-stu-id="86af4-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="86af4-1535">Para mais informações sobre as regras que determinam quando e como um destruidor é executado, consulte [gerenciamento automático de memória](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="86af4-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="86af4-1536">A saída do exemplo</span><span class="sxs-lookup"><span data-stu-id="86af4-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="86af4-1537">is</span><span class="sxs-lookup"><span data-stu-id="86af4-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="86af4-1538">uma vez que os destruidores em uma cadeia de herança são chamados na ordem, do mais derivado para menos derivado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="86af4-1539">Os destruidores são implementados, substituindo o método virtual `Finalize` em `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="86af4-1540">Programas em C# não tem permissão para substituir este método ou ligue para (ou substituições dele) diretamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="86af4-1541">Por exemplo, o programa</span><span class="sxs-lookup"><span data-stu-id="86af4-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="86af4-1542">contém dois erros.</span><span class="sxs-lookup"><span data-stu-id="86af4-1542">contains two errors.</span></span>

<span data-ttu-id="86af4-1543">O compilador se comporta como se esse método e substituições do mesmo, não existem em todos os.</span><span class="sxs-lookup"><span data-stu-id="86af4-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="86af4-1544">Portanto, este programa:</span><span class="sxs-lookup"><span data-stu-id="86af4-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="86af4-1545">é válido, e o método mostrado oculta `System.Object`do `Finalize` método.</span><span class="sxs-lookup"><span data-stu-id="86af4-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="86af4-1546">Para obter uma discussão sobre o comportamento quando uma exceção for lançada de um destruidor, consulte [como exceções são tratadas](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="86af4-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="86af4-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="86af4-1547">Iterators</span></span>

<span data-ttu-id="86af4-1548">Um membro da função ([membros de função](expressions.md#function-members)) implementado usando um bloco de iteradores ([blocos](statements.md#blocks)) é chamado um ***iterador***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="86af4-1549">Um bloco de iteradores pode ser usado como o corpo de um membro da função, desde que o tipo de retorno do membro da função correspondente é uma das interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) ou uma das interfaces enumeráveis ([Interfaces enumeráveis](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="86af4-1550">Ele pode ocorrer como um *method_body*, *operator_body* ou *accessor_body*, enquanto eventos, construtores de instância, estáticas construtores e destruidores não podem ser implementadas como iteradores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="86af4-1551">Quando um membro da função é implementado usando um bloco de iteradores, ele é um erro de tempo de compilação para a lista de parâmetros formais do membro da função para especificar qualquer `ref` ou `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="86af4-1552">Interfaces de enumerador</span><span class="sxs-lookup"><span data-stu-id="86af4-1552">Enumerator interfaces</span></span>

<span data-ttu-id="86af4-1553">O ***interfaces de enumerador*** são a interface não genérica `System.Collections.IEnumerator` e em todas as instanciações da interface genérica `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="86af4-1554">Por questão de brevidade, neste capítulo essas interfaces são referenciadas como `IEnumerator` e `IEnumerator<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="86af4-1555">Interfaces enumeráveis</span><span class="sxs-lookup"><span data-stu-id="86af4-1555">Enumerable interfaces</span></span>

<span data-ttu-id="86af4-1556">O ***interfaces enumeráveis*** são a interface não genérica `System.Collections.IEnumerable` e em todas as instanciações da interface genérica `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="86af4-1557">Por questão de brevidade, neste capítulo essas interfaces são referenciadas como `IEnumerable` e `IEnumerable<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="86af4-1558">Tipo de rendimento</span><span class="sxs-lookup"><span data-stu-id="86af4-1558">Yield type</span></span>

<span data-ttu-id="86af4-1559">Um iterador produz uma sequência de valores, todos do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="86af4-1560">Esse tipo é chamado de ***yield tipo*** do iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="86af4-1561">O tipo de rendimento de um iterador que retorna `IEnumerator` ou `IEnumerable` é `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="86af4-1562">O tipo de rendimento de um iterador que retorna `IEnumerator<T>` ou `IEnumerable<T>` é `T`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="86af4-1563">Objetos de enumerador</span><span class="sxs-lookup"><span data-stu-id="86af4-1563">Enumerator objects</span></span>

<span data-ttu-id="86af4-1564">Quando um membro da função retornando um enumerador de tipo de interface é implementado usando um bloco de iteradores, invocar o membro da função não é imediatamente executado o código no bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="86af4-1565">Em vez disso, uma ***objeto enumerador*** é criada e retornada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="86af4-1566">Esse objeto encapsula o código especificado no bloco de iterador e a execução do código no bloco iterador ocorre quando o objeto de enumerador `MoveNext` método é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="86af4-1567">Um objeto de enumerador tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="86af4-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="86af4-1568">Ele implementa `IEnumerator` e `IEnumerator<T>`, onde `T` é o tipo de yield do iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="86af4-1569">Ele implementa `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="86af4-1570">Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor da instância passada para o membro da função.</span><span class="sxs-lookup"><span data-stu-id="86af4-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="86af4-1571">Ele tem quatro estados possíveis, ***antes de***, ***executando***, ***suspenso***, e ***depois***e é inicialmente no ***antes***  estado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="86af4-1572">Um objeto de enumerador normalmente é uma instância de uma classe de enumerador gerado pelo compilador que encapsula o código no bloco de iterador e implementa as interfaces de enumerador, mas outros métodos de implementação são possíveis.</span><span class="sxs-lookup"><span data-stu-id="86af4-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="86af4-1573">Se uma classe de enumerador é gerada pelo compilador, essa classe será ser aninhada, direta ou indiretamente, na classe que contém o membro da função, ela tem acessibilidade privada e ele terá um nome reservado para uso pelo compilador ([identificadores ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="86af4-1574">Um objeto de enumerador pode implementar mais interfaces daqueles especificados acima.</span><span class="sxs-lookup"><span data-stu-id="86af4-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="86af4-1575">As seções a seguir descrevem o comportamento exato do `MoveNext`, `Current`, e `Dispose` membros a `IEnumerable` e `IEnumerable<T>` fornecidas por um objeto de enumerador de implementações de interface.</span><span class="sxs-lookup"><span data-stu-id="86af4-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="86af4-1576">Observe que não têm suporte para objetos de enumerador a `IEnumerator.Reset` método.</span><span class="sxs-lookup"><span data-stu-id="86af4-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="86af4-1577">Invocar esse método faz com que um `System.NotSupportedException` seja lançada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="86af4-1578">O método MoveNext</span><span class="sxs-lookup"><span data-stu-id="86af4-1578">The MoveNext method</span></span>

<span data-ttu-id="86af4-1579">O `MoveNext` método de um objeto de enumerador encapsula o código de um bloco de iteradores.</span><span class="sxs-lookup"><span data-stu-id="86af4-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="86af4-1580">Invocar o `MoveNext` método executa o código no bloco de iteradores e conjuntos de `Current` propriedade do objeto enumerador conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="86af4-1581">Precisa ação executada pelo `MoveNext` depende do estado do objeto enumerador quando `MoveNext` é invocado:</span><span class="sxs-lookup"><span data-stu-id="86af4-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="86af4-1582">Se for o estado do objeto enumerador ***antes de***, chamar `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="86af4-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="86af4-1583">Altera o estado para ***executando***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="86af4-1584">Inicializa os parâmetros (incluindo `this`) do bloco de iterador com os valores de argumento e o valor de instância salvo quando o objeto de enumerador foi inicializado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="86af4-1585">Executa o bloco de iteradores desde o início até que a execução é interrompida (conforme descrito abaixo).</span><span class="sxs-lookup"><span data-stu-id="86af4-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="86af4-1586">Se for o estado do objeto enumerador ***em execução***, o resultado da invocação `MoveNext` não está especificado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="86af4-1587">Se for o estado do objeto enumerador ***suspensos***, chamar `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="86af4-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="86af4-1588">Altera o estado para ***executando***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="86af4-1589">Restaura os valores de todas as variáveis locais e parâmetros (inclusive este) para os valores salvos quando a execução do bloco iterador foi suspenso pela última vez.</span><span class="sxs-lookup"><span data-stu-id="86af4-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="86af4-1590">Observe que o conteúdo de todos os objetos referenciados por essas variáveis pode ter mudado desde a chamada anterior a MoveNext.</span><span class="sxs-lookup"><span data-stu-id="86af4-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="86af4-1591">Retoma a execução do bloco de iterador imediatamente após o `yield return` instrução que causou a suspensão de execução e continua até que a execução é interrompida (conforme descrito abaixo).</span><span class="sxs-lookup"><span data-stu-id="86af4-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="86af4-1592">Se for o estado do objeto enumerador ***após***, chamar `MoveNext` retorna `false`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="86af4-1593">Quando `MoveNext` executa o bloco de iteradores, execução pode ser interrompida de quatro maneiras: Por um `yield return` instrução, por um `yield break` instrução, encontrando o final do bloco de iterador e por uma exceção sendo lançada e propagada para fora do bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="86af4-1594">Quando um `yield return` instrução for encontrada ([a instrução yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="86af4-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="86af4-1595">A expressão fornecida na instrução é avaliada implicitamente convertida no tipo de rendimento e atribuída ao `Current` propriedade do objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="86af4-1596">Execução do corpo do iterador é suspenso.</span><span class="sxs-lookup"><span data-stu-id="86af4-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="86af4-1597">Os valores de todos os parâmetros e variáveis locais (incluindo `this`) são salvas, como é o local deste `yield return` instrução.</span><span class="sxs-lookup"><span data-stu-id="86af4-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="86af4-1598">Se o `yield return` instrução está dentro de um ou mais `try` bloqueia associado `finally` blocos não são executados no momento.</span><span class="sxs-lookup"><span data-stu-id="86af4-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="86af4-1599">O estado do objeto enumerador é alterado para ***suspenso***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="86af4-1600">O `MoveNext` retorno do método `true` para seu chamador, que indica que a iteração avançado com êxito para o próximo valor.</span><span class="sxs-lookup"><span data-stu-id="86af4-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="86af4-1601">Quando um `yield break` instrução for encontrada ([a instrução yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="86af4-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="86af4-1602">Se o `yield break` instrução está dentro de um ou mais `try` bloqueia associado `finally` blocos são executados.</span><span class="sxs-lookup"><span data-stu-id="86af4-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="86af4-1603">O estado do objeto enumerador é alterado para ***depois de***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="86af4-1604">O `MoveNext` retorno do método `false` para seu chamador, que indica que a iteração foi concluída.</span><span class="sxs-lookup"><span data-stu-id="86af4-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="86af4-1605">Quando o final do corpo do iterador for encontrado:</span><span class="sxs-lookup"><span data-stu-id="86af4-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="86af4-1606">O estado do objeto enumerador é alterado para ***depois de***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="86af4-1607">O `MoveNext` retorno do método `false` para seu chamador, que indica que a iteração foi concluída.</span><span class="sxs-lookup"><span data-stu-id="86af4-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="86af4-1608">Quando uma exceção é lançada e propagada para fora do bloco de iterador:</span><span class="sxs-lookup"><span data-stu-id="86af4-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="86af4-1609">Apropriado `finally` blocos no corpo do iterador serão foram executados pela propagação de exceção.</span><span class="sxs-lookup"><span data-stu-id="86af4-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="86af4-1610">O estado do objeto enumerador é alterado para ***depois de***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="86af4-1611">A propagação de exceção continuará para o chamador do `MoveNext` método.</span><span class="sxs-lookup"><span data-stu-id="86af4-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="86af4-1612">A propriedade atual</span><span class="sxs-lookup"><span data-stu-id="86af4-1612">The Current property</span></span>

<span data-ttu-id="86af4-1613">Um objeto de enumerador `Current` propriedade é afetada pela `yield return` instruções no bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="86af4-1614">Quando um objeto de enumerador está no ***suspensos*** de estado, o valor de `Current` é o valor definido pela chamada anterior para `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="86af4-1615">Quando um objeto de enumerador está no ***antes de***, ***executando***, ou ***depois*** afirma, o resultado de acessar `Current` não está especificado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="86af4-1616">Para um iterador com um rendimento tipo diferente de `object`, o resultado de acessando `Current` por meio do objeto de enumerador `IEnumerable` implementação corresponde ao acessar `Current` por meio do objeto de enumerador `IEnumerator<T>` implementação e convertendo o resultado para `object`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="86af4-1617">O método Dispose</span><span class="sxs-lookup"><span data-stu-id="86af4-1617">The Dispose method</span></span>

<span data-ttu-id="86af4-1618">O `Dispose` método é usado para limpar a iteração, colocando o objeto enumerador o ***depois*** estado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="86af4-1619">Se for o estado do objeto enumerador ***antes de***, chamar `Dispose` altera o estado para ***depois***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="86af4-1620">Se for o estado do objeto enumerador ***em execução***, o resultado da invocação `Dispose` não está especificado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="86af4-1621">Se for o estado do objeto enumerador ***suspensos***, chamar `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="86af4-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="86af4-1622">Altera o estado para ***executando***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="86af4-1623">Executa qualquer finalmente blocos como se o último executado `yield return` instrução foram um `yield break` instrução.</span><span class="sxs-lookup"><span data-stu-id="86af4-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="86af4-1624">Se isso faz com que uma exceção seja gerada e propagadas para fora do corpo do iterador, o estado do objeto enumerador é definido como ***após*** e a exceção é propagada para o chamador do `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="86af4-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="86af4-1625">Altera o estado para ***depois de***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="86af4-1626">Se for o estado do objeto enumerador ***após***, chamar `Dispose` não tem nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="86af4-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="86af4-1627">Objetos enumeráveis</span><span class="sxs-lookup"><span data-stu-id="86af4-1627">Enumerable objects</span></span>

<span data-ttu-id="86af4-1628">Quando um membro da função retornando um tipo enumerável interface é implementado usando um bloco de iteradores, invocar o membro da função não é imediatamente executado o código no bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="86af4-1629">Em vez disso, uma ***objeto enumerável*** é criada e retornada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="86af4-1630">O objeto enumerável `GetEnumerator` método retorna um objeto enumerador que encapsula o código especificado no bloco de iterador e a execução do código no bloco iterador ocorre quando o objeto de enumerador `MoveNext` método é invocado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="86af4-1631">Um objeto enumerável tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="86af4-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="86af4-1632">Ele implementa `IEnumerable` e `IEnumerable<T>`, onde `T` é o tipo de yield do iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="86af4-1633">Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor da instância passada para o membro da função.</span><span class="sxs-lookup"><span data-stu-id="86af4-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="86af4-1634">Um objeto enumerável normalmente é uma instância de uma classe enumerable gerado pelo compilador que encapsula o código no bloco de iterador e implementa as interfaces enumeráveis, mas outros métodos de implementação são possíveis.</span><span class="sxs-lookup"><span data-stu-id="86af4-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="86af4-1635">Se uma classe enumerable é gerada pelo compilador, essa classe será ser aninhada, direta ou indiretamente, na classe que contém o membro da função, ela tem acessibilidade privada e ele terá um nome reservado para uso pelo compilador ([identificadores ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="86af4-1636">Um objeto enumerável pode implementar mais interfaces daqueles especificados acima.</span><span class="sxs-lookup"><span data-stu-id="86af4-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="86af4-1637">Em particular, também pode implementar um objeto enumerável `IEnumerator` e `IEnumerator<T>`, habilitá-la para servir como um enumerável e um enumerador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="86af4-1638">Esse tipo de implementação, a primeira vez que um objeto enumerável `GetEnumerator` método é invocado, o objeto enumerável em si é retornado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="86af4-1639">As invocações subsequentes do objeto enumerável `GetEnumerator`, se houver, retornar uma cópia do objeto enumerável.</span><span class="sxs-lookup"><span data-stu-id="86af4-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="86af4-1640">Assim, cada retornada enumerador tem seu próprio estado e as alterações em um enumerador não afetará outro.</span><span class="sxs-lookup"><span data-stu-id="86af4-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="86af4-1641">O método GetEnumerator</span><span class="sxs-lookup"><span data-stu-id="86af4-1641">The GetEnumerator method</span></span>

<span data-ttu-id="86af4-1642">Um objeto enumerável fornece uma implementação do `GetEnumerator` métodos do `IEnumerable` e `IEnumerable<T>` interfaces.</span><span class="sxs-lookup"><span data-stu-id="86af4-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="86af4-1643">Os dois `GetEnumerator` métodos compartilham uma implementação comum que adquire e retorna um objeto de enumerador disponíveis.</span><span class="sxs-lookup"><span data-stu-id="86af4-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="86af4-1644">O objeto de enumerador é inicializada com os valores de argumento e a instância valor salvo quando o objeto enumerável foi inicializado, mas caso contrário, as funções de objeto de enumerador conforme descrito em [objetos do enumerador](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="86af4-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="86af4-1645">Exemplo de implementação</span><span class="sxs-lookup"><span data-stu-id="86af4-1645">Implementation example</span></span>

<span data-ttu-id="86af4-1646">Esta seção descreve uma possível implementação de iteradores em termos de construções C# padrão.</span><span class="sxs-lookup"><span data-stu-id="86af4-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="86af4-1647">A implementação descrita aqui baseia-se os mesmos princípios usados pelo compilador Microsoft C#, mas ele não é uma implementação de conformidade ou o único possível.</span><span class="sxs-lookup"><span data-stu-id="86af4-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="86af4-1648">O seguinte `Stack<T>` classe implementa seu `GetEnumerator` método usando um iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="86af4-1649">O iterador enumera os elementos da pilha na parte superior para a parte inferior.</span><span class="sxs-lookup"><span data-stu-id="86af4-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="86af4-1650">O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerado pelo compilador que encapsula o código no bloco de iterador, conforme mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="86af4-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="86af4-1651">A conversão anterior, o código no bloco iterador é transformado em uma máquina de estado e colocado no `MoveNext` método da classe de enumerador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="86af4-1652">Além disso, a variável local `i` será transformado em um campo no objeto de enumerador para que ele possa continuar existindo entre invocações de `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="86af4-1653">O exemplo a seguir imprime uma simple tabela de multiplicação de números inteiros de 1 a 10.</span><span class="sxs-lookup"><span data-stu-id="86af4-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="86af4-1654">O `FromTo` método no exemplo retorna um objeto enumerável e é implementado usando um iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="86af4-1655">O `FromTo` método pode ser convertido em uma instanciação de uma classe enumerable gerado pelo compilador que encapsula o código no bloco de iterador, conforme mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="86af4-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="86af4-1656">A classe enumerable implementa as interfaces enumeráveis e as interfaces de enumerador, habilitá-la para servir como um enumerável e um enumerador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="86af4-1657">Na primeira vez o `GetEnumerator` método é invocado, o objeto enumerável em si é retornado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="86af4-1658">As invocações subsequentes do objeto enumerável `GetEnumerator`, se houver, retornar uma cópia do objeto enumerável.</span><span class="sxs-lookup"><span data-stu-id="86af4-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="86af4-1659">Assim, cada retornada enumerador tem seu próprio estado e as alterações em um enumerador não afetará outro.</span><span class="sxs-lookup"><span data-stu-id="86af4-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="86af4-1660">O `Interlocked.CompareExchange` método é usado para garantir a operação de thread-safe.</span><span class="sxs-lookup"><span data-stu-id="86af4-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="86af4-1661">O `from` e `to` parâmetros são transformados em campos na classe enumerable.</span><span class="sxs-lookup"><span data-stu-id="86af4-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="86af4-1662">Porque `from` é modificada no bloco de iterador, adicional `__from` campo é introduzido para conter o valor inicial fornecido para `from` em cada enumerador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="86af4-1663">O `MoveNext` método lança um `InvalidOperationException` se ele é chamado quando `__state` é `0`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="86af4-1664">Isso protege contra o uso do objeto enumerável como um objeto de enumerador sem primeiro chamar `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="86af4-1665">O exemplo a seguir mostra uma classe simples de árvore.</span><span class="sxs-lookup"><span data-stu-id="86af4-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="86af4-1666">O `Tree<T>` classe implementa seu `GetEnumerator` método usando um iterador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="86af4-1667">O iterador enumera os elementos da árvore em ordem de infixo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="86af4-1668">O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerado pelo compilador que encapsula o código no bloco de iterador, conforme mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="86af4-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="86af4-1669">Os de temporários gerados pelo compilador usados na `foreach` instruções são elevadas para o `__left` e `__right` campos do objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="86af4-1670">O `__state` campo do objeto enumerador é atualizado com cuidado para que o correto `Dispose()` método será chamado corretamente se uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="86af4-1671">Observe que não é possível escrever o código traduzido com simples `foreach` instruções.</span><span class="sxs-lookup"><span data-stu-id="86af4-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="86af4-1672">Funções assíncronas</span><span class="sxs-lookup"><span data-stu-id="86af4-1672">Async functions</span></span>

<span data-ttu-id="86af4-1673">Um método ([métodos](classes.md#methods)) ou função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)) com o `async` modificador é chamado um ***função assíncrona***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="86af4-1674">Em geral, o termo ***async*** é usado para descrever qualquer tipo de função que tem o `async` modificador.</span><span class="sxs-lookup"><span data-stu-id="86af4-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="86af4-1675">É um erro de tempo de compilação para a lista de parâmetros formais de uma função assíncrona para especificar qualquer `ref` ou `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="86af4-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="86af4-1676">O *return_type* de async método deve ser `void` ou uma ***tipo de tarefa***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="86af4-1677">Os tipos de tarefa são `System.Threading.Tasks.Task` e tipos construídos de `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="86af4-1678">Por questão de brevidade, neste capítulo esses tipos são referenciados como `Task` e `Task<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="86af4-1679">Um método assíncrono retorna um tipo de tarefa deve ser o retorno de tarefa.</span><span class="sxs-lookup"><span data-stu-id="86af4-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="86af4-1680">A definição exata dos tipos de tarefa é definido pela implementação, mas do ponto de vista da linguagem um tipo de tarefa está em um dos Estados incompletos, teve êxito ou falha.</span><span class="sxs-lookup"><span data-stu-id="86af4-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="86af4-1681">Uma tarefa com falha registra uma exceção pertinente.</span><span class="sxs-lookup"><span data-stu-id="86af4-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="86af4-1682">Uma bem-sucedida `Task<T>` registra um resultado do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="86af4-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="86af4-1683">Tipos de tarefa são aguardável e, portanto, podem ser os operandos de expressões await ([expressões Await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="86af4-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="86af4-1684">Uma invocação de função async tem a capacidade de suspender a avaliação por meio de expressões await ([expressões Await](expressions.md#await-expressions)) em seu corpo.</span><span class="sxs-lookup"><span data-stu-id="86af4-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="86af4-1685">Avaliação pode ser retomada posteriormente no ponto de suspensão await expressão por meio de um ***delegado de continuação***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="86af4-1686">O delegado de continuação é do tipo `System.Action`, e quando ele é chamado, a avaliação da invocação de função async será retomada da expressão await onde parou.</span><span class="sxs-lookup"><span data-stu-id="86af4-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="86af4-1687">O ***chamador atual*** de uma função assíncrona invocação é o chamador original, se a invocação da função nunca foi suspenso ou o chamador mais recente do delegado de continuação caso contrário.</span><span class="sxs-lookup"><span data-stu-id="86af4-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="86af4-1688">Avaliação de uma função de retorno de tarefa de async</span><span class="sxs-lookup"><span data-stu-id="86af4-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="86af4-1689">Invocação de uma função de retorno de tarefa de async faz com que uma instância do tipo de tarefa retornada seja gerado.</span><span class="sxs-lookup"><span data-stu-id="86af4-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="86af4-1690">Isso é chamado de ***retornar task*** da função async.</span><span class="sxs-lookup"><span data-stu-id="86af4-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="86af4-1691">A tarefa é inicialmente em um estado incompleto.</span><span class="sxs-lookup"><span data-stu-id="86af4-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="86af4-1692">O corpo da função de async é avaliado até que ele seja suspenso (por atingir uma expressão await) ou for encerrado, no qual o ponto de controle é retornado ao chamador, junto com a tarefa de retorno.</span><span class="sxs-lookup"><span data-stu-id="86af4-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="86af4-1693">Quando o corpo da função async é encerrado, a tarefa de retornada é movida para fora do estado incompleto:</span><span class="sxs-lookup"><span data-stu-id="86af4-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="86af4-1694">Se o corpo da função for encerrado como resultado de se atingir uma instrução return ou ao final do corpo, qualquer valor de resultado é registrada no task de retorno, que é colocado em um estado de êxito.</span><span class="sxs-lookup"><span data-stu-id="86af4-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="86af4-1695">Se o corpo da função será encerrado devido a uma exceção não tratada ([a instrução throw](statements.md#the-throw-statement)) a exceção é registrada no task de retorno que é colocado em um estado de falha.</span><span class="sxs-lookup"><span data-stu-id="86af4-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="86af4-1696">Avaliação de uma função de async que retornam void</span><span class="sxs-lookup"><span data-stu-id="86af4-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="86af4-1697">Se o tipo de retorno da função async é `void`, avaliação difere acima da seguinte maneira: Porque nenhuma tarefa é retornada, a função se comunica em vez disso, conclusão e exceções para o thread atual ***contexto de sincronização***.</span><span class="sxs-lookup"><span data-stu-id="86af4-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="86af4-1698">A definição exata de contexto de sincronização é dependente da implementação, mas é uma representação de "onde" o thread atual está em execução.</span><span class="sxs-lookup"><span data-stu-id="86af4-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="86af4-1699">O contexto de sincronização é notificado quando a avaliação de uma função de async que retornam void começa, for concluída com êxito ou faz com que uma exceção não percebida seja lançada.</span><span class="sxs-lookup"><span data-stu-id="86af4-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="86af4-1700">Isso permite que o contexto para controlar a execução de funções de async que retornam void quantos sob ele e decidir como propagar exceções provenientes proveito delas.</span><span class="sxs-lookup"><span data-stu-id="86af4-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
