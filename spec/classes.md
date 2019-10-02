---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703977"
---
# <a name="classes"></a><span data-ttu-id="c8a10-101">Classes</span><span class="sxs-lookup"><span data-stu-id="c8a10-101">Classes</span></span>

<span data-ttu-id="c8a10-102">Uma classe é uma estrutura de dados que pode conter membros de dados (constantes e campos), membros de função (métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e tipos aninhados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="c8a10-103">Os tipos de classe dão suporte à herança, um mecanismo no qual uma classe derivada pode estender e especializar uma classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="c8a10-104">Declarações de classe</span><span class="sxs-lookup"><span data-stu-id="c8a10-104">Class declarations</span></span>

<span data-ttu-id="c8a10-105">Um *class_declaration* é um *type_declaration* ([declarações de tipo](namespaces.md#type-declarations)) que declara uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="c8a10-106">Um *class_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *class_modifier*s ([modificadores de classe](classes.md#class-modifiers)), seguido por um modificador opcional `partial`, seguido pela palavra-chave `class` e um *identificador* que nomeia a classe, seguido por um *type_parameter_list* opcional ([parâmetros de tipo](classes.md#type-parameters)), seguido por uma especificação de *Class_base* opcional (especificação de[base de classe](classes.md#class-base-specification)), seguida por um conjunto opcional de *type_parameter_constraints_clause*s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), seguido por um *class_body* ([corpo de classe](classes.md#class-body)), opcionalmente seguido por um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="c8a10-107">Uma declaração de classe não pode fornecer *type_parameter_constraints_clause*s, a menos que ele também forneça um *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="c8a10-108">Uma declaração de classe que fornece um *type_parameter_list* é uma ***declaração de classe genérica***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="c8a10-109">Além disso, qualquer classe aninhada dentro de uma declaração de classe genérica ou uma declaração struct genérica é, em si, uma declaração de classe genérica, já que parâmetros de tipo para o tipo recipiente devem ser fornecidos para criar um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="c8a10-110">Modificadores de classe</span><span class="sxs-lookup"><span data-stu-id="c8a10-110">Class modifiers</span></span>

<span data-ttu-id="c8a10-111">Um *class_declaration* pode, opcionalmente, incluir uma sequência de modificadores de classe:</span><span class="sxs-lookup"><span data-stu-id="c8a10-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="c8a10-112">É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="c8a10-113">O `new` modificador é permitido em classes aninhadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="c8a10-114">Ele especifica que a classe oculta um membro herdado com o mesmo nome, conforme descrito no [novo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="c8a10-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="c8a10-115">É um erro de tempo de compilação para que `new` o modificador apareça em uma declaração de classe que não seja uma declaração de classe aninhada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="c8a10-116">Os `public`modificadores `internal`, `protected`, e`private` controlam a acessibilidade da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="c8a10-117">Dependendo do contexto no qual a declaração de classe ocorre, alguns desses modificadores podem não ser permitidos ([acessibilidade declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="c8a10-118">Os `abstract`modificadores e`static`são discutidos nas seções a seguir. `sealed`</span><span class="sxs-lookup"><span data-stu-id="c8a10-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="c8a10-119">Classes abstratas</span><span class="sxs-lookup"><span data-stu-id="c8a10-119">Abstract classes</span></span>

<span data-ttu-id="c8a10-120">O `abstract` modificador é usado para indicar que uma classe está incompleta e que se destina a ser usada somente como uma classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="c8a10-121">Uma classe abstrata difere de uma classe não abstrata das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="c8a10-122">Uma classe abstrata não pode ser instanciada diretamente e é um erro de tempo de compilação para usar o `new` operador em uma classe abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="c8a10-123">Embora seja possível ter variáveis e valores cujos tipos de tempo de compilação sejam abstratos, tais variáveis e valores serão necessariamente `null` ou contêm referências a instâncias de classes não abstratas derivadas dos tipos abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="c8a10-124">Uma classe abstrata é permitida (mas não obrigatória) para conter membros abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="c8a10-125">Uma classe abstrata não pode ser selada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="c8a10-126">Quando uma classe não abstrata é derivada de uma classe abstrata, a classe não abstrata deve incluir implementações reais de todos os membros abstratos herdados, substituindo assim esses membros abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="c8a10-127">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-127">In the example</span></span>
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
<span data-ttu-id="c8a10-128">a classe `A` abstract introduz um método `F`abstract.</span><span class="sxs-lookup"><span data-stu-id="c8a10-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="c8a10-129">A `B` classe introduz um método `G`adicional, mas como não fornece uma implementação de `F`, `B` também deve ser declarada abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="c8a10-130">As `C` substituições `F` de classe e fornecem uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="c8a10-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="c8a10-131">Como não há membros abstratos no `C`, `C` o é permitido (mas não obrigatório) para ser não abstrato.</span><span class="sxs-lookup"><span data-stu-id="c8a10-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="c8a10-132">Classes lacradas</span><span class="sxs-lookup"><span data-stu-id="c8a10-132">Sealed classes</span></span>

<span data-ttu-id="c8a10-133">O `sealed` modificador é usado para impedir a derivação de uma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="c8a10-134">Um erro em tempo de compilação ocorrerá se uma classe selada for especificada como a classe base de outra classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="c8a10-135">Uma classe selada também não pode ser uma classe abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="c8a10-136">O `sealed` modificador é usado principalmente para evitar derivação não intencional, mas também permite determinadas otimizações de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="c8a10-137">Em particular, como uma classe selada é conhecida por nunca ter classes derivadas, é possível transformar invocações de membro de função virtual em instâncias de classe seladas em invocações não virtuais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="c8a10-138">Classes estáticas</span><span class="sxs-lookup"><span data-stu-id="c8a10-138">Static classes</span></span>

<span data-ttu-id="c8a10-139">O `static` modificador é usado para marcar a classe que está sendo declarada como uma ***classe estática***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="c8a10-140">Uma classe estática não pode ser instanciada, não pode ser usada como um tipo e pode conter somente membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="c8a10-141">Somente uma classe estática pode conter declarações de métodos de extensão ([métodos de extensão](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="c8a10-142">Uma declaração de classe estática está sujeita às seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="c8a10-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="c8a10-143">Uma classe estática não pode incluir um `sealed` modificador ou `abstract` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="c8a10-144">No entanto, observe que, como uma classe estática não pode ser instanciada ou derivada de, ela se comporta como se fosse selada e abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="c8a10-145">Uma classe estática pode não incluir uma especificação de *class_base* ([especificação de base de classe](classes.md#class-base-specification)) e não pode especificar explicitamente uma classe base ou uma lista de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="c8a10-146">Uma classe estática herda implicitamente do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="c8a10-147">Uma classe estática só pode conter membros estáticos ([membros estáticos e de instância](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="c8a10-148">Observe que as constantes e os tipos aninhados são classificados como membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="c8a10-149">Uma classe estática não pode ter membros `protected` ou `protected internal` a acessibilidade declarada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="c8a10-150">É um erro de tempo de compilação para violar qualquer uma dessas restrições.</span><span class="sxs-lookup"><span data-stu-id="c8a10-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="c8a10-151">Uma classe estática não tem construtores de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-151">A static class has no instance constructors.</span></span> <span data-ttu-id="c8a10-152">Não é possível declarar um construtor de instância em uma classe estática e nenhum construtor de instância padrão ([construtores padrão](classes.md#default-constructors)) é fornecido para uma classe estática.</span><span class="sxs-lookup"><span data-stu-id="c8a10-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="c8a10-153">Os membros de uma classe estática não são estáticos automaticamente e as declarações de membro devem incluir explicitamente `static` um modificador (exceto para constantes e tipos aninhados).</span><span class="sxs-lookup"><span data-stu-id="c8a10-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="c8a10-154">Quando uma classe é aninhada em uma classe externa estática, a classe aninhada não é uma classe estática, a menos `static` que inclua explicitamente um modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="c8a10-155">__Referenciando tipos de classe estática__</span><span class="sxs-lookup"><span data-stu-id="c8a10-155">__Referencing static class types__</span></span>

<span data-ttu-id="c8a10-156">Um *namespace_or_type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)) tem permissão para fazer referência a uma classe estática se</span><span class="sxs-lookup"><span data-stu-id="c8a10-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="c8a10-157">O *namespace_or_type_name* é o `T` em um *namespace_or_type_name* do formulário `T.I` ou</span><span class="sxs-lookup"><span data-stu-id="c8a10-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="c8a10-158">O *namespace_or_type_name* é o `T` em um *typeof_expression* ([lista de argumentos](expressions.md#argument-lists)1) do formato `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="c8a10-159">Um *primary_expression* ([membros da função](expressions.md#function-members)) tem permissão para fazer referência a uma classe estática se</span><span class="sxs-lookup"><span data-stu-id="c8a10-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="c8a10-160">O *primary_expression* é o `E` em um *member_access* ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) do formulário `E.I`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="c8a10-161">Em qualquer outro contexto, é um erro de tempo de compilação para fazer referência a uma classe estática.</span><span class="sxs-lookup"><span data-stu-id="c8a10-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="c8a10-162">Por exemplo, é um erro para uma classe estática ser usada como uma classe base, um tipo constituinte ([tipos aninhados](classes.md#nested-types)) de um membro, um argumento de tipo genérico ou uma restrição de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="c8a10-163">Da mesma forma, uma classe estática não pode ser usada em um tipo de matriz, um `new` tipo de ponteiro, uma expressão, `is` uma expressão de `as` conversão, uma `sizeof` expressão, uma expressão, uma expressão ou uma expressão de valor padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="c8a10-164">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="c8a10-164">Partial modifier</span></span>

<span data-ttu-id="c8a10-165">O modificador `partial` é usado para indicar que esse *class_declaration* é uma declaração de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="c8a10-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="c8a10-166">Várias declarações de tipo parcial com o mesmo nome dentro de um namespace delimitador ou declaração de tipo são combinadas para formar uma declaração de tipo, seguindo as regras especificadas em [tipos parciais](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="c8a10-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="c8a10-167">Ter a declaração de uma classe distribuída em segmentos separados de texto do programa pode ser útil se esses segmentos são produzidos ou mantidos em diferentes contextos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="c8a10-168">Por exemplo, uma parte de uma declaração de classe pode ser gerada pela máquina, enquanto a outra é criada manualmente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="c8a10-169">A separação textual dos dois impede que as atualizações de um entrem em conflito com as atualizações do outro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="c8a10-170">Parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="c8a10-170">Type parameters</span></span>

<span data-ttu-id="c8a10-171">Um parâmetro de tipo é um identificador simples que denota um espaço reservado para um argumento de tipo fornecido para criar um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="c8a10-172">Um parâmetro de tipo é um espaço reservado formal para um tipo que será fornecido posteriormente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="c8a10-173">Por outro lado, um argumento de tipo ([argumentos de tipo](types.md#type-arguments)) é o tipo real que é substituído pelo parâmetro de tipo quando um tipo construído é criado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="c8a10-174">Cada parâmetro de tipo em uma declaração de classe define um nome no espaço de declaração ([declarações](basic-concepts.md#declarations)) dessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="c8a10-175">Portanto, ele não pode ter o mesmo nome que outro parâmetro de tipo ou um membro declarado nessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="c8a10-176">Um parâmetro de tipo não pode ter o mesmo nome que o próprio tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="c8a10-177">Especificação de base de classe</span><span class="sxs-lookup"><span data-stu-id="c8a10-177">Class base specification</span></span>

<span data-ttu-id="c8a10-178">Uma declaração de classe pode incluir uma especificação *class_base* , que define a classe base direta da classe e as interfaces ([interfaces](interfaces.md)) diretamente implementadas pela classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="c8a10-179">A classe base especificada em uma declaração de classe pode ser um tipo de classe construído ([tipos construídos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="c8a10-180">Uma classe base não pode ser um parâmetro de tipo por conta própria, embora possa envolver os parâmetros de tipo que estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="c8a10-181">Classes base</span><span class="sxs-lookup"><span data-stu-id="c8a10-181">Base classes</span></span>

<span data-ttu-id="c8a10-182">Quando um *class_type* é incluído no *class_base*, ele especifica a classe base direta da classe que está sendo declarada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="c8a10-183">Se uma declaração de classe não tiver nenhum *class_base*ou se o *class_base* listar apenas os tipos de interface, a classe base direta será considerada `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="c8a10-184">Uma classe herda membros de sua classe base direta, conforme descrito em [herança](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="c8a10-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="c8a10-185">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="c8a10-186">a `A` classe é considerada como a classe base direta de `B`e `B` é considerada derivada de `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="c8a10-187">Como `A` o não especifica explicitamente uma classe base direta, sua classe base direta é `object`implicitamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="c8a10-188">Para um tipo de classe construída, se uma classe base for especificada na declaração de classe genérica, a classe base do tipo construído será obtida pela substituição, para cada *type_parameter* na declaração de classe base, o type_argument correspondentedo tipo construído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="c8a10-189">Dadas as declarações de classe genéricas</span><span class="sxs-lookup"><span data-stu-id="c8a10-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="c8a10-190">a classe base do tipo `G<int>` construído `B<string,int[]>`seria.</span><span class="sxs-lookup"><span data-stu-id="c8a10-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="c8a10-191">A classe base direta de um tipo de classe deve ser pelo menos acessível como o próprio tipo de classe ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="c8a10-192">Por exemplo, é um erro de tempo de compilação para uma `public` classe derivar de uma `private` classe `internal` ou.</span><span class="sxs-lookup"><span data-stu-id="c8a10-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="c8a10-193">A classe base direta de um tipo de classe não deve ser um dos seguintes tipos: `System.Array` `System.MulticastDelegate`, `System.Delegate` `System.Enum`,, ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="c8a10-194">Além disso, uma declaração de classe genérica `System.Attribute` não pode usar como uma classe base direta ou indireta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="c8a10-195">Ao determinar o significado da `A` especificação de classe base direta de uma classe `B`, a classe base direta de `B` é temporariamente considerada. `object`</span><span class="sxs-lookup"><span data-stu-id="c8a10-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="c8a10-196">Intuitivamente, isso garante que o significado de uma especificação de classe base não possa depender recursivamente por si só.</span><span class="sxs-lookup"><span data-stu-id="c8a10-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="c8a10-197">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="c8a10-198">está em erro porque, `A<C.B>` na especificação da classe base, a classe base direta de `C` é considerada `object`como sendo e, portanto (pelas regras de [namespace e nomes](basic-concepts.md#namespace-and-type-names)de `C` tipo) não é considerada como um membro `B`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="c8a10-199">As classes base de um tipo de classe são a classe base direta e suas classes base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="c8a10-200">Em outras palavras, o conjunto de classes base é o fechamento transitivo da relação de classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="c8a10-201">Fazendo referência ao exemplo acima, as classes base do `B` são `A` e `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="c8a10-202">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="c8a10-203">as classes base do `D<int>` são `C<int[]>`, `B<IComparable<int[]>>`, `A`, e `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="c8a10-204">Exceto para classe `object`, cada tipo de classe tem exatamente uma classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="c8a10-205">A `object` classe não tem nenhuma classe base direta e é a classe base definitiva de todas as outras classes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="c8a10-206">Quando uma classe `B` deriva de uma classe `A`, é um `B`erro de tempo de compilação para `A` o qual depender.</span><span class="sxs-lookup"><span data-stu-id="c8a10-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="c8a10-207">Uma classe ***depende diretamente*** de sua classe base direta (se houver) e ***depende diretamente*** da classe na qual ela é imediatamente aninhada (se houver).</span><span class="sxs-lookup"><span data-stu-id="c8a10-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="c8a10-208">Dada essa definição, o conjunto completo de classes sobre as quais uma classe depende é o fechamento reflexivo e transitivo da relação ***depende diretamente*** de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="c8a10-209">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="c8a10-210">é errado porque a classe depende dele mesmo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="c8a10-211">Da mesma forma, o exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="c8a10-212">é um erro porque as classes dependem circularmente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="c8a10-213">Por fim, o exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="c8a10-214">resulta em um erro de tempo de compilação `A` porque depende `B.C` de (sua classe base direta `B` ), que depende (sua `A`classe de circunscrição imediatamente), que depende circularmente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="c8a10-215">Observe que uma classe não depende das classes que estão aninhadas dentro dela.</span><span class="sxs-lookup"><span data-stu-id="c8a10-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="c8a10-216">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="c8a10-217">`B`depende de `A` (porque `A` é sua classe base direta e sua classe imediatamente delimitadora), `B` mas `A` não depende (porque `B` não é uma classe base nem uma classe de circunscrição de `A`).</span><span class="sxs-lookup"><span data-stu-id="c8a10-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="c8a10-218">Portanto, o exemplo é válido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-218">Thus, the example is valid.</span></span>

<span data-ttu-id="c8a10-219">Não é possível derivar de uma `sealed` classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="c8a10-220">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="c8a10-221">`A`a `B` classeestácomerroporquetentaderivardaclasse.`sealed`</span><span class="sxs-lookup"><span data-stu-id="c8a10-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="c8a10-222">Implementações de interfaces</span><span class="sxs-lookup"><span data-stu-id="c8a10-222">Interface implementations</span></span>

<span data-ttu-id="c8a10-223">Uma especificação *class_base* pode incluir uma lista de tipos de interface, caso em que a classe é mencionada para implementar diretamente os tipos de interface fornecidos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="c8a10-224">Implementações de interface são discutidas mais detalhadamente em [implementações de interface](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="c8a10-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="c8a10-225">Restrições de parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="c8a10-225">Type parameter constraints</span></span>

<span data-ttu-id="c8a10-226">As declarações de tipo genérico e de método podem opcionalmente especificar restrições de parâmetro de tipo, incluindo *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="c8a10-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="c8a10-227">Cada *type_parameter_constraints_clause* consiste no token `where`, seguido pelo nome de um parâmetro de tipo, seguido por dois-pontos e pela lista de restrições para esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="c8a10-228">Pode haver no máximo uma `where` cláusula para cada parâmetro de tipo e as `where` cláusulas podem ser listadas em qualquer ordem.</span><span class="sxs-lookup"><span data-stu-id="c8a10-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="c8a10-229">Como os `get` tokens e `set` em um acessador `where` de propriedade, o token não é uma palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="c8a10-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="c8a10-230">A lista de restrições fornecida em uma `where` cláusula pode incluir qualquer um dos seguintes componentes, nesta ordem: uma única restrição primária, uma ou mais restrições secundárias e a restrição de construtor, `new()`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="c8a10-231">Uma restrição PRIMARY pode ser um tipo de classe ou a ***restrição*** `class` de tipo de referência ou a ***restrição*** `struct`de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="c8a10-232">Uma restrição secundária pode ser um *type_parameter* ou um *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="c8a10-233">A restrição de tipo de referência especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="c8a10-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="c8a10-234">Todos os tipos de classe, tipos de interface, tipos delegados, tipos de matriz e parâmetros de tipo conhecidos como um tipo de referência (conforme definido abaixo) atendem a essa restrição.</span><span class="sxs-lookup"><span data-stu-id="c8a10-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="c8a10-235">A restrição de tipo de valor especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de valor não anulável.</span><span class="sxs-lookup"><span data-stu-id="c8a10-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="c8a10-236">Todos os tipos de struct não anuláveis, tipos de enumeração e parâmetros de tipo que têm a restrição de tipo de valor atendem a essa restrição.</span><span class="sxs-lookup"><span data-stu-id="c8a10-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="c8a10-237">Observe que, embora seja classificado como um tipo de valor, um tipo anulável ([tipos anuláveis](types.md#nullable-types)) não satisfaz a restrição de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="c8a10-238">Um parâmetro de tipo com a restrição de tipo de valor também não pode ter o *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="c8a10-239">Os tipos de ponteiro nunca podem ser argumentos de tipo e não são considerados para satisfazer as restrições de tipo de referência ou tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="c8a10-240">Se uma restrição for um tipo de classe, um tipo de interface ou um parâmetro de tipo, esse tipo especificará um "tipo base" mínimo que cada argumento de tipo usado para esse parâmetro de tipo deve dar suporte a.</span><span class="sxs-lookup"><span data-stu-id="c8a10-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="c8a10-241">Sempre que um tipo construído ou um método genérico é usado, o argumento de tipo é verificado em relação às restrições no parâmetro de tipo em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="c8a10-242">O argumento de tipo fornecido deve atender às condições descritas em [satisfazer restrições](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="c8a10-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="c8a10-243">Uma restrição *class_type* deve atender às seguintes regras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="c8a10-244">O tipo deve ser um tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-244">The type must be a class type.</span></span>
*  <span data-ttu-id="c8a10-245">O tipo não deve ser `sealed`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="c8a10-246">O tipo não deve ser um dos seguintes `System.Array`tipos: `System.Enum`, `System.Delegate`, ou `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="c8a10-247">O tipo não deve ser `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-247">The type must not be `object`.</span></span> <span data-ttu-id="c8a10-248">Como todos os tipos derivam de `object`, essa restrição não teria nenhum efeito se fosse permitida.</span><span class="sxs-lookup"><span data-stu-id="c8a10-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="c8a10-249">No máximo uma restrição para um determinado parâmetro de tipo pode ser um tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="c8a10-250">Um tipo especificado como uma restrição *interface_type* deve atender às seguintes regras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="c8a10-251">O tipo deve ser um tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="c8a10-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="c8a10-252">Um tipo não deve ser especificado mais de uma vez em uma `where` determinada cláusula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="c8a10-253">Em ambos os casos, a restrição pode envolver qualquer um dos parâmetros de tipo do tipo associado ou declaração de método como parte de um tipo construído e pode envolver o tipo que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="c8a10-254">Qualquer classe ou tipo de interface especificado como uma restrição de parâmetro de tipo deve ser pelo menos como acessível ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)) como o tipo genérico ou o método que está sendo declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="c8a10-255">Um tipo especificado como uma restrição *type_parameter* deve atender às seguintes regras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="c8a10-256">O tipo deve ser um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="c8a10-257">Um tipo não deve ser especificado mais de uma vez em uma `where` determinada cláusula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="c8a10-258">Além disso, não deve haver ciclos no grafo de dependência dos parâmetros de tipo, em que a dependência é uma relação transitiva definida por:</span><span class="sxs-lookup"><span data-stu-id="c8a10-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="c8a10-259">Se um `T` parâmetro de tipo for usado como uma restrição para o `S` parâmetro `S` de tipo, ***dependerá de*** `T`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="c8a10-260">Se um parâmetro `S` de tipo depender de um parâmetro `T` de `T` tipo e depender de um `U` parâmetro `S` de tipo, ***dependerá de*** `U`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="c8a10-261">Dada essa relação, é um erro de tempo de compilação para um parâmetro de tipo depender de si mesmo (direta ou indiretamente).</span><span class="sxs-lookup"><span data-stu-id="c8a10-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="c8a10-262">Todas as restrições devem ser consistentes entre os parâmetros de tipo dependentes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="c8a10-263">Se o parâmetro `S` de tipo depender do `T` parâmetro de tipo, então:</span><span class="sxs-lookup"><span data-stu-id="c8a10-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="c8a10-264">`T`Não deve ter a restrição de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="c8a10-265">Caso contrário `T` , é efetivamente lacrado, portanto `S` `T`, seria forçado a ser o mesmo tipo de, eliminando a necessidade de dois parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="c8a10-266">Se `S` tiver a restrição de tipo de valor, `T` não deverá ter uma restrição *class_type* .</span><span class="sxs-lookup"><span data-stu-id="c8a10-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="c8a10-267">Se `S` tiver uma restrição *class_type* `A` e `T` tiver uma restrição *class_type* `B`, deverá haver uma conversão de identidade ou conversão de referência implícita de `A` para `B` ou uma conversão de referência implícita de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="c8a10-268">Se `S` também depender do parâmetro de tipo `U` e `U` tiver uma restrição *class_type* `A` e `T` tiver uma restrição *class_type* `B`, deverá haver uma conversão de identidade ou conversão implícita de referência de `A` para `B` ou uma conversão de referência implícita de 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="c8a10-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="c8a10-269">É válido para `S` que o tenha a restrição de tipo de `T` valor e tenha a restrição de tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="c8a10-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="c8a10-270">Efetivamente, isso `T` limita os tipos `System.Object`, `System.ValueType`, `System.Enum`e qualquer tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="c8a10-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="c8a10-271">Se a `where` cláusula de um parâmetro de tipo incluir uma restrição de Construtor (que tem `new()`o formulário), será possível usar o `new` operador para criar instâncias do tipo ([expressões de criação de objeto](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="c8a10-272">Qualquer argumento de tipo usado para um parâmetro de tipo com uma restrição de construtor deve ter um construtor público sem parâmetros (esse construtor existe implicitamente para qualquer tipo de valor) ou ser um parâmetro de tipo que tenha a restrição de tipo de valor ou restrição de Construtor (consulte [Restrições de parâmetro de tipo](classes.md#type-parameter-constraints) para obter detalhes).</span><span class="sxs-lookup"><span data-stu-id="c8a10-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="c8a10-273">Veja a seguir exemplos de restrições:</span><span class="sxs-lookup"><span data-stu-id="c8a10-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="c8a10-274">O exemplo a seguir está em erro porque causa uma circularidade no grafo de dependência dos parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="c8a10-275">Os exemplos a seguir ilustram situações inválidas adicionais:</span><span class="sxs-lookup"><span data-stu-id="c8a10-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="c8a10-276">A ***classe base efetiva*** de um parâmetro `T` de tipo é definida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c8a10-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="c8a10-277">Se `T` o não tiver restrições primárias ou restrições de parâmetro de tipo, sua `object`classe base efetiva será.</span><span class="sxs-lookup"><span data-stu-id="c8a10-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="c8a10-278">Se `T` tiver a restrição de tipo de valor, sua classe base `System.ValueType`efetiva será.</span><span class="sxs-lookup"><span data-stu-id="c8a10-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="c8a10-279">Se `T` tiver uma restrição *class_type* `C`, mas nenhuma restrição de *type_parameter* , sua classe base efetiva será `C`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="c8a10-280">Se `T` não tiver nenhuma restrição *class_type* , mas tiver uma ou mais restrições *type_parameter* , sua classe base efetiva será o tipo mais abrangedo ([operadores de conversão levantados](conversions.md#lifted-conversion-operators)) no conjunto de classes base efetivas de seu *type_* restrições de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="c8a10-281">As regras de consistência asseguram que exista um tipo mais abrangente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="c8a10-282">Se `T` tiver uma restrição *class_type* e uma ou mais restrições *type_parameter* , sua classe base efetiva será o tipo mais abrangedo (operadores de[conversão levantados](conversions.md#lifted-conversion-operators)) no conjunto que consiste em *class_type* restrição de `T` e as classes base efetivas de suas restrições *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="c8a10-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="c8a10-283">As regras de consistência asseguram que exista um tipo mais abrangente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="c8a10-284">Se `T` tiver a restrição de tipo de referência, mas não houver restrições *class_type* , sua classe base efetiva será `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="c8a10-285">Para fins dessas regras, se T tiver uma restrição `V` que seja um *value_type*, use em vez disso o tipo base mais específico de `V` que seja um *class_type*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="c8a10-286">Isso nunca pode ocorrer em uma restrição explicitamente determinada, mas pode ocorrer quando as restrições de um método genérico são implicitamente herdadas por uma declaração de método de substituição ou uma implementação explícita de um método de interface.</span><span class="sxs-lookup"><span data-stu-id="c8a10-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="c8a10-287">Essas regras garantem que a classe base efetiva seja sempre uma *class_type*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="c8a10-288">O ***conjunto de interfaces efetivas*** de um `T` parâmetro de tipo é definido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c8a10-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="c8a10-289">Se `T` não tiver nenhum *secondary_constraints*, seu conjunto de interface eficaz estará vazio.</span><span class="sxs-lookup"><span data-stu-id="c8a10-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="c8a10-290">Se `T` tiver restrições *interface_type* , mas nenhuma restrição de *type_parameter* , seu conjunto de interface eficaz será seu conjunto de restrições *interface_type* .</span><span class="sxs-lookup"><span data-stu-id="c8a10-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="c8a10-291">Se `T` não tiver restrições *interface_type* , mas tiver restrições *type_parameter* , seu conjunto de interface eficaz será a União dos conjuntos de interface efetivos de suas restrições *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="c8a10-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="c8a10-292">Se `T` tiver restrições *interface_type* e restrições *type_parameter* , seu conjunto de interface eficaz será a União de seu conjunto de restrições *interface_type* e os conjuntos de interface efetivos de seu *type_parameter* reflexiva.</span><span class="sxs-lookup"><span data-stu-id="c8a10-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="c8a10-293">Um parâmetro de tipo é ***conhecido como um tipo de referência*** se ele tiver a restrição de tipo de referência ou se sua classe `object` base `System.ValueType`efetiva não for ou.</span><span class="sxs-lookup"><span data-stu-id="c8a10-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="c8a10-294">Os valores de um tipo de parâmetro de tipo restrito podem ser usados para acessar os membros de instância implícitos pelas restrições.</span><span class="sxs-lookup"><span data-stu-id="c8a10-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="c8a10-295">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-295">In the example</span></span>
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
<span data-ttu-id="c8a10-296">os métodos de `IPrintable` podem ser invocados diretamente `x` no `T` porque o é restrito a sempre implementar `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="c8a10-297">Corpo da classe</span><span class="sxs-lookup"><span data-stu-id="c8a10-297">Class body</span></span>

<span data-ttu-id="c8a10-298">O *class_body* de uma classe define os membros dessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="c8a10-299">Tipos parciais</span><span class="sxs-lookup"><span data-stu-id="c8a10-299">Partial types</span></span>

<span data-ttu-id="c8a10-300">Uma declaração de tipo pode ser dividida em várias ***declarações de tipo parciais***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="c8a10-301">A declaração de tipo é construída de suas partes seguindo as regras nesta seção, momento ela é tratada como uma única declaração durante o restante do processamento de tempo de compilação e tempo de execução do programa.</span><span class="sxs-lookup"><span data-stu-id="c8a10-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="c8a10-302">Um *class_declaration*, *struct_declaration* ou *interface_declaration* representa uma declaração de tipo parcial se incluir um modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="c8a10-303">`partial`Não é uma palavra-chave e atua apenas como um modificador se aparecer imediatamente antes de uma das palavras- `class`chave `struct` , `interface` ou em uma declaração de tipo ou antes do `void` tipo em uma declaração de método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="c8a10-304">Em outros contextos, ele pode ser usado como um identificador normal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="c8a10-305">Cada parte de uma declaração de tipo parcial deve incluir `partial` um modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="c8a10-306">Ele deve ter o mesmo nome e ser declarado no mesmo namespace ou em uma declaração de tipo que as outras partes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="c8a10-307">O `partial` modificador indica que partes adicionais da declaração de tipo podem existir em outro lugar, mas a existência dessas partes adicionais não é um requisito; ela é válida para um tipo com uma única declaração para `partial` incluir o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="c8a10-308">Todas as partes de um tipo parcial devem ser compiladas em conjunto, de modo que as partes possam ser mescladas em tempo de compilação em uma única declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="c8a10-309">Tipos parciais especificamente não permitem que tipos já compilados sejam estendidos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="c8a10-310">Tipos aninhados podem ser declarados em várias partes `partial` usando o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="c8a10-311">Normalmente, o tipo recipiente é declarado usando `partial` também, e cada parte do tipo aninhado é declarada em uma parte diferente do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="c8a10-312">O `partial` modificador não é permitido em declarações delegate ou enum.</span><span class="sxs-lookup"><span data-stu-id="c8a10-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="c8a10-313">Atributos</span><span class="sxs-lookup"><span data-stu-id="c8a10-313">Attributes</span></span>

<span data-ttu-id="c8a10-314">Os atributos de um tipo parcial são determinados pela combinação, em uma ordem não especificada, os atributos de cada uma das partes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="c8a10-315">Se um atributo for colocado em várias partes, será equivalente a especificar o atributo várias vezes no tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="c8a10-316">Por exemplo, as duas partes:</span><span class="sxs-lookup"><span data-stu-id="c8a10-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="c8a10-317">são equivalentes a uma declaração como:</span><span class="sxs-lookup"><span data-stu-id="c8a10-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="c8a10-318">Os atributos nos parâmetros de tipo são combinados de maneira semelhante.</span><span class="sxs-lookup"><span data-stu-id="c8a10-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="c8a10-319">Modificadores</span><span class="sxs-lookup"><span data-stu-id="c8a10-319">Modifiers</span></span>

<span data-ttu-id="c8a10-320">Quando uma declaração de tipo parcial inclui uma especificação de acessibilidade `public`( `protected`os `internal`modificadores `private` ,, e), ela deve concordar com todas as outras partes que incluem uma especificação de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="c8a10-321">Se nenhuma parte de um tipo parcial incluir uma especificação de acessibilidade, o tipo receberá a acessibilidade padrão apropriada (a[acessibilidade declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="c8a10-322">Se uma ou mais declarações parciais de um tipo aninhado `new` incluírem um modificador, nenhum aviso será relatado se o tipo aninhado ocultar um membro herdado ([ocultando a herança](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="c8a10-323">Se uma ou mais declarações parciais de uma classe incluírem um `abstract` modificador, a classe será considerada abstrata ([classes abstratas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="c8a10-324">Caso contrário, a classe será considerada não abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="c8a10-325">Se uma ou mais declarações parciais de uma classe incluírem um `sealed` modificador, a classe será considerada selada ([classes seladas](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="c8a10-326">Caso contrário, a classe será considerada sem lacre.</span><span class="sxs-lookup"><span data-stu-id="c8a10-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="c8a10-327">Observe que uma classe não pode ser abstrata e selada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="c8a10-328">Quando o `unsafe` modificador é usado em uma declaração de tipo parcial, somente essa parte específica é considerada um contexto não seguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="c8a10-329">Parâmetros de tipo e restrições</span><span class="sxs-lookup"><span data-stu-id="c8a10-329">Type parameters and constraints</span></span>

<span data-ttu-id="c8a10-330">Se um tipo genérico for declarado em várias partes, cada parte deverá declarar os parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="c8a10-331">Cada parte deve ter o mesmo número de parâmetros de tipo e o mesmo nome para cada parâmetro de tipo, em ordem.</span><span class="sxs-lookup"><span data-stu-id="c8a10-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="c8a10-332">Quando uma declaração de tipo genérico parcial inclui restrições`where` (cláusulas), as restrições devem concordar com todas as outras partes que incluem restrições.</span><span class="sxs-lookup"><span data-stu-id="c8a10-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="c8a10-333">Especificamente, cada parte que inclui restrições deve ter restrições para o mesmo conjunto de parâmetros de tipo e, para cada parâmetro de tipo, os conjuntos de restrições primária, secundária e de construtor devem ser equivalentes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="c8a10-334">Dois conjuntos de restrições são equivalentes se contiverem os mesmos membros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="c8a10-335">Se nenhuma parte de um tipo genérico parcial especificar restrições de parâmetro de tipo, os parâmetros de tipo serão considerados irrestrito.</span><span class="sxs-lookup"><span data-stu-id="c8a10-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="c8a10-336">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-336">The example</span></span>
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
<span data-ttu-id="c8a10-337">está correto porque essas partes que incluem restrições (as duas primeiras) efetivamente especificam o mesmo conjunto de restrições primárias, secundárias e de construtor para o mesmo conjunto de parâmetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="c8a10-338">Classe base</span><span class="sxs-lookup"><span data-stu-id="c8a10-338">Base class</span></span>

<span data-ttu-id="c8a10-339">Quando uma declaração de classe parcial inclui uma especificação de classe base, ela deve concordar com todas as outras partes que incluem uma especificação de classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="c8a10-340">Se nenhuma parte de uma classe parcial incluir uma especificação de classe base, a classe base `System.Object` se tornará ([classes base](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="c8a10-341">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="c8a10-341">Base interfaces</span></span>

<span data-ttu-id="c8a10-342">O conjunto de interfaces base para um tipo declarado em várias partes é a União das interfaces base especificadas em cada parte.</span><span class="sxs-lookup"><span data-stu-id="c8a10-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="c8a10-343">Uma interface base específica só pode ser nomeada uma vez em cada parte, mas é permitida para várias partes nomear as mesmas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="c8a10-344">Deve haver apenas uma implementação dos membros de qualquer interface base fornecida.</span><span class="sxs-lookup"><span data-stu-id="c8a10-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="c8a10-345">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="c8a10-346">o conjunto de interfaces base para a `C` classe `IA`é `IB`, e `IC`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="c8a10-347">Normalmente, cada parte fornece uma implementação das interfaces declaradas nessa parte; no entanto, isso não é um requisito.</span><span class="sxs-lookup"><span data-stu-id="c8a10-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="c8a10-348">Uma parte pode fornecer a implementação para uma interface declarada em uma parte diferente:</span><span class="sxs-lookup"><span data-stu-id="c8a10-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="c8a10-349">Membros</span><span class="sxs-lookup"><span data-stu-id="c8a10-349">Members</span></span>

<span data-ttu-id="c8a10-350">Com exceção dos métodos parciais ([métodos parciais](classes.md#partial-methods)), o conjunto de membros de um tipo declarado em várias partes é simplesmente a União do conjunto de membros declarado em cada parte.</span><span class="sxs-lookup"><span data-stu-id="c8a10-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="c8a10-351">Os corpos de todas as partes da declaração de tipo compartilham o mesmo espaço de declaração ([declarações](basic-concepts.md#declarations)), e o escopo de cada membro ([escopos](basic-concepts.md#scopes)) se estende aos corpos de todas as partes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="c8a10-352">O domínio de acessibilidade de qualquer membro sempre inclui todas as partes do tipo delimitador; um `private` membro declarado em uma parte pode ser acessado livremente de outra parte.</span><span class="sxs-lookup"><span data-stu-id="c8a10-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="c8a10-353">É um erro de tempo de compilação para declarar o mesmo membro em mais de uma parte do tipo, a menos que esse membro seja um tipo com `partial` o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="c8a10-354">A ordenação de membros dentro de um tipo é raramente C# significativa para o código, mas pode ser significativa ao fazer a interface com outras linguagens e ambientes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="c8a10-355">Nesses casos, a ordenação de membros dentro de um tipo declarado em várias partes é indefinida.</span><span class="sxs-lookup"><span data-stu-id="c8a10-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="c8a10-356">Métodos parciais</span><span class="sxs-lookup"><span data-stu-id="c8a10-356">Partial methods</span></span>

<span data-ttu-id="c8a10-357">Os métodos parciais podem ser definidos em uma parte de uma declaração de tipo e implementados em outro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="c8a10-358">A implementação é opcional; Se nenhuma parte implementar o método parcial, a declaração de método parcial e todas as chamadas para ele serão removidas da declaração de tipo resultante da combinação das partes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="c8a10-359">Os métodos parciais não podem definir modificadores de acesso, `private`mas são implicitamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="c8a10-360">O tipo de retorno deve `void`ser, e seus parâmetros não podem `out` ter o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="c8a10-361">O identificador `partial` será reconhecido como uma palavra-chave especial em uma declaração de método somente se aparecer imediatamente `void` antes do tipo; caso contrário, ele poderá ser usado como um identificador normal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="c8a10-362">Um método parcial não pode implementar explicitamente métodos de interface.</span><span class="sxs-lookup"><span data-stu-id="c8a10-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="c8a10-363">Há dois tipos de declarações de método parciais: Se o corpo da declaração do método for um ponto e vírgula, a declaração será considerada uma ***declaração de método parcial de definição***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="c8a10-364">Se o corpo for fornecido como um *bloco*, a declaração será considerada uma declaração de ***método parcial de implementação***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="c8a10-365">Em todas as partes de uma declaração de tipo, pode haver apenas uma definição de declaração de método parcial com uma determinada assinatura, e pode haver apenas uma implementação de declaração de método parcial com uma determinada assinatura.</span><span class="sxs-lookup"><span data-stu-id="c8a10-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="c8a10-366">Se uma declaração de método parcial de implementação for fornecida, uma declaração de método parcial de definição correspondente deverá existir e as declarações deverão corresponder conforme especificado no seguinte:</span><span class="sxs-lookup"><span data-stu-id="c8a10-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="c8a10-367">As declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem), nome do método, número de parâmetros de tipo e número de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="c8a10-368">Os parâmetros correspondentes nas declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem) e os mesmos tipos (diferenças de módulo em nomes de parâmetro de tipo).</span><span class="sxs-lookup"><span data-stu-id="c8a10-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="c8a10-369">Os parâmetros de tipo correspondentes nas declarações devem ter as mesmas restrições (diferenças de módulo em nomes de parâmetro de tipo).</span><span class="sxs-lookup"><span data-stu-id="c8a10-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="c8a10-370">Uma declaração de método parcial de implementação pode aparecer na mesma parte da declaração de método parcial de definição correspondente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="c8a10-371">Apenas um método parcial de definição participa da resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="c8a10-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="c8a10-372">Portanto, se uma declaração de implementação for ou não determinada, as expressões de invocação podem ser resolvidas para invocações do método parcial.</span><span class="sxs-lookup"><span data-stu-id="c8a10-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="c8a10-373">Como um método parcial sempre retorna `void`, essas expressões de invocação sempre serão instruções de expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="c8a10-374">Além disso, como um método parcial é implicitamente `private`, essas instruções sempre ocorrerão dentro de uma das partes da declaração de tipo dentro da qual o método parcial é declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="c8a10-375">Se nenhuma parte de uma declaração de tipo parcial contiver uma declaração de implementação para um determinado método parcial, qualquer instrução de expressão invocando-a será simplesmente removida da declaração de tipo combinado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="c8a10-376">Portanto, a expressão de invocação, incluindo quaisquer expressões constituintes, não tem nenhum efeito no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="c8a10-377">O próprio método parcial também é removido e não será um membro da declaração de tipo combinado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="c8a10-378">Se existir uma declaração de implementação para um determinado método parcial, as invocações dos métodos parciais serão mantidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="c8a10-379">O método parcial fornece a elevação para uma declaração de método semelhante à declaração de método parcial de implementação, exceto para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c8a10-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="c8a10-380">O `partial` modificador não está incluído</span><span class="sxs-lookup"><span data-stu-id="c8a10-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="c8a10-381">Os atributos na declaração de método resultante são os atributos combinados da definição e a declaração de método parcial de implementação em ordem não especificada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="c8a10-382">Duplicatas não são removidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="c8a10-383">Os atributos nos parâmetros da declaração de método resultante são os atributos combinados dos parâmetros correspondentes da definição e a declaração de método parcial de implementação em ordem não especificada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="c8a10-384">Duplicatas não são removidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-384">Duplicates are not removed.</span></span>

<span data-ttu-id="c8a10-385">Se uma declaração de definição, mas não uma declaração de implementação, for fornecida para um método parcial M, as seguintes restrições serão aplicáveis:</span><span class="sxs-lookup"><span data-stu-id="c8a10-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="c8a10-386">É um erro de tempo de compilação para criar um delegate para o método ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="c8a10-387">É um erro de tempo de compilação para fazer referência `M` a dentro de uma função anônima que é convertida em um tipo[de árvore de expressão (avaliação de conversões de função anônima para tipos de árvore de expressão](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="c8a10-388">As expressões que ocorrem como parte de uma invocação `M` do não afetam o estado de atribuição definido ([atribuição definitiva](variables.md#definite-assignment)), o que pode levar a erros em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="c8a10-389">`M`Não pode ser o ponto de entrada para um aplicativo ([inicialização do aplicativo](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="c8a10-390">Os métodos parciais são úteis para permitir que uma parte de uma declaração de tipo Personalize o comportamento de outra parte, por exemplo, uma que seja gerada por uma ferramenta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="c8a10-391">Considere a seguinte declaração de classe parcial:</span><span class="sxs-lookup"><span data-stu-id="c8a10-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="c8a10-392">Se essa classe for compilada sem nenhuma outra parte, a definição de declarações de método parcial e suas invocações será removida e a declaração de classe combinada resultante será equivalente à seguinte:</span><span class="sxs-lookup"><span data-stu-id="c8a10-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="c8a10-393">Suponha que outra parte seja fornecida, no entanto, que fornece declarações de implementação dos métodos parciais:</span><span class="sxs-lookup"><span data-stu-id="c8a10-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="c8a10-394">Em seguida, a declaração de classe combinada resultante será equivalente à seguinte:</span><span class="sxs-lookup"><span data-stu-id="c8a10-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="c8a10-395">Associação de nome</span><span class="sxs-lookup"><span data-stu-id="c8a10-395">Name binding</span></span>

<span data-ttu-id="c8a10-396">Embora cada parte de um tipo extensível deva ser declarada dentro do mesmo namespace, as partes normalmente são escritas em diferentes declarações de namespace.</span><span class="sxs-lookup"><span data-stu-id="c8a10-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="c8a10-397">Portanto, diretivas `using` diferentes ([usando diretivas](namespaces.md#using-directives)) podem estar presentes para cada parte.</span><span class="sxs-lookup"><span data-stu-id="c8a10-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="c8a10-398">Ao interpretar nomes simples ([inferência de tipos](expressions.md#type-inference)) dentro de uma parte, `using` somente as diretivas das declarações de namespace que envolvem essa parte são consideradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="c8a10-399">Isso pode resultar no mesmo identificador com significados diferentes em diferentes partes:</span><span class="sxs-lookup"><span data-stu-id="c8a10-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="c8a10-400">Membros de classe</span><span class="sxs-lookup"><span data-stu-id="c8a10-400">Class members</span></span>

<span data-ttu-id="c8a10-401">Os membros de uma classe consistem nos membros introduzidos por seus *class_member_declaration*s e nos membros herdados da classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="c8a10-402">Os membros de um tipo de classe são divididos nas seguintes categorias:</span><span class="sxs-lookup"><span data-stu-id="c8a10-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="c8a10-403">Constantes, que representam valores constantes associados à classe ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="c8a10-404">Campos, que são as variáveis da classe ([campos](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="c8a10-405">Métodos, que implementam os cálculos e as ações que podem ser executadas pela classe ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="c8a10-406">Propriedades, que definem características nomeadas e as ações associadas à leitura e à gravação dessas características ([Propriedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="c8a10-407">Eventos, que definem as notificações que podem ser geradas pela classe ([eventos](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="c8a10-408">Indexadores, que permitem que as instâncias da classe sejam indexadas da mesma forma (sintaticamente) como matrizes ([indexadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="c8a10-409">Operadores, que definem os operadores de expressão que podem ser aplicados a instâncias da classe ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="c8a10-410">Construtores de instância, que implementam as ações necessárias para inicializar instâncias da classe ([construtores de instância](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="c8a10-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="c8a10-411">Destruidores, que implementam as ações a serem executadas antes que as instâncias da classe sejam descartadas permanentemente ([destruidores](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="c8a10-412">Construtores estáticos, que implementam as ações necessárias para inicializar a própria classe ([construtores estáticos](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="c8a10-413">Tipos, que representam os tipos que são locais para a classe ([tipos aninhados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="c8a10-414">Membros que podem conter código executável são coletivamente conhecidos como *membros da função* do tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="c8a10-415">Os membros da função de um tipo de classe são os métodos, as propriedades, os eventos, os indexadores, os operadores, os construtores de instância, os destruidores e os construtores estáticos desse tipo de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="c8a10-416">Um *class_declaration* cria um novo espaço de declaração ([declarações](basic-concepts.md#declarations)) e os *class_member_declaration*s imediatamente contidos pelo *class_declaration* introduzem novos membros nesse espaço de declaração.</span><span class="sxs-lookup"><span data-stu-id="c8a10-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="c8a10-417">As regras a seguir se aplicam a *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="c8a10-418">Construtores de instância, destruidores e construtores estáticos devem ter o mesmo nome que a classe de circunscrição imediata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="c8a10-419">Todos os outros membros devem ter nomes que diferem do nome da classe imediatamente delimitadora.</span><span class="sxs-lookup"><span data-stu-id="c8a10-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="c8a10-420">O nome de uma constante, campo, propriedade, evento ou tipo deve ser diferente dos nomes de todos os outros membros declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="c8a10-421">O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="c8a10-422">Além disso, a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe podem não ter assinaturas que diferem exclusivamente por `ref` e `out`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="c8a10-423">A assinatura de um construtor de instância deve ser diferente das assinaturas de todos os outros construtores de instância declarados na mesma classe, e dois construtores declarados na mesma classe podem não ter assinaturas que `ref` diferem exclusivamente pelo and. `out`</span><span class="sxs-lookup"><span data-stu-id="c8a10-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="c8a10-424">A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="c8a10-425">A assinatura de um operador deve ser diferente das assinaturas de todos os outros operadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="c8a10-426">Os membros herdados de um tipo de classe ([herança](classes.md#inheritance)) não fazem parte do espaço de declaração de uma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="c8a10-427">Assim, uma classe derivada tem permissão para declarar um membro com o mesmo nome ou assinatura que um membro herdado (que em vigor oculta o membro herdado).</span><span class="sxs-lookup"><span data-stu-id="c8a10-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="c8a10-428">O tipo de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-428">The instance type</span></span>

<span data-ttu-id="c8a10-429">Cada declaração de classe tem um tipo de associado associado ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)), o ***tipo de instância***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="c8a10-430">Para uma declaração de classe genérica, o tipo de instância é formado criando um tipo construído ([tipos construídos](types.md#constructed-types)) da declaração de tipo, com cada um dos argumentos de tipo fornecidos sendo o parâmetro de tipo correspondente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="c8a10-431">Como o tipo de instância usa os parâmetros de tipo, ele só pode ser usado quando os parâmetros de tipo estão no escopo; ou seja, dentro da declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="c8a10-432">O tipo de instância é o tipo `this` de para código escrito dentro da declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="c8a10-433">Para classes não genéricas, o tipo de instância é simplesmente a classe declarada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="c8a10-434">O exemplo a seguir mostra várias declarações de classe junto com seus tipos de instância:</span><span class="sxs-lookup"><span data-stu-id="c8a10-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="c8a10-435">Membros de tipos construídos</span><span class="sxs-lookup"><span data-stu-id="c8a10-435">Members of constructed types</span></span>

<span data-ttu-id="c8a10-436">Os membros não herdados de um tipo construído são obtidos pela substituição, para cada *type_parameter* na declaração de membro, o *type_argument* correspondente do tipo construído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="c8a10-437">O processo de substituição é baseado no significado semântico das declarações de tipo e não é simplesmente a substituição textual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="c8a10-438">Por exemplo, considerando a declaração de classe genérica</span><span class="sxs-lookup"><span data-stu-id="c8a10-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="c8a10-439">o tipo `Gen<int[],IComparable<string>>` construído tem os seguintes membros:</span><span class="sxs-lookup"><span data-stu-id="c8a10-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="c8a10-440">O `a` tipo do membro na Declaração `Gen` de classe genérica é "matriz bidimensional de `T`", portanto, o tipo do membro `a` no tipo construído acima é "matriz bidimensional de uma matriz unidimensional de `int`", ou `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="c8a10-441">Em membros da função de instância, o `this` tipo de é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da declaração que a contém.</span><span class="sxs-lookup"><span data-stu-id="c8a10-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="c8a10-442">Todos os membros de uma classe genérica podem usar parâmetros de tipo de qualquer classe delimitadora, seja diretamente ou como parte de um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="c8a10-443">Quando um tipo construído específico fechado ([tipos abertos e fechados](types.md#open-and-closed-types)) é usado em tempo de execução, cada uso de um parâmetro de tipo é substituído pelo argumento de tipo real fornecido para o tipo construído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="c8a10-444">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="c8a10-445">Herança</span><span class="sxs-lookup"><span data-stu-id="c8a10-445">Inheritance</span></span>

<span data-ttu-id="c8a10-446">Uma classe ***herda*** os membros de seu tipo de classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="c8a10-447">Herança significa que uma classe implicitamente contém todos os membros de seu tipo de classe base direta, exceto para construtores de instância, destruidores e construtores estáticos da classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="c8a10-448">Alguns aspectos importantes da herança são:</span><span class="sxs-lookup"><span data-stu-id="c8a10-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="c8a10-449">A herança é transitiva.</span><span class="sxs-lookup"><span data-stu-id="c8a10-449">Inheritance is transitive.</span></span> <span data-ttu-id="c8a10-450">Se `C` é derivado de `B`, e `B` é derivado de `A`, `C` herda os membros declarados `B` em, bem como os membros declarados em. `A`</span><span class="sxs-lookup"><span data-stu-id="c8a10-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="c8a10-451">Uma classe derivada estende sua classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="c8a10-452">Uma classe derivada pode adicionar novos membros aos que ela herda, mas ela não pode remover a definição de um membro herdado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="c8a10-453">Construtores de instância, destruidores e construtores estáticos não são herdados, mas todos os outros membros são, independentemente de sua acessibilidade declarada ([acesso de membro](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="c8a10-454">No entanto, dependendo da acessibilidade declarada, os membros herdados podem não estar acessíveis em uma classe derivada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="c8a10-455">Uma classe derivada pode ***ocultar*** membros herdados ([ocultando a herança](basic-concepts.md#hiding-through-inheritance)) declarando novos membros com o mesmo nome ou assinatura.</span><span class="sxs-lookup"><span data-stu-id="c8a10-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="c8a10-456">No entanto, observe que ocultar um membro herdado não remove esse membro — ele simplesmente torna esse membro inacessível diretamente por meio da classe derivada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="c8a10-457">Uma instância de uma classe contém um conjunto de todos os campos de instância declarados na classe e suas classes base, e uma conversão implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe de um tipo de classe derivada para qualquer um de seus tipos de classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="c8a10-458">Assim, uma referência a uma instância de uma classe derivada pode ser tratada como uma referência a uma instância de qualquer uma de suas classes base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="c8a10-459">Uma classe pode declarar métodos, propriedades e indexadores virtuais, e classes derivadas podem substituir a implementação desses membros da função.</span><span class="sxs-lookup"><span data-stu-id="c8a10-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="c8a10-460">Isso permite que as classes exibam o comportamento polimórfico em que as ações executadas por uma invocação de membro de função variam dependendo do tipo de tempo de execução da instância por meio da qual esse membro de função é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="c8a10-461">O membro herdado de um tipo de classe construído são os membros do tipo de classe base imediato ([classes base](classes.md#base-classes)), que é encontrado substituindo os argumentos de tipo do tipo construído para cada ocorrência dos parâmetros de tipo correspondentes noespecificação de class_base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="c8a10-462">Esses membros, por sua vez, são transformados substituindo, para cada *type_parameter* na declaração de membro, o *type_argument* correspondente da especificação *class_base* .</span><span class="sxs-lookup"><span data-stu-id="c8a10-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="c8a10-463">No exemplo acima, o `D<int>` tipo construído tem um membro `public int G(string s)` não herdado obtido pela substituição do argumento `int` de tipo para o parâmetro `T`de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="c8a10-464">`D<int>`também tem um membro herdado da Declaração `B`de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="c8a10-465">Esse membro herdado é determinado pela primeira vez que determina o `B<int[]>` tipo `D<int>` de classe base `int` de `T` substituindo para na especificação `B<T[]>`de classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="c8a10-466">Em seguida, como um argumento de `B`tipo `int[]` para, é substituído `U` por `public U F(long index)`no, produzindo o membro `public int[] F(long index)`herdado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="c8a10-467">O novo modificador</span><span class="sxs-lookup"><span data-stu-id="c8a10-467">The new modifier</span></span>

<span data-ttu-id="c8a10-468">Um *class_member_declaration* tem permissão para declarar um membro com o mesmo nome ou assinatura de um membro herdado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="c8a10-469">Quando isso ocorre, o membro da classe derivada é dito para ***ocultar*** o membro da classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="c8a10-470">Ocultar um membro herdado não é considerado um erro, mas faz com que o compilador emita um aviso.</span><span class="sxs-lookup"><span data-stu-id="c8a10-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="c8a10-471">Para suprimir o aviso, a declaração do membro da classe derivada pode incluir `new` um modificador para indicar que o membro derivado deve ocultar o membro base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="c8a10-472">Este tópico é abordado mais detalhadamente em [ocultar por meio de herança](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="c8a10-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="c8a10-473">Se um `new` modificador for incluído em uma declaração que não oculta um membro herdado, um aviso para esse efeito será emitido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="c8a10-474">Esse aviso é suprimido com a remoção `new` do modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="c8a10-475">Modificadores de acesso</span><span class="sxs-lookup"><span data-stu-id="c8a10-475">Access modifiers</span></span>

<span data-ttu-id="c8a10-476">Um *class_member_declaration* pode ter qualquer um dos cinco tipos possíveis de acessibilidade declarada ([acessibilidade declarada](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` ou `private`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="c8a10-477">Exceto para a `protected internal` combinação, é um erro de tempo de compilação para especificar mais de um modificador de acesso.</span><span class="sxs-lookup"><span data-stu-id="c8a10-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="c8a10-478">Quando um *class_member_declaration* não inclui nenhum modificador de acesso, `private` é assumido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="c8a10-479">Tipos constituintes</span><span class="sxs-lookup"><span data-stu-id="c8a10-479">Constituent types</span></span>

<span data-ttu-id="c8a10-480">Os tipos que são usados na declaração de um membro são chamados de tipos constituintes desse membro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="c8a10-481">Os tipos constituintes possíveis são o tipo de uma constante, campo, propriedade, evento ou indexador, o tipo de retorno de um método ou operador e os tipos de parâmetro de um construtor de método, indexador, operador ou instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="c8a10-482">Os tipos constituintes de um membro devem ser pelo menos tão acessíveis quanto o próprio membro ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="c8a10-483">Membros estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-483">Static and instance members</span></span>

<span data-ttu-id="c8a10-484">Os membros de uma classe são membros ***estáticos*** ou ***membros de instância***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="c8a10-485">Em geral, é útil considerar os membros estáticos como pertencentes a tipos de classe e membros de instância como pertencentes a objetos (instâncias de tipos de classe).</span><span class="sxs-lookup"><span data-stu-id="c8a10-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="c8a10-486">Quando um campo, método, propriedade, evento, operador ou declaração de Construtor inclui um `static` modificador, ele declara um membro estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="c8a10-487">Além disso, uma constante ou declaração de tipo declara implicitamente um membro estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="c8a10-488">Os membros estáticos têm as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="c8a10-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="c8a10-489">Quando um membro estático `M` é referenciado em um *member_access* ([acesso de membro](expressions.md#member-access)) do formato `E.M`, `E` deve indicar um tipo contendo `M`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="c8a10-490">É um erro de tempo de compilação para `E` indicar uma instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="c8a10-491">Um campo estático identifica exatamente um local de armazenamento a ser compartilhado por todas as instâncias de um determinado tipo de classe fechado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="c8a10-492">Independentemente de quantas instâncias de um determinado tipo de classe fechada forem criadas, há apenas uma cópia de um campo estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="c8a10-493">Um membro de função estática (método, propriedade, evento, operador ou Construtor) não opera em uma instância específica e é um erro de tempo de compilação para fazer referência a `this` esse membro de função.</span><span class="sxs-lookup"><span data-stu-id="c8a10-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="c8a10-494">Quando um campo, método, propriedade, evento, indexador, Construtor ou declaração de destruidor não inclui um `static` modificador, ele declara um membro de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="c8a10-495">(Um membro de instância é, às vezes, chamado de membro não estático.) Os membros da instância têm as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="c8a10-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="c8a10-496">Quando um membro de instância `M` é referenciado em um *member_access* ([acesso de membro](expressions.md#member-access)) do formato `E.M`, `E` deve indicar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="c8a10-497">É um erro de tempo de ligação para `E` indicar um tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="c8a10-498">Cada instância de uma classe contém um conjunto separado de todos os campos de instância da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="c8a10-499">Um membro de função de instância (método, propriedade, indexador, Construtor de instância ou destruidor) opera em uma determinada instância da classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="c8a10-500">O exemplo a seguir ilustra as regras para acessar membros estáticos e de instância:</span><span class="sxs-lookup"><span data-stu-id="c8a10-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="c8a10-501">O método `F` mostra que, em um membro da função de instância, um *Simple_name* ([nomes simples](expressions.md#simple-names)) pode ser usado para acessar membros de instância e membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="c8a10-502">O método `G` mostra que em um membro de função estática, é um erro de tempo de compilação para acessar um membro de instância por meio de um *Simple_name*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="c8a10-503">O método `Main` mostra que em um *member_access* ([acesso de membro](expressions.md#member-access)), os membros da instância devem ser acessados por meio de instâncias e os membros estáticos devem ser acessados por meio de tipos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="c8a10-504">Tipos aninhados</span><span class="sxs-lookup"><span data-stu-id="c8a10-504">Nested types</span></span>

<span data-ttu-id="c8a10-505">Um tipo declarado em uma declaração de classe ou struct é chamado de ***tipo aninhado***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="c8a10-506">Um tipo declarado em uma unidade de compilação ou namespace é chamado de ***tipo não aninhado***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="c8a10-507">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-507">In the example</span></span>
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
<span data-ttu-id="c8a10-508">Class `B` é um tipo aninhado porque é declarado dentro de `A`Class e Class `A` é um tipo não aninhado porque é declarado em uma unidade de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="c8a10-509">Nome totalmente qualificado</span><span class="sxs-lookup"><span data-stu-id="c8a10-509">Fully qualified name</span></span>

<span data-ttu-id="c8a10-510">O nome totalmente qualificado ([nomes totalmente qualificados](basic-concepts.md#fully-qualified-names)) para um tipo aninhado `S.N` é `S` onde é o nome totalmente qualificado do tipo no qual o `N` tipo é declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="c8a10-511">Acessibilidade declarada</span><span class="sxs-lookup"><span data-stu-id="c8a10-511">Declared accessibility</span></span>

<span data-ttu-id="c8a10-512">Tipos não aninhados podem ter `public` ou `internal` declarar acessibilidade e ter `internal` declarado acessibilidade por padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="c8a10-513">Os tipos aninhados também podem ter esses formulários de acessibilidade declarados, além de uma ou mais formas adicionais de acessibilidade declarada, dependendo se o tipo recipiente é uma classe ou estrutura:</span><span class="sxs-lookup"><span data-stu-id="c8a10-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="c8a10-514">Um tipo aninhado declarado em uma classe pode ter qualquer uma das cinco formas de acessibilidade declaradas `protected internal`( `protected``public`, `internal`,, `private`ou) e, como outros membros da classe, o `private` padrão é declarado acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="c8a10-515">Um tipo aninhado declarado em um struct pode ter qualquer uma das três formas de acessibilidade declarada `internal`(`public`, `private`ou) e, como outros membros de struct, usa `private` acessibilidade declarada por padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="c8a10-516">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-516">The example</span></span>
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
<span data-ttu-id="c8a10-517">declara uma classe `Node`aninhada privada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="c8a10-518">Ocultar</span><span class="sxs-lookup"><span data-stu-id="c8a10-518">Hiding</span></span>

<span data-ttu-id="c8a10-519">Um tipo aninhado pode ocultar ([ocultar o nome](basic-concepts.md#name-hiding)) um membro base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="c8a10-520">O `new` modificador é permitido em declarações de tipo aninhadas para que a ocultação possa ser expressa explicitamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="c8a10-521">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-521">The example</span></span>
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
<span data-ttu-id="c8a10-522">mostra uma classe `M` aninhada que oculta o método `M` definido em `Base`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="c8a10-523">Este acesso</span><span class="sxs-lookup"><span data-stu-id="c8a10-523">this access</span></span>

<span data-ttu-id="c8a10-524">Um tipo aninhado e seu tipo recipiente não têm uma relação especial com relação a *this_access* ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="c8a10-525">Especificamente, `this` dentro de um tipo aninhado não pode ser usado para fazer referência a membros de instância do tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="c8a10-526">Nos casos em que um tipo aninhado precisa de acesso aos membros da instância de seu tipo recipiente, o acesso pode ser `this` fornecido fornecendo o para a instância do tipo recipiente como um argumento de construtor para o tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="c8a10-527">O exemplo a seguir</span><span class="sxs-lookup"><span data-stu-id="c8a10-527">The following example</span></span>
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
<span data-ttu-id="c8a10-528">mostra essa técnica.</span><span class="sxs-lookup"><span data-stu-id="c8a10-528">shows this technique.</span></span> <span data-ttu-id="c8a10-529">Uma instância do `C` cria uma instância do `Nested` e passa seu próprio `this` Construtor `Nested`para o para fornecer acesso subsequente aos `C`membros da instância do.</span><span class="sxs-lookup"><span data-stu-id="c8a10-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="c8a10-530">Acesso a membros privados e protegidos do tipo recipiente</span><span class="sxs-lookup"><span data-stu-id="c8a10-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="c8a10-531">Um tipo aninhado tem acesso a todos os membros que são acessíveis para seu tipo recipiente, incluindo membros do tipo recipiente que têm e `private` `protected` têm acessibilidade declarada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="c8a10-532">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-532">The example</span></span>
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
<span data-ttu-id="c8a10-533">mostra uma classe `C` que contém uma classe `Nested`aninhada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="c8a10-534">Em `Nested`, o método `G` chama o método `F` estático definido em `C`e `F` tem acessibilidade definida como particular.</span><span class="sxs-lookup"><span data-stu-id="c8a10-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="c8a10-535">Um tipo aninhado também pode acessar membros protegidos definidos em um tipo base de seu tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="c8a10-536">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-536">In the example</span></span>
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
<span data-ttu-id="c8a10-537">a `Derived.Nested` classe aninhada acessa o método `F` protegido definido na `Derived`classe base do, `Base`, chamando por meio de uma instância `Derived`do.</span><span class="sxs-lookup"><span data-stu-id="c8a10-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="c8a10-538">Tipos aninhados em classes genéricas</span><span class="sxs-lookup"><span data-stu-id="c8a10-538">Nested types in generic classes</span></span>

<span data-ttu-id="c8a10-539">Uma declaração de classe genérica pode conter declarações de tipo aninhadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="c8a10-540">Os parâmetros de tipo da classe delimitadora podem ser usados dentro dos tipos aninhados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="c8a10-541">Uma declaração de tipo aninhado pode conter parâmetros de tipo adicionais que se aplicam somente ao tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="c8a10-542">Cada declaração de tipo contida em uma declaração de classe genérica é implicitamente uma declaração de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="c8a10-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="c8a10-543">Ao gravar uma referência a um tipo aninhado em um tipo genérico, o tipo construído que o contém, incluindo seus argumentos de tipo, deve ser nomeado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="c8a10-544">No entanto, de dentro da classe externa, o tipo aninhado pode ser usado sem qualificação; o tipo de instância da classe externa pode ser usado implicitamente ao construir o tipo aninhado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="c8a10-545">O exemplo a seguir mostra três maneiras corretas diferentes de se referir a um `Inner`tipo construído criado a partir de; as duas primeiras são equivalentes:</span><span class="sxs-lookup"><span data-stu-id="c8a10-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="c8a10-546">Embora seja um estilo de programação inadequado, um parâmetro de tipo em um tipo aninhado pode ocultar um membro ou parâmetro de tipo declarado no tipo externo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="c8a10-547">Nomes de membros reservados</span><span class="sxs-lookup"><span data-stu-id="c8a10-547">Reserved member names</span></span>

<span data-ttu-id="c8a10-548">Para facilitar a implementação C# de tempo de execução subjacente, para cada declaração de membro de origem que seja uma propriedade, um evento ou um indexador, a implementação deve reservar duas assinaturas de método com base no tipo de declaração de membro, seu nome e seu tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="c8a10-549">É um erro de tempo de compilação para um programa declarar um membro cuja assinatura corresponde a uma dessas assinaturas reservadas, mesmo que a implementação de tempo de execução subjacente não faça uso dessas reservas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="c8a10-550">Os nomes reservados não introduzem declarações, portanto, eles não participam da pesquisa de membros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="c8a10-551">No entanto, as assinaturas de método reservado associadas de uma declaração participam da herança ([herança](classes.md#inheritance)) e podem ser `new` ocultadas com o modificador ([o novo modificador](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="c8a10-552">A reserva desses nomes serve para três finalidades:</span><span class="sxs-lookup"><span data-stu-id="c8a10-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="c8a10-553">Para permitir que a implementação subjacente use um identificador comum como um nome de método para obter ou definir o acesso C# ao recurso de idioma.</span><span class="sxs-lookup"><span data-stu-id="c8a10-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="c8a10-554">Para permitir que outras linguagens interoperem usando um identificador comum como um nome de método para obter ou definir C# o acesso ao recurso de idioma.</span><span class="sxs-lookup"><span data-stu-id="c8a10-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="c8a10-555">Para ajudar a garantir que a origem aceita por um compilador em conformidade é aceita por outro, tornando-se as especificidades dos nomes de membro reservados consistentes em todas as C# implementações.</span><span class="sxs-lookup"><span data-stu-id="c8a10-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="c8a10-556">A declaração de um destruidor ([destruidores](classes.md#destructors)) também faz com que uma assinatura seja reservada ([nomes de membros reservados para destruidores](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="c8a10-557">Nomes de membro reservados para propriedades</span><span class="sxs-lookup"><span data-stu-id="c8a10-557">Member names reserved for properties</span></span>

<span data-ttu-id="c8a10-558">Para uma propriedade `P` ([Propriedades](classes.md#properties)) do tipo `T`, as seguintes assinaturas são reservadas:</span><span class="sxs-lookup"><span data-stu-id="c8a10-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="c8a10-559">Ambas as assinaturas são reservadas, mesmo que a propriedade seja somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="c8a10-560">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-560">In the example</span></span>
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
<span data-ttu-id="c8a10-561">uma classe `A` define uma propriedade `P`somente leitura, reservando, assim, assinaturas `get_P` para `set_P` os métodos e.</span><span class="sxs-lookup"><span data-stu-id="c8a10-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="c8a10-562">Uma classe `B` deriva de `A` e oculta essas duas assinaturas reservadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="c8a10-563">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="c8a10-564">Nomes de membro reservados para eventos</span><span class="sxs-lookup"><span data-stu-id="c8a10-564">Member names reserved for events</span></span>

<span data-ttu-id="c8a10-565">Para um evento `E` ([eventos](classes.md#events)) do tipo `T`delegate, as seguintes assinaturas são reservadas:</span><span class="sxs-lookup"><span data-stu-id="c8a10-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="c8a10-566">Nomes de membro reservados para indexadores</span><span class="sxs-lookup"><span data-stu-id="c8a10-566">Member names reserved for indexers</span></span>

<span data-ttu-id="c8a10-567">Para um indexador ([indexadores](classes.md#indexers)) do tipo `T` com a lista `L`de parâmetros, as seguintes assinaturas são reservadas:</span><span class="sxs-lookup"><span data-stu-id="c8a10-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="c8a10-568">Ambas as assinaturas são reservadas, mesmo que o indexador seja somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="c8a10-569">Além disso, o `Item` nome do membro é reservado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="c8a10-570">Nomes de membro reservados para destruidores</span><span class="sxs-lookup"><span data-stu-id="c8a10-570">Member names reserved for destructors</span></span>

<span data-ttu-id="c8a10-571">Para uma classe que contém um destruidor ([destruidores](classes.md#destructors)), a assinatura a seguir é reservada:</span><span class="sxs-lookup"><span data-stu-id="c8a10-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="c8a10-572">Constantes</span><span class="sxs-lookup"><span data-stu-id="c8a10-572">Constants</span></span>

<span data-ttu-id="c8a10-573">Uma ***constante*** é um membro de classe que representa um valor constante: um valor que pode ser calculado em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="c8a10-574">Um *constant_declaration* introduz uma ou mais constantes de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="c8a10-575">Um *constant_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um modificador `new` ([o novo modificador](classes.md#the-new-modifier)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="c8a10-576">Os atributos e os modificadores se aplicam a todos os membros declarados pelo *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="c8a10-577">Embora constantes sejam consideradas membros estáticos, um *constant_declaration* não requer nem permite um modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="c8a10-578">É um erro para que o mesmo modificador apareça várias vezes em uma declaração de constante.</span><span class="sxs-lookup"><span data-stu-id="c8a10-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="c8a10-579">O *tipo* de um *constant_declaration* especifica o tipo dos membros introduzidos pela declaração.</span><span class="sxs-lookup"><span data-stu-id="c8a10-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="c8a10-580">O tipo é seguido por uma lista de *constant_declarator*s, cada um dos quais introduz um novo membro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="c8a10-581">Um *constant_declarator* consiste em um *identificador* que nomeia o membro, seguido por um token "`=`", seguido por um *constant_expression* ([expressões constantes](expressions.md#constant-expressions)) que fornece o valor do membro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="c8a10-582">O *tipo* especificado em uma declaração de constante deve ser `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4, um *enum_type*ou um *reference_ Digite*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="c8a10-583">Cada *constant_expression* deve produzir um valor do tipo de destino ou de um tipo que possa ser convertido para o tipo de destino por uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="c8a10-584">O *tipo* de uma constante deve ser pelo menos tão acessível quanto a própria constante ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-585">O valor de uma constante é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou um *member_access* ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="c8a10-586">Uma constante pode participar de um *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="c8a10-587">Portanto, uma constante pode ser usada em qualquer construção que exija um *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="c8a10-588">Exemplos dessas construções incluem `case` rótulos, `goto case` instruções, `enum` declarações de membro, atributos e outras declarações de constantes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="c8a10-589">Conforme descrito em [expressões constantes](expressions.md#constant-expressions), um *constant_expression* é uma expressão que pode ser totalmente avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="c8a10-590">Como a única maneira de criar um valor não nulo de um *reference_type* diferente de `string` é aplicar o operador `new` e, como o operador `new` não é permitido em um *constant_expression*, o único valor possível para constantes de  *reference_type*s diferentes de `string` é `null`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="c8a10-591">Quando é desejado um nome simbólico para um valor constante, mas quando o tipo desse valor não é permitido em uma declaração de constante, ou quando o valor não pode ser computado em tempo de compilação por um *constant_expression*, um campo `readonly` ([campos ReadOnly](classes.md#readonly-fields)) pode ser usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c8a10-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="c8a10-592">Uma declaração constante que declara várias constantes é equivalente a várias declarações de constantes únicas com os mesmos atributos, modificadores e tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="c8a10-593">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="c8a10-594">equivale a</span><span class="sxs-lookup"><span data-stu-id="c8a10-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="c8a10-595">As constantes podem depender de outras constantes no mesmo programa, contanto que as dependências não sejam de natureza circular.</span><span class="sxs-lookup"><span data-stu-id="c8a10-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="c8a10-596">O compilador organiza automaticamente para avaliar as declarações constantes na ordem apropriada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="c8a10-597">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-597">In the example</span></span>
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
<span data-ttu-id="c8a10-598">`A.Y`o compilador primeiro avalia, depois `B.Z`avalia e `A.X`, finalmente, avalia, produzindo os valores `10`, `11`e `12`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="c8a10-599">Declarações constantes podem depender de constantes de outros programas, mas essas dependências só são possíveis em uma direção.</span><span class="sxs-lookup"><span data-stu-id="c8a10-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="c8a10-600">Fazendo referência ao exemplo acima, se `A` e `B` foram declarados em programas separados, seria `B.Z`possível `A.X` depender, mas `B.Z` , em seguida, não depender `A.Y`simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="c8a10-601">Campos</span><span class="sxs-lookup"><span data-stu-id="c8a10-601">Fields</span></span>

<span data-ttu-id="c8a10-602">Um ***campo*** é um membro que representa uma variável associada a um objeto ou classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="c8a10-603">Um *field_declaration* introduz um ou mais campos de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="c8a10-604">Um *field_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um modificador `new` ([o novo modificador](classes.md#the-new-modifier)), uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) e um modificador de `static` ([ Campos estáticos e de instância](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="c8a10-605">Além disso, um *field_declaration* pode incluir um modificador `readonly` ([campos somente leitura](classes.md#readonly-fields)) ou um modificador de `volatile` ([campos voláteis](classes.md#volatile-fields)), mas não ambos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="c8a10-606">Os atributos e os modificadores se aplicam a todos os membros declarados pelo *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="c8a10-607">É um erro para que o mesmo modificador apareça várias vezes em uma declaração de campo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="c8a10-608">O *tipo* de um *field_declaration* especifica o tipo dos membros introduzidos pela declaração.</span><span class="sxs-lookup"><span data-stu-id="c8a10-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="c8a10-609">O tipo é seguido por uma lista de *variable_declarator*s, cada um dos quais introduz um novo membro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="c8a10-610">Um *variable_declarator* consiste em um *identificador* que nomeia esse membro, opcionalmente seguido por um token "`=`" e um *variable_initializer* ([inicializadores de variável](classes.md#variable-initializers)) que fornece o valor inicial desse membro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="c8a10-611">O *tipo* de um campo deve ser pelo menos tão acessível quanto o próprio campo ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-612">O valor de um campo é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou um *member_access* ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="c8a10-613">O valor de um campo não ReadOnly é modificado usando uma *atribuição* ([operadores de atribuição](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="c8a10-614">O valor de um campo não ReadOnly pode ser obtido e modificado usando o incremento de sufixo e os operadores de diminuição ([operadores de incremento e diminuição de sufixo](expressions.md#postfix-increment-and-decrement-operators)) e os operadores de incremento de prefixo e decréscimo ([incremento de prefixo e decréscimo operadores](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="c8a10-615">Uma declaração de campo que declara vários campos é equivalente a várias declarações de campos únicos com os mesmos atributos, modificadores e tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="c8a10-616">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="c8a10-617">equivale a</span><span class="sxs-lookup"><span data-stu-id="c8a10-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="c8a10-618">Campos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-618">Static and instance fields</span></span>

<span data-ttu-id="c8a10-619">Quando uma declaração de campo inclui `static` um modificador, os campos introduzidos pela declaração são ***campos estáticos***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="c8a10-620">Quando nenhum `static` modificador está presente, os campos introduzidos pela declaração são ***campos de instância***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="c8a10-621">Campos estáticos e campos de instância são dois dos vários tipos de variáveis ([variáveis](variables.md)) com C#suporte do e às vezes eles são chamados de ***variáveis estáticas*** e ***variáveis de instância***, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="c8a10-622">Um campo estático não faz parte de uma instância específica; em vez disso, ele é compartilhado entre todas as instâncias de um tipo fechado ([tipos abertos e fechados](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="c8a10-623">Não importa quantas instâncias de um tipo de classe fechada são criadas, há apenas uma cópia de um campo estático para o domínio de aplicativo associado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="c8a10-624">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-624">For example:</span></span>
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

<span data-ttu-id="c8a10-625">Um campo de instância pertence a uma instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="c8a10-626">Especificamente, cada instância de uma classe contém um conjunto separado de todos os campos de instância dessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="c8a10-627">Quando um campo é referenciado em um *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` for um campo estático, `E` deverá indicar um tipo contendo `M` e se `M` for um campo de instância, e deverá indicar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c8a10-628">As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c8a10-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="c8a10-629">Campos somente leitura</span><span class="sxs-lookup"><span data-stu-id="c8a10-629">Readonly fields</span></span>

<span data-ttu-id="c8a10-630">Quando um *field_declaration* inclui um modificador `readonly`, os campos introduzidos pela declaração são ***campos somente leitura***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="c8a10-631">Atribuições diretas a campos ReadOnly só podem ocorrer como parte dessa declaração ou em um construtor de instância ou construtor estático na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="c8a10-632">(Um campo ReadOnly pode ser atribuído a várias vezes nesses contextos.) Especificamente, as atribuições diretas a um `readonly` campo são permitidas apenas nos seguintes contextos:</span><span class="sxs-lookup"><span data-stu-id="c8a10-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="c8a10-633">No *variable_declarator* que introduz o campo (incluindo um *variable_initializer* na declaração).</span><span class="sxs-lookup"><span data-stu-id="c8a10-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="c8a10-634">Para um campo de instância, nos construtores de instância da classe que contém a declaração de campo; para um campo estático, no construtor estático da classe que contém a declaração de campo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="c8a10-635">Esses são também os únicos contextos nos quais é válido passar um `readonly` campo como um `out` parâmetro ou `ref` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="c8a10-636">A tentativa de atribuir a um `readonly` campo ou passá-lo como `out` um `ref` parâmetro ou em qualquer outro contexto é um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="c8a10-637">Usando campos somente leitura estáticos para constantes</span><span class="sxs-lookup"><span data-stu-id="c8a10-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="c8a10-638">Um `static readonly` campo é útil quando um nome simbólico para um valor constante é desejado, mas quando o tipo do valor não é permitido em uma `const` declaração ou quando o valor não pode ser computado em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="c8a10-639">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-639">In the example</span></span>
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
<span data-ttu-id="c8a10-640">os `Black`Membros `White` `const` ,, ,`Green`e não`Blue` podem ser declarados como membros porque seus valores não podem ser computados em tempo de compilação. `Red`</span><span class="sxs-lookup"><span data-stu-id="c8a10-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="c8a10-641">No entanto, `static readonly` declará-las, em vez disso, tem muito o mesmo efeito.</span><span class="sxs-lookup"><span data-stu-id="c8a10-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="c8a10-642">Controle de versão de constantes e campos somente leitura estáticos</span><span class="sxs-lookup"><span data-stu-id="c8a10-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="c8a10-643">Os campos Constants e ReadOnly têm semântica de versão binária diferente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="c8a10-644">Quando uma expressão faz referência a uma constante, o valor da constante é obtido em tempo de compilação, mas quando uma expressão faz referência a um campo ReadOnly, o valor do campo não é obtido até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="c8a10-645">Considere um aplicativo que consiste em dois programas separados:</span><span class="sxs-lookup"><span data-stu-id="c8a10-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="c8a10-646">Os `Program1` namespaces e `Program2` denotam dois programas que são compilados separadamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="c8a10-647">Como `Program1.Utils.X` é declarado como um campo somente leitura estático, a saída do valor `Console.WriteLine` pela instrução não é conhecida em tempo de compilação, mas é obtida em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="c8a10-648">Portanto, se o valor de `X` for alterado e `Program1` for recompilado, a `Console.WriteLine` instrução produzirá o novo valor, mesmo se `Program2` não for recompilado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="c8a10-649">No entanto `X` , era uma constante, o valor `X` de teria sido obtido no momento `Program2` em que foi compilado e permaneceria inalterado por alterações no `Program1` até que `Program2` seja recompilado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="c8a10-650">Campos voláteis</span><span class="sxs-lookup"><span data-stu-id="c8a10-650">Volatile fields</span></span>

<span data-ttu-id="c8a10-651">Quando um *field_declaration* inclui um modificador `volatile`, os campos introduzidos por essa declaração são ***campos voláteis***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="c8a10-652">Para campos não voláteis, as técnicas de otimização que reordenam instruções podem levar a resultados inesperados e imprevisíveis em programas multi-threaded que acessam campos sem sincronização, como o fornecido pelo *lock_statement* ([o instrução Lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="c8a10-653">Essas otimizações podem ser executadas pelo compilador, pelo sistema de tempo de execução ou por hardware.</span><span class="sxs-lookup"><span data-stu-id="c8a10-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="c8a10-654">Para campos voláteis, essas otimizações de reordenação são restritas:</span><span class="sxs-lookup"><span data-stu-id="c8a10-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="c8a10-655">Uma leitura de um campo volátil é chamada de ***leitura volátil***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="c8a10-656">Uma leitura volátil tem "adquirir semântica"; ou seja, é garantido que ocorra antes de qualquer referência à memória que ocorra depois dela na sequência de instruções.</span><span class="sxs-lookup"><span data-stu-id="c8a10-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="c8a10-657">Uma gravação de um campo volátil é chamada de ***gravação volátil***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="c8a10-658">Uma gravação volátil tem "semântica de liberação"; ou seja, é garantido que ocorra após qualquer referência de memória antes da instrução de gravação na sequência de instruções.</span><span class="sxs-lookup"><span data-stu-id="c8a10-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="c8a10-659">Essas restrições garantem que todos os threads observem as gravações voláteis executadas por qualquer outro thread na ordem em que elas foram realizadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="c8a10-660">Uma implementação de conformidade não é necessária para fornecer uma única ordenação total de gravações voláteis, como visto de todos os threads de execução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="c8a10-661">O tipo de um campo volátil deve ser um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="c8a10-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="c8a10-662">Um *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-662">A *reference_type*.</span></span>
*  <span data-ttu-id="c8a10-663">O tipo `byte` `sbyte` ,`uint` ,,`System.UIntPtr`,,,, ,`bool`, ou`System.IntPtr`. `short` `ushort` `int` `char` `float`</span><span class="sxs-lookup"><span data-stu-id="c8a10-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="c8a10-664">Um *enum_type* tem um tipo base de enumeração de `byte`, `sbyte`, `short`, `ushort`, `int` ou `uint`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="c8a10-665">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-665">The example</span></span>
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
<span data-ttu-id="c8a10-666">produz a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="c8a10-667">Neste exemplo, o método `Main` inicia um novo thread que executa o método. `Thread2`</span><span class="sxs-lookup"><span data-stu-id="c8a10-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="c8a10-668">Esse método armazena um valor em um campo não volátil chamado `result`e, em seguida, armazena `true` no campo `finished`volátil.</span><span class="sxs-lookup"><span data-stu-id="c8a10-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="c8a10-669">O thread principal aguarda que o campo `finished` seja definido como `true`e, em seguida, lê o `result`campo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="c8a10-670">Como `finished` foi declarado `volatile`, o thread principal deve ler o valor `143` do campo `result`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="c8a10-671">Se o campo `finished` não tivesse sido declarado `volatile`, seria `result` permitido que o repositório fosse visível `finished`para o thread principal após o repositório e, portanto, para o thread principal ler o valor `0` do campo `result`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="c8a10-672">Declarar `finished` como um `volatile` campo impede qualquer inconsistência.</span><span class="sxs-lookup"><span data-stu-id="c8a10-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="c8a10-673">Inicialização de campo</span><span class="sxs-lookup"><span data-stu-id="c8a10-673">Field initialization</span></span>

<span data-ttu-id="c8a10-674">O valor inicial de um campo, seja um campo estático ou um campo de instância, é o valor padrão ([valores padrão](variables.md#default-values)) do tipo do campo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="c8a10-675">Não é possível observar o valor de um campo antes que essa inicialização padrão tenha ocorrido, e um campo nunca é "não inicializado".</span><span class="sxs-lookup"><span data-stu-id="c8a10-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="c8a10-676">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-676">The example</span></span>
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
<span data-ttu-id="c8a10-677">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="c8a10-678">Porque `b` e`i` são inicializados automaticamente para valores padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="c8a10-679">Inicializadores de variável</span><span class="sxs-lookup"><span data-stu-id="c8a10-679">Variable initializers</span></span>

<span data-ttu-id="c8a10-680">Declarações de campo podem incluir *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="c8a10-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="c8a10-681">Para campos estáticos, inicializadores de variáveis correspondem a instruções de atribuição que são executadas durante a inicialização da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="c8a10-682">Para campos de instância, inicializadores de variáveis correspondem a instruções de atribuição que são executadas quando uma instância da classe é criada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="c8a10-683">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-683">The example</span></span>
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
<span data-ttu-id="c8a10-684">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="c8a10-685">Porque uma atribuição `x` ocorre quando inicializadores de campo estáticos são executados e `i` atribuições `s` e ocorrem quando os inicializadores de campo de instância são executados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="c8a10-686">A inicialização de valor padrão descrita na [inicialização do campo](classes.md#field-initialization) ocorre para todos os campos, incluindo campos que têm inicializadores variáveis.</span><span class="sxs-lookup"><span data-stu-id="c8a10-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="c8a10-687">Assim, quando uma classe é inicializada, todos os campos estáticos nessa classe são inicializados primeiro com seus valores padrão e, em seguida, os inicializadores de campo estático são executados em ordem textual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="c8a10-688">Da mesma forma, quando uma instância de uma classe é criada, todos os campos de instância nessa instância são inicializados primeiro com seus valores padrão e, em seguida, os inicializadores de campo de instância são executados na ordem textual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="c8a10-689">É possível que campos estáticos com inicializadores de variáveis sejam observados em seu estado de valor padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="c8a10-690">No entanto, isso é fortemente desencorajado como uma questão de estilo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="c8a10-691">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-691">The example</span></span>
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
<span data-ttu-id="c8a10-692">exibe esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-692">exhibits this behavior.</span></span> <span data-ttu-id="c8a10-693">Apesar das definições circulares de a e b, o programa é válido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="c8a10-694">Isso resulta na saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="c8a10-695">Porque os `a` campos estáticos `b` e são inicializados para `0` (o valor `int`padrão de) antes de seus inicializadores serem executados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="c8a10-696">Quando o inicializador `a` for executado, o valor `b` de será zero e, `a` portanto, será `1`inicializado para.</span><span class="sxs-lookup"><span data-stu-id="c8a10-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="c8a10-697">Quando o inicializador `b` for executado, o valor `a` de já `1`será e, `b` portanto, será `2`inicializado para.</span><span class="sxs-lookup"><span data-stu-id="c8a10-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="c8a10-698">Inicialização de campo estático</span><span class="sxs-lookup"><span data-stu-id="c8a10-698">Static field initialization</span></span>

<span data-ttu-id="c8a10-699">Os inicializadores de variável de campo estático de uma classe correspondem a uma sequência de atribuições que são executadas na ordem textual na qual aparecem na declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="c8a10-700">Se existir um construtor estático ([construtores estáticos](classes.md#static-constructors)) na classe, a execução dos inicializadores de campo estáticos ocorrerá imediatamente antes da execução desse construtor estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="c8a10-701">Caso contrário, os inicializadores de campo estáticos são executados em uma hora dependente da implementação antes do primeiro uso de um campo estático dessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="c8a10-702">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-702">The example</span></span>
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
<span data-ttu-id="c8a10-703">pode produzir a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="c8a10-704">ou a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="c8a10-705">como a execução do `X`inicializador e `Y`do inicializador do pode ocorrer em qualquer ordem; eles só são restritos a ocorrer antes das referências a esses campos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="c8a10-706">No entanto, no exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-706">However, in the example:</span></span>
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
<span data-ttu-id="c8a10-707">a saída deve ser:</span><span class="sxs-lookup"><span data-stu-id="c8a10-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="c8a10-708">como as regras para quando os construtores estáticos são executados (conforme definido em [construtores estáticos](classes.md#static-constructors)) `B`fornecem esse construtor estático (e `B`, portanto, inicializadores de campo estáticos `A`) devem ser executados antes do estático inicializadores de construtor e de campo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="c8a10-709">Inicialização do campo de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-709">Instance field initialization</span></span>

<span data-ttu-id="c8a10-710">Os inicializadores de variável de campo de instância de uma classe correspondem a uma sequência de atribuições que são executadas imediatamente após a entrada para qualquer um dos construtores de instância ([inicializadores de Construtor](classes.md#constructor-initializers)) dessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="c8a10-711">Os inicializadores de variável são executados na ordem textual em que aparecem na declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="c8a10-712">O processo de criação e inicialização da instância de classe é descrito mais detalhadamente em [construtores de instância](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="c8a10-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="c8a10-713">Um inicializador de variável para um campo de instância não pode fazer referência à instância que está sendo criada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="c8a10-714">Portanto, é um erro de tempo de compilação para referenciar `this` em um inicializador de variável, pois é um erro de tempo de compilação para um inicializador de variável referenciar qualquer membro de instância por meio de um *Simple_name*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="c8a10-715">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="c8a10-716">o inicializador de `y` variável para resulta em um erro de tempo de compilação porque faz referência a um membro da instância que está sendo criada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="c8a10-717">Métodos</span><span class="sxs-lookup"><span data-stu-id="c8a10-717">Methods</span></span>

<span data-ttu-id="c8a10-718">Um ***método*** é um membro que implementa um cálculo ou uma ação que pode ser executada por um objeto ou classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="c8a10-719">Os métodos são declarados usando *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-720">Um *method_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([estático e de instância métodos](classes.md#static-and-instance-methods)), o `virtual` ([métodos virtuais](classes.md#virtual-methods)), os 0 ([métodos de substituição](classes.md#override-methods)), os modificadores 2 ([métodos lacrados](classes.md#sealed-methods)), 4 ([métodos abstratos](classes.md#abstract-methods)) e 6 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c8a10-721">Uma declaração tem uma combinação válida de modificadores se todas as seguintes opções forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="c8a10-722">A declaração inclui uma combinação válida de modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="c8a10-723">A declaração não inclui o mesmo modificador várias vezes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="c8a10-724">A declaração inclui, no máximo, um dos seguintes modificadores `static`: `virtual`, e `override`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="c8a10-725">A declaração inclui, no máximo, um dos seguintes modificadores `new` : `override`e.</span><span class="sxs-lookup"><span data-stu-id="c8a10-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="c8a10-726">Se a declaração incluir o `abstract` modificador, a declaração não incluirá nenhum dos seguintes modificadores: `static`, `virtual` `sealed` ou `extern`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="c8a10-727">Se a declaração incluir o `private` modificador, a declaração não incluirá nenhum dos seguintes modificadores: `virtual`, `override`ou `abstract`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="c8a10-728">Se a declaração incluir o `sealed` modificador, a declaração também incluirá `override` o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="c8a10-729">Se a declaração incluir o `partial` modificador, ele não incluirá nenhum dos seguintes modificadores: `protected` `new` `virtual`, `sealed` `private` `public` `internal`,,,,,, `override` , `abstract`ou .`extern`</span><span class="sxs-lookup"><span data-stu-id="c8a10-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="c8a10-730">Um método que tem o `async` modificador é uma função assíncrona e segue as regras descritas em [funções assíncronas](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="c8a10-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="c8a10-731">O *return_type* de uma declaração de método especifica o tipo do valor calculado e retornado pelo método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="c8a10-732">O *return_type* será `void` se o método não retornar um valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="c8a10-733">Se a declaração incluir o `partial` modificador, o tipo de retorno deverá `void`ser.</span><span class="sxs-lookup"><span data-stu-id="c8a10-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="c8a10-734">O *member_name* especifica o nome do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="c8a10-735">A menos que o método seja uma implementação de membro de interface explícita ([implementações explícitas de membro de interface](interfaces.md#explicit-interface-member-implementations)), o *member_name* é simplesmente um *identificador*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="c8a10-736">Para uma implementação de membro de interface explícita, o *member_name* consiste em um *interface_type* seguido por um "`.`" e um *identificador*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="c8a10-737">O *type_parameter_list* opcional especifica os parâmetros de tipo do método ([parâmetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="c8a10-738">Se um *type_parameter_list* for especificado, o método será um ***método genérico***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="c8a10-739">Se o método tiver um modificador `extern`, um *type_parameter_list* não poderá ser especificado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="c8a10-740">O *formal_parameter_list* opcional especifica os parâmetros do método ([parâmetros do método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="c8a10-741">Os *type_parameter_constraints_clause*opcionais especificam restrições em parâmetros de tipo individuais ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) e só podem ser especificados se um *type_parameter_list* também for fornecido e o método não tiver um modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="c8a10-742">O *return_type* e cada um dos tipos referenciados no *formal_parameter_list* de um método devem ser pelo menos tão acessíveis quanto o próprio método ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-743">O *method_body* é um ponto e vírgula, um ***corpo de instrução*** ou um corpo de ***expressão***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="c8a10-744">Um corpo de instrução consiste em um *bloco*, que especifica as instruções a serem executadas quando o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="c8a10-745">Um corpo de `=>` expressão consiste em seguido por uma *expressão* e um ponto e vírgula e denota uma única expressão a ser executada quando o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="c8a10-746">Para os métodos `abstract` e `extern`, o *method_body* consiste em apenas um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="c8a10-747">Para métodos `partial`, o *method_body* pode consistir em um ponto e vírgula, um corpo de bloco ou um corpo de expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="c8a10-748">Para todos os outros métodos, o *method_body* é um corpo de bloco ou um corpo de expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="c8a10-749">Se o *method_body* consistir em um ponto e vírgula, a declaração pode não incluir o modificador `async`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="c8a10-750">O nome, a lista de parâmetros de tipo e a lista de parâmetros formais de um método definem a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="c8a10-751">Especificamente, a assinatura de um método consiste em seu nome, o número de parâmetros de tipo e o número, os modificadores e os tipos de seus parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="c8a10-752">Para essas finalidades, qualquer parâmetro de tipo do método que ocorre no tipo de um parâmetro formal é identificado não por seu nome, mas por sua posição ordinal na lista de argumentos de tipo do método. O tipo de retorno não faz parte da assinatura de um método, nem os nomes dos parâmetros de tipo ou dos parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="c8a10-753">O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="c8a10-754">Além disso, a assinatura de um método deve diferir das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe podem não ter assinaturas que `ref` diferem exclusivamente por e. `out`</span><span class="sxs-lookup"><span data-stu-id="c8a10-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="c8a10-755">As *type_parameter*s do método estão no escopo em todo o *method_declaration*e podem ser usadas para formar tipos em todo esse escopo em *return_type*, *method_body*e *type_parameter_constraints_clause*s, mas não em *atributos*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="c8a10-756">Todos os parâmetros formais e parâmetros de tipo devem ter nomes diferentes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="c8a10-757">Parâmetros do método</span><span class="sxs-lookup"><span data-stu-id="c8a10-757">Method parameters</span></span>

<span data-ttu-id="c8a10-758">Os parâmetros de um método, se houver, são declarados pelo *formal_parameter_list*do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="c8a10-759">A lista de parâmetros formais consiste em um ou mais parâmetros separados por vírgulas, dos quais apenas o último pode ser um *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="c8a10-760">Um *fixed_parameter* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um modificador opcional `ref`, `out` ou `this`, um *tipo*, um *identificador* e um *default_argument*opcional.</span><span class="sxs-lookup"><span data-stu-id="c8a10-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="c8a10-761">Cada *fixed_parameter* declara um parâmetro do tipo fornecido com o nome fornecido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="c8a10-762">O `this` modificador designa o método como um método de extensão e só é permitido no primeiro parâmetro de um método estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="c8a10-763">Os métodos de extensão são descritos mais detalhadamente em [métodos de extensão](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="c8a10-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="c8a10-764">Um *fixed_parameter* com um *default_argument* é conhecido como um ***parâmetro opcional***, enquanto que um *fixed_parameter* sem um *default_argument* é um ***parâmetro necessário***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="c8a10-765">Um parâmetro necessário pode não aparecer após um parâmetro opcional em um *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="c8a10-766">Um parâmetro `ref` ou `out` não pode ter um *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="c8a10-767">A *expressão* em um *default_argument* deve ser uma das seguintes:</span><span class="sxs-lookup"><span data-stu-id="c8a10-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="c8a10-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="c8a10-768">a *constant_expression*</span></span>
*  <span data-ttu-id="c8a10-769">uma expressão do formulário `new S()` em que `S` é um tipo de valor</span><span class="sxs-lookup"><span data-stu-id="c8a10-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="c8a10-770">uma expressão do formulário `default(S)` em que `S` é um tipo de valor</span><span class="sxs-lookup"><span data-stu-id="c8a10-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="c8a10-771">A *expressão* deve ser conversível implicitamente por uma identidade ou conversão anulável para o tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="c8a10-772">Se os parâmetros opcionais ocorrerem em uma declaração de método parcial de implementação ([métodos parciais](classes.md#partial-methods)), uma implementação de membro de interface explícita ([implementações explícitas de membro de interface](interfaces.md#explicit-interface-member-implementations)) ou em uma declaração de indexador de parâmetro único ([ Indexadores](classes.md#indexers)) que o compilador deve dar a um aviso, já que esses membros nunca podem ser invocados de forma a permitir que os argumentos sejam omitidos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="c8a10-773">Um *parameter_array* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um modificador `params`, um *array_type*e um *identificador*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="c8a10-774">Uma matriz de parâmetros declara um único parâmetro do tipo de matriz fornecido com o nome fornecido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="c8a10-775">O *array_type* de uma matriz de parâmetros deve ser um tipo de matriz de dimensão única ([tipos de matriz](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="c8a10-776">Em uma invocação de método, uma matriz de parâmetros permite que um único argumento do tipo de matriz fornecido seja especificado ou que zero ou mais argumentos do tipo de elemento de matriz sejam especificados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="c8a10-777">Matrizes de parâmetros são descritas em mais detalhes em [matrizes de parâmetros](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="c8a10-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="c8a10-778">Um *parameter_array* pode ocorrer após um parâmetro opcional, mas não pode ter um valor padrão – a omissão de argumentos para um *parameter_array* resultaria na criação de uma matriz vazia.</span><span class="sxs-lookup"><span data-stu-id="c8a10-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="c8a10-779">O exemplo a seguir ilustra os diferentes tipos de parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c8a10-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="c8a10-780">No *formal_parameter_list* para `M`, `i` é um parâmetro de referência necessário, `d` é um parâmetro de valor obrigatório, `b`, `s`, `o` e `t` são parâmetros de valor opcionais e `a` é uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="c8a10-781">Uma declaração de método cria um espaço de declaração separado para parâmetros, parâmetros de tipo e variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="c8a10-782">Os nomes são introduzidos nesse espaço de declaração pela lista de parâmetros de tipo e pela lista de parâmetros formais do método e por declarações de variáveis locais no *bloco* do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="c8a10-783">É um erro para dois membros de um espaço de declaração de método ter o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="c8a10-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="c8a10-784">É um erro para o espaço de declaração de método e o espaço de declaração de variável local de um espaço de declaração aninhada para conter elementos com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="c8a10-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="c8a10-785">Uma invocação de método ([invocações de método](expressions.md#method-invocations)) cria uma cópia, específica para essa invocação, dos parâmetros formais e das variáveis locais do método, e a lista de argumentos da invocação atribui valores ou referências de variáveis ao formal recém-criado parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="c8a10-786">Dentro do *bloco* de um método, parâmetros formais podem ser referenciados por seus identificadores em expressões *Simple_name* ([nomes simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="c8a10-787">Há quatro tipos de parâmetros formais:</span><span class="sxs-lookup"><span data-stu-id="c8a10-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="c8a10-788">Parâmetros de valor, que são declarados sem nenhum modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="c8a10-789">Parâmetros de referência, que são declarados com o `ref` modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="c8a10-790">Parâmetros de saída, que são declarados com o `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="c8a10-791">Matrizes de parâmetros, que são declaradas com o `params` modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="c8a10-792">Conforme descrito em [assinaturas e sobrecargas](basic-concepts.md#signatures-and-overloading), os `ref` modificadores e `out` são parte da assinatura de um método, mas o `params` modificador não é.</span><span class="sxs-lookup"><span data-stu-id="c8a10-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="c8a10-793">Parâmetros de valor</span><span class="sxs-lookup"><span data-stu-id="c8a10-793">Value parameters</span></span>

<span data-ttu-id="c8a10-794">Um parâmetro declarado sem nenhum modificador é um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="c8a10-795">Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento correspondente fornecido na invocação do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="c8a10-796">Quando um parâmetro formal é um parâmetro de valor, o argumento correspondente em uma invocação de método deve ser uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="c8a10-797">Um método tem permissão para atribuir novos valores a um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="c8a10-798">Essas atribuições afetam apenas o local de armazenamento local representado pelo parâmetro value — elas não têm nenhum efeito sobre o argumento real fornecido na invocação do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="c8a10-799">Parâmetros de referência</span><span class="sxs-lookup"><span data-stu-id="c8a10-799">Reference parameters</span></span>

<span data-ttu-id="c8a10-800">Um parâmetro declarado com um `ref` modificador é um parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="c8a10-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="c8a10-801">Ao contrário de um parâmetro de valor, um parâmetro de referência não cria um novo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="c8a10-802">Em vez disso, um parâmetro de referência representa o mesmo local de armazenamento que a variável fornecida como o argumento na invocação do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="c8a10-803">Quando um parâmetro formal é um parâmetro de referência, o argumento correspondente em uma invocação de método deve consistir na palavra-chave `ref` seguido por um *variable_reference* ([regras precisas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo tipo que o parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="c8a10-804">Uma variável deve ser definitivamente atribuída antes de poder ser passada como um parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="c8a10-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="c8a10-805">Dentro de um método, um parâmetro de referência é sempre considerado definitivamente atribuído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="c8a10-806">Um método declarado como iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de referência.</span><span class="sxs-lookup"><span data-stu-id="c8a10-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="c8a10-807">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-807">The example</span></span>
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
<span data-ttu-id="c8a10-808">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="c8a10-809">Para a invocação de `Swap` em `Main`, `x` representa `i` e `y` representa `j`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="c8a10-810">Assim, a invocação tem o efeito de trocar os valores de `i` e. `j`</span><span class="sxs-lookup"><span data-stu-id="c8a10-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="c8a10-811">Em um método que usa parâmetros de referência, é possível que vários nomes representem o mesmo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="c8a10-812">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-812">In the example</span></span>
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
<span data-ttu-id="c8a10-813">a invocação do `F` no `G` passa uma referência para `s` o `a` e `b`o.</span><span class="sxs-lookup"><span data-stu-id="c8a10-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="c8a10-814">Portanto, para essa invocação, os nomes `s`, `a`e `b` todos fazem referência ao mesmo local de armazenamento e as três atribuições modificam o campo `s`de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="c8a10-815">Parâmetros de saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-815">Output parameters</span></span>

<span data-ttu-id="c8a10-816">Um parâmetro declarado com um `out` modificador é um parâmetro de saída.</span><span class="sxs-lookup"><span data-stu-id="c8a10-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="c8a10-817">Semelhante a um parâmetro de referência, um parâmetro de saída não cria um novo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="c8a10-818">Em vez disso, um parâmetro de saída representa o mesmo local de armazenamento que a variável fornecida como o argumento na invocação do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="c8a10-819">Quando um parâmetro formal é um parâmetro de saída, o argumento correspondente em uma invocação de método deve consistir na palavra-chave `out` seguido de um *variable_reference* ([regras precisas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo tipo que o parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="c8a10-820">Uma variável não precisa ser definitivamente atribuída antes de poder ser passada como um parâmetro de saída, mas após uma invocação em que uma variável foi passada como um parâmetro de saída, a variável é considerada definitivamente atribuída.</span><span class="sxs-lookup"><span data-stu-id="c8a10-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="c8a10-821">Dentro de um método, assim como uma variável local, um parâmetro de saída é inicialmente considerado não atribuído e deve ser definitivamente atribuído antes de seu valor ser usado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="c8a10-822">Cada parâmetro de saída de um método deve ser definitivamente atribuído antes do retorno do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="c8a10-823">Um método declarado como um método parcial ([métodos parciais](classes.md#partial-methods)) ou um iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de saída.</span><span class="sxs-lookup"><span data-stu-id="c8a10-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="c8a10-824">Os parâmetros de saída normalmente são usados em métodos que produzem vários valores de retorno.</span><span class="sxs-lookup"><span data-stu-id="c8a10-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="c8a10-825">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-825">For example:</span></span>
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

<span data-ttu-id="c8a10-826">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="c8a10-827">Observe que as `dir` variáveis `name` e podem ser desatribuídas antes de serem passadas para `SplitPath`e que elas sejam consideradas definitivamente atribuídas após a chamada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="c8a10-828">Matrizes de parâmetros</span><span class="sxs-lookup"><span data-stu-id="c8a10-828">Parameter arrays</span></span>

<span data-ttu-id="c8a10-829">Um parâmetro declarado com um `params` modificador é uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="c8a10-830">Se uma lista de parâmetros formais incluir uma matriz de parâmetros, ela deverá ser o último parâmetro na lista e deverá ser de um tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="c8a10-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="c8a10-831">Por exemplo, os tipos `string[]` e `string[][]` podem ser usados como o tipo de uma matriz de parâmetros, mas o `string[,]` tipo não pode.</span><span class="sxs-lookup"><span data-stu-id="c8a10-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="c8a10-832">Não é possível combinar o `params` modificador com os `ref` modificadores e `out`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="c8a10-833">Uma matriz de parâmetros permite que argumentos sejam especificados de uma das duas maneiras em uma invocação de método:</span><span class="sxs-lookup"><span data-stu-id="c8a10-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="c8a10-834">O argumento fornecido para uma matriz de parâmetros pode ser uma única expressão conversível implicitamente ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="c8a10-835">Nesse caso, a matriz de parâmetros funciona precisamente como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="c8a10-836">Como alternativa, a invocação pode especificar zero ou mais argumentos para a matriz de parâmetros, em que cada argumento é uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="c8a10-837">Nesse caso, a invocação cria uma instância do tipo de matriz de parâmetros com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecidos e usa a instância de matriz recém-criada como o real argumento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="c8a10-838">Exceto para permitir um número variável de argumentos em uma invocação, uma matriz de parâmetros é precisamente equivalente a um parâmetro de valor ([parâmetros de valor](classes.md#value-parameters)) do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="c8a10-839">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-839">The example</span></span>
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
<span data-ttu-id="c8a10-840">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="c8a10-841">A primeira invocação de `F` simplesmente passa a matriz `a` como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="c8a10-842">A segunda invocação de `F` cria automaticamente um quatro elementos `int[]` com os valores de elemento fornecidos e passa essa instância de matriz como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="c8a10-843">Da mesma forma, a terceira invocação de `F` cria um elemento `int[]` zero e passa essa instância como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="c8a10-844">A segunda e terceira invocações são precisamente equivalentes a escrever:</span><span class="sxs-lookup"><span data-stu-id="c8a10-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="c8a10-845">Ao executar a resolução de sobrecarga, um método com uma matriz de parâmetros pode ser aplicável em seu formato normal ou em sua forma expandida ([membro de função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="c8a10-846">A forma expandida de um método só estará disponível se a forma normal do método não for aplicável e somente se um método aplicável com a mesma assinatura do formulário expandido ainda não estiver declarado no mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="c8a10-847">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-847">The example</span></span>
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
<span data-ttu-id="c8a10-848">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="c8a10-849">No exemplo, duas das possíveis formas expandidas do método com uma matriz de parâmetros já estão incluídas na classe como métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="c8a10-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="c8a10-850">Portanto, esses formulários expandidos não são considerados ao executar a resolução de sobrecarga e a primeira e terceira invocações de método, portanto, selecionam os métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="c8a10-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="c8a10-851">Quando uma classe declara um método com uma matriz de parâmetros, não é incomum também incluir alguns dos formulários expandidos como métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="c8a10-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="c8a10-852">Ao fazer isso, é possível evitar a alocação de uma instância de matriz que ocorre quando uma forma expandida de um método com uma matriz de parâmetros é invocada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="c8a10-853">Quando o tipo de uma matriz de parâmetros `object[]`é, ocorre uma possível ambiguidade entre a forma normal do método e o formulário informado para um único `object` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="c8a10-854">O motivo da ambiguidade é que um `object[]` é implicitamente conversível no tipo. `object`</span><span class="sxs-lookup"><span data-stu-id="c8a10-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="c8a10-855">No entanto, a ambiguidade não apresenta nenhum problema, pois ela pode ser resolvida com a inserção de uma conversão, se necessário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="c8a10-856">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-856">The example</span></span>
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
<span data-ttu-id="c8a10-857">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="c8a10-858">Na primeira e última invocações do `F`, a forma normal do `F` é aplicável porque existe uma conversão implícita do tipo de argumento para o tipo de parâmetro (ambos são do tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="c8a10-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="c8a10-859">Portanto, a resolução de sobrecarga seleciona a forma `F`normal de e o argumento é passado como um parâmetro de valor regular.</span><span class="sxs-lookup"><span data-stu-id="c8a10-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="c8a10-860">Na segunda e terceira invocações, a forma normal de `F` não é aplicável porque não existe nenhuma conversão implícita do tipo de argumento para o tipo de parâmetro (o tipo `object` não pode ser convertido implicitamente `object[]`no tipo).</span><span class="sxs-lookup"><span data-stu-id="c8a10-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="c8a10-861">No entanto, a forma `F` expandida do é aplicável, portanto, é selecionada pela resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="c8a10-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="c8a10-862">Como resultado, um elemento `object[]` One é criado pela invocação, e o único elemento da matriz é inicializado com o valor do argumento fornecido (que, por sua vez, é uma referência a um `object[]`).</span><span class="sxs-lookup"><span data-stu-id="c8a10-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="c8a10-863">Métodos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-863">Static and instance methods</span></span>

<span data-ttu-id="c8a10-864">Quando uma declaração de método inclui `static` um modificador, esse método é considerado um método estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="c8a10-865">Quando nenhum `static` modificador está presente, o método é considerado um método de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="c8a10-866">Um método estático não funciona em uma instância específica e é um erro de tempo de compilação para fazer referência a `this` um método estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="c8a10-867">Um método de instância opera em uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="c8a10-868">Quando um método é referenciado em um *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` for um método estático, `E` deverá indicar um tipo contendo `M` e, se `M` for um método de instância, `E` deverá indicar uma instância de um tipo contendo `M`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c8a10-869">As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c8a10-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="c8a10-870">Métodos virtuais</span><span class="sxs-lookup"><span data-stu-id="c8a10-870">Virtual methods</span></span>

<span data-ttu-id="c8a10-871">Quando uma declaração de método de instância `virtual` inclui um modificador, esse método é considerado um método virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="c8a10-872">Quando nenhum `virtual` modificador está presente, o método é considerado um método não virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="c8a10-873">A implementação de um método não virtual é invariável: A implementação é a mesma se o método é invocado em uma instância da classe na qual ela é declarada ou uma instância de uma classe derivada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="c8a10-874">Por outro lado, a implementação de um método virtual pode ser substituída por classes derivadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="c8a10-875">O processo de substituição da implementação de um método virtual herdado é conhecido como ***substituição*** desse método ([métodos de substituição](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="c8a10-876">Em uma invocação de método virtual, o ***tipo de tempo de execução*** da instância para o qual a invocação ocorre determina a implementação do método real a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="c8a10-877">Em uma invocação de método não virtual, o ***tipo de tempo de compilação*** da instância é o fator determinante.</span><span class="sxs-lookup"><span data-stu-id="c8a10-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="c8a10-878">Em termos precisos, quando um método `N` chamado é invocado com uma `A` lista de argumentos em uma instância com um tipo `C` de tempo de compilação e um `R` tipo de `R` tempo de `C` execução (onde é ou uma classe derivada de `C`), a invocação é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c8a10-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="c8a10-879">Primeiro, a resolução de sobrecarga é `C`aplicada `N`a, `A`e, para selecionar um método `M` específico do conjunto de métodos declarado em e herdado `C`por.</span><span class="sxs-lookup"><span data-stu-id="c8a10-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="c8a10-880">Isso é descrito em [invocações de método](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="c8a10-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="c8a10-881">Em seguida, `M` se for um método não virtual, `M` será invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="c8a10-882">Caso contrário `M` , é um método virtual e a implementação mais derivada de `M` com relação a `R` é invocada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="c8a10-883">Para cada método virtual declarado em ou herdado por uma classe, existe uma ***implementação mais derivada*** do método em relação a essa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="c8a10-884">A implementação mais derivada de um método `M` virtual em relação a uma classe `R` é determinada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c8a10-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="c8a10-885">Se `R` contiver a `virtual` declaração de `M`introdução de, essa será a implementação mais derivada `M`de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="c8a10-886">Caso contrário, `R` se contiver `M`um `override` de, essa será a implementação mais derivada `M`de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="c8a10-887">Caso contrário, a implementação mais derivada `M` de em relação `R` ao é igual à implementação mais derivada de `M` em relação à classe base direta do `R`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="c8a10-888">O exemplo a seguir ilustra as diferenças entre métodos virtuais e não virtuais:</span><span class="sxs-lookup"><span data-stu-id="c8a10-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="c8a10-889">No exemplo, `A` apresenta um método `F` não virtual e um método `G`virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="c8a10-890">A classe `B` introduz um novo método `F`não virtual, ocultando o herdado `F`e também substitui o método `G`herdado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="c8a10-891">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="c8a10-892">Observe que a instrução `a.G()` `B.G`invoca, não `A.G`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="c8a10-893">Isso ocorre porque o tipo de tempo de execução da instância (que é `B`), não o tipo de tempo de compilação da instância (que é `A`), determina a implementação do método real a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="c8a10-894">Como os métodos têm permissão para ocultar os métodos herdados, é possível que uma classe contenha vários métodos virtuais com a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="c8a10-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="c8a10-895">Isso não apresenta um problema de ambiguidade, pois todos, exceto o método mais derivado, estão ocultos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="c8a10-896">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-896">In the example</span></span>
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
<span data-ttu-id="c8a10-897">as `C` classes `D` e contêm dois métodos virtuais com a mesma assinatura: Aquele introduzido pelo `A` e aquele introduzido pelo `C`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="c8a10-898">O método introduzido por `C` oculta o método herdado de `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="c8a10-899">Assim, a declaração de substituição `D` em substitui o método introduzido `C`pelo, e não é possível `D` substituir o método introduzido pelo `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="c8a10-900">O exemplo produz a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="c8a10-901">Observe que é possível invocar o método virtual oculto acessando uma instância do `D` por meio de um tipo menos derivado no qual o método não está oculto.</span><span class="sxs-lookup"><span data-stu-id="c8a10-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="c8a10-902">Métodos de substituição</span><span class="sxs-lookup"><span data-stu-id="c8a10-902">Override methods</span></span>

<span data-ttu-id="c8a10-903">Quando uma declaração de método de instância `override` inclui um modificador, o método é considerado um ***método de substituição***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="c8a10-904">Um método override substitui um método virtual herdado pela mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="c8a10-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="c8a10-905">Enquanto uma declaração de método virtual apresenta um novo método, uma declaração de método de substituição restringe um método virtual herdado existente fornecendo uma nova implementação do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="c8a10-906">O método substituído por uma `override` declaração é conhecido como ***método base substituído***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="c8a10-907">Para um método `M` override declarado em uma classe `C`, o método base substituído é determinado examinando cada tipo de classe base `C`de, começando com o tipo de `C` classe base direta e continuando com cada sucesso o tipo de classe base direta, até em um determinado tipo de classe base, pelo menos um método acessível está localizado, que `M` tem a mesma assinatura que após a substituição dos argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="c8a10-908">Para fins de localização do método base substituído, um método é considerado acessível `public`se for `protected` `protected internal`, se for, se for, ou se `internal` for e declarado no mesmo programa que `C`o.</span><span class="sxs-lookup"><span data-stu-id="c8a10-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="c8a10-909">Um erro de tempo de compilação ocorre a menos que todos os itens a seguir sejam verdadeiros para uma declaração de substituição:</span><span class="sxs-lookup"><span data-stu-id="c8a10-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="c8a10-910">Um método base substituído pode ser localizado conforme descrito acima.</span><span class="sxs-lookup"><span data-stu-id="c8a10-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="c8a10-911">Há exatamente um desses métodos de base substituído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="c8a10-912">Essa restrição só terá efeito se o tipo de classe base for um tipo construído em que a substituição de argumentos de tipo torna a assinatura de dois métodos o mesmo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="c8a10-913">O método base substituído é um método virtual, abstract ou override.</span><span class="sxs-lookup"><span data-stu-id="c8a10-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="c8a10-914">Em outras palavras, o método base substituído não pode ser estático ou não virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="c8a10-915">O método base substituído não é um método lacrado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="c8a10-916">O método override e o método base substituído têm o mesmo tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="c8a10-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="c8a10-917">A declaração de substituição e o método base substituído têm a mesma acessibilidade declarada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="c8a10-918">Em outras palavras, uma declaração de substituição não pode alterar a acessibilidade do método virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="c8a10-919">No entanto, se o método base substituído for protegido internamente e for declarado em um assembly diferente do assembly que contém o método override, a acessibilidade declarada do método de substituição deverá ser protegida.</span><span class="sxs-lookup"><span data-stu-id="c8a10-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="c8a10-920">A declaração de substituição não especifica cláusulas de tipo-parâmetro-Constraints.</span><span class="sxs-lookup"><span data-stu-id="c8a10-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="c8a10-921">Em vez disso, as restrições são herdadas do método base substituído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="c8a10-922">Observe que as restrições que são parâmetros de tipo no método substituído podem ser substituídas por argumentos de tipo na restrição herdada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="c8a10-923">Isso pode levar a restrições que não são legais quando explicitamente especificados, como tipos de valor ou tipos lacrados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="c8a10-924">O exemplo a seguir demonstra como as regras de substituição funcionam para classes genéricas:</span><span class="sxs-lookup"><span data-stu-id="c8a10-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="c8a10-925">Uma declaração de substituição pode acessar o método base substituído usando um *base_access* ([acesso de base](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="c8a10-926">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-926">In the example</span></span>
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
<span data-ttu-id="c8a10-927">a `base.PrintFields()` invocação no `B` invoca o `PrintFields` método declarado em `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="c8a10-928">Um *base_access* desabilita o mecanismo de invocação virtual e simplesmente trata o método base como um método não virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="c8a10-929">A invocação `B` foi escrita `A` `PrintFields` `((A)this).PrintFields()`, ela invocaria recursivamente o método declarado em `B`, e não o declarado em, pois `PrintFields` é virtual e o tipo de tempo de execução de `((A)this)` é .`B`</span><span class="sxs-lookup"><span data-stu-id="c8a10-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="c8a10-930">Somente ao incluir um `override` modificador, um método pode substituir outro método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="c8a10-931">Em todos os outros casos, um método com a mesma assinatura que um método herdado simplesmente oculta o método herdado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="c8a10-932">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-932">In the example</span></span>
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
<span data-ttu-id="c8a10-933">o `F` método no `B` não inclui um `override` modificador e, portanto, não substitui `F` o método `A`em.</span><span class="sxs-lookup"><span data-stu-id="c8a10-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="c8a10-934">Em vez disso `F` , o `B` método em oculta o método `A`em e um aviso é relatado porque a declaração não inclui um `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="c8a10-935">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-935">In the example</span></span>
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
<span data-ttu-id="c8a10-936">o `F` método em `B` oculta o método virtual `F` herdado de `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="c8a10-937">Como o novo `F` no `B` tem acesso privado, seu escopo inclui apenas o corpo da classe `B` de e não se estende `C`para.</span><span class="sxs-lookup"><span data-stu-id="c8a10-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="c8a10-938">Portanto, a declaração de `F` no `C` tem permissão para substituir o `F` herdado `A`de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="c8a10-939">Métodos lacrados</span><span class="sxs-lookup"><span data-stu-id="c8a10-939">Sealed methods</span></span>

<span data-ttu-id="c8a10-940">Quando uma declaração de método de instância `sealed` inclui um modificador, esse método é considerado um ***método lacrado***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="c8a10-941">Se uma declaração de método de instância `sealed` incluir o modificador, ele também `override` deverá incluir o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="c8a10-942">O uso do `sealed` modificador impede que uma classe derivada substitua ainda mais o método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="c8a10-943">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-943">In the example</span></span>
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
<span data-ttu-id="c8a10-944">a classe `B` fornece dois métodos de substituição: `F` um método que tem `sealed` o modificador `G` e um método que não tem.</span><span class="sxs-lookup"><span data-stu-id="c8a10-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="c8a10-945">`B`o uso do `modifier` desealed `C` impede a substituição `F`adicional.</span><span class="sxs-lookup"><span data-stu-id="c8a10-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="c8a10-946">Métodos abstratos</span><span class="sxs-lookup"><span data-stu-id="c8a10-946">Abstract methods</span></span>

<span data-ttu-id="c8a10-947">Quando uma declaração de método de instância `abstract` inclui um modificador, esse método é considerado um ***método abstrato***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="c8a10-948">Embora um método abstract também seja implicitamente um método virtual, ele não pode ter o `virtual`modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="c8a10-949">Uma declaração de método abstract apresenta um novo método virtual, mas não fornece uma implementação desse método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="c8a10-950">Em vez disso, as classes derivadas não abstratas são necessárias para fornecer sua própria implementação substituindo esse método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="c8a10-951">Como um método abstract não fornece implementação real, o *method_body* de um método abstract simplesmente consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="c8a10-952">Declarações de método abstract só são permitidas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="c8a10-953">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-953">In the example</span></span>
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
<span data-ttu-id="c8a10-954">a `Shape` classe define a noção abstrata de um objeto de forma geométrica que pode ser pintada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="c8a10-955">O `Paint` método é abstrato porque não há nenhuma implementação padrão significativa.</span><span class="sxs-lookup"><span data-stu-id="c8a10-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="c8a10-956">As `Ellipse` classes `Box` e são implementações concretas `Shape` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="c8a10-957">Como essas classes não são abstratas, elas são necessárias para substituir o `Paint` método e fornecer uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="c8a10-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="c8a10-958">É um erro de tempo de compilação para um *base_access* ([acesso de base](expressions.md#base-access)) para referenciar um método abstrato.</span><span class="sxs-lookup"><span data-stu-id="c8a10-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="c8a10-959">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-959">In the example</span></span>
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
<span data-ttu-id="c8a10-960">um erro de tempo de compilação é relatado para `base.F()` a invocação porque faz referência a um método abstrato.</span><span class="sxs-lookup"><span data-stu-id="c8a10-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="c8a10-961">Uma declaração de método abstract tem permissão para substituir um método virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="c8a10-962">Isso permite que uma classe abstrata force a reimplementação do método em classes derivadas e torna a implementação original do método indisponível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="c8a10-963">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-963">In the example</span></span>
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
<span data-ttu-id="c8a10-964">classe `A` declara um método virtual, a classe `B` substitui esse método por um método abstrato e a classe `C` substitui o método abstract para fornecer sua própria implementação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="c8a10-965">Métodos externos</span><span class="sxs-lookup"><span data-stu-id="c8a10-965">External methods</span></span>

<span data-ttu-id="c8a10-966">Quando uma declaração de método inclui `extern` um modificador, esse método é considerado um ***método externo***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="c8a10-967">Os métodos externos são implementados externamente, normalmente usando uma linguagem C#diferente de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="c8a10-968">Como uma declaração de método externo não fornece implementação real, o *method_body* de um método externo simplesmente consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="c8a10-969">Um método externo não pode ser genérico.</span><span class="sxs-lookup"><span data-stu-id="c8a10-969">An external method may not be generic.</span></span>

<span data-ttu-id="c8a10-970">O `extern` modificador é normalmente usado em conjunto com `DllImport` um atributo ([interoperação com componentes com e Win32](attributes.md#interoperation-with-com-and-win32-components)), permitindo que métodos externos sejam implementados por DLLs (bibliotecas de vínculo dinâmico).</span><span class="sxs-lookup"><span data-stu-id="c8a10-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="c8a10-971">O ambiente de execução pode dar suporte a outros mecanismos nos quais implementações de métodos externos podem ser fornecidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="c8a10-972">Quando um método externo inclui um `DllImport` atributo, a declaração de método também deve incluir `static` um modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="c8a10-973">Este exemplo demonstra o uso do `extern` modificador e do `DllImport` atributo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="c8a10-974">Métodos parciais (recapitulação)</span><span class="sxs-lookup"><span data-stu-id="c8a10-974">Partial methods (recap)</span></span>

<span data-ttu-id="c8a10-975">Quando uma declaração de método inclui `partial` um modificador, esse método é considerado um ***método parcial***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="c8a10-976">Os métodos parciais só podem ser declarados como membros de tipos parciais ([tipos parciais](classes.md#partial-types)) e estão sujeitos a várias restrições.</span><span class="sxs-lookup"><span data-stu-id="c8a10-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="c8a10-977">Os métodos parciais são descritos mais detalhadamente em [métodos parciais](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="c8a10-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="c8a10-978">Métodos de extensão</span><span class="sxs-lookup"><span data-stu-id="c8a10-978">Extension methods</span></span>

<span data-ttu-id="c8a10-979">Quando o primeiro parâmetro de um método inclui o `this` modificador, esse método é considerado um ***método de extensão***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="c8a10-980">Os métodos de extensão só podem ser declarados em classes estáticas não genéricas e não aninhadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="c8a10-981">O primeiro parâmetro de um método de extensão não pode ter nenhum modificador `this`diferente de e o tipo de parâmetro não pode ser um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="c8a10-982">Veja a seguir um exemplo de uma classe estática que declara dois métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="c8a10-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="c8a10-983">Um método de extensão é um método estático regular.</span><span class="sxs-lookup"><span data-stu-id="c8a10-983">An extension method is a regular static method.</span></span> <span data-ttu-id="c8a10-984">Além disso, onde sua classe estática delimitadora está no escopo, um método de extensão pode ser invocado usando a sintaxe de invocação do método de instância ([invocações de método de extensão](expressions.md#extension-method-invocations)), usando a expressão Receiver como o primeiro argumento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="c8a10-985">O programa a seguir usa os métodos de extensão declarados acima:</span><span class="sxs-lookup"><span data-stu-id="c8a10-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="c8a10-986">O `Slice` método está disponível `string[]`no, e o `ToInt32` método está disponível em `string`, porque eles foram declarados como métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="c8a10-987">O significado do programa é o mesmo que o seguinte, usando chamadas de método estáticos comuns:</span><span class="sxs-lookup"><span data-stu-id="c8a10-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="c8a10-988">Corpo do método</span><span class="sxs-lookup"><span data-stu-id="c8a10-988">Method body</span></span>

<span data-ttu-id="c8a10-989">O *method_body* de uma declaração de método consiste em um corpo de bloco, um corpo de expressão ou um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="c8a10-990">O ***tipo de resultado*** de um método `void` é se o tipo de `void`retorno for ou se o método for Async e o tipo de `System.Threading.Tasks.Task`retorno for.</span><span class="sxs-lookup"><span data-stu-id="c8a10-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="c8a10-991">Caso contrário, o tipo de resultado de um método não assíncrono é seu tipo de retorno, e o tipo de resultado de um método assíncrono `System.Threading.Tasks.Task<T>` com `T`tipo de retorno é.</span><span class="sxs-lookup"><span data-stu-id="c8a10-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="c8a10-992">Quando um método tem um `void` tipo de resultado e um corpo de `return` bloco,[as instruções (a instrução return](statements.md#the-return-statement)) no bloco não têm permissão para especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="c8a10-993">Se a execução do bloco de um método void for concluída normalmente (ou seja, o controle flui para fora do final do corpo do método), esse método simplesmente retorna ao seu chamador atual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="c8a10-994">Quando um método tem um resultado `void` e um corpo de expressão, a expressão `E` deve ser um *statement_expression*, e o corpo é exatamente equivalente a um corpo de bloco do formulário `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="c8a10-995">Quando um método tem um tipo de resultado não void e um corpo de bloco, `return` cada instrução no bloco deve especificar uma expressão que é implicitamente conversível para o tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="c8a10-996">O ponto de extremidade de um corpo de bloco de um método de retorno de valor não deve ser acessível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="c8a10-997">Em outras palavras, em um método de retorno de valor com um corpo de bloco, o controle não tem permissão para fluir para fora do final do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="c8a10-998">Quando um método tem um tipo de resultado não void e um corpo de expressão, a expressão deve ser conversível implicitamente no tipo de resultado e o corpo é exatamente equivalente a um corpo de bloco do `{ return E; }`formulário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="c8a10-999">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-999">In the example</span></span>
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
<span data-ttu-id="c8a10-1000">o método de retorno `F` de valor resulta em um erro de tempo de compilação porque o controle pode fluir para fora do final do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="c8a10-1001">Os `G` métodos `H` e estão corretos porque todos os caminhos de execução possíveis terminam em uma instrução Return que especifica um valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="c8a10-1002">O `I` método está correto, pois seu corpo é equivalente a um bloco de instrução com apenas uma única instrução de retorno.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="c8a10-1003">Sobrecarga de método</span><span class="sxs-lookup"><span data-stu-id="c8a10-1003">Method overloading</span></span>

<span data-ttu-id="c8a10-1004">As regras de resolução de sobrecarga de método são descritas em [inferência de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="c8a10-1005">Properties</span><span class="sxs-lookup"><span data-stu-id="c8a10-1005">Properties</span></span>

<span data-ttu-id="c8a10-1006">Uma ***Propriedade*** é um membro que fornece acesso a uma característica de um objeto ou de uma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="c8a10-1007">Exemplos de propriedades incluem o comprimento de uma cadeia de caracteres, o tamanho de uma fonte, a legenda de uma janela, o nome de um cliente e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="c8a10-1008">As propriedades são uma extensão natural dos campos – ambos são membros nomeados com tipos associados e a sintaxe para acessar campos e propriedades é a mesma.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="c8a10-1009">No entanto, diferentemente dos campos, as propriedades não denotam locais de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="c8a10-1010">Em vez disso, as propriedades têm ***acessadores*** que especificam as instruções a serem executadas quando os valores forem lidos ou gravados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="c8a10-1011">Portanto, as propriedades fornecem um mecanismo para associar ações com a leitura e gravação de atributos de um objeto; Além disso, eles permitem que esses atributos sejam computados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="c8a10-1012">As propriedades são declaradas usando *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-1013">Um *property_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([estático e de instância métodos](classes.md#static-and-instance-methods)), o `virtual` ([métodos virtuais](classes.md#virtual-methods)), os 0 ([métodos de substituição](classes.md#override-methods)), os modificadores 2 ([métodos lacrados](classes.md#sealed-methods)), 4 ([métodos abstratos](classes.md#abstract-methods)) e 6 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c8a10-1014">As declarações de propriedade estão sujeitas às mesmas regras que as declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="c8a10-1015">O *tipo* de uma declaração de propriedade especifica o tipo da propriedade introduzida pela declaração e *member_name* especifica o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="c8a10-1016">A menos que a propriedade seja uma implementação de membro de interface explícita, o *member_name* é simplesmente um *identificador*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="c8a10-1017">Para uma implementação explícita de membro de interface ([implementações explícitas de membro de interface](interfaces.md#explicit-interface-member-implementations)), o *member_name* consiste em um *interface_type* seguido por um "`.`" e um *identificador*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="c8a10-1018">O *tipo* de uma propriedade deve ser pelo menos tão acessível quanto a própria propriedade ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-1019">Um *property_body* pode consistir em um ***corpo de acessador*** ou um ***corpo de expressão***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="c8a10-1020">Em um corpo de acessador, *accessor_declarations*, que deve ser incluído nos tokens "`{`" e "`}`", declare os acessadores ([acessadores](classes.md#accessors)) da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="c8a10-1021">Os acessadores especificam as instruções Executáveis associadas à leitura e gravação da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="c8a10-1022">Um corpo de `=>` expressão que consiste em seguido por uma *expressão* `E` e um ponto-e-vírgula é exatamente equivalente ao corpo `{ get { return E; } }`da instrução e, portanto, só pode ser usado para especificar propriedades somente getter em que o resultado de o getter é fornecido por uma única expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="c8a10-1023">Um *property_initializer* só pode ser fornecido para uma propriedade implementada automaticamente ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)) e causa a inicialização do campo subjacente de tais propriedades com o valor fornecido pela *expressão* .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="c8a10-1024">Embora a sintaxe para acessar uma propriedade seja a mesma de um campo, uma propriedade não é classificada como uma variável.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="c8a10-1025">Portanto, não é possível passar uma propriedade como um `ref` argumento ou. `out`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="c8a10-1026">Quando uma declaração de propriedade inclui `extern` um modificador, a propriedade é considerada uma ***Propriedade externa***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="c8a10-1027">Como uma declaração de propriedade externa não fornece implementação real, cada uma de suas *accessor_declarations* consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="c8a10-1028">Propriedades de instância e estática</span><span class="sxs-lookup"><span data-stu-id="c8a10-1028">Static and instance properties</span></span>

<span data-ttu-id="c8a10-1029">Quando uma declaração de propriedade inclui `static` um modificador, a propriedade é considerada uma ***propriedade estática***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="c8a10-1030">Quando nenhum `static` modificador estiver presente, a propriedade será considerada uma ***propriedade de instância***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="c8a10-1031">Uma propriedade estática não está associada a uma instância específica e é um erro de tempo de compilação para se referir `this` aos acessadores de uma propriedade estática.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="c8a10-1032">Uma propriedade de instância é associada a uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores dessa propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="c8a10-1033">Quando uma propriedade é referenciada em um *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` for uma propriedade estática, `E` deverá indicar um tipo contendo `M` e se `M` for uma propriedade de instância, e deverá denotar uma instância de um tipo contendo `M`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c8a10-1034">As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="c8a10-1035">Acessadores</span><span class="sxs-lookup"><span data-stu-id="c8a10-1035">Accessors</span></span>

<span data-ttu-id="c8a10-1036">O *accessor_declarations* de uma propriedade especifica as instruções Executáveis associadas à leitura e gravação dessa propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="c8a10-1037">As declarações de acessador consistem em um *get_accessor_declaration*, um *set_accessor_declaration*ou ambos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="c8a10-1038">Cada declaração de acessador consiste no token `get` ou `set` seguido por um *accessor_modifier* opcional e um *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="c8a10-1039">O uso de *accessor_modifier*s é regido pelas seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="c8a10-1040">Um *accessor_modifier* não pode ser usado em uma interface ou em uma implementação de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="c8a10-1041">Para uma propriedade ou um indexador que não tem um modificador `override`, um *accessor_modifier* será permitido somente se a propriedade ou o indexador tiver um acessador `get` e `set` e, em seguida, for permitido somente em um desses acessadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="c8a10-1042">Para uma propriedade ou um indexador que inclui um modificador `override`, um acessador deve corresponder ao *accessor_modifier*, se houver, do acessador que está sendo substituído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="c8a10-1043">O *accessor_modifier* deve declarar uma acessibilidade que seja estritamente mais restritiva do que a acessibilidade declarada da propriedade ou do indexador em si.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="c8a10-1044">Para ser preciso:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1044">To be precise:</span></span>
   * <span data-ttu-id="c8a10-1045">Se a propriedade ou o indexador tiver uma acessibilidade declarada de `public`, o *accessor_modifier* poderá ser `protected internal`, `internal`, `protected` ou `private`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="c8a10-1046">Se a propriedade ou o indexador tiver uma acessibilidade declarada de `protected internal`, o *accessor_modifier* poderá ser `internal`, `protected` ou `private`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="c8a10-1047">Se a propriedade ou o indexador tiver uma acessibilidade declarada de `internal` ou `protected`, o *accessor_modifier* deverá ser `private`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="c8a10-1048">Se a propriedade ou o indexador tiver uma acessibilidade declarada de `private`, nenhum *accessor_modifier* poderá ser usado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="c8a10-1049">Para as propriedades `abstract` e `extern`, o *accessor_body* para cada acessador especificado é simplesmente um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="c8a10-1050">Uma propriedade não abstrata que não seja externa pode ter cada *accessor_body* um ponto-e-vírgula; nesse caso, é uma ***propriedade implementada automaticamente*** ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="c8a10-1051">Uma propriedade implementada automaticamente deve ter pelo menos um acessador get.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="c8a10-1052">Para os acessadores de qualquer outra propriedade não abstrata e não externa, o *accessor_body* é um *bloco* que especifica as instruções a serem executadas quando o acessador correspondente é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="c8a10-1053">Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="c8a10-1054">Exceto como o destino de uma atribuição, quando uma propriedade é referenciada em uma expressão, `get` o acessador da propriedade é invocado para calcular o valor da propriedade ([valores de expressões](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="c8a10-1055">O corpo de um `get` acessador deve estar em conformidade com as regras para métodos de retorno de valor descritos no [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c8a10-1056">Em particular, todas `return` as instruções no corpo de um `get` acessador devem especificar uma expressão que é implicitamente conversível para o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="c8a10-1057">Além disso, o ponto de `get` extremidade de um acessador não deve ser acessível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="c8a10-1058">Um `set` acessador corresponde a um método com um parâmetro de valor único do tipo de `void` Propriedade e um tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="c8a10-1059">O parâmetro implícito de um `set` acessador é `value`sempre denominado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="c8a10-1060">Quando uma propriedade é referenciada como o destino de uma atribuição[(operadores de atribuição](expressions.md#assignment-operators)) ou como o operando `++` de `--` ou (operadores de[incremento de sufixo e decréscimo](expressions.md#postfix-increment-and-decrement-operators), [incremento de prefixo e diminuir os operadores](expressions.md#prefix-increment-and-decrement-operators)), o acessador é invocado com um argumento (cujo valor é o lado direito da atribuição ou o operando `++` do operador or `--` ) que fornece o novo valor ([atribuição simples](expressions.md#simple-assignment)). `set`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="c8a10-1061">O corpo de um `set` acessador deve estar em conformidade `void` com as regras para métodos descritos no [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c8a10-1062">Em particular, `return` as instruções no `set` corpo do acessador não têm permissão para especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="c8a10-1063">Como um `set` acessador implicitamente tem um `value`parâmetro chamado, ele é um erro de tempo de compilação para uma variável local ou uma `set` declaração de constante em um acessador para ter esse nome.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="c8a10-1064">Com base na presença ou ausência de `get` `set` acessadores, uma propriedade é classificada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="c8a10-1065">Uma propriedade que inclui um `get` acessador e um `set` acessador é considerada uma propriedade de ***leitura/gravação*** .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="c8a10-1066">Uma propriedade que tem apenas um `get` acessador é considerada uma propriedade ***somente leitura*** .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="c8a10-1067">É um erro de tempo de compilação para uma propriedade somente leitura ser o destino de uma atribuição.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="c8a10-1068">Uma propriedade que tem apenas um `set` acessador é considerada uma propriedade ***somente gravação*** .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="c8a10-1069">Exceto como o destino de uma atribuição, é um erro de tempo de compilação para referenciar uma propriedade somente gravação em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="c8a10-1070">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1070">In the example</span></span>
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
<span data-ttu-id="c8a10-1071">o `Button` controle declara uma propriedade pública `Caption` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="c8a10-1072">O `get` acessador `Caption` da propriedade retorna a cadeia de caracteres armazenada no campo particular `caption` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="c8a10-1073">O `set` acessador verifica se o novo valor é diferente do valor atual e, nesse caso, armazena o novo valor e redesenha o controle.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="c8a10-1074">As propriedades geralmente seguem o padrão mostrado acima: O `get` acessador simplesmente retorna um valor armazenado em um campo particular, `set` e o acessador modifica esse campo particular e, em seguida, executa quaisquer ações adicionais necessárias para atualizar totalmente o estado do objeto.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="c8a10-1075">Considerando a `Button` classe acima, veja a seguir um exemplo de uso `Caption` da propriedade:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="c8a10-1076">Aqui, o `set` acessador é invocado atribuindo um valor à propriedade, e o `get` acessador é invocado referenciando a propriedade em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="c8a10-1077">O `get` e`set` os acessadores de uma propriedade não são membros distintos e não é possível declarar os acessadores de uma propriedade separadamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="c8a10-1078">Assim, não é possível que os dois acessadores de uma propriedade de leitura/gravação tenham acessibilidade diferente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="c8a10-1079">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1079">The example</span></span>
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
<span data-ttu-id="c8a10-1080">não declara uma única propriedade de leitura/gravação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="c8a10-1081">Em vez disso, ele declara duas propriedades com o mesmo nome, uma somente leitura e uma somente gravação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="c8a10-1082">Como dois membros declarados na mesma classe não podem ter o mesmo nome, o exemplo causa a ocorrência de um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="c8a10-1083">Quando uma classe derivada declara uma propriedade com o mesmo nome de uma propriedade herdada, a propriedade derivada oculta a propriedade herdada em relação à leitura e à gravação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="c8a10-1084">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1084">In the example</span></span>
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
<span data-ttu-id="c8a10-1085">a `P` Propriedade em `B` oculta a `P` Propriedade em `A` com relação à leitura e gravação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="c8a10-1086">Portanto, nas instruções</span><span class="sxs-lookup"><span data-stu-id="c8a10-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="c8a10-1087">a atribuição para `b.P` causa a reportação de um erro em tempo de compilação, pois a `P` propriedade somente `B` leitura no oculta a propriedade somente `P` gravação no `A`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="c8a10-1088">Observe, no entanto, que uma conversão pode ser usada para acessar `P` a Propriedade Hidden.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="c8a10-1089">Ao contrário dos campos públicos, as propriedades fornecem uma separação entre o estado interno de um objeto e sua interface pública.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="c8a10-1090">Considere o exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1090">Consider the example:</span></span>
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

<span data-ttu-id="c8a10-1091">Aqui, a `Label` classe usa dois `int` campos `x` e `y`, para armazenar seu local.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="c8a10-1092">O local é exposto `X` publicamente como um e uma `Y` Propriedade e como uma `Location` Propriedade do tipo `Point`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="c8a10-1093">Se, em uma versão futura do `Label`, for mais conveniente armazenar o local como um `Point` internamente, a alteração poderá ser feita sem afetar a interface pública da classe:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="c8a10-1094">Tinha `x` `public readonly` `Label` e `y` , em vez de campos, seria impossível fazer essa alteração na classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="c8a10-1095">Expor o estado por meio de propriedades não é necessariamente qualquer menos eficiente do que expor os campos diretamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="c8a10-1096">Em particular, quando uma propriedade é não virtual e contém apenas uma pequena quantidade de código, o ambiente de execução pode substituir chamadas para acessadores pelo código real dos acessadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="c8a10-1097">Esse processo é conhecido como ***inlining***e torna o acesso à propriedade tão eficiente quanto o acesso ao campo, mas preserva a maior flexibilidade das propriedades.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="c8a10-1098">Como invocar um `get` acessador é conceitualmente equivalente a ler o valor de um campo, ele é considerado um estilo `get` de programação insatisfatório para que os acessadores tenham efeitos colaterais observáveis.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="c8a10-1099">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="c8a10-1100">o valor da `Next` Propriedade depende do número de vezes que a propriedade foi acessada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="c8a10-1101">Portanto, o acesso à propriedade produz um efeito colateral observável, e a propriedade deve ser implementada como um método em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="c8a10-1102">A Convenção "sem efeitos colaterais" para `get` acessadores não significa `get` que os acessadores sempre devem ser escritos para simplesmente retornar valores armazenados em campos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="c8a10-1103">Na verdade `get` , os acessadores geralmente calculam o valor de uma propriedade acessando vários campos ou invocando métodos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="c8a10-1104">No entanto, um `get` acessador projetado corretamente não executa nenhuma ação que cause alterações observáveis no estado do objeto.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="c8a10-1105">As propriedades podem ser usadas para atrasar a inicialização de um recurso até o momento em que ele é referenciado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="c8a10-1106">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1106">For example:</span></span>
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

<span data-ttu-id="c8a10-1107">A `Console` classe contém três propriedades `Out`, `In`, e `Error`, que representam os dispositivos de entrada, saída e erro padrão, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="c8a10-1108">Ao expor esses membros como propriedades, a `Console` classe pode atrasar a inicialização até que elas sejam realmente usadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="c8a10-1109">Por exemplo, após a primeira referência `Out` à propriedade, como em</span><span class="sxs-lookup"><span data-stu-id="c8a10-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="c8a10-1110">o subjacente `TextWriter` para o dispositivo de saída é criado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="c8a10-1111">Mas se o aplicativo não fizer nenhuma referência às `In` propriedades `Error` e, nenhum objeto será criado para esses dispositivos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="c8a10-1112">Propriedades implementadas automaticamente</span><span class="sxs-lookup"><span data-stu-id="c8a10-1112">Automatically implemented properties</span></span>

<span data-ttu-id="c8a10-1113">Uma propriedade implementada automaticamente (ou ***Propriedade automática*** para curto), é uma propriedade não abstrata não abstraida com corpos de acessadores somente de ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="c8a10-1114">As propriedades automáticas devem ter um acessador get e, opcionalmente, podem ter um acessador set.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="c8a10-1115">Quando uma propriedade é especificada como uma propriedade implementada automaticamente, um campo de apoio oculto fica automaticamente disponível para a propriedade e os acessadores são implementados para ler e gravar nesse campo de backup.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="c8a10-1116">Se a propriedade automática não tiver nenhum acessador set, o campo de backup `readonly` será considerado ([campos somente leitura](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="c8a10-1117">Assim como um `readonly` campo, uma propriedade automática somente getter também pode ser atribuída ao corpo de um construtor da classe delimitadora.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="c8a10-1118">Tal atribuição atribui diretamente ao campo de apoio somente leitura da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="c8a10-1119">Uma propriedade automática pode, opcionalmente, ter um *property_initializer*, que é aplicado diretamente ao campo de backup como um *variable_initializer* ([inicializadores de variável](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="c8a10-1120">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="c8a10-1121">é equivalente à declaração a seguir:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="c8a10-1122">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="c8a10-1123">é equivalente à declaração a seguir:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="c8a10-1124">Observe que as atribuições para o campo ReadOnly são legais, pois elas ocorrem dentro do construtor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="c8a10-1125">Acessibilidade</span><span class="sxs-lookup"><span data-stu-id="c8a10-1125">Accessibility</span></span>

<span data-ttu-id="c8a10-1126">Se um acessador tiver um *accessor_modifier*, o domínio de acessibilidade ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)) do acessador será determinado usando a acessibilidade declarada do *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="c8a10-1127">Se um acessador não tiver um *accessor_modifier*, o domínio de acessibilidade do acessador será determinado da acessibilidade declarada da propriedade ou do indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="c8a10-1128">A presença de um *accessor_modifier* nunca afeta a pesquisa de Membros ([operadores](expressions.md#operators)) ou a resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="c8a10-1129">Os modificadores na propriedade ou no indexador sempre determinam a qual propriedade ou indexador está associado, independentemente do contexto do acesso.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="c8a10-1130">Depois que uma determinada propriedade ou um indexador tiver sido selecionado, os domínios de acessibilidade dos acessadores específicos envolvidos serão usados para determinar se esse uso é válido:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="c8a10-1131">Se o uso for como um valor ([valores de expressões](expressions.md#values-of-expressions)), o `get` acessador deverá existir e estar acessível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="c8a10-1132">Se o uso for como o destino de uma atribuição simples ([atribuição simples](expressions.md#simple-assignment)), o `set` acessador deverá existir e estar acessível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="c8a10-1133">Se o uso for como o destino da atribuição composta ([atribuição composta](expressions.md#compound-assignment)) ou como o destino dos `++` operadores ou `--` (membros de[função](expressions.md#function-members). 9, expressões de `get` [invocação](expressions.md#invocation-expressions)), os acessadores e o `set` acessador deve existir e estar acessível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="c8a10-1134">No exemplo a seguir, a propriedade `A.Text` é ocultada pela propriedade `B.Text`, mesmo em contextos onde apenas o `set` acessador é chamado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="c8a10-1135">Por outro lado, a `B.Count` propriedade não é acessível para `M`classe, portanto, a `A.Count` Propriedade acessível é usada em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="c8a10-1136">Um acessador que é usado para implementar uma interface pode não ter um *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="c8a10-1137">Se apenas um acessador for usado para implementar uma interface, o outro acessador poderá ser declarado com um *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="c8a10-1138">Acessadores de propriedade virtual, sealed, override e abstract</span><span class="sxs-lookup"><span data-stu-id="c8a10-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="c8a10-1139">Uma `virtual` declaração de propriedade especifica que os acessadores da propriedade são virtuais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="c8a10-1140">O `virtual` modificador se aplica a ambos os acessadores de uma propriedade de leitura/gravação — não é possível que apenas um acessador de uma propriedade de leitura/gravação seja virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="c8a10-1141">Uma `abstract` declaração de propriedade especifica que os acessadores da propriedade são virtuais, mas não fornecem uma implementação real dos acessadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="c8a10-1142">Em vez disso, as classes derivadas não abstratas são necessárias para fornecer sua própria implementação para os acessadores, substituindo a propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="c8a10-1143">Como um acessador de uma declaração de propriedade abstrata não fornece implementação real, seu *accessor_body* simplesmente consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="c8a10-1144">Uma declaração de propriedade que inclui os `abstract` modificadores e `override` especifica que a propriedade é abstrata e substitui uma propriedade base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="c8a10-1145">Os acessadores de tal propriedade também são abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="c8a10-1146">As declarações de propriedade abstratas só são permitidas em classes abstratas ([classes abstratas](classes.md#abstract-classes)). Os acessadores de uma propriedade virtual herdada podem ser substituídos em uma classe derivada, incluindo uma declaração de `override` propriedade que especifica uma diretiva.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="c8a10-1147">Isso é conhecido como uma ***declaração de propriedade de substituição***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="c8a10-1148">Uma declaração de propriedade de substituição não declara uma nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="c8a10-1149">Em vez disso, ele simplesmente especializa as implementações dos acessadores de uma propriedade virtual existente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="c8a10-1150">Uma declaração de propriedade de substituição deve especificar exatamente os mesmos modificadores de acessibilidade, tipo e nome que a propriedade herdada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="c8a10-1151">Se a propriedade Inherited tiver apenas um único acessador (ou seja, se a propriedade herdada for somente leitura ou somente gravação), a propriedade de substituição deverá incluir somente esse acessador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="c8a10-1152">Se a propriedade Inherited incluir os acessadores (ou seja, se a propriedade herdada for Read-Write), a propriedade de substituição poderá incluir um único acessador ou ambos os acessadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="c8a10-1153">Uma declaração de propriedade de substituição pode `sealed` incluir o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="c8a10-1154">O uso desse modificador impede que uma classe derivada substitua ainda mais a propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="c8a10-1155">Os acessadores de uma propriedade selada também são lacrados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="c8a10-1156">Exceto pelas diferenças na sintaxe de declaração e invocação, os acessadores virtual, sealed, override e abstract se comportam exatamente como métodos virtuais, lacrados, de substituição e abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="c8a10-1157">Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [métodos de substituição](classes.md#override-methods), métodos [lacrados](classes.md#sealed-methods)e [métodos abstratos](classes.md#abstract-methods) se aplicam como se os acessadores fossem métodos de um formulário correspondente:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="c8a10-1158">Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade e os mesmos modificadores que a propriedade recipiente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="c8a10-1159">Um `set` acessador corresponde a um método com um parâmetro de valor único do tipo de `void` Propriedade, um tipo de retorno e os mesmos modificadores que a propriedade recipiente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="c8a10-1160">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1160">In the example</span></span>
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
<span data-ttu-id="c8a10-1161">`X`é uma propriedade somente leitura virtual, `Y` é uma propriedade de leitura/gravação virtual e `Z` é uma propriedade de leitura/gravação abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="c8a10-1162">Como `Z` é abstrato, a classe `A` recipiente também deve ser declarada abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="c8a10-1163">Uma classe derivada de `A` é mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="c8a10-1164">Aqui, as declarações de `X`, `Y`e `Z` estão substituindo declarações de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="c8a10-1165">Cada declaração de propriedade corresponde exatamente aos modificadores de acessibilidade, ao tipo e ao nome da propriedade herdada correspondente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="c8a10-1166">O `get` acessador do `set` `X` e o `Y` acessador do usam a `base` palavra-chave para acessar os acessadores herdados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="c8a10-1167">A declaração de `Z` substitui ambos os acessadores abstratos – portanto, não há nenhum membro de `B`função abstrata `B` pendente no e é permitido ser uma classe não abstrata.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="c8a10-1168">Quando uma propriedade é declarada `override`como um, todos os acessadores substituídos devem ser acessíveis para o código de substituição.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="c8a10-1169">Além disso, a acessibilidade declarada da propriedade ou do indexador em si, e dos acessadores, deve corresponder à do membro e aos acessadores substituídos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="c8a10-1170">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="c8a10-1171">Events</span><span class="sxs-lookup"><span data-stu-id="c8a10-1171">Events</span></span>

<span data-ttu-id="c8a10-1172">Um ***evento*** é um membro que permite que um objeto ou classe forneça notificações.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="c8a10-1173">Os clientes podem anexar código executável para eventos fornecendo ***manipuladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="c8a10-1174">Os eventos são declarados usando *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-1175">Um *event_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([estático e de instância métodos](classes.md#static-and-instance-methods)), o `virtual` ([métodos virtuais](classes.md#virtual-methods)), os 0 ([métodos de substituição](classes.md#override-methods)), os modificadores 2 ([métodos lacrados](classes.md#sealed-methods)), 4 ([métodos abstratos](classes.md#abstract-methods)) e 6 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c8a10-1176">As declarações de evento estão sujeitas às mesmas regras que as declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="c8a10-1177">O *tipo* de uma declaração de evento deve ser um *delegate_type* ([tipos de referência](types.md#reference-types)) e esse *delegate_type* deve ser pelo menos acessível como o próprio evento ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-1178">Uma declaração de evento pode incluir *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="c8a10-1179">No entanto, se não for, para eventos não-extern e não abstratos, o compilador fornecerá esses itens automaticamente ([eventos semelhantes a campo](classes.md#field-like-events)); para eventos extern, os acessadores são fornecidos externamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="c8a10-1180">Uma declaração de evento que omite *event_accessor_declarations* define um ou mais eventos, um para cada um dos *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="c8a10-1181">Os atributos e os modificadores se aplicam a todos os membros declarados por tal *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="c8a10-1182">É um erro de tempo de compilação para um *event_declaration* incluir o modificador `abstract` e *event_accessor_declarations*delimitado por chave.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="c8a10-1183">Quando uma declaração de evento inclui `extern` um modificador, o evento é considerado um ***evento externo***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="c8a10-1184">Como uma declaração de evento externo não fornece implementação real, é um erro para que ela inclua o modificador `extern` e *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="c8a10-1185">É um erro de tempo de compilação para um *variable_declarator* de uma declaração de evento com um modificador `abstract` ou `external` para incluir um *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="c8a10-1186">Um evento pode ser usado como o operando esquerdo dos operadores `+=` e `-=` (atribuição de[evento](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="c8a10-1187">Esses operadores são usados, respectivamente, para anexar manipuladores de eventos ou para remover manipuladores de eventos de um evento, e os modificadores de acesso do evento controlam os contextos nos quais essas operações são permitidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="c8a10-1188">Como `+=` e`-=` são as únicas operações que são permitidas em um evento fora do tipo que declara o evento, o código externo pode adicionar e remover manipuladores para um evento, mas não pode obter ou modificar a lista subjacente de eventos manipuladores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="c8a10-1189">Em uma operação do formulário `x += y` ou `x -= y`, quando `x` é um evento e a referência ocorre fora do tipo que contém a declaração de `x`, o resultado da operação tem o tipo `void` (em vez de ter o tipo de `x`, com o valor de `x` após a atribuição).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="c8a10-1190">Essa regra proíbe que o código externo examine indiretamente o delegado subjacente de um evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="c8a10-1191">O exemplo a seguir mostra como os manipuladores de eventos são anexados `Button` a instâncias da classe:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="c8a10-1192">Aqui, o `LoginDialog` Construtor de instância cria `Button` duas instâncias e anexa manipuladores de `Click` eventos aos eventos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="c8a10-1193">Eventos do tipo campo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1193">Field-like events</span></span>

<span data-ttu-id="c8a10-1194">Dentro do texto do programa da classe ou struct que contém a declaração de um evento, determinados eventos podem ser usados como campos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="c8a10-1195">Para ser usado dessa forma, um evento não deve ser `abstract` ou `extern` e não deve incluir explicitamente *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="c8a10-1196">Tal evento pode ser usado em qualquer contexto que permita um campo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="c8a10-1197">O campo contém um delegado ([delegados](delegates.md)) que se refere à lista de manipuladores de eventos que foram adicionados ao evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="c8a10-1198">Se nenhum manipulador de eventos tiver sido adicionado, o campo conterá `null`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="c8a10-1199">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1199">In the example</span></span>
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
<span data-ttu-id="c8a10-1200">`Click`é usado como um campo dentro da `Button` classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="c8a10-1201">Como demonstra o exemplo, o campo pode ser examinado, modificado e usado em expressões de invocação de delegado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="c8a10-1202">O `OnClick` método `Click` na classe "gera" o evento. `Button`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="c8a10-1203">A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento — assim, não há constructos de linguagem especial para gerar eventos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="c8a10-1204">Observe que a invocação de delegado é precedida por uma verificação que garante que o delegado não seja nulo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="c8a10-1205">`Button` Fora da declaração da classe, o `Click` membro só pode ser usado no lado esquerdo dos `+=` operadores e `-=` , como em</span><span class="sxs-lookup"><span data-stu-id="c8a10-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="c8a10-1206">que acrescenta um delegado à lista de invocação do `Click` evento e</span><span class="sxs-lookup"><span data-stu-id="c8a10-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="c8a10-1207">que remove um delegado da lista de invocação do `Click` evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="c8a10-1208">Ao compilar um evento do tipo campo, o compilador cria automaticamente o armazenamento para manter o delegado e cria acessadores para o evento que adiciona ou remove manipuladores de eventos ao campo delegado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="c8a10-1209">As operações de adição e remoção são thread-safe e podem (mas não são necessárias) serem realizadas enquanto mantém o bloqueio ([a instrução Lock](statements.md#the-lock-statement)) no objeto recipiente para um evento de instância ou o tipo Object ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) para um evento estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="c8a10-1210">Assim, uma declaração de evento de instância do formulário:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="c8a10-1211">será compilado para algo equivalente a:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="c8a10-1212">Dentro da classe `X`, referências à `Ev` no lado esquerdo dos `+=` operadores e `-=` fazem com que os acessadores adicionar e remover sejam invocados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="c8a10-1213">Todas as outras referências `Ev` a são compiladas para referenciar `__Ev` o campo oculto em vez disso ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="c8a10-1214">O nome "`__Ev`" é arbitrário; o campo oculto pode ter qualquer nome ou nenhum nome.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="c8a10-1215">Acessadores de eventos</span><span class="sxs-lookup"><span data-stu-id="c8a10-1215">Event accessors</span></span>

<span data-ttu-id="c8a10-1216">As declarações de evento normalmente omitem *event_accessor_declarations*, como no exemplo `Button` acima.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="c8a10-1217">Uma situação para isso envolve o caso em que o custo de armazenamento de um campo por evento não é aceitável.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="c8a10-1218">Nesses casos, uma classe pode incluir *event_accessor_declarations* e usar um mecanismo privado para armazenar a lista de manipuladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="c8a10-1219">O *event_accessor_declarations* de um evento especifica as instruções Executáveis associadas à adição e remoção de manipuladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="c8a10-1220">As declarações de acessador consistem em um *add_accessor_declaration* e um *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="c8a10-1221">Cada declaração de acessador consiste `add` no `remove` token ou seguido por um *bloco*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="c8a10-1222">O *bloco* associado a um *add_accessor_declaration* especifica as instruções a serem executadas quando um manipulador de eventos é adicionado e o *bloco* associado a um *remove_accessor_declaration* especifica as instruções a serem executadas Quando um manipulador de eventos é removido.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="c8a10-1223">Cada *add_accessor_declaration* e *remove_accessor_declaration* corresponde a um método com um parâmetro de valor único do tipo de evento e um tipo de retorno `void`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="c8a10-1224">O parâmetro implícito de um acessador de `value`eventos é nomeado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="c8a10-1225">Quando um evento é usado em uma atribuição de evento, o acessador de evento apropriado é usado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="c8a10-1226">Especificamente, se o operador de atribuição `+=` for, o acessador Add será usado e, se o `-=` operador de atribuição for, o acessador de remoção será usado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="c8a10-1227">Em ambos os casos, o operando do lado direito do operador de atribuição é usado como o argumento para o acessador de eventos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="c8a10-1228">O bloco de um *add_accessor_declaration* ou *remove_accessor_declaration* deve estar em conformidade com as regras para os métodos `void` descritos no [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c8a10-1229">Em particular, `return` instruções nesse bloco não são permitidas para especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="c8a10-1230">Como um acessador de evento implicitamente tem `value`um parâmetro chamado, ele é um erro de tempo de compilação para uma variável local ou constante declarada em um acessador de evento para ter esse nome.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="c8a10-1231">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1231">In the example</span></span>
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
<span data-ttu-id="c8a10-1232">a `Control` classe implementa um mecanismo de armazenamento interno para eventos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="c8a10-1233">O `AddEventHandler` método associa um valor delegado a uma chave, o `GetEventHandler` método retorna o delegado atualmente associado a uma chave e o `RemoveEventHandler` método Remove um delegado como um manipulador de eventos para o evento especificado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="c8a10-1234">Supostamente, o mecanismo de armazenamento subjacente foi projetado de forma que não há nenhum custo para associar um `null` valor delegado a uma chave e, portanto, eventos sem tratamento não consomem nenhum armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="c8a10-1235">Eventos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-1235">Static and instance events</span></span>

<span data-ttu-id="c8a10-1236">Quando uma declaração de evento inclui `static` um modificador, o evento é considerado um ***evento estático***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="c8a10-1237">Quando nenhum `static` modificador está presente, o evento é considerado um ***evento de instância***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="c8a10-1238">Um evento estático não está associado a uma instância específica e é um erro de tempo de compilação para se referir `this` aos acessadores de um evento estático.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="c8a10-1239">Um evento de instância é associado a uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores desse evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="c8a10-1240">Quando um evento é referenciado em um *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` for um evento estático, `E` deverá indicar um tipo contendo `M` e se `M` for um evento de instância, e deverá indicar uma instância de um tipo que contém `M`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c8a10-1241">As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="c8a10-1242">Acessadores de evento virtual, sealed, override e abstract</span><span class="sxs-lookup"><span data-stu-id="c8a10-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="c8a10-1243">Uma `virtual` declaração de evento especifica que os acessadores desse evento são virtuais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="c8a10-1244">O `virtual` modificador se aplica a ambos os acessadores de um evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="c8a10-1245">Uma `abstract` declaração de evento especifica que os acessadores do evento são virtuais, mas não fornecem uma implementação real dos acessadores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="c8a10-1246">Em vez disso, as classes derivadas não abstratas são necessárias para fornecer sua própria implementação para os acessadores, substituindo o evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="c8a10-1247">Como uma declaração de evento abstract não fornece implementação real, ela não pode fornecer *event_accessor_declarations*delimitado por chaves.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="c8a10-1248">Uma declaração de evento que inclui os `abstract` modificadores e `override` especifica que o evento é abstrato e substitui um evento base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="c8a10-1249">Os acessadores de tal evento também são abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="c8a10-1250">As declarações de evento abstract só são permitidas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="c8a10-1251">Os acessadores de um evento virtual herdado podem ser substituídos em uma classe derivada, incluindo uma declaração de `override` evento que especifica um modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="c8a10-1252">Isso é conhecido como uma ***declaração de evento de substituição***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="c8a10-1253">Uma declaração de evento de substituição não declara um novo evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="c8a10-1254">Em vez disso, ele simplesmente especializa as implementações dos acessadores de um evento virtual existente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="c8a10-1255">Uma declaração de evento de substituição deve especificar exatamente os mesmos modificadores de acessibilidade, tipo e nome que o evento substituído.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="c8a10-1256">Uma declaração de evento de substituição pode `sealed` incluir o modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="c8a10-1257">O uso desse modificador impede que uma classe derivada substitua o evento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="c8a10-1258">Os acessadores de um evento lacrado também são lacrados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="c8a10-1259">É um erro de tempo de compilação para uma declaração de evento de substituição para `new` incluir um modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="c8a10-1260">Exceto pelas diferenças na sintaxe de declaração e invocação, os acessadores virtual, sealed, override e abstract se comportam exatamente como métodos virtuais, lacrados, de substituição e abstratos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="c8a10-1261">Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [métodos de substituição](classes.md#override-methods), métodos [lacrados](classes.md#sealed-methods)e [métodos abstratos](classes.md#abstract-methods) se aplicam como se os acessadores fossem métodos de um formulário correspondente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="c8a10-1262">Cada acessador corresponde a um método com um parâmetro de valor único do tipo de `void` evento, um tipo de retorno e os mesmos modificadores que o evento recipiente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="c8a10-1263">Indexadores</span><span class="sxs-lookup"><span data-stu-id="c8a10-1263">Indexers</span></span>

<span data-ttu-id="c8a10-1264">Um ***indexador*** é um membro que permite que um objeto seja indexado da mesma maneira que uma matriz.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="c8a10-1265">Indexadores são declarados usando *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-1266">Um *indexer_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `virtual` ([métodos virtuais ](classes.md#virtual-methods)), `override` ([métodos de substituição](classes.md#override-methods)), 0[(métodos lacrados](classes.md#sealed-methods)), os modificadores 2 ([métodos abstratos](classes.md#abstract-methods)) e 4 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c8a10-1267">As declarações do indexador estão sujeitas às mesmas regras que as declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores, com a única exceção de que o modificador estático não é permitido em uma declaração de indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="c8a10-1268">Os modificadores `virtual`, `override`e `abstract` são mutuamente exclusivos, exceto em um caso.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="c8a10-1269">Os `abstract` modificadores e `override` podem ser usados juntos para que um indexador abstrato possa substituir um virtual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="c8a10-1270">O *tipo* de uma declaração de indexador especifica o tipo de elemento do indexador introduzido pela declaração.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="c8a10-1271">A menos que o indexador seja uma implementação de membro de interface explícita, o *tipo* é `this`seguido pela palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="c8a10-1272">Para uma implementação de membro de interface explícita, o *tipo* é seguido por um *interface_type*, um "`.`" e a palavra-chave `this`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="c8a10-1273">Ao contrário de outros membros, os indexadores não têm nomes definidos pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="c8a10-1274">O *formal_parameter_list* especifica os parâmetros do indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="c8a10-1275">A lista de parâmetros formais de um indexador corresponde ao de um método ([parâmetros de método](classes.md#method-parameters)), exceto pelo menos um parâmetro que deve ser especificado e que `ref` os `out` modificadores e de parâmetro não são permitidos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="c8a10-1276">O *tipo* de um indexador e cada um dos tipos referenciados no *formal_parameter_list* devem ser pelo menos tão acessíveis quanto o próprio indexador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-1277">Um *indexer_body* pode consistir em um ***corpo de acessador*** ou um ***corpo de expressão***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="c8a10-1278">Em um corpo de acessador, *accessor_declarations*, que deve ser incluído nos tokens "`{`" e "`}`", declare os acessadores ([acessadores](classes.md#accessors)) da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="c8a10-1279">Os acessadores especificam as instruções Executáveis associadas à leitura e gravação da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="c8a10-1280">Um corpo de expressão que consiste em`=>`"" seguido por uma `E` expressão e um ponto-e-vírgula é exatamente `{ get { return E; } }`equivalente ao corpo da instrução e, portanto, só pode ser usado para especificar indexadores somente getter em que o resultado do getter é fornecido por uma única expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="c8a10-1281">Embora a sintaxe para acessar um elemento do indexador seja a mesma que para um elemento de matriz, um elemento de indexador não é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="c8a10-1282">Portanto, não é possível passar um elemento do indexador como um `ref` argumento ou. `out`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="c8a10-1283">A lista de parâmetros formais de um indexador define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="c8a10-1284">Especificamente, a assinatura de um indexador consiste no número e tipos de seus parâmetros formais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="c8a10-1285">O tipo de elemento e os nomes dos parâmetros formais não fazem parte da assinatura de um indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="c8a10-1286">A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="c8a10-1287">Indexadores e propriedades são muito semelhantes em conceito, mas diferem das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="c8a10-1288">Uma propriedade é identificada por seu nome, enquanto um indexador é identificado por sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="c8a10-1289">Uma propriedade é acessada por meio de um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou um *member_access* ([acesso de membro](expressions.md#member-access)), enquanto um elemento de indexador é acessado por meio de um *element_access* ([acesso do indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="c8a10-1290">Uma propriedade pode ser um `static` membro, enquanto um indexador é sempre um membro de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="c8a10-1291">Um `get` acessador de uma propriedade corresponde a um método sem parâmetros, enquanto `get` um acessador de um indexador corresponde a um método com a mesma lista de parâmetros formais que o indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="c8a10-1292">Um `set` acessador de uma propriedade corresponde a um método com um único `value`parâmetro chamado, `set` enquanto que um acessador de um indexador corresponde a um método com a mesma lista de parâmetros formais que o indexador, além de um parâmetro adicional nomeado `value`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="c8a10-1293">É um erro de tempo de compilação para um acessador de indexador declarar uma variável local com o mesmo nome que um parâmetro de indexador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="c8a10-1294">Em uma declaração de propriedade de substituição, a propriedade herdada é acessada `P` usando a sintaxe `base.P`, onde é o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="c8a10-1295">Em uma declaração de substituição do indexador, o indexador herdado é acessado `E` usando a sintaxe `base[E]`, em que é uma lista separada por vírgulas de expressões.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="c8a10-1296">Não há nenhum conceito de "indexador implementado automaticamente".</span><span class="sxs-lookup"><span data-stu-id="c8a10-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="c8a10-1297">É um erro ter um indexador não-abstrato e não externo com acessadores de ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="c8a10-1298">Além dessas diferenças, todas as regras definidas em [acessadores](classes.md#accessors) e [Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties) se aplicam a acessadores indexadores, bem como a acessadores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="c8a10-1299">Quando uma declaração de indexador inclui `extern` um modificador, o indexador é considerado um ***indexador externo***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="c8a10-1300">Como uma declaração externa do indexador não fornece nenhuma implementação real, cada uma de suas *accessor_declarations* consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="c8a10-1301">O exemplo a seguir declara uma `BitArray` classe que implementa um indexador para acessar os bits individuais na matriz de bits.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="c8a10-1302">Uma instância da `BitArray` classe consome substancialmente menos memória do que uma correspondente `bool[]` (já que cada valor do primeiro ocupa apenas um bit em vez do último byte), mas permite as mesmas operações que um. `bool[]`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="c8a10-1303">A classe `CountPrimes` a seguir usa `BitArray` um e o algoritmo clássico "sieve" para calcular o número de primos entre 1 e um determinado máximo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="c8a10-1304">Observe que a sintaxe para acessar elementos do `BitArray` é exatamente a mesma do para um. `bool[]`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="c8a10-1305">O exemplo a seguir mostra uma classe de grade 26 \* 10 que tem um indexador com dois parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="c8a10-1306">O primeiro parâmetro deve ser uma letra maiúscula ou minúscula no intervalo de A-Z e o segundo deve ser um número inteiro no intervalo de 0-9.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="c8a10-1307">Sobrecarga do indexador</span><span class="sxs-lookup"><span data-stu-id="c8a10-1307">Indexer overloading</span></span>

<span data-ttu-id="c8a10-1308">As regras de resolução de sobrecarga do indexador são descritas em [inferência de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="c8a10-1309">Operadores</span><span class="sxs-lookup"><span data-stu-id="c8a10-1309">Operators</span></span>

<span data-ttu-id="c8a10-1310">Um ***operador*** é um membro que define o significado de um operador de expressão que pode ser aplicado a instâncias da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="c8a10-1311">Os operadores são declarados usando *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-1312">Há três categorias de operadores que podem ser sobrecarregados: Operadores unários ([operadores unários](classes.md#unary-operators)), operadores binários ([operadores binários](classes.md#binary-operators)) e operadores de conversão ([operadores de conversão](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="c8a10-1313">O *operator_body* é um ponto e vírgula, um ***corpo de instrução*** ou um corpo de ***expressão***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="c8a10-1314">Um corpo de instrução consiste em um *bloco*, que especifica as instruções a serem executadas quando o operador é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="c8a10-1315">O *bloco* deve estar em conformidade com as regras para métodos de retorno de valor descritos no [corpo do método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c8a10-1316">Um corpo de `=>` expressão consiste em seguido por uma expressão e um ponto e vírgula e denota uma única expressão a ser executada quando o operador é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="c8a10-1317">Para operadores `extern`, o *operator_body* consiste apenas de um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="c8a10-1318">Para todos os outros operadores, o *operator_body* é um corpo de bloco ou um corpo de expressão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="c8a10-1319">As regras a seguir se aplicam a todas as declarações de operador:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="c8a10-1320">Uma declaração de operador deve incluir um `public` e um `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="c8a10-1321">Os parâmetros de um operador devem ser parâmetros de valor (parâmetros de[valor](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="c8a10-1322">É um erro de tempo de compilação para uma declaração de operador especificar `ref` ou `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="c8a10-1323">A assinatura de um operador ([operadores unários](classes.md#unary-operators), [operadores binários](classes.md#binary-operators), [operadores de conversão](classes.md#conversion-operators)) deve ser diferente das assinaturas de todos os outros operadores declarados na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="c8a10-1324">Todos os tipos referenciados em uma declaração de operador devem ser pelo menos tão acessíveis quanto o próprio operador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="c8a10-1325">É um erro para que o mesmo modificador apareça várias vezes em uma declaração de operador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="c8a10-1326">Cada categoria de operador impõe restrições adicionais, conforme descrito nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="c8a10-1327">Como outros membros, os operadores declarados em uma classe base são herdados por classes derivadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="c8a10-1328">Como as declarações de operador sempre exigem a classe ou struct em que o operador é declarado para participar da assinatura do operador, não é possível que um operador declarado em uma classe derivada oculte um operador declarado em uma classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="c8a10-1329">Assim, o `new` modificador nunca é necessário e, portanto, nunca é permitido em uma declaração de operador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="c8a10-1330">Informações adicionais sobre operadores unários e binários podem ser encontradas em [operadores](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="c8a10-1331">Informações adicionais sobre operadores de conversão podem ser encontradas em [conversões definidas pelo usuário](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="c8a10-1332">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="c8a10-1332">Unary operators</span></span>

<span data-ttu-id="c8a10-1333">As regras a seguir se aplicam a declarações de `T` operador unários, onde denota o tipo de instância da classe ou struct que contém a declaração do operador:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="c8a10-1334">Um operador `+` `-`unário `~` `T` ,, ou deve usar um único parâmetro do tipo ou `T?` pode retornar qualquer tipo. `!`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="c8a10-1335">Um operador `++` or `--` unário deve usar um único parâmetro do `T` tipo `T?` ou deve retornar o mesmo tipo ou um tipo derivado dele.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="c8a10-1336">Um operador `true` or `false` unário deve usar um único parâmetro do `T` tipo `T?` ou deve retornar o `bool`tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="c8a10-1337">A assinatura de um operador unário consiste no token do operador`+`( `-` `!`, `--` `~` `++`,,,, `true`, ou `false`) e no tipo do único parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="c8a10-1338">O tipo de retorno não faz parte da assinatura de um operador unário, nem é o nome do parâmetro formal.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="c8a10-1339">Os `true` operadores `false` e unários exigem declarações emparelhadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="c8a10-1340">Ocorrerá um erro em tempo de compilação se uma classe declarar um desses operadores sem também declarar o outro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="c8a10-1341">Os `true` operadores `false` e são descritos mais detalhadamente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators) e [expressões booleanas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="c8a10-1342">O exemplo a seguir mostra uma implementação e o uso `operator ++` subsequente de para uma classe de vetor de inteiro:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="c8a10-1343">Observe como o método Operator retorna o valor produzido pela adição de 1 ao operando, assim como os operadores de incremento de sufixo e decréscimo ([incremento de sufixo e diminuição de operadores](expressions.md#postfix-increment-and-decrement-operators)) e os operadores de incremento de prefixo e decréscimo ([prefixo operadores de incremento e decréscimo](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="c8a10-1344">Diferentemente C++do no, esse método não precisa modificar o valor de seu operando diretamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="c8a10-1345">Na verdade, modificar o valor do operando violaria a semântica padrão do operador de incremento de sufixo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="c8a10-1346">Operadores binários</span><span class="sxs-lookup"><span data-stu-id="c8a10-1346">Binary operators</span></span>

<span data-ttu-id="c8a10-1347">As regras a seguir se aplicam a declarações de `T` operador binários, onde denota o tipo de instância da classe ou struct que contém a declaração do operador:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="c8a10-1348">Um operador binário non-Shift deve usar dois parâmetros, pelo menos um dos quais deve ter tipo `T` ou `T?`, e pode retornar qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="c8a10-1349">Um operador `<<` ou `>>` binário deve usar dois parâmetros, o primeiro deles deve ter o tipo `T` ou `T?` e o segundo deve ter o tipo `int` ou `int?`, e pode retornar qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="c8a10-1350">A assinatura de um operador binário consiste no token do operador (`+` `*`, `-` `&` `|` `/` `%` ,,`^`,,,,,,, `>>` `<<` `==` ,,,`<=`, ou`>=`) e os tipos dos dois parâmetros formais. `<` `!=` `>`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="c8a10-1351">O tipo de retorno e os nomes dos parâmetros formais não fazem parte da assinatura de um operador binário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="c8a10-1352">Determinados operadores binários exigem declarações emparelhadas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="c8a10-1353">Para cada declaração de qualquer operador de um par, deve haver uma declaração correspondente do outro operador do par.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="c8a10-1354">Duas declarações de operador correspondem quando têm o mesmo tipo de retorno e o mesmo tipo para cada parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="c8a10-1355">Os seguintes operadores exigem a declaração de pares:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="c8a10-1356">`operator ==` e `operator !=`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="c8a10-1357">`operator >` e `operator <`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="c8a10-1358">`operator >=` e `operator <=`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="c8a10-1359">Operadores de conversão</span><span class="sxs-lookup"><span data-stu-id="c8a10-1359">Conversion operators</span></span>

<span data-ttu-id="c8a10-1360">Uma declaração de operador de conversão apresenta uma ***conversão definida pelo usuário*** ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)) que aumenta as conversões implícitas e explícitas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="c8a10-1361">Uma declaração de operador de conversão que `implicit` inclui a palavra-chave apresenta uma conversão implícita definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="c8a10-1362">Conversões implícitas podem ocorrer em várias situações, incluindo invocações de membro de função, expressões de conversão e atribuições.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="c8a10-1363">Isso é descrito mais detalhadamente em [conversões implícitas](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="c8a10-1364">Uma declaração de operador de conversão que `explicit` inclui a palavra-chave apresenta uma conversão explícita definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="c8a10-1365">Conversões explícitas podem ocorrer em expressões de conversão e são descritas em [conversões explícitas](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="c8a10-1366">Um operador de conversão converte de um tipo de origem, indicado pelo tipo de parâmetro do operador de conversão, em um tipo de destino, indicado pelo tipo de retorno do operador de conversão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="c8a10-1367">Para um `S` determinado tipo de origem e tipo `T`de destino, se `S` ou `T` forem tipos anuláveis `T0` , avise `S0` e faça referência aos seus `S0` tipos `T0` subjacentes, caso contrário, e serão igual a `S` e `T` respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="c8a10-1368">Uma classe ou estrutura tem permissão para declarar uma conversão de um tipo `S` de origem para um tipo `T` de destino somente se todas as seguintes opções forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="c8a10-1369">`S0`e `T0` são tipos diferentes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="c8a10-1370">`S0` Ou`T0` é o tipo de classe ou struct no qual a declaração do operador ocorre.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="c8a10-1371">Nem `S0` nem `T0` é um *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="c8a10-1372">Excluindo conversões definidas `S` pelo usuário, uma conversão não existe de `T` ou `T` `S`para.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="c8a10-1373">Para os fins dessas regras, quaisquer parâmetros de tipo associados `S` a ou `T` são considerados como tipos exclusivos que não têm nenhuma relação de herança com outros tipos, e quaisquer restrições nesses parâmetros de tipo são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="c8a10-1374">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="c8a10-1375">as primeiras duas declarações de operador são permitidas porque, para fins de [indexadores](classes.md#indexers)3 `T` e `int` , `string` respectivamente, são consideradas tipos exclusivos sem nenhuma relação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="c8a10-1376">No entanto, o terceiro operador é um `C<T>` erro porque é a classe `D<T>`base de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="c8a10-1377">Da segunda regra, ela segue que um operador de conversão deve converter de ou para o tipo class ou struct no qual o operador é declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="c8a10-1378">Por exemplo, é `C` possível que um tipo de classe ou estrutura defina uma conversão de `C` para `int` e `int` de `C`para, mas não de `int` para `bool`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="c8a10-1379">Não é possível redefinir diretamente uma conversão predefinida.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="c8a10-1380">Assim, os operadores de conversão não têm permissão para converter de `object` ou para porque as conversões implícitas e explícitas já existem entre `object` o e todos os outros tipos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="c8a10-1381">Da mesma forma, nem os tipos de origem nem de destino de uma conversão podem ser um tipo base do outro, já que uma conversão já existirá.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="c8a10-1382">No entanto, é possível declarar operadores em tipos genéricos que, para argumentos de tipo específicos, especificar conversões que já existem como conversões predefinidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="c8a10-1383">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="c8a10-1384">Quando Type `object` é especificado como um argumento de tipo `T`para, o segundo operador declara uma conversão que já existe (um implícito e, portanto, também uma conversão explícita existe de qualquer tipo para tipo `object`).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="c8a10-1385">Nos casos em que existe uma conversão predefinida entre dois tipos, todas as conversões definidas pelo usuário entre esses tipos serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="c8a10-1386">Especificamente:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1386">Specifically:</span></span>

*  <span data-ttu-id="c8a10-1387">Se uma conversão implícita predefinida ([conversões implícitas](conversions.md#implicit-conversions)) `S` existir de tipo para tipo `T`, todas as conversões definidas pelo usuário (implícitas ou explícitas) `S` de `T` para serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="c8a10-1388">Se uma conversão explícita predefinida ([conversões explícitas](conversions.md#explicit-conversions)) `S` existir de tipo para tipo `T`, todas as conversões explícitas definidas pelo usuário do `S` para `T` serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="c8a10-1389">Além</span><span class="sxs-lookup"><span data-stu-id="c8a10-1389">Furthermore:</span></span>

<span data-ttu-id="c8a10-1390">Se `T` for um tipo de interface, conversões implícitas definidas pelo usuário `S` de `T` para serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="c8a10-1391">Caso contrário, conversões implícitas definidas pelo usuário `S` de `T` para o ainda serão consideradas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="c8a10-1392">Para todos os tipos `object`, mas os operadores declarados `Convertible<T>` pelo tipo acima não entram em conflito com conversões predefinidas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="c8a10-1393">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="c8a10-1394">No entanto, `object`para conversões de tipo predefinidas, oculte as conversões definidas pelo usuário em todos os casos, exceto uma:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="c8a10-1395">As conversões definidas pelo usuário não têm permissão para converter de ou para *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="c8a10-1396">Em particular, essa restrição garante que nenhuma transformação definida pelo usuário ocorra durante a conversão para um *interface_type*e que uma conversão em um *interface_type* só terá sucesso se o objeto que está sendo convertido realmente implementar o *interface_type*especificado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="c8a10-1397">A assinatura de um operador de conversão consiste no tipo de origem e no tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="c8a10-1398">(Observe que essa é a única forma de membro para a qual o tipo de retorno participa na assinatura.) A `implicit` classificação `explicit` ou de um operador de conversão não faz parte da assinatura do operador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="c8a10-1399">Assim, uma classe ou struct não pode declarar um `implicit` operador de `explicit` conversão e um com os mesmos tipos de origem e destino.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="c8a10-1400">Em geral, as conversões implícitas definidas pelo usuário devem ser projetadas para nunca gerar exceções e nunca perderem informações.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="c8a10-1401">Se uma conversão definida pelo usuário puder dar origem às exceções (por exemplo, porque o argumento de origem está fora do intervalo) ou a perda de informações (como descartar bits de ordem superior), essa conversão deve ser definida como uma conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="c8a10-1402">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1402">In the example</span></span>
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
<span data-ttu-id="c8a10-1403">a conversão de `Digit` para `byte` é implícita porque ela nunca gera exceções ou perde informações, mas a conversão de `byte` para `Digit` é explícita, `Digit` já que só pode representar um subconjunto do possível valores de a `byte`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="c8a10-1404">Construtores de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-1404">Instance constructors</span></span>

<span data-ttu-id="c8a10-1405">Um ***construtor de instância*** é um membro que implementa as ações necessárias para inicializar uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="c8a10-1406">Construtores de instância são declarados usando *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-1407">Um *constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) e um modificador de `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="c8a10-1408">Uma declaração de construtor não tem permissão para incluir o mesmo modificador várias vezes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="c8a10-1409">O *identificador* de um *constructor_declarator* deve nomear a classe na qual o construtor de instância é declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="c8a10-1410">Se qualquer outro nome for especificado, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="c8a10-1411">O *formal_parameter_list* opcional de um construtor de instância está sujeito às mesmas regras que o *formal_parameter_list* de um método ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="c8a10-1412">A lista de parâmetros formais define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um construtor de instância e governa o processo pelo qual a resolução de sobrecarga ([inferência de tipos](expressions.md#type-inference)) seleciona um construtor de instância específico em uma invocação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="c8a10-1413">Cada um dos tipos referenciados no *formal_parameter_list* de um construtor de instância deve ser pelo menos acessível como o próprio Construtor ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c8a10-1414">O *constructor_initializer* opcional especifica outro construtor de instância para invocar antes de executar as instruções fornecidas no *constructor_body* deste construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="c8a10-1415">Isso é descrito mais detalhadamente em [inicializadores de Construtor](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="c8a10-1416">Quando uma declaração de Construtor inclui `extern` um modificador, o construtor é considerado um ***Construtor externo***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="c8a10-1417">Como uma declaração de Construtor externo não fornece nenhuma implementação real, seu *constructor_body* consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="c8a10-1418">Para todos os outros construtores, o *constructor_body* consiste em um *bloco* que especifica as instruções para inicializar uma nova instância da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="c8a10-1419">Isso corresponde exatamente ao *bloco* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="c8a10-1420">Construtores de instância não são herdados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="c8a10-1421">Portanto, uma classe não tem construtores de instância diferentes daqueles realmente declarados na classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="c8a10-1422">Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão será fornecido automaticamente ([construtores padrão](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="c8a10-1423">Os construtores de instância são invocados por *object_creation_expression*s ([expressões de criação de objeto](expressions.md#object-creation-expressions)) e por meio de *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="c8a10-1424">Inicializadores de construtores</span><span class="sxs-lookup"><span data-stu-id="c8a10-1424">Constructor initializers</span></span>

<span data-ttu-id="c8a10-1425">Todos os construtores de instância (exceto aqueles para a classe `object`) incluem implicitamente uma invocação de outro construtor de instância imediatamente antes do *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="c8a10-1426">O construtor para invocar implicitamente é determinado pelo *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="c8a10-1427">Um inicializador de construtor de instância `base(argument_list)` do `base()` formulário ou faz com que um construtor de instância da classe base direta seja invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="c8a10-1428">Esse construtor é selecionado usando *argument_list* , se presente, e as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="c8a10-1429">O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessíveis contidos na classe base direta ou no construtor padrão ([construtores padrão](classes.md#default-constructors)), se nenhum construtor de instância for declarado na classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="c8a10-1430">Se esse conjunto estiver vazio, ou se um único Construtor de instância recomendada não puder ser identificado, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="c8a10-1431">Um inicializador de construtor de instância `this(argument-list)` do `this()` formulário ou faz com que um construtor de instância da própria classe seja invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="c8a10-1432">O construtor é selecionado usando *argument_list* , se presente, e as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="c8a10-1433">O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessíveis declarados na própria classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="c8a10-1434">Se esse conjunto estiver vazio, ou se um único Construtor de instância recomendada não puder ser identificado, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="c8a10-1435">Se uma declaração de construtor de instância incluir um inicializador de construtor que invoca o Construtor em si, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="c8a10-1436">Se um construtor de instância não tiver nenhum inicializador de construtor, um inicializador de construtor do formulário `base()` será fornecido implicitamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="c8a10-1437">Portanto, uma declaração de construtor de instância do formulário</span><span class="sxs-lookup"><span data-stu-id="c8a10-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="c8a10-1438">é exatamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="c8a10-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="c8a10-1439">O escopo dos parâmetros fornecidos pelo *formal_parameter_list* de uma declaração de construtor de instância inclui o inicializador de construtor dessa declaração.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="c8a10-1440">Assim, um inicializador de construtor tem permissão para acessar os parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="c8a10-1441">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1441">For example:</span></span>
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

<span data-ttu-id="c8a10-1442">Um inicializador de construtor de instância não pode acessar a instância que está sendo criada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="c8a10-1443">Portanto, é um erro de tempo de compilação para referenciar `this` em uma expressão de argumento do inicializador de construtor, pois é um erro de tempo de compilação para uma expressão de argumento referenciar qualquer membro de instância por meio de um *Simple_name*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="c8a10-1444">Inicializadores de variável de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-1444">Instance variable initializers</span></span>

<span data-ttu-id="c8a10-1445">Quando um construtor de instância não tem nenhum inicializador de construtor ou tem um inicializador de construtor do formulário `base(...)`, esse construtor executa implicitamente as inicializações especificadas pelos *variable_initializer*s dos campos de instância declarados em sua classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="c8a10-1446">Isso corresponde a uma sequência de atribuições que são executadas imediatamente após a entrada para o construtor e antes da invocação implícita do construtor da classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="c8a10-1447">Os inicializadores de variável são executados na ordem textual em que aparecem na declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="c8a10-1448">Execução do Construtor</span><span class="sxs-lookup"><span data-stu-id="c8a10-1448">Constructor execution</span></span>

<span data-ttu-id="c8a10-1449">Inicializadores de variáveis são transformados em instruções de atribuição, e essas instruções de atribuição são executadas antes da invocação do construtor da instância da classe base.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="c8a10-1450">Essa ordenação garante que todos os campos de instância sejam inicializados por seus inicializadores variáveis antes que qualquer instrução que tenha acesso a essa instância seja executada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="c8a10-1451">Dado o exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1451">Given the example</span></span>
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
<span data-ttu-id="c8a10-1452">Quando `new B()` é usado para criar uma instância do `B`, a seguinte saída é produzida:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="c8a10-1453">O valor de `x` é 1 porque o inicializador de variável é executado antes que o construtor da instância da classe base seja invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="c8a10-1454">No entanto, o `y` valor de é 0 (o valor padrão `int`de um) porque a `y` atribuição a não é executada até que o construtor da classe base seja retornado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="c8a10-1455">É útil considerar inicializadores de variável de instância e inicializadores de construtor como instruções que são inseridas automaticamente antes de *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="c8a10-1456">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1456">The example</span></span>
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
<span data-ttu-id="c8a10-1457">contém vários inicializadores de variável; Ele também contém inicializadores de construtor de ambos os`base` formulários `this`(e).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="c8a10-1458">O exemplo corresponde ao código mostrado abaixo, onde cada comentário indica uma instrução inserida automaticamente (a sintaxe usada para as invocações de Construtor inseridas automaticamente não é válida, mas meramente serve para ilustrar o mecanismo).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="c8a10-1459">Construtores padrão</span><span class="sxs-lookup"><span data-stu-id="c8a10-1459">Default constructors</span></span>

<span data-ttu-id="c8a10-1460">Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão será fornecido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="c8a10-1461">Esse construtor padrão simplesmente invoca o construtor sem parâmetros da classe base direta.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="c8a10-1462">Se a classe for abstrata, a acessibilidade declarada para o construtor padrão será protegida.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="c8a10-1463">Caso contrário, a acessibilidade declarada para o construtor padrão é pública.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="c8a10-1464">Portanto, o construtor padrão sempre está no formato</span><span class="sxs-lookup"><span data-stu-id="c8a10-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="c8a10-1465">ou</span><span class="sxs-lookup"><span data-stu-id="c8a10-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="c8a10-1466">em `C` que é o nome da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="c8a10-1467">Se a resolução de sobrecarga não puder determinar um melhor candidato exclusivo para o inicializador de construtor da classe base, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="c8a10-1468">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="c8a10-1469">um construtor padrão é fornecido porque a classe não contém nenhuma declaração de construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="c8a10-1470">Portanto, o exemplo é precisamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="c8a10-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="c8a10-1471">Construtores particulares</span><span class="sxs-lookup"><span data-stu-id="c8a10-1471">Private constructors</span></span>

<span data-ttu-id="c8a10-1472">Quando uma classe `T` declara apenas construtores de instância privada, não é possível para classes fora do texto do programa de `T` para derivar de `T` ou para criar instâncias diretamente do `T`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="c8a10-1473">Portanto, se uma classe contiver apenas membros estáticos e não tiver a intenção de ser instanciada, a adição de um construtor de instância particular vazio impedirá a instanciação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="c8a10-1474">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1474">For example:</span></span>
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

<span data-ttu-id="c8a10-1475">A `Trig` classe agrupa métodos e constantes relacionados, mas não deve ser instanciada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="c8a10-1476">Portanto, ele declara um único Construtor de instância particular vazio.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="c8a10-1477">Pelo menos um construtor de instância deve ser declarado para suprimir a geração automática de um construtor padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="c8a10-1478">Parâmetros opcionais do construtor de instância</span><span class="sxs-lookup"><span data-stu-id="c8a10-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="c8a10-1479">A `this(...)` forma de inicializador de construtor é comumente usada em conjunto com sobrecarga para implementar parâmetros de construtor de instância opcionais.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="c8a10-1480">No exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1480">In the example</span></span>
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
<span data-ttu-id="c8a10-1481">os primeiros dois construtores de instância fornecem apenas os valores padrão para os argumentos ausentes.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="c8a10-1482">Ambos usam um `this(...)` inicializador de construtor para invocar o terceiro construtor de instância, que realmente faz o trabalho de inicializar a nova instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="c8a10-1483">O efeito é o dos parâmetros de Construtor opcionais:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="c8a10-1484">Construtores estáticos</span><span class="sxs-lookup"><span data-stu-id="c8a10-1484">Static constructors</span></span>

<span data-ttu-id="c8a10-1485">Um ***construtor estático*** é um membro que implementa as ações necessárias para inicializar um tipo de classe fechado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="c8a10-1486">Construtores estáticos são declarados usando *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="c8a10-1487">Um *static_constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e um modificador de `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="c8a10-1488">O *identificador* de um *static_constructor_declaration* deve nomear a classe na qual o construtor estático é declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="c8a10-1489">Se qualquer outro nome for especificado, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="c8a10-1490">Quando uma declaração de construtor estático inclui `extern` um modificador, o construtor estático é considerado um ***construtor estático externo***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="c8a10-1491">Como uma declaração de construtor estático externa não fornece nenhuma implementação real, seu *static_constructor_body* consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="c8a10-1492">Para todas as outras declarações de construtor estático, o *static_constructor_body* consiste em um *bloco* que especifica as instruções a serem executadas para inicializar a classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="c8a10-1493">Isso corresponde exatamente ao *method_body* de um método estático com um tipo de retorno `void` ([corpo do método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="c8a10-1494">Construtores estáticos não são herdados e não podem ser chamados diretamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="c8a10-1495">O construtor estático para um tipo de classe fechada é executado no máximo uma vez em um determinado domínio de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="c8a10-1496">A execução de um construtor estático é disparada pelo primeiro dos seguintes eventos para ocorrer dentro de um domínio de aplicativo:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="c8a10-1497">Uma instância do tipo de classe é criada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="c8a10-1498">Qualquer um dos membros estáticos do tipo de classe é referenciado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="c8a10-1499">Se uma classe contiver `Main` o método ([inicialização do aplicativo](basic-concepts.md#application-startup)) no qual a execução começa, o construtor estático para aquela classe é `Main` executado antes de o método ser chamado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="c8a10-1500">Para inicializar um novo tipo de classe fechada, primeiro um novo conjunto de campos estáticos ([campos estáticos e de instância](classes.md#static-and-instance-fields)) para esse tipo fechado específico é criado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="c8a10-1501">Cada um dos campos estáticos é inicializado para seu valor padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="c8a10-1502">Em seguida, os inicializadores de campo estáticos ([inicialização de campo estático](classes.md#static-field-initialization)) são executados para esses campos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="c8a10-1503">Por fim, o construtor estático é executado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="c8a10-1504">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1504">The example</span></span>
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
<span data-ttu-id="c8a10-1505">deve produzir a saída:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="c8a10-1506">Porque a execução do `A`construtor estático do é disparada pela chamada `A.F`para, e a execução `B`do construtor estático do é disparada pela `B.F`chamada para.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="c8a10-1507">É possível construir dependências circulares que permitem que campos estáticos com inicializadores de variáveis sejam observados em seu estado de valor padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="c8a10-1508">O exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1508">The example</span></span>
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
<span data-ttu-id="c8a10-1509">produz a saída</span><span class="sxs-lookup"><span data-stu-id="c8a10-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="c8a10-1510">Para executar o `Main` método, o sistema primeiro executa o inicializador `B.Y`para, antes do `B`construtor estático da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="c8a10-1511">`Y`o inicializador `A`faz com que o construtor estático seja executado porque o `A.X` valor de é referenciado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="c8a10-1512">O construtor estático de `A` , por sua vez, prossegue para calcular o `X`valor de e, ao fazer isso, busca o valor `Y`padrão de, que é zero.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="c8a10-1513">`A.X`é, portanto, inicializado como 1.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="c8a10-1514">O processo de execução `A`de inicializadores de campo estático e construtor estático é concluído, retornando ao cálculo do valor inicial de `Y`, o resultado é 2.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="c8a10-1515">Como o construtor estático é executado exatamente uma vez para cada tipo de classe construída fechada, é um local conveniente para impor verificações de tempo de execução no parâmetro de tipo que não pode ser verificado em tempo de compilação via restrições ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="c8a10-1516">Por exemplo, o tipo a seguir usa um construtor estático para impor que o argumento de tipo seja uma enumeração:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="c8a10-1517">Destruidores</span><span class="sxs-lookup"><span data-stu-id="c8a10-1517">Destructors</span></span>

<span data-ttu-id="c8a10-1518">Um ***destruidor*** é um membro que implementa as ações necessárias para destruir uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="c8a10-1519">Um destruidor é declarado usando um *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="c8a10-1520">Um *destructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="c8a10-1521">O *identificador* de um *destructor_declaration* deve nomear a classe na qual o destruidor é declarado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="c8a10-1522">Se qualquer outro nome for especificado, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="c8a10-1523">Quando uma declaração de destruidor inclui `extern` um modificador, diz-se que o destruidor é um ***destruidor externo***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="c8a10-1524">Como uma declaração de destruidor externo não fornece implementação real, seu *destructor_body* consiste em um ponto-e-vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="c8a10-1525">Para todos os outros destruidores, o *destructor_body* consiste em um *bloco* que especifica as instruções a serem executadas a fim de destruir uma instância da classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="c8a10-1526">Um *destructor_body* corresponde exatamente ao *method_body* de um método de instância com um tipo de retorno `void` ([corpo do método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="c8a10-1527">Os destruidores não são herdados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1527">Destructors are not inherited.</span></span> <span data-ttu-id="c8a10-1528">Portanto, uma classe não tem destruidores além daquele que pode ser declarado nessa classe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="c8a10-1529">Como um destruidor é necessário para não ter parâmetros, ele não pode ser sobrecarregado, portanto, uma classe pode ter, no máximo, um destruidor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="c8a10-1530">Os destruidores são invocados automaticamente e não podem ser invocados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="c8a10-1531">Uma instância fica qualificada para destruição quando não é mais possível que qualquer código use essa instância.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="c8a10-1532">A execução do destruidor para a instância pode ocorrer a qualquer momento depois que a instância for qualificada para destruição.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="c8a10-1533">Quando uma instância é destruída, os destruidores na cadeia de herança dessa instância são chamados, em ordem, da mais derivada para a menos derivada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="c8a10-1534">Um destruidor pode ser executado em qualquer thread.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="c8a10-1535">Para obter mais informações sobre as regras que regem quando e como um destruidor é executado, consulte [gerenciamento automático de memória](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="c8a10-1536">A saída do exemplo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1536">The output of the example</span></span>
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
<span data-ttu-id="c8a10-1537">is</span><span class="sxs-lookup"><span data-stu-id="c8a10-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="c8a10-1538">como os destruidores em uma cadeia de herança são chamados em ordem, da mais derivada para a menos derivada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="c8a10-1539">Os destruidores são implementados substituindo o `Finalize` método `System.Object`virtual em.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="c8a10-1540">C#os programas não têm permissão para substituir esse método ou chamá-lo (ou substituí-lo) diretamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="c8a10-1541">Por exemplo, o programa</span><span class="sxs-lookup"><span data-stu-id="c8a10-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="c8a10-1542">contém dois erros.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1542">contains two errors.</span></span>

<span data-ttu-id="c8a10-1543">O compilador se comporta como se esse método e as substituições, não existem.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="c8a10-1544">Portanto, este programa:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="c8a10-1545">é válido e o método mostrado oculta `System.Object` `Finalize` o método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="c8a10-1546">Para obter uma discussão sobre o comportamento quando uma exceção é gerada de um destruidor, consulte [como as exceções são tratadas](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="c8a10-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="c8a10-1547">Iterators</span></span>

<span data-ttu-id="c8a10-1548">Um membro de função ([membros da função](expressions.md#function-members)) implementado usando um bloco do iterador ([blocos](statements.md#blocks)) é chamado de ***iterador***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="c8a10-1549">Um bloco do iterador pode ser usado como o corpo de um membro da função, desde que o tipo de retorno do membro da função correspondente seja uma das interfaces do enumerador ([interfaces do enumerador](classes.md#enumerator-interfaces)) ou uma das interfaces enumeráveis ([interfaces enumeráveis](classes.md#enumerable-interfaces)) .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="c8a10-1550">Ele pode ocorrer como um *method_body*, *operator_body* ou *accessor_body*, enquanto eventos, construtores de instância, construtores estáticos e destruidores não podem ser implementados como iteradores.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="c8a10-1551">Quando um membro de função é implementado usando um bloco iterador, ele é um erro de tempo de compilação para a lista de parâmetros formais do membro `ref` da `out` função para especificar qualquer parâmetro ou.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="c8a10-1552">Interfaces do enumerador</span><span class="sxs-lookup"><span data-stu-id="c8a10-1552">Enumerator interfaces</span></span>

<span data-ttu-id="c8a10-1553">As ***interfaces do enumerador*** são a interface `System.Collections.IEnumerator` não genérica e todas as instanciações da interface `System.Collections.Generic.IEnumerator<T>`genérica.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="c8a10-1554">Por questão de brevidade, neste capítulo, essas interfaces são referenciadas como `IEnumerator` e `IEnumerator<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="c8a10-1555">Interfaces enumeráveis</span><span class="sxs-lookup"><span data-stu-id="c8a10-1555">Enumerable interfaces</span></span>

<span data-ttu-id="c8a10-1556">As ***interfaces enumeráveis*** são a interface `System.Collections.IEnumerable` não genérica e todas as instanciações da interface `System.Collections.Generic.IEnumerable<T>`genérica.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="c8a10-1557">Por questão de brevidade, neste capítulo, essas interfaces são referenciadas como `IEnumerable` e `IEnumerable<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="c8a10-1558">Tipo yield</span><span class="sxs-lookup"><span data-stu-id="c8a10-1558">Yield type</span></span>

<span data-ttu-id="c8a10-1559">Um iterador produz uma sequência de valores, todos do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="c8a10-1560">Esse tipo é chamado de ***tipo yield*** do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="c8a10-1561">O tipo yield de um iterador que `IEnumerator` retorna `IEnumerable` ou `object`é.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="c8a10-1562">O tipo yield de um iterador que `IEnumerator<T>` retorna `IEnumerable<T>` ou `T`é.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="c8a10-1563">Objetos do enumerador</span><span class="sxs-lookup"><span data-stu-id="c8a10-1563">Enumerator objects</span></span>

<span data-ttu-id="c8a10-1564">Quando um membro de função que retorna um tipo de interface de enumerador é implementado usando um bloco de iterador, invocar o membro da função não executa imediatamente o código no bloco do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="c8a10-1565">Em vez disso, um ***objeto enumerador*** é criado e retornado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="c8a10-1566">Esse objeto encapsula o código especificado no bloco do iterador e a execução do código no bloco do iterador ocorre quando o método do `MoveNext` objeto enumerador é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="c8a10-1567">Um objeto enumerador tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="c8a10-1568">Ele implementa `IEnumerator` e `IEnumerator<T>`, onde `T` é o tipo yield do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="c8a10-1569">Ele implementa `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="c8a10-1570">Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor de instância passado para o membro da função.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="c8a10-1571">Ele tem quatro Estados potenciais, ***antes***, ***em execução***, ***suspenso***e ***depois***, e está inicialmente no estado ***anterior*** .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="c8a10-1572">Um objeto enumerador normalmente é uma instância de uma classe enumeradora gerada pelo compilador que encapsula o código no bloco iterador e implementa as interfaces do enumerador, mas outros métodos de implementação são possíveis.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="c8a10-1573">Se uma classe de enumerador for gerada pelo compilador, essa classe será aninhada, direta ou indiretamente, na classe que contém o membro da função, ela terá uma acessibilidade privada e terá um nome reservado para uso do compilador ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="c8a10-1574">Um objeto enumerador pode implementar mais interfaces do que aquelas especificadas acima.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="c8a10-1575">As seções a seguir descrevem o comportamento exato dos `MoveNext`Membros `Current`, e `Dispose` , das `IEnumerable` implementações `IEnumerable<T>` de interface e fornecidas por um objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="c8a10-1576">Observe que os objetos do enumerador não `IEnumerator.Reset` oferecem suporte ao método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="c8a10-1577">Invocar esse método faz com `System.NotSupportedException` que um seja gerado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="c8a10-1578">O método MoveNext</span><span class="sxs-lookup"><span data-stu-id="c8a10-1578">The MoveNext method</span></span>

<span data-ttu-id="c8a10-1579">O `MoveNext` método de um objeto de enumerador encapsula o código de um bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="c8a10-1580">Invocar o `MoveNext` método executa o código no bloco do iterador e define `Current` a propriedade do objeto enumerador conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="c8a10-1581">A ação precisa executada `MoveNext` depende do estado do objeto do enumerador quando `MoveNext` o é invocado:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="c8a10-1582">Se o estado do objeto enumerador for ***antes***, invocando `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="c8a10-1583">Altera o estado para ***em execução***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="c8a10-1584">Inicializa os parâmetros (incluindo `this`) do bloco do iterador para os valores de argumento e o valor de instância salvos quando o objeto enumerador foi inicializado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="c8a10-1585">Executa o bloco de iteradores desde o início até que a execução seja interrompida (conforme descrito abaixo).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="c8a10-1586">Se o estado do objeto enumerador estiver ***em execução***, o resultado da invocação `MoveNext` será não especificado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="c8a10-1587">Se o estado do objeto do enumerador for ***suspenso***, invocando `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="c8a10-1588">Altera o estado para ***em execução***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="c8a10-1589">Restaura os valores de todas as variáveis e parâmetros locais (incluindo isso) para os valores salvos quando a execução do bloco do iterador foi suspensa pela última vez.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="c8a10-1590">Observe que o conteúdo de quaisquer objetos referenciados por essas variáveis pode ter sido alterado desde a chamada anterior para MoveNext.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="c8a10-1591">Retoma a execução do bloco iterador imediatamente após a `yield return` instrução que causou a suspensão da execução e continua até que a execução seja interrompida (conforme descrito abaixo).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="c8a10-1592">Se o estado do objeto enumerador for ***After***, invocando `MoveNext` retorna `false`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="c8a10-1593">Quando `MoveNext` o executa o bloco do iterador, a execução pode ser interrompida de quatro maneiras: Por uma `yield return` instrução, por uma `yield break` instrução, encontrando o final do bloco do iterador e, por uma exceção sendo gerada e propagada do bloco do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="c8a10-1594">Quando uma `yield return` instrução é encontrada ([a instrução yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="c8a10-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="c8a10-1595">A expressão fornecida na instrução é avaliada, implicitamente convertida para o tipo yield e atribuída à `Current` Propriedade do objeto Enumerator.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="c8a10-1596">A execução do corpo do iterador é suspensa.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="c8a10-1597">Os valores de todas as variáveis e parâmetros locais ( `this`incluindo) são salvos, como é o local `yield return` desta instrução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="c8a10-1598">Se a `yield return` instrução estiver dentro de um ou `try` mais blocos, os `finally` blocos associados não serão executados neste momento.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="c8a10-1599">O estado do objeto do enumerador é alterado para ***suspenso***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="c8a10-1600">O `MoveNext` método retorna `true` ao chamador, indicando que a iteração foi avançada com êxito para o próximo valor.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="c8a10-1601">Quando uma `yield break` instrução é encontrada ([a instrução yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="c8a10-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="c8a10-1602">Se a `yield break` instrução estiver dentro de um ou `try` mais blocos, os `finally` blocos associados serão executados.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="c8a10-1603">O estado do objeto enumerador é alterado para ***After***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="c8a10-1604">O `MoveNext` método retorna `false` ao seu chamador, indicando que a iteração foi concluída.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="c8a10-1605">Quando o final do corpo do iterador for encontrado:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="c8a10-1606">O estado do objeto enumerador é alterado para ***After***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="c8a10-1607">O `MoveNext` método retorna `false` ao seu chamador, indicando que a iteração foi concluída.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="c8a10-1608">Quando uma exceção é gerada e propagada para fora do bloco do iterador:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="c8a10-1609">Os `finally` blocos apropriados no corpo do iterador serão executados pela propagação de exceção.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="c8a10-1610">O estado do objeto enumerador é alterado para ***After***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="c8a10-1611">A propagação de exceção continua para o chamador do `MoveNext` método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="c8a10-1612">A propriedade atual</span><span class="sxs-lookup"><span data-stu-id="c8a10-1612">The Current property</span></span>

<span data-ttu-id="c8a10-1613">A propriedade de `Current` um objeto enumerador é `yield return` afetada por instruções no bloco do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="c8a10-1614">Quando um objeto de enumerador está no estado ***suspenso*** , o valor `Current` de é o valor definido pela chamada anterior para `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="c8a10-1615">Quando um objeto enumerador está nos Estados ***anterior***, ***em execução***ou ***depois*** , o resultado do acesso `Current` não é especificado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="c8a10-1616">Para um iterador com um tipo yield diferente `object`de, o resultado do `Current` acesso por meio da implementação `IEnumerable` do objeto enumerador `Current` `IEnumerator<T>` corresponde ao acesso por meio do objeto enumerador implementação e conversão do resultado em `object`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="c8a10-1617">O método Dispose</span><span class="sxs-lookup"><span data-stu-id="c8a10-1617">The Dispose method</span></span>

<span data-ttu-id="c8a10-1618">O `Dispose` método é usado para limpar a iteração, trazendo o objeto enumerador para o estado ***After*** .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="c8a10-1619">Se o estado do objeto enumerador for ***antes***, invocar `Dispose` altera o estado para ***After***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="c8a10-1620">Se o estado do objeto enumerador estiver ***em execução***, o resultado da invocação `Dispose` será não especificado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="c8a10-1621">Se o estado do objeto do enumerador for ***suspenso***, invocando `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="c8a10-1622">Altera o estado para ***em execução***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="c8a10-1623">Executa qualquer bloco finally como se a última instrução executada `yield return` fosse uma `yield break` instrução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="c8a10-1624">Se isso fizer com que uma exceção seja lançada e propagada do corpo do iterador, o estado do objeto enumerador será definido como ***After*** e a exceção será propagada para o `Dispose` chamador do método.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="c8a10-1625">Altera o estado para ***depois***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="c8a10-1626">Se o estado do objeto enumerador for ***After***, a invocação `Dispose` não terá nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="c8a10-1627">Objetos enumeráveis</span><span class="sxs-lookup"><span data-stu-id="c8a10-1627">Enumerable objects</span></span>

<span data-ttu-id="c8a10-1628">Quando um membro de função que retorna um tipo de interface enumerável é implementado usando um bloco de iterador, invocar o membro da função não executa imediatamente o código no bloco do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="c8a10-1629">Em vez disso, um ***objeto enumerável*** é criado e retornado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="c8a10-1630">O método do `GetEnumerator` objeto Enumerable retorna um objeto enumerador que encapsula o código especificado no bloco do iterador e a execução do código no bloco do iterador ocorre quando o método do `MoveNext` objeto enumerador é invocado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="c8a10-1631">Um objeto enumerável tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="c8a10-1632">Ele implementa `IEnumerable` e `IEnumerable<T>`, onde `T` é o tipo yield do iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="c8a10-1633">Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor de instância passado para o membro da função.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="c8a10-1634">Um objeto enumerável normalmente é uma instância de uma classe enumerável gerada pelo compilador que encapsula o código no bloco do iterador e implementa as interfaces enumeráveis, mas outros métodos de implementação são possíveis.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="c8a10-1635">Se uma classe enumerável for gerada pelo compilador, essa classe será aninhada, direta ou indiretamente, na classe que contém o membro da função, ela terá uma acessibilidade privada e terá um nome reservado para uso do compilador ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="c8a10-1636">Um objeto enumerável pode implementar mais interfaces do que aquelas especificadas acima.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="c8a10-1637">Em particular, um objeto enumerável também pode `IEnumerator` implementar `IEnumerator<T>`e, permitindo que ele sirva como um enumerável e um enumerador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="c8a10-1638">Nesse tipo de implementação, na primeira vez que um método de `GetEnumerator` objeto enumerável é invocado, o próprio objeto enumerável é retornado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="c8a10-1639">As invocações subsequentes do objeto `GetEnumerator`Enumerable, se houver, retornam uma cópia do objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="c8a10-1640">Assim, cada enumerador retornado tem seu próprio Estado e as alterações em um enumerador não afetarão outra.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="c8a10-1641">O método GetEnumerator</span><span class="sxs-lookup"><span data-stu-id="c8a10-1641">The GetEnumerator method</span></span>

<span data-ttu-id="c8a10-1642">Um objeto enumerável fornece uma implementação dos `GetEnumerator` métodos `IEnumerable` das interfaces e `IEnumerable<T>` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="c8a10-1643">Os dois `GetEnumerator` métodos compartilham uma implementação comum que adquire e retorna um objeto enumerador disponível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="c8a10-1644">O objeto enumerador é inicializado com os valores de argumento e o valor da instância salvos quando o objeto Enumerable foi inicializado, mas, caso contrário, o objeto enumerador funciona conforme descrito em [objetos do enumerador](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="c8a10-1645">Exemplo de implementação</span><span class="sxs-lookup"><span data-stu-id="c8a10-1645">Implementation example</span></span>

<span data-ttu-id="c8a10-1646">Esta seção descreve uma possível implementação de iteradores em termos de construções C# padrão.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="c8a10-1647">A implementação descrita aqui se baseia nos mesmos princípios usados pelo compilador da Microsoft C# , mas não é por isso uma implementação obrigatória ou a única possível.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="c8a10-1648">A classe `Stack<T>` a seguir implementa `GetEnumerator` seu método usando um iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="c8a10-1649">O iterador enumera os elementos da pilha na ordem superior para a inferior.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="c8a10-1650">O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerada pelo compilador que encapsula o código no bloco do iterador, conforme mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="c8a10-1651">Na tradução anterior, o código no bloco do iterador é transformado em um computador de estado e `MoveNext` colocado no método da classe do enumerador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="c8a10-1652">Além disso, a variável `i` local é transformada em um campo no objeto Enumerator para que ele possa continuar a existir em invocações de. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="c8a10-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="c8a10-1653">O exemplo a seguir imprime uma tabela de multiplicação simples dos inteiros de 1 a 10.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="c8a10-1654">O `FromTo` método no exemplo retorna um objeto enumerável e é implementado usando um iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="c8a10-1655">O `FromTo` método pode ser convertido em uma instanciação de uma classe enumerável gerada pelo compilador que encapsula o código no bloco do iterador, conforme mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="c8a10-1656">A classe Enumerable implementa as interfaces enumeráveis e as interfaces do enumerador, permitindo que ele sirva como um enumerável e um enumerador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="c8a10-1657">Na primeira vez que `GetEnumerator` o método é invocado, o próprio objeto Enumerable é retornado.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="c8a10-1658">As invocações subsequentes do objeto `GetEnumerator`Enumerable, se houver, retornam uma cópia do objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="c8a10-1659">Assim, cada enumerador retornado tem seu próprio Estado e as alterações em um enumerador não afetarão outra.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="c8a10-1660">O `Interlocked.CompareExchange` método é usado para garantir a operação thread-safe.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="c8a10-1661">Os `from` parâmetros `to` e são transformados em campos na classe Enumerable.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="c8a10-1662">Como `from` é modificado no bloco do iterador, um `__from` campo adicional é introduzido para conter o valor inicial fornecido `from` para em cada enumerador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="c8a10-1663">O `MoveNext` método lançará `InvalidOperationException` um se for chamado quando `__state` for `0`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="c8a10-1664">Isso protege contra o uso do objeto Enumerable como um objeto enumerador sem primeiro `GetEnumerator`chamar.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="c8a10-1665">O exemplo a seguir mostra uma classe de árvore simples.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="c8a10-1666">A `Tree<T>` classe implementa seu `GetEnumerator` método usando um iterador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="c8a10-1667">O iterador enumera os elementos da árvore em ordem de infixo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="c8a10-1668">O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerada pelo compilador que encapsula o código no bloco do iterador, conforme mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="c8a10-1669">O compilador gerado temporaries usado nas `foreach` instruções é `__left` dividido nos campos e `__right` do objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="c8a10-1670">O `__state` campo do objeto enumerador é cuidadosamente atualizado para que o método `Dispose()` correto seja chamado corretamente se uma exceção for lançada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="c8a10-1671">Observe que não é possível escrever o código traduzido com instruções simples `foreach` .</span><span class="sxs-lookup"><span data-stu-id="c8a10-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="c8a10-1672">Funções assíncronas</span><span class="sxs-lookup"><span data-stu-id="c8a10-1672">Async functions</span></span>

<span data-ttu-id="c8a10-1673">Um método ([métodos](classes.md#methods)) ou uma função anônima ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) com o `async` modificador é chamado de ***função Async***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="c8a10-1674">Em geral, o termo ***Async*** é usado para descrever qualquer tipo de função que tenha o `async` modificador.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="c8a10-1675">É um erro de tempo de compilação para a lista de parâmetros formais de uma função Async para `ref` especificar `out` qualquer parâmetro ou.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="c8a10-1676">O *return_type* de um método assíncrono deve ser `void` ou um ***tipo de tarefa***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="c8a10-1677">Os tipos de tarefa `System.Threading.Tasks.Task` são e os tipos `System.Threading.Tasks.Task<T>`construídos a partir de.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="c8a10-1678">Para fins de brevidade, neste capítulo, esses tipos são referenciados como `Task` e `Task<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="c8a10-1679">Um método assíncrono que retorna um tipo de tarefa é considerado como sendo retornado por tarefa.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="c8a10-1680">A definição exata dos tipos de tarefa é a implementação definida, mas, no ponto de vista do idioma, um tipo de tarefa está em um dos Estados incompletos, com êxito ou com falha.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="c8a10-1681">Uma tarefa com falha registra uma exceção pertinente.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="c8a10-1682">Um êxito `Task<T>` registra um resultado do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="c8a10-1683">Os tipos de tarefa são awaitable e, portanto, podem ser os operandos das expressões Await ([Await expressões](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c8a10-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="c8a10-1684">Uma invocação de função assíncrona tem a capacidade de suspender a avaliação por meio de expressões Await ([expressões Await](expressions.md#await-expressions)) em seu corpo.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="c8a10-1685">Posteriormente, a avaliação pode ser retomada no ponto da expressão Await de suspensão por meio de um ***delegado de continuação***.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="c8a10-1686">O delegado de continuação é do tipo `System.Action`e, quando é invocado, a avaliação da invocação de função assíncrona retomará a partir da expressão Await onde ela parou.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="c8a10-1687">O ***chamador atual*** de uma invocação de função assíncrona é o chamador original se a invocação de função nunca foi suspensa ou o chamador mais recente do delegado de continuação, caso contrário.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="c8a10-1688">Avaliação de uma função assíncrona de retorno de tarefa</span><span class="sxs-lookup"><span data-stu-id="c8a10-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="c8a10-1689">A invocação de uma função assíncrona de retorno de tarefa faz com que uma instância do tipo de tarefa retornada seja gerada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="c8a10-1690">Isso é chamado de ***tarefa de retorno*** da função Async.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="c8a10-1691">A tarefa está inicialmente em um estado incompleto.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="c8a10-1692">O corpo da função assíncrona é então avaliado até que seja suspenso (atingindo uma expressão Await) ou seja encerrado, no qual o controle de ponto é retornado ao chamador, junto com a tarefa de retorno.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="c8a10-1693">Quando o corpo da função Async é encerrado, a tarefa de retorno é movida do estado incompleto:</span><span class="sxs-lookup"><span data-stu-id="c8a10-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="c8a10-1694">Se o corpo da função terminar como o resultado de atingir uma instrução de retorno ou o final do corpo, qualquer valor de resultado será registrado na tarefa de retorno, que será colocado em um estado de êxito.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="c8a10-1695">Se o corpo da função for encerrado como resultado de uma exceção não percebida ([a instrução Throw](statements.md#the-throw-statement)), a exceção será registrada na tarefa de retorno que será colocada em um estado com falha.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="c8a10-1696">Avaliação de uma função assíncrona de retorno nulo</span><span class="sxs-lookup"><span data-stu-id="c8a10-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="c8a10-1697">Se o tipo de retorno da função Async for `void`, a avaliação será diferente da seguinte maneira: Como nenhuma tarefa é retornada, a função comunica a conclusão e as exceções ao ***contexto de sincronização***do thread atual.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="c8a10-1698">A definição exata do contexto de sincronização é dependente da implementação, mas é uma representação de "onde" o thread atual está em execução.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="c8a10-1699">O contexto de sincronização é notificado quando a avaliação de uma função assíncrona de retorno nulo começa, é concluída com êxito ou faz com que uma exceção não percebida seja gerada.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="c8a10-1700">Isso permite que o contexto Mantenha o controle de quantas funções assíncronas de retorno nulo estão em execução e para decidir como propagar exceções saindo delas.</span><span class="sxs-lookup"><span data-stu-id="c8a10-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
