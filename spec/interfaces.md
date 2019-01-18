---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229479"
---
# <a name="interfaces"></a><span data-ttu-id="163d5-101">Interfaces</span><span class="sxs-lookup"><span data-stu-id="163d5-101">Interfaces</span></span>

<span data-ttu-id="163d5-102">Uma interface define um contrato.</span><span class="sxs-lookup"><span data-stu-id="163d5-102">An interface defines a contract.</span></span> <span data-ttu-id="163d5-103">Uma classe ou struct que implementa uma interface deve cumprir o contrato.</span><span class="sxs-lookup"><span data-stu-id="163d5-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="163d5-104">Uma interface pode herdar de várias interfaces base e uma classe ou struct pode implementar várias interfaces.</span><span class="sxs-lookup"><span data-stu-id="163d5-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="163d5-105">Interfaces podem conter métodos, propriedades, eventos e indexadores.</span><span class="sxs-lookup"><span data-stu-id="163d5-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="163d5-106">A própria interface não fornece implementações dos membros que ela define.</span><span class="sxs-lookup"><span data-stu-id="163d5-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="163d5-107">A interface simplesmente especifica os membros que devem ser fornecidos por classes ou estruturas que implementam a interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="163d5-108">Declarações de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-108">Interface declarations</span></span>

<span data-ttu-id="163d5-109">Uma *interface_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara um novo tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="163d5-110">Uma *interface_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *interface_modifier*s ([Interface modificadores](interfaces.md#interface-modifiers)), seguido por um recurso opcional `partial` modificador, seguido da palavra-chave `interface` e uma *identificador* que nomeia a interface, seguido por um recurso opcional *variant_type_parameter_list* especificação ([listas de parâmetros de tipo de variante](interfaces.md#variant-type-parameter-lists)), seguido por um recurso opcional *interface_base* especificação ([interfaces Base](interfaces.md#base-interfaces)), seguido por um recurso opcional *type_parameter_constraints_clause*especificação s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) , seguido por um *interface_body* ([Interface corpo](interfaces.md#interface-body)), opcionalmente seguido por um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="163d5-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="163d5-111">Modificadores de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-111">Interface modifiers</span></span>

<span data-ttu-id="163d5-112">Uma *interface_declaration* opcionalmente pode incluir uma sequência de modificadores de interface:</span><span class="sxs-lookup"><span data-stu-id="163d5-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="163d5-113">Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="163d5-114">O `new` modificador é permitido somente em interfaces definidas dentro de uma classe.</span><span class="sxs-lookup"><span data-stu-id="163d5-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="163d5-115">Especifica que a interface oculta um membro herdado com o mesmo nome, conforme descrito em [o novo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="163d5-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="163d5-116">O `public`, `protected`, `internal`, e `private` modificadores controlam a acessibilidade da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="163d5-117">Dependendo do contexto no qual ocorre a declaração de interface, apenas alguns desses modificadores podem ser permitida ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="163d5-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="163d5-118">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="163d5-118">Partial modifier</span></span>

<span data-ttu-id="163d5-119">O `partial` modificador indica que isso *interface_declaration* é uma declaração de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="163d5-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="163d5-120">Combinam várias declarações de interface parcial com o mesmo nome dentro de uma declaração de namespace ou tipo delimitador à declaração de uma interface de formulário, seguindo as regras especificadas na [tipos parciais](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="163d5-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="163d5-121">Listas de parâmetros de tipo de variante</span><span class="sxs-lookup"><span data-stu-id="163d5-121">Variant type parameter lists</span></span>

<span data-ttu-id="163d5-122">Listas de parâmetros de tipo de variante podem ocorrer somente em tipos de interface e delegados.</span><span class="sxs-lookup"><span data-stu-id="163d5-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="163d5-123">A diferença da comum *type_parameter_list*s é opcional *variance_annotation* em cada parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="163d5-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="163d5-124">Se for a anotação de variância `out`, o parâmetro de tipo será considerado ***covariante***.</span><span class="sxs-lookup"><span data-stu-id="163d5-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="163d5-125">Se for a anotação de variância `in`, o parâmetro de tipo será considerado ***contravariante***.</span><span class="sxs-lookup"><span data-stu-id="163d5-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="163d5-126">Se não houver nenhuma anotação de variância, o parâmetro de tipo deve ser ***invariável***.</span><span class="sxs-lookup"><span data-stu-id="163d5-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="163d5-127">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="163d5-128">`X` é covariante, `Y` é contravariante e `Z` é invariável.</span><span class="sxs-lookup"><span data-stu-id="163d5-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="163d5-129">Segurança de variação</span><span class="sxs-lookup"><span data-stu-id="163d5-129">Variance safety</span></span>

<span data-ttu-id="163d5-130">A ocorrência de anotações de variância na lista de parâmetros de tipo de um tipo restringe os locais onde os tipos podem ocorrer dentro da declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="163d5-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="163d5-131">Um tipo `T` está ***saída unsafe*** se tiver mantido por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="163d5-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="163d5-132">`T` é um parâmetro de tipo contravariantes</span><span class="sxs-lookup"><span data-stu-id="163d5-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="163d5-133">`T` é um tipo de matriz com um tipo de elemento não segura de saída</span><span class="sxs-lookup"><span data-stu-id="163d5-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="163d5-134">`T` é um tipo de interface ou delegado `S<A1,...,Ak>` construídos a partir de um tipo genérico `S<X1,...,Xk>` where para pelo menos um `Ai` mantém por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="163d5-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="163d5-135">`Xi` é covariante ou invariável e `Ai` não é seguro de saída.</span><span class="sxs-lookup"><span data-stu-id="163d5-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="163d5-136">`Xi` é contravariante ou invariável e `Ai` segura para a entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="163d5-137">Um tipo `T` está ***entrada unsafe*** se tiver mantido por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="163d5-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="163d5-138">`T` é um parâmetro de tipo covariantes</span><span class="sxs-lookup"><span data-stu-id="163d5-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="163d5-139">`T` é um tipo de matriz com um tipo de elemento de entrada-unsafe</span><span class="sxs-lookup"><span data-stu-id="163d5-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="163d5-140">`T` é um tipo de interface ou delegado `S<A1,...,Ak>` construídos a partir de um tipo genérico `S<X1,...,Xk>` where para pelo menos um `Ai` mantém por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="163d5-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="163d5-141">`Xi` é covariante ou invariável e `Ai` não é seguro de entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="163d5-142">`Xi` é contravariante ou invariável e `Ai` não é seguro de saída.</span><span class="sxs-lookup"><span data-stu-id="163d5-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="163d5-143">Intuitivamente, um tipo de saída-unsafe é proibido em uma posição de saída e um tipo de entrada-unsafe for proibido em uma posição de entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="163d5-144">É um tipo ***saída-safe*** se não for inseguro saída, e ***safe entrada*** se não for inseguro de entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="163d5-145">Conversão de variância</span><span class="sxs-lookup"><span data-stu-id="163d5-145">Variance conversion</span></span>

<span data-ttu-id="163d5-146">A finalidade de anotações de variância é fornecer conversões mais branda (mas ainda fortemente tipado) em tipos de interface e delegados.</span><span class="sxs-lookup"><span data-stu-id="163d5-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="163d5-147">Para isso terminar as definições de implícita ([conversões implícitas](conversions.md#implicit-conversions)) e as conversões explícitas ([conversões explícitas](conversions.md#explicit-conversions)) fazer usam a noção da convertibilidade de variância, que é definida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="163d5-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="163d5-148">Um tipo `T<A1,...,An>` é convertido por variância em um tipo `T<B1,...,Bn>` se `T` é uma interface ou um tipo de delegado declarado com os parâmetros de tipo de variante `T<X1,...,Xn>`e para cada parâmetro de tipo de variante `Xi` um dos seguintes contém:</span><span class="sxs-lookup"><span data-stu-id="163d5-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="163d5-149">`Xi` é covariante e existe uma conversão implícita de referência ou a identidade do `Ai` para `Bi`</span><span class="sxs-lookup"><span data-stu-id="163d5-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="163d5-150">`Xi` é contravariante e uma referência implícita ou existe conversão de identidade do `Bi` para `Ai`</span><span class="sxs-lookup"><span data-stu-id="163d5-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="163d5-151">`Xi` é invariável e uma identidade existe conversão do `Ai` para `Bi`</span><span class="sxs-lookup"><span data-stu-id="163d5-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="163d5-152">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="163d5-152">Base interfaces</span></span>

<span data-ttu-id="163d5-153">Uma interface pode herdar de zero ou mais tipos de interface, que são chamados de ***interfaces base explícitas*** da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="163d5-154">Quando uma interface tem um ou mais interfaces base explícitas, em seguida, na declaração de interface, o identificador de interface é seguido por dois-pontos e vírgulas lista dos tipos de base de interface separada.</span><span class="sxs-lookup"><span data-stu-id="163d5-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="163d5-155">Para um tipo de interface construída, as interfaces base explícitas são formadas retirando as declarações explícitas de interface base na declaração de tipo genérico e substituir, para cada *type_parameter* na interface de base declaração, correspondente *type_argument* do tipo construído.</span><span class="sxs-lookup"><span data-stu-id="163d5-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="163d5-156">As interfaces base explícitas de uma interface devem ser pelo menos tão acessíveis quanto a própria interface ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="163d5-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="163d5-157">Por exemplo, ele é um erro de tempo de compilação para especificar uma `private` ou `internal` interface na *interface_base* de um `public` interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="163d5-158">Ele é um erro de tempo de compilação para uma interface herdada direta ou indiretamente de si mesma.</span><span class="sxs-lookup"><span data-stu-id="163d5-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="163d5-159">O ***interfaces base*** de uma interface são as interfaces base explícitas e suas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="163d5-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="163d5-160">Em outras palavras, o conjunto de interfaces base é o fechamento transitivo completo as interfaces base explícitas, suas interfaces base explícitas e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="163d5-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="163d5-161">Uma interface herda todos os membros de suas interfaces de base.</span><span class="sxs-lookup"><span data-stu-id="163d5-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="163d5-162">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="163d5-163">as interfaces base da `IComboBox` estão `IControl`, `ITextBox`, e `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="163d5-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="163d5-164">Em outras palavras, o `IComboBox` acima da interface herda membros `SetText` e `SetItems` , bem como `Paint`.</span><span class="sxs-lookup"><span data-stu-id="163d5-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="163d5-165">Cada interface base de uma interface deve ser segura de saída ([safety variação](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="163d5-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="163d5-166">Uma classe ou struct que implementa uma interface implicitamente também implementa todas as interfaces de base da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="163d5-167">Corpo de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-167">Interface body</span></span>

<span data-ttu-id="163d5-168">O *interface_body* de uma interface define os membros da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="163d5-169">Membros de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-169">Interface members</span></span>

<span data-ttu-id="163d5-170">Os membros de uma interface são os membros herdados de interfaces base e os membros declarados pela interface em si.</span><span class="sxs-lookup"><span data-stu-id="163d5-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="163d5-171">Uma declaração de interface pode declarar zero ou mais membros.</span><span class="sxs-lookup"><span data-stu-id="163d5-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="163d5-172">Os membros de uma interface devem ser métodos, propriedades, eventos ou indexadores.</span><span class="sxs-lookup"><span data-stu-id="163d5-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="163d5-173">Uma interface não pode conter constantes, campos, operadores, construtores de instância, destruidores ou tipos, nem pode conter de uma interface membros estáticos de qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="163d5-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="163d5-174">Todos os membros de interface implicitamente têm acesso público.</span><span class="sxs-lookup"><span data-stu-id="163d5-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="163d5-175">É um erro de tempo de compilação para declarações de membro de interface incluir qualquer modificador.</span><span class="sxs-lookup"><span data-stu-id="163d5-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="163d5-176">Em particular, os membros de interfaces não podem ser declarados com os modificadores `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, ou `static`.</span><span class="sxs-lookup"><span data-stu-id="163d5-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="163d5-177">O exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="163d5-178">declara uma interface que contém um dos tipos possíveis de membros: Um método, uma propriedade, um evento e um indexador.</span><span class="sxs-lookup"><span data-stu-id="163d5-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="163d5-179">Uma *interface_declaration* cria um novo espaço de declaração ([declarações](basic-concepts.md#declarations)) e o *interface_member_declaration*s imediatamente contido pelo *interface_declaration* introduzir novos membros nesse espaço de declaração.</span><span class="sxs-lookup"><span data-stu-id="163d5-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="163d5-180">As seguintes regras se aplicam a *interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="163d5-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="163d5-181">O nome de um método deve ser diferente dos nomes de todas as propriedades e eventos declarados na mesma interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="163d5-182">Além disso, a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma interface, e dois métodos declarados na mesma interface podem não ter assinaturas que diferem somente por `ref` e `out`.</span><span class="sxs-lookup"><span data-stu-id="163d5-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="163d5-183">O nome de uma propriedade ou evento deve ser diferente dos nomes de todos os outros membros declarados na mesma interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="163d5-184">A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="163d5-185">Os membros herdados de uma interface são especificamente não faz parte do espaço de declaração da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="163d5-186">Portanto, uma interface pode declarar um membro com o mesmo nome ou assinatura como um membro herdado.</span><span class="sxs-lookup"><span data-stu-id="163d5-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="163d5-187">Quando isso ocorrer, o membro de interface derivado deve ocultar o membro de interface base.</span><span class="sxs-lookup"><span data-stu-id="163d5-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="163d5-188">Ocultar um membro herdado não é considerado um erro, mas ele faz com que o compilador emita um aviso.</span><span class="sxs-lookup"><span data-stu-id="163d5-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="163d5-189">Para suprimir o aviso, a declaração do membro de interface derivado deve incluir um `new` modificador para indicar que o membro derivado destina-se para ocultar o membro base.</span><span class="sxs-lookup"><span data-stu-id="163d5-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="163d5-190">Este tópico é discutido mais detalhadamente em [ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="163d5-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="163d5-191">Se um `new` modificador é incluído em uma declaração que não oculta um membro herdado, um aviso será emitido para esse efeito.</span><span class="sxs-lookup"><span data-stu-id="163d5-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="163d5-192">Esse aviso é suprimido, removendo o `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="163d5-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="163d5-193">Observe que os membros na classe `object` não é, estritamente falando, os membros de qualquer interface ([membros da Interface](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="163d5-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="163d5-194">No entanto, os membros na classe `object` estão disponíveis por meio de pesquisa de membro em qualquer tipo de interface ([pesquisa de membro](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="163d5-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="163d5-195">Métodos de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-195">Interface methods</span></span>

<span data-ttu-id="163d5-196">Métodos de interface são declarados usando *interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="163d5-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="163d5-197">O *atributos*, *return_type*, *identificador*, e *formal_parameter_list* de uma declaração de método de interface têm o mesmo que significa que as de uma declaração de método em uma classe ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="163d5-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="163d5-198">Uma declaração de método de interface não é permitida especificar um corpo de método e a declaração, portanto, sempre termine com um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="163d5-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="163d5-199">Cada tipo de parâmetro formal de um método de interface deve ser de entrada-safe ([safety variação](interfaces.md#variance-safety)), e o tipo de retorno deve ser `void` ou saída-safe.</span><span class="sxs-lookup"><span data-stu-id="163d5-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="163d5-200">Além disso, cada restrição de tipo de classe, a restrição de tipo de interface e a restrição de parâmetro de tipo em qualquer parâmetro de tipo do método devem ser seguro de entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="163d5-201">Essas regras garantem que qualquer covariant ou contravariant uso da interface permanece fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="163d5-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="163d5-202">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="163d5-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="163d5-203">é ilegal porque o uso de `T` como uma restrição de parâmetro de tipo em `U` não for seguro de entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="163d5-204">Não foram essa restrição em vigor seria possível de violar a segurança de tipos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="163d5-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="163d5-205">Isso é, na verdade, uma chamada para `C.M<E>`.</span><span class="sxs-lookup"><span data-stu-id="163d5-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="163d5-206">Mas essa chamada requer que `E` derivam `D`, portanto, a segurança de tipo seja violada aqui.</span><span class="sxs-lookup"><span data-stu-id="163d5-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="163d5-207">Propriedades de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-207">Interface properties</span></span>

<span data-ttu-id="163d5-208">Propriedades de interface são declaradas usando *interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="163d5-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="163d5-209">O *atributos*, *tipo*, e *identificador* de uma declaração de propriedade de interface têm o mesmo significado que aquelas de uma declaração de propriedade em uma classe ([ Propriedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="163d5-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="163d5-210">Os acessadores de uma declaração de propriedade de interface correspondem aos acessadores de uma declaração de propriedade de classe ([acessadores](classes.md#accessors)), exceto que o corpo do acessador deve sempre ser um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="163d5-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="163d5-211">Portanto, os acessadores simplesmente indicam se a propriedade é leitura / gravação, somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="163d5-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="163d5-212">O tipo de uma propriedade de interface deve ser segura de saída se houver um acessador get e deve ser a entrada segura se não houver um acessador set.</span><span class="sxs-lookup"><span data-stu-id="163d5-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="163d5-213">Eventos de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-213">Interface events</span></span>

<span data-ttu-id="163d5-214">Eventos de interface são declarados usando *interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="163d5-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="163d5-215">O *atributos*, *tipo*, e *identificador* de uma declaração de evento de interface têm o mesmo significado que aquelas de uma declaração de evento em uma classe ([eventos ](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="163d5-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="163d5-216">O tipo de um evento de interface deve ser seguro de entrada.</span><span class="sxs-lookup"><span data-stu-id="163d5-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="163d5-217">Indexadores de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-217">Interface indexers</span></span>

<span data-ttu-id="163d5-218">Indexadores de interface são declarados usando *interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="163d5-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="163d5-219">O *atributos*, *tipo*, e *formal_parameter_list* de uma declaração de interface do indexador têm o mesmo significado que aquelas de uma declaração de indexador em uma classe ([ Indexadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="163d5-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="163d5-220">Os acessadores de uma declaração de interface do indexador correspondem aos acessadores de uma declaração de indexador de classe ([indexadores](classes.md#indexers)), exceto que o corpo do acessador deve sempre ser um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="163d5-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="163d5-221">Assim, os acessadores simplesmente indicam se o indexador é leitura / gravação, somente leitura ou somente gravação.</span><span class="sxs-lookup"><span data-stu-id="163d5-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="163d5-222">Todos os tipos de parâmetro formal de um indexador de interface devem ser de entrada-safe.</span><span class="sxs-lookup"><span data-stu-id="163d5-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="163d5-223">Além disso, qualquer `out` ou `ref` tipos de parâmetro formal também devem ser segura de saída.</span><span class="sxs-lookup"><span data-stu-id="163d5-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="163d5-224">Observe que mesmo `out` parâmetros devem ser de entrada-safe, devido a uma limitação da plataforma subjacente da execução.</span><span class="sxs-lookup"><span data-stu-id="163d5-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="163d5-225">O tipo de um indexador de interface deve ser segura de saída se houver um acessador get e deve ser a entrada segura se não houver um acessador set.</span><span class="sxs-lookup"><span data-stu-id="163d5-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="163d5-226">Acesso de membro de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-226">Interface member access</span></span>

<span data-ttu-id="163d5-227">Os membros de interface são acessados por meio do acesso de membro ([acesso de membro](expressions.md#member-access)) e acesso do indexador ([acesso ao indexador](expressions.md#indexer-access)) expressões do formulário `I.M` e `I[A]`, onde `I` é um tipo de interface `M` é um método, propriedade ou evento desse tipo de interface, e `A` é uma lista de argumentos do indexador.</span><span class="sxs-lookup"><span data-stu-id="163d5-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="163d5-228">Para interfaces que são estritamente herança única (a cada interface na cadeia de herança tem exatamente zero ou uma interface base direta), os efeitos da pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)), invocação de método ([ As invocações de método](expressions.md#method-invocations)) e o acesso do indexador ([acesso do indexador](expressions.md#indexer-access)) as regras são exatamente as mesmas para classes e estruturas: Mais derivado ocultar membros menos derivados membros com o mesmo nome ou assinatura.</span><span class="sxs-lookup"><span data-stu-id="163d5-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="163d5-229">No entanto, para as interfaces de herança múltipla, as ambiguidades podem ocorrer quando dois ou mais interfaces base não relacionadas declaram membros com o mesmo nome ou assinatura.</span><span class="sxs-lookup"><span data-stu-id="163d5-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="163d5-230">Esta seção mostra vários exemplos de tais situações.</span><span class="sxs-lookup"><span data-stu-id="163d5-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="163d5-231">Em todos os casos, as conversões explícitas podem ser usados para resolver as ambiguidades.</span><span class="sxs-lookup"><span data-stu-id="163d5-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="163d5-232">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="163d5-233">as duas primeiras instruções causam erros de tempo de compilação, porque a pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `Count` em `IListCounter` é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="163d5-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="163d5-234">Conforme ilustrado pelo exemplo, a ambiguidade é resolvida com a conversão `x` para o tipo de interface base adequada.</span><span class="sxs-lookup"><span data-stu-id="163d5-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="163d5-235">Tal conversão ter sem custos de tempo de execução — eles simplesmente consistem em exibindo a instância como um tipo menos derivado no tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="163d5-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="163d5-236">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="163d5-237">a invocação `n.Add(1)` seleciona `IInteger.Add` aplicando as regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="163d5-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="163d5-238">Da mesma forma, a invocação `n.Add(1.0)` seleciona `IDouble.Add`.</span><span class="sxs-lookup"><span data-stu-id="163d5-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="163d5-239">Quando conversões explícitas são inseridas, há apenas um candidato método e, portanto, nenhuma ambiguidade.</span><span class="sxs-lookup"><span data-stu-id="163d5-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="163d5-240">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="163d5-241">o `IBase.F` membro é ocultado pelo `ILeft.F` membro.</span><span class="sxs-lookup"><span data-stu-id="163d5-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="163d5-242">A invocação `d.F(1)` , portanto, seleciona `ILeft.F`, embora `IBase.F` parece não estar oculto no caminho de acesso que o orienta ao longo das `IRight`.</span><span class="sxs-lookup"><span data-stu-id="163d5-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="163d5-243">A regra intuitiva para ocultação de nas interfaces de herança múltipla é simplesmente isso: Se um membro é ocultado em qualquer caminho de acesso, ele ficará oculta em todos os caminhos de acesso.</span><span class="sxs-lookup"><span data-stu-id="163d5-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="163d5-244">Porque o caminho de acesso de `IDerived` à `ILeft` para `IBase` oculta `IBase.F`, o membro é ocultado também no caminho de acesso de `IDerived` para `IRight` para `IBase`.</span><span class="sxs-lookup"><span data-stu-id="163d5-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="163d5-245">Nomes de membro de interface totalmente qualificado</span><span class="sxs-lookup"><span data-stu-id="163d5-245">Fully qualified interface member names</span></span>

<span data-ttu-id="163d5-246">Às vezes, um membro de interface é referenciado por seu ***nome totalmente qualificado***.</span><span class="sxs-lookup"><span data-stu-id="163d5-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="163d5-247">O nome totalmente qualificado de um membro de interface consiste do nome da interface na qual o membro é declarado, seguido por um ponto, seguido do nome do membro.</span><span class="sxs-lookup"><span data-stu-id="163d5-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="163d5-248">O nome totalmente qualificado de um membro faz referência a interface na qual o membro é declarado.</span><span class="sxs-lookup"><span data-stu-id="163d5-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="163d5-249">Por exemplo, dadas as declarações</span><span class="sxs-lookup"><span data-stu-id="163d5-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="163d5-250">o nome totalmente qualificado da `Paint` está `IControl.Paint` e o nome totalmente qualificado do `SetText` é `ITextBox.SetText`.</span><span class="sxs-lookup"><span data-stu-id="163d5-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="163d5-251">No exemplo acima, não é possível se referir `Paint` como `ITextBox.Paint`.</span><span class="sxs-lookup"><span data-stu-id="163d5-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="163d5-252">Quando uma interface é parte de um namespace, o nome totalmente qualificado de um membro de interface inclui o nome do namespace.</span><span class="sxs-lookup"><span data-stu-id="163d5-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="163d5-253">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="163d5-254">Aqui, o nome totalmente qualificado do `Clone` método é `System.ICloneable.Clone`.</span><span class="sxs-lookup"><span data-stu-id="163d5-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="163d5-255">Implementações de interfaces</span><span class="sxs-lookup"><span data-stu-id="163d5-255">Interface implementations</span></span>

<span data-ttu-id="163d5-256">Interfaces podem ser implementados por classes e estruturas.</span><span class="sxs-lookup"><span data-stu-id="163d5-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="163d5-257">Para indicar que uma classe ou struct implementa diretamente uma interface, o identificador de interface está incluído na lista de classe base da classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="163d5-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="163d5-258">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="163d5-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="163d5-259">Uma classe ou struct que implementa diretamente uma interface diretamente também implementa todas as interfaces de base da interface implicitamente.</span><span class="sxs-lookup"><span data-stu-id="163d5-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="163d5-260">Isso é verdadeiro mesmo se a classe ou struct explicitamente não lista todas as interfaces base na lista de classes base.</span><span class="sxs-lookup"><span data-stu-id="163d5-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="163d5-261">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="163d5-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="163d5-262">Aqui, a classe `TextBox` implementa ambos `IControl` e `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="163d5-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="163d5-263">Quando uma classe `C` diretamente implementa uma interface, todas as classes derivadas C também implementa a interface implicitamente.</span><span class="sxs-lookup"><span data-stu-id="163d5-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="163d5-264">As interfaces base especificadas em uma declaração de classe podem ser tipos de interface construída ([construído tipos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="163d5-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="163d5-265">Uma interface de base não pode ser um parâmetro de tipo por conta própria, embora isso pode envolver os parâmetros de tipo que estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="163d5-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="163d5-266">O código a seguir ilustra como uma classe pode implementar e estender tipos construídos:</span><span class="sxs-lookup"><span data-stu-id="163d5-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="163d5-267">As interfaces de base de uma declaração de classe genérica devem satisfazer a regra de exclusividade descrita em [exclusividade de interfaces implementadas](interfaces.md#uniqueness-of-implemented-interfaces).</span><span class="sxs-lookup"><span data-stu-id="163d5-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="163d5-268">Implementações de membros de interface explícita</span><span class="sxs-lookup"><span data-stu-id="163d5-268">Explicit interface member implementations</span></span>

<span data-ttu-id="163d5-269">Para fins de implementação de interfaces, uma classe ou struct pode declarar ***implementações de membros de interface explícita***.</span><span class="sxs-lookup"><span data-stu-id="163d5-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="163d5-270">Uma implementação de membro de interface explícita é uma declaração de método, propriedade, evento ou indexador que faz referência a um nome de membro de interface totalmente qualificado.</span><span class="sxs-lookup"><span data-stu-id="163d5-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="163d5-271">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="163d5-272">Aqui `IDictionary<int,T>.this` e `IDictionary<int,T>.Add` é implementações de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="163d5-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="163d5-273">Em alguns casos, o nome de um membro de interface não pode ser apropriado para a classe de implementação, nesse caso, o membro de interface pode ser implementado usando a implementação de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="163d5-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="163d5-274">Uma classe que implementa uma abstração de arquivo, por exemplo, provavelmente seria implementar uma `Close` função de membro que tem o efeito de liberar o recurso de arquivo e implementar o `Dispose` método da `IDisposable` usando a interface explícita da interface implementação de membro:</span><span class="sxs-lookup"><span data-stu-id="163d5-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="163d5-275">Não é possível acessar uma implementação de membro de interface explícita por meio de seu nome totalmente qualificado em uma invocação de método, acesso de propriedade ou acesso do indexador.</span><span class="sxs-lookup"><span data-stu-id="163d5-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="163d5-276">Uma implementação de membro de interface explícita só pode ser acessada por meio de uma instância da interface e, nesse caso é referenciada apenas por seu nome de membro.</span><span class="sxs-lookup"><span data-stu-id="163d5-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="163d5-277">É um erro de tempo de compilação para uma implementação de membro de interface explícita incluir os modificadores de acesso, e ele é um erro de tempo de compilação para incluir os modificadores `abstract`, `virtual`, `override`, ou `static`.</span><span class="sxs-lookup"><span data-stu-id="163d5-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="163d5-278">Implementações de membros de interface explícita têm características de acessibilidade diferente de outros membros.</span><span class="sxs-lookup"><span data-stu-id="163d5-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="163d5-279">Como as implementações de membros de interface explícita nunca são acessíveis por meio de seus nomes totalmente qualificados em uma invocação de método ou um acesso de propriedade, eles são de certa forma privada.</span><span class="sxs-lookup"><span data-stu-id="163d5-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="163d5-280">No entanto, desde que eles podem ser acessados por meio de uma instância da interface, eles estão em um sentido público também.</span><span class="sxs-lookup"><span data-stu-id="163d5-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="163d5-281">Implementações de membros de interface explícita têm duas finalidades principais:</span><span class="sxs-lookup"><span data-stu-id="163d5-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="163d5-282">Como as implementações de membros de interface explícita não são acessíveis por meio de instâncias de classe ou struct, eles permitem que as implementações de interface a ser excluído da interface pública de uma classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="163d5-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="163d5-283">Isso é particularmente útil quando uma classe ou struct implementa uma interface interna que não interessa para um consumidor dessa classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="163d5-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="163d5-284">Implementações de membros de interface explícita permitem desambiguidade de membros de interface com a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="163d5-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="163d5-285">Sem as implementações de membros de interface explícita seria impossível para uma classe ou struct têm implementações diferentes de membros com a mesma assinatura de interface e tipo de retorno, como ela seria impossível para uma classe ou struct ter qualquer implementação em todos os membros de interface com a mesma assinatura, mas com diferentes tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="163d5-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="163d5-286">Para uma implementação de membro de interface explícita seja válida, a classe ou struct deve nomear uma interface em sua lista de classes base que contém um membro cujo nome totalmente qualificado, tipo e tipos de parâmetro corresponderem exatamente do membro de interface explícita implementação.</span><span class="sxs-lookup"><span data-stu-id="163d5-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="163d5-287">Assim, na classe</span><span class="sxs-lookup"><span data-stu-id="163d5-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="163d5-288">a declaração de `IComparable.CompareTo` resulta em um erro de tempo de compilação porque `IComparable` não estiver na lista de classes base `Shape` e não é uma interface de base de `ICloneable`.</span><span class="sxs-lookup"><span data-stu-id="163d5-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="163d5-289">Da mesma forma, nas declarações</span><span class="sxs-lookup"><span data-stu-id="163d5-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="163d5-290">a declaração de `ICloneable.Clone` na `Ellipse` resulta em um erro de tempo de compilação porque `ICloneable` não listados explicitamente na lista de classes base `Ellipse`.</span><span class="sxs-lookup"><span data-stu-id="163d5-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="163d5-291">O nome totalmente qualificado de um membro de interface deve fazer referência a interface em que o membro foi declarado.</span><span class="sxs-lookup"><span data-stu-id="163d5-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="163d5-292">Dessa forma, nas declarações</span><span class="sxs-lookup"><span data-stu-id="163d5-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="163d5-293">a implementação de membro de interface explícita `Paint` deve ser escrito como `IControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="163d5-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="163d5-294">Exclusividade de interfaces implementadas</span><span class="sxs-lookup"><span data-stu-id="163d5-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="163d5-295">As interfaces implementadas por uma declaração de tipo genérico devem permanecer exclusivas para todos os possíveis tipos construídos.</span><span class="sxs-lookup"><span data-stu-id="163d5-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="163d5-296">Sem essa regra, seria impossível determinar o método correto para chamar para determinados tipos construídos.</span><span class="sxs-lookup"><span data-stu-id="163d5-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="163d5-297">Por exemplo, suponha que uma declaração de classe genérica puderem ser escrita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="163d5-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="163d5-298">Foram isso permitido, seria impossível determinar qual código deve ser executado no caso a seguir:</span><span class="sxs-lookup"><span data-stu-id="163d5-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="163d5-299">Para determinar se a lista de interface de uma declaração de tipo genérico é válida, as etapas a seguir são executadas:</span><span class="sxs-lookup"><span data-stu-id="163d5-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="163d5-300">Deixe `L` a lista de interfaces especificada diretamente em uma classe genérica, struct ou declaração de interface `C`.</span><span class="sxs-lookup"><span data-stu-id="163d5-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="163d5-301">Adicionar ao `L` quaisquer base interfaces das interfaces já está sendo `L`.</span><span class="sxs-lookup"><span data-stu-id="163d5-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="163d5-302">Remova todas as duplicatas de `L`.</span><span class="sxs-lookup"><span data-stu-id="163d5-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="163d5-303">Se qualquer possível construído criado a partir do tipo `C` seria, depois de argumentos de tipo são substituídos na `L`, fazer com que duas interfaces no `L` ser idênticos, em seguida, a declaração de `C` é inválido.</span><span class="sxs-lookup"><span data-stu-id="163d5-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="163d5-304">Declarações de restrição não são consideradas ao determinar todos os possíveis tipos construídos.</span><span class="sxs-lookup"><span data-stu-id="163d5-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="163d5-305">Na declaração da classe `X` acima, a lista de interfaces `L` consiste `I<U>` e `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="163d5-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="163d5-306">A declaração é inválida porque qualquer tipo com construído `U` e `V` sendo do mesmo tipo faria com que essas duas interfaces são tipos idênticos.</span><span class="sxs-lookup"><span data-stu-id="163d5-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="163d5-307">É possível para interfaces especificadas em níveis diferentes de herança para unificar:</span><span class="sxs-lookup"><span data-stu-id="163d5-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="163d5-308">Esse código é válido, embora `Derived<U,V>` implementa ambos `I<U>` e `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="163d5-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="163d5-309">O código</span><span class="sxs-lookup"><span data-stu-id="163d5-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="163d5-310">invoca o método no `Derived`, já que `Derived<int,int>` efetivamente reimplementa `I<int>` ([Reimplementação da Interface](interfaces.md#interface-re-implementation)).</span><span class="sxs-lookup"><span data-stu-id="163d5-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="163d5-311">Implementação de métodos genéricos</span><span class="sxs-lookup"><span data-stu-id="163d5-311">Implementation of generic methods</span></span>

<span data-ttu-id="163d5-312">Quando um método genérico implicitamente implementa um método de interface fornecidas para cada parâmetro de tipo de método deve ser equivalente em ambas as declarações (após qualquer tipo de interface parâmetros são substituídos com os argumentos de tipo apropriado), em que as restrições de método parâmetros de tipo são identificados por posições ordinais, da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="163d5-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="163d5-313">Quando um método genérico explicitamente implementa um método de interface, no entanto, não são permitidas restrições sobre o método de implementação.</span><span class="sxs-lookup"><span data-stu-id="163d5-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="163d5-314">Em vez disso, as restrições são herdadas do método de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="163d5-315">O método `C.F<T>` implicitamente implementa `I<object,C,string>.F<T>`.</span><span class="sxs-lookup"><span data-stu-id="163d5-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="163d5-316">Nesse caso, `C.F<T>` não são necessários (nem permitido) para especificar a restrição `T:object` , pois `object` é uma restrição implícita em todos os parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="163d5-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="163d5-317">O método `C.G<T>` implicitamente implementa `I<object,C,string>.G<T>` porque as restrições coincide com a interface, depois que os parâmetros de tipo de interface são substituídos com os argumentos de tipo correspondente.</span><span class="sxs-lookup"><span data-stu-id="163d5-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="163d5-318">A restrição para o método `C.H<T>` é um erro porque tipos lacrados (`string` nesse caso) não podem ser usados como restrições.</span><span class="sxs-lookup"><span data-stu-id="163d5-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="163d5-319">Omitindo a restrição também seria um erro, pois as restrições de implementações de método de interface implícita são necessárias para corresponder.</span><span class="sxs-lookup"><span data-stu-id="163d5-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="163d5-320">Portanto, é impossível implementar implicitamente `I<object,C,string>.H<T>`.</span><span class="sxs-lookup"><span data-stu-id="163d5-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="163d5-321">Esse método de interface só pode ser implementado usando uma implementação de membro de interface explícita:</span><span class="sxs-lookup"><span data-stu-id="163d5-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="163d5-322">Neste exemplo, a implementação de membro de interface explícita invoca um método público ter restrições estritamente mais fracas.</span><span class="sxs-lookup"><span data-stu-id="163d5-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="163d5-323">Observe que a atribuição da `t` à `s` é válido desde `T` herda de uma restrição de `T:string`, mesmo que essa restrição não é expressada no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="163d5-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="163d5-324">Mapeamento de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-324">Interface mapping</span></span>

<span data-ttu-id="163d5-325">Uma classe ou struct deve fornecer implementações de todos os membros das interfaces que são listados na lista de classe base da classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="163d5-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="163d5-326">O processo de localização de implementações de membros de interface em uma classe ou struct a implementação é conhecido como ***mapeamento de interface***.</span><span class="sxs-lookup"><span data-stu-id="163d5-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="163d5-327">Mapeamento de interface para uma classe ou struct `C` localiza uma implementação para cada membro de cada interface especificada na lista de classes base `C`.</span><span class="sxs-lookup"><span data-stu-id="163d5-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="163d5-328">A implementação de um membro de interface específica `I.M`, onde `I` é a interface na qual o membro `M` é declarado, é determinada pelo exame de cada classe ou struct `S`, começando com `C` e Repetir para cada classe base sucessiva do `C`, até que uma correspondência for encontrada:</span><span class="sxs-lookup"><span data-stu-id="163d5-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="163d5-329">Se `S` contém uma declaração de uma implementação de membro de interface explícita corresponde `I` e `M`, em seguida, esse membro é a implementação de `I.M`.</span><span class="sxs-lookup"><span data-stu-id="163d5-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="163d5-330">Caso contrário, se `S` contém uma declaração de um membro público não estático que corresponde à `M`, em seguida, esse membro é a implementação de `I.M`.</span><span class="sxs-lookup"><span data-stu-id="163d5-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="163d5-331">Se mais de um membro correspondências, não está especificado qual membro é a implementação do `I.M`.</span><span class="sxs-lookup"><span data-stu-id="163d5-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="163d5-332">Essa situação pode ocorrer somente se `S` for um tipo construído em que os dois membros conforme declarado no tipo genérico têm assinaturas diferentes, mas os argumentos de tipo que seja idêntico de suas assinaturas.</span><span class="sxs-lookup"><span data-stu-id="163d5-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="163d5-333">Ocorrerá um erro de tempo de compilação se implementações não podem ser localizadas para todos os membros de todas as interfaces especificados na lista de classes base `C`.</span><span class="sxs-lookup"><span data-stu-id="163d5-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="163d5-334">Observe que os membros de uma interface incluem aqueles membros que são herdados de interfaces base.</span><span class="sxs-lookup"><span data-stu-id="163d5-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="163d5-335">Para fins de mapeamento de interface, um membro da classe `A` corresponde a um membro de interface `B` quando:</span><span class="sxs-lookup"><span data-stu-id="163d5-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="163d5-336">`A` e `B` são métodos e o nome, tipo, e listas de parâmetros formais de `A` e `B` são idênticos.</span><span class="sxs-lookup"><span data-stu-id="163d5-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="163d5-337">`A` e `B` são propriedades, o nome e tipo de `A` e `B` são idênticas, e `A` tem o mesmos acessadores, conforme `B` (`A` é permitido ter acessadores adicionais se não for uma interface explícita implementação de membro).</span><span class="sxs-lookup"><span data-stu-id="163d5-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="163d5-338">`A` e `B` são os eventos e o nome e o tipo de `A` e `B` são idênticos.</span><span class="sxs-lookup"><span data-stu-id="163d5-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="163d5-339">`A` e `B` são os indexadores, o tipo e listas de parâmetros formais das `A` e `B` são idênticas, e `A` tem o mesmos acessadores, conforme `B` (`A` é permitido ter acessadores adicionais se não for um implementação de membro de interface explícita).</span><span class="sxs-lookup"><span data-stu-id="163d5-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="163d5-340">Implicações importantes do algoritmo de mapeamento de interface são:</span><span class="sxs-lookup"><span data-stu-id="163d5-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="163d5-341">Implementações de membros de interface explícita têm precedência sobre os outros membros na mesma classe ou struct ao determinar o membro de classe ou struct que implementa um membro de interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="163d5-342">Membros não não públicas nem estáticos participarem no mapeamento de interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="163d5-343">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="163d5-344">o `ICloneable.Clone` membro do `C` torna-se a implementação de `Clone` em `ICloneable` como as implementações de membros de interface explícita têm precedência sobre os outros membros.</span><span class="sxs-lookup"><span data-stu-id="163d5-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="163d5-345">Se uma classe ou struct implementa duas ou mais interfaces que contém um membro com o mesmo nome, tipo e tipos de parâmetro, é possível mapear cada um desses membros de interface para um único membro de classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="163d5-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="163d5-346">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="163d5-347">Aqui, o `Paint` métodos de ambos `IControl` e `IForm` são mapeadas para o `Paint` método na `Page`.</span><span class="sxs-lookup"><span data-stu-id="163d5-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="163d5-348">É claro também é possível ter implementações de membros de interface explícita separada para os dois métodos.</span><span class="sxs-lookup"><span data-stu-id="163d5-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="163d5-349">Se uma classe ou struct implementa uma interface que contém membros ocultos, alguns membros necessariamente precisa ser implementados por meio de implementações de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="163d5-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="163d5-350">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="163d5-351">Uma implementação dessa interface exige pelo menos uma implementação de membro de interface explícita e levaria a uma das seguintes formas</span><span class="sxs-lookup"><span data-stu-id="163d5-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="163d5-352">Quando uma classe implementa várias interfaces que têm a mesma interface base, pode haver apenas uma implementação da interface base.</span><span class="sxs-lookup"><span data-stu-id="163d5-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="163d5-353">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="163d5-354">não é possível ter implementações separadas para o `IControl` nomeado na lista de classes base, o `IControl` herdadas por `ITextBox`e o `IControl` herdada por `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="163d5-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="163d5-355">Na verdade, não há nenhuma noção de uma identidade separada para essas interfaces.</span><span class="sxs-lookup"><span data-stu-id="163d5-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="163d5-356">Em vez disso, as implementações de `ITextBox` e `IListBox` compartilham a mesma implementação de `IControl`, e `ComboBox` simplesmente é considerado para implementar as interfaces de três `IControl`, `ITextBox`, e `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="163d5-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="163d5-357">Os membros de uma classe base participarem no mapeamento de interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="163d5-358">No exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="163d5-359">o método `F` na `Class1` é usado em `Class2`da implementação de `Interface1`.</span><span class="sxs-lookup"><span data-stu-id="163d5-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="163d5-360">Herança de implementação de interface</span><span class="sxs-lookup"><span data-stu-id="163d5-360">Interface implementation inheritance</span></span>

<span data-ttu-id="163d5-361">Uma classe herda todas as implementações de interface fornecidas por suas classes base.</span><span class="sxs-lookup"><span data-stu-id="163d5-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="163d5-362">Sem explicitamente ***reimplementação*** uma interface, uma classe derivada não é possível de qualquer maneira alterar os mapeamentos de interface que herda de suas classes base.</span><span class="sxs-lookup"><span data-stu-id="163d5-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="163d5-363">Por exemplo, nas declarações</span><span class="sxs-lookup"><span data-stu-id="163d5-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="163d5-364">o `Paint` método na `TextBox` oculta a `Paint` método na `Control`, mas não altera o mapeamento de `Control.Paint` até `IControl.Paint`e chamadas para `Paint` por meio da classe serão instâncias e instâncias de interface ter os seguintes efeitos</span><span class="sxs-lookup"><span data-stu-id="163d5-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="163d5-365">No entanto, quando um método de interface é mapeado em um método virtual em uma classe, é possível que as classes derivadas substituir o método virtual e alterar a implementação da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="163d5-366">Por exemplo, reescrever as declarações acima para</span><span class="sxs-lookup"><span data-stu-id="163d5-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="163d5-367">os seguintes efeitos agora serão observados</span><span class="sxs-lookup"><span data-stu-id="163d5-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="163d5-368">Uma vez que as implementações de membros de interface explícita não podem ser declaradas como virtuais, não é possível substituir uma implementação de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="163d5-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="163d5-369">No entanto, é perfeitamente válido para uma implementação de membro de interface explícita chamar outro método, e que outro método pode ser declarado virtual para permitir que classes para substituí-lo derivadas.</span><span class="sxs-lookup"><span data-stu-id="163d5-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="163d5-370">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="163d5-371">Aqui, as classes derivadas `Control` possível especializar a implementação de `IControl.Paint` , substituindo o `PaintControl` método.</span><span class="sxs-lookup"><span data-stu-id="163d5-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="163d5-372">Reimplementação da interface</span><span class="sxs-lookup"><span data-stu-id="163d5-372">Interface re-implementation</span></span>

<span data-ttu-id="163d5-373">Uma classe que herda de uma implementação de interface tem permissão para ***reimplementar*** a interface incluindo-o na lista de classe base.</span><span class="sxs-lookup"><span data-stu-id="163d5-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="163d5-374">Uma nova implementação de uma interface segue exatamente as mesma interface regras de mapeamento como uma implementação inicial de uma interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="163d5-375">Portanto, o mapeamento de interface herdada não tem efeito depende do mapeamento de interface estabelecida para a reimplementação da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="163d5-376">Por exemplo, nas declarações</span><span class="sxs-lookup"><span data-stu-id="163d5-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="163d5-377">o fato de que `Control` mapeia `IControl.Paint` até `Control.IControl.Paint` não afeta a reimplementação na `MyControl`, que mapeia `IControl.Paint` até `MyControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="163d5-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="163d5-378">Herdado de declarações de membro público e o membro de interface explícita herdada declarações participarem no processo de mapeamento de interface de interfaces implementadas novamente.</span><span class="sxs-lookup"><span data-stu-id="163d5-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="163d5-379">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="163d5-380">Aqui, a implementação de `IMethods` na `Derived` mapeia os métodos de interface em `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, e `Base.I`.</span><span class="sxs-lookup"><span data-stu-id="163d5-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="163d5-381">Quando uma classe implementa uma interface, ele implicitamente também implementa todas as interfaces de base dessa interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="163d5-382">Da mesma forma, uma nova implementação de uma interface também é implicitamente uma Reimplementação de todas as interfaces de base da interface.</span><span class="sxs-lookup"><span data-stu-id="163d5-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="163d5-383">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="163d5-384">Aqui, a nova implementação da `IDerived` também reimplementa `IBase`, o mapeamento `IBase.F` em `D.F`.</span><span class="sxs-lookup"><span data-stu-id="163d5-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="163d5-385">Interfaces e classes abstratas</span><span class="sxs-lookup"><span data-stu-id="163d5-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="163d5-386">Como uma classe não abstrata, uma classe abstrata deve fornecer implementações de todos os membros das interfaces que são listados na lista de classe base da classe.</span><span class="sxs-lookup"><span data-stu-id="163d5-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="163d5-387">No entanto, uma classe abstrata é permitida para mapear os métodos de interface em métodos abstratos.</span><span class="sxs-lookup"><span data-stu-id="163d5-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="163d5-388">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="163d5-389">Aqui, a implementação de `IMethods` mapeia `F` e `G` em métodos abstratos, que deve ser substituído em classes não abstratas que derivam de `C`.</span><span class="sxs-lookup"><span data-stu-id="163d5-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="163d5-390">Observe que as implementações de membros de interface explícita não podem ser abstratas, mas as implementações de membros de interface explícita certamente têm permissão para chamar métodos abstratos.</span><span class="sxs-lookup"><span data-stu-id="163d5-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="163d5-391">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="163d5-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="163d5-392">Aqui, as classes não abstratas que derivam de `C` seria necessário para substituir `FF` e `GG`, fornecendo assim a implementação real da `IMethods`.</span><span class="sxs-lookup"><span data-stu-id="163d5-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
