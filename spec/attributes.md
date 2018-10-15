# <a name="attributes"></a><span data-ttu-id="86381-101">Atributos</span><span class="sxs-lookup"><span data-stu-id="86381-101">Attributes</span></span>

<span data-ttu-id="86381-102">Grande parte da linguagem c# permite que o programador especificar informações declarativas sobre as entidades definidas no programa.</span><span class="sxs-lookup"><span data-stu-id="86381-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="86381-103">Por exemplo, a acessibilidade de um método em uma classe for especificada, decorando-o com o *method_modifier*s `public`, `protected`, `internal`, e `private`.</span><span class="sxs-lookup"><span data-stu-id="86381-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="86381-104">C# habilita os programadores a novos tipos de informações declarativas, chamado de estoque ***atributos***.</span><span class="sxs-lookup"><span data-stu-id="86381-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="86381-105">Os programadores podem anexar atributos para várias entidades de programa e recuperar informações de atributo em um ambiente de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86381-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="86381-106">Por exemplo, uma estrutura pode definir um `HelpAttribute` atributo que pode ser colocado em determinados elementos do programa (como classes e métodos) para fornecer um mapeamento a partir desses elementos de programa para a documentação.</span><span class="sxs-lookup"><span data-stu-id="86381-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="86381-107">Os atributos são definidos por meio da declaração de classes de atributos ([classes de atributo](attributes.md#attribute-classes)), que pode ter parâmetros posicionais e nomeados ([Positional e parâmetros nomeados](attributes.md#positional-and-named-parameters)).</span><span class="sxs-lookup"><span data-stu-id="86381-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="86381-108">Atributos anexados a entidades em um programa em C# usando as especificações do atributo ([especificação do atributo](attributes.md#attribute-specification)) e podem ser recuperados em tempo de execução como instâncias de atributo ([instâncias de atributo](attributes.md#attribute-instances)).</span><span class="sxs-lookup"><span data-stu-id="86381-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="86381-109">Classes de atributo</span><span class="sxs-lookup"><span data-stu-id="86381-109">Attribute classes</span></span>

<span data-ttu-id="86381-110">Uma classe que deriva da classe abstrata `System.Attribute`, seja direta ou indiretamente, é um ***classe de atributo***.</span><span class="sxs-lookup"><span data-stu-id="86381-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="86381-111">A declaração de uma classe de atributo define um novo tipo de ***atributo*** que pode ser colocado em uma declaração.</span><span class="sxs-lookup"><span data-stu-id="86381-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="86381-112">Por convenção, as classes de atributos são nomeadas com um sufixo de `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="86381-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="86381-113">Usos de um atributo podem incluir ou omitir esse sufixo.</span><span class="sxs-lookup"><span data-stu-id="86381-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="86381-114">Uso do atributo</span><span class="sxs-lookup"><span data-stu-id="86381-114">Attribute usage</span></span>

<span data-ttu-id="86381-115">O atributo `AttributeUsage` ([atributo AttributeUsage o](attributes.md#the-attributeusage-attribute)) é usado para descrever como uma classe de atributo pode ser usada.</span><span class="sxs-lookup"><span data-stu-id="86381-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="86381-116">`AttributeUsage` tem um parâmetro posicional ([Positional e parâmetros nomeados](attributes.md#positional-and-named-parameters)) que permite que uma classe de atributo especificar os tipos de declaração na qual ele pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="86381-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="86381-117">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="86381-118">define uma classe de atributo nomeada `SimpleAttribute` que pode ser colocado na *class_declaration*s e *interface_declaration*somente s.</span><span class="sxs-lookup"><span data-stu-id="86381-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="86381-119">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="86381-120">mostra vários usos do `Simple` atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="86381-121">Embora esse atributo é definido com o nome `SimpleAttribute`, quando esse atributo é usado, o `Attribute` sufixo pode ser omitido, resultando no nome curto `Simple`.</span><span class="sxs-lookup"><span data-stu-id="86381-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="86381-122">Assim, o exemplo acima é semanticamente equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="86381-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="86381-123">`AttributeUsage` tem um parâmetro nomeado ([Positional e parâmetros nomeados](attributes.md#positional-and-named-parameters)) chamado `AllowMultiple`, que indica se o atributo pode ser especificado mais de uma vez para uma determinada entidade.</span><span class="sxs-lookup"><span data-stu-id="86381-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="86381-124">Se `AllowMultiple` para um atributo de classe é true e, em seguida, classe de atributo é um ***classe de atributo multiuso***e pode ser especificado mais de uma vez em uma entidade.</span><span class="sxs-lookup"><span data-stu-id="86381-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="86381-125">Se `AllowMultiple` para um atributo de classe é falsa ou não está especificado, essa classe de atributo é um ***uso classe de atributo único***e podem ser especificadas no máximo uma vez em uma entidade.</span><span class="sxs-lookup"><span data-stu-id="86381-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="86381-126">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="86381-127">define uma classe de atributo multiuso chamada `AuthorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="86381-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="86381-128">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="86381-129">mostra uma declaração de classe com dois usos do `Author` atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="86381-130">`AttributeUsage` tem outro parâmetro nomeado chamado `Inherited`, que indica se o atributo, quando especificado em uma classe base, também é herdado por classes que derivam dessa classe base.</span><span class="sxs-lookup"><span data-stu-id="86381-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="86381-131">Se `Inherited` para um atributo de classe é true e, em seguida, esse atributo é herdado.</span><span class="sxs-lookup"><span data-stu-id="86381-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="86381-132">Se `Inherited` para um atributo de classe é false, em seguida, esse atributo não é herdado.</span><span class="sxs-lookup"><span data-stu-id="86381-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="86381-133">Se não for especificado, o valor padrão é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="86381-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="86381-134">Uma classe de atributo `X` não tem um `AttributeUsage` atributo anexado a ele, como em</span><span class="sxs-lookup"><span data-stu-id="86381-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="86381-135">é equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="86381-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="86381-136">Parâmetros posicionais e nomeados</span><span class="sxs-lookup"><span data-stu-id="86381-136">Positional and named parameters</span></span>

<span data-ttu-id="86381-137">Classes de atributo podem ter ***parâmetros posicionais*** e ***parâmetros nomeados***.</span><span class="sxs-lookup"><span data-stu-id="86381-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="86381-138">Cada construtor de instância pública para uma classe de atributo define uma sequência válida de parâmetros posicionais para essa classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="86381-139">Cada campo de leitura / gravação público não estático e a propriedade para uma classe de atributo definem um parâmetro nomeado para a classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="86381-140">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="86381-141">define uma classe de atributo nomeada `HelpAttribute` que tem um parâmetro posicional, `url`e um parâmetro nomeado, `Topic`.</span><span class="sxs-lookup"><span data-stu-id="86381-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="86381-142">Embora seja não estático e público, a propriedade `Url` não define um parâmetro nomeado, pois ele não é leitura / gravação.</span><span class="sxs-lookup"><span data-stu-id="86381-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="86381-143">Essa classe de atributo pode ser usado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86381-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="86381-144">Tipos de parâmetro de atributo</span><span class="sxs-lookup"><span data-stu-id="86381-144">Attribute parameter types</span></span>

<span data-ttu-id="86381-145">Os tipos de parâmetros posicionais e nomeados para uma classe de atributo são limitados para o ***tipos de parâmetro de atributo***, que são:</span><span class="sxs-lookup"><span data-stu-id="86381-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="86381-146">Um dos seguintes tipos: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span><span class="sxs-lookup"><span data-stu-id="86381-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="86381-147">O tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="86381-147">The type `object`.</span></span>
*  <span data-ttu-id="86381-148">O tipo `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="86381-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="86381-149">Um tipo de enumeração fornecido tem acessibilidade pública e os tipos na qual ele está aninhado (se houver) também tem acessibilidade pública ([especificação do atributo](attributes.md#attribute-specification)).</span><span class="sxs-lookup"><span data-stu-id="86381-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="86381-150">Matrizes unidimensionais de tipos mencionados acima.</span><span class="sxs-lookup"><span data-stu-id="86381-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="86381-151">Um argumento de construtor ou um campo público que não tem um desses tipos, não pode ser usado como um parâmetro posicional ou nomeado em uma especificação de atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="86381-152">Especificação de atributo</span><span class="sxs-lookup"><span data-stu-id="86381-152">Attribute specification</span></span>

<span data-ttu-id="86381-153">***Especificação do atributo*** é o aplicativo de um atributo definido anteriormente para uma declaração.</span><span class="sxs-lookup"><span data-stu-id="86381-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="86381-154">Um atributo é uma parte das informações declarativas adicionais que são especificadas para uma declaração.</span><span class="sxs-lookup"><span data-stu-id="86381-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="86381-155">Atributos podem ser especificados no escopo global (para especificar atributos no assembly ou módulo que contém) e para *type_declaration*s ([as declarações de tipo](namespaces.md#type-declarations)), *class_member_declaration* s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([membros de Interface](interfaces.md#interface-members)), *struct_member _declaration*s ([membros de Struct](structs.md#struct-members)), *enum_member_declaration*s ([membros Enum](enums.md#enum-members)), *accessor_declarations*  ([Acessadores](classes.md#accessors)), *event_accessor_declarations* ([eventos semelhantes a campo](classes.md#field-like-events)), e *formal_parameter_list*s ([parâmetros de método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="86381-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="86381-156">Os atributos são especificados em ***seções de atributo***.</span><span class="sxs-lookup"><span data-stu-id="86381-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="86381-157">Uma seção de atributo consiste em um par de colchetes, que envolvem uma lista separada por vírgulas de atributos de um ou mais.</span><span class="sxs-lookup"><span data-stu-id="86381-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="86381-158">A ordem em que atributos são especificados em uma lista desse tipo e a ordem em que seções anexadas à mesma entidade programa são organizadas, não é significativa.</span><span class="sxs-lookup"><span data-stu-id="86381-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="86381-159">Por exemplo, as especificações do atributo `[A][B]`, `[B][A]`, `[A,B]`, e `[B,A]` são equivalentes.</span><span class="sxs-lookup"><span data-stu-id="86381-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="86381-160">Um atributo consiste em um *attribute_name* e uma lista opcional de argumentos posicionais e nomeados.</span><span class="sxs-lookup"><span data-stu-id="86381-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="86381-161">Os argumentos posicionais (se houver) precedem os argumentos nomeados.</span><span class="sxs-lookup"><span data-stu-id="86381-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="86381-162">Um argumento posicional consiste em uma *attribute_argument_expression*; um argumento nomeado consiste em um nome, seguido por um sinal de igual, seguido por um *attribute_argument_expression*, que, juntos , são restringidas pelas mesmas regras de atribuição simples.</span><span class="sxs-lookup"><span data-stu-id="86381-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="86381-163">A ordem dos argumentos nomeados não é significativa.</span><span class="sxs-lookup"><span data-stu-id="86381-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="86381-164">O *attribute_name* identifica uma classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="86381-165">Se o formulário da *attribute_name* é *type_name* e em seguida, esse nome deve se referir a uma classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="86381-166">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="86381-167">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="86381-168">resulta em um erro de tempo de compilação, porque ele tenta usar `Class1` como um atributo de classe quando `Class1` não é uma classe de atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="86381-169">Determinados contextos permitem a especificação de um atributo em mais de um destino.</span><span class="sxs-lookup"><span data-stu-id="86381-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="86381-170">Um programa pode especificar explicitamente o destino, incluindo uma *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="86381-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="86381-171">Quando um atributo é colocado no nível global, uma *global_attribute_target_specifier* é necessária.</span><span class="sxs-lookup"><span data-stu-id="86381-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="86381-172">Em todos os outros locais, é aplicada a uma opção razoável, mas um *attribute_target_specifier* pode ser usado para confirmar ou substituir o padrão em certos casos ambíguos (ou apenas afirmar o padrão em casos não ambígua).</span><span class="sxs-lookup"><span data-stu-id="86381-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="86381-173">Portanto, normalmente, *attribute_target_specifier*s pode ser omitido, exceto no nível global.</span><span class="sxs-lookup"><span data-stu-id="86381-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="86381-174">Os contextos potencialmente ambíguos são resolvidos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86381-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="86381-175">Um atributo especificado no escopo global pode aplicar para o assembly de destino ou o módulo de destino.</span><span class="sxs-lookup"><span data-stu-id="86381-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="86381-176">Nenhum padrão existe para este contexto, então um *attribute_target_specifier* sempre é necessário neste contexto.</span><span class="sxs-lookup"><span data-stu-id="86381-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="86381-177">A presença do `assembly` *attribute_target_specifier* indica que o atributo se aplica ao destino assembly; a presença dos `module` *attribute_target_specifier* indica que o atributo se aplica ao módulo de destino.</span><span class="sxs-lookup"><span data-stu-id="86381-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="86381-178">Um atributo especificado em uma declaração delegate pode aplicar para o delegado que está sendo declarado ou seu valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="86381-179">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao delegado.</span><span class="sxs-lookup"><span data-stu-id="86381-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="86381-180">A presença do `type` *attribute_target_specifier* indica que o atributo se aplica ao delegado; a presença dos `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="86381-181">Um atributo especificado em uma declaração de método pode aplicar para o método que está sendo declarado ou seu valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="86381-182">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método.</span><span class="sxs-lookup"><span data-stu-id="86381-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="86381-183">A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `return` *attribute_target_specifier* indica Se o atributo se aplica ao valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="86381-184">Um atributo especificado em uma declaração do operador pode aplicar o operador que está sendo declarado ou seu valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="86381-185">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao operador.</span><span class="sxs-lookup"><span data-stu-id="86381-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="86381-186">A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao operador; a presença dos `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="86381-187">Um atributo especificado em uma declaração de evento que omite os acessadores de eventos pode aplicar ao evento que está sendo declarado, para o campo associado (se o evento não é abstrato) ou para adicionar associado e remover métodos.</span><span class="sxs-lookup"><span data-stu-id="86381-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="86381-188">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao evento.</span><span class="sxs-lookup"><span data-stu-id="86381-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="86381-189">A presença do `event` *attribute_target_specifier* indica que o atributo se aplica para o evento; a presença dos `field` *attribute_target_specifier* indica o atributo aplica-se ao campo; e a presença do `method` *attribute_target_specifier* indica que o atributo se aplica aos métodos.</span><span class="sxs-lookup"><span data-stu-id="86381-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="86381-190">Um atributo especificado em uma declaração do acessador get para uma declaração de propriedade ou indexador pode aplicar para o método associado ou seu valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="86381-191">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método.</span><span class="sxs-lookup"><span data-stu-id="86381-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="86381-192">A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `return` *attribute_target_specifier* indica Se o atributo se aplica ao valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="86381-193">Um atributo especificado em um acessador set de uma declaração de propriedade ou indexador pode aplicar para o método associado ou seu único parâmetro implícito.</span><span class="sxs-lookup"><span data-stu-id="86381-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="86381-194">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método.</span><span class="sxs-lookup"><span data-stu-id="86381-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="86381-195">A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `param` *attribute_target_specifier* indica o atributo aplica-se para o parâmetro; a presença do `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="86381-196">Um atributo especificado em uma declaração de acessador add ou remove, para uma declaração de evento pode aplicar para o método associado ou seu único parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86381-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="86381-197">Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método.</span><span class="sxs-lookup"><span data-stu-id="86381-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="86381-198">A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `param` *attribute_target_specifier* indica o atributo aplica-se para o parâmetro; a presença do `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="86381-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="86381-199">Em outros contextos, a inclusão de um *attribute_target_specifier* é permitido, mas desnecessários.</span><span class="sxs-lookup"><span data-stu-id="86381-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="86381-200">Por exemplo, uma declaração de classe pode incluir ou omitir o especificador de `type`:</span><span class="sxs-lookup"><span data-stu-id="86381-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="86381-201">É um erro especificar um inválido *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="86381-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="86381-202">Por exemplo, o especificador de `param` não pode ser usado em uma declaração de classe:</span><span class="sxs-lookup"><span data-stu-id="86381-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="86381-203">Por convenção, as classes de atributos são nomeadas com um sufixo de `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="86381-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="86381-204">Uma *attribute_name* do formulário *type_name* pode incluir ou omitir esse sufixo.</span><span class="sxs-lookup"><span data-stu-id="86381-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="86381-205">Se uma classe de atributo for encontrada com e sem esse sufixo, uma ambiguidade está presente, e um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="86381-206">Se o *attribute_name* está escrito, de modo que sua direita *identificador* é um identificador textual ([identificadores](lexical-structure.md#identifiers)), em seguida, apenas um atributo sem um sufixo é correspondido, permitindo que tal uma ambiguidade de ser resolvido.</span><span class="sxs-lookup"><span data-stu-id="86381-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="86381-207">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="86381-208">mostra duas classes chamadas de atributo `X` e `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="86381-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="86381-209">O atributo `[X]` é ambíguo, já que ela pode se referir a um `X` ou `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="86381-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="86381-210">Usar um identificador textual permite que a intenção exata a ser especificado em tais casos raros.</span><span class="sxs-lookup"><span data-stu-id="86381-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="86381-211">O atributo `[XAttribute]` não é ambígua (embora seria se havia uma classe de atributo nomeada `XAttributeAttribute`!).</span><span class="sxs-lookup"><span data-stu-id="86381-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="86381-212">Se a declaração de classe `X` for removido, os dois atributos se referir à classe de atributo nomeada `XAttribute`, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="86381-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="86381-213">Ele é um erro de tempo de compilação para usar uma classe de atributo de uso único mais de uma vez na mesma entidade.</span><span class="sxs-lookup"><span data-stu-id="86381-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="86381-214">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="86381-215">resulta em um erro de tempo de compilação, porque ele tenta usar `HelpString`, que é uma classe de atributo de uso único, mais de uma vez na declaração de `Class1`.</span><span class="sxs-lookup"><span data-stu-id="86381-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="86381-216">Uma expressão `E` é um *attribute_argument_expression* se todas as instruções a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="86381-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="86381-217">O tipo de `E` é um tipo de parâmetro de atributo ([tipos de parâmetro de atributo](attributes.md#attribute-parameter-types)).</span><span class="sxs-lookup"><span data-stu-id="86381-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="86381-218">Em tempo de compilação, o valor de `E` pode ser resolvida para um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="86381-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="86381-219">Um valor constante.</span><span class="sxs-lookup"><span data-stu-id="86381-219">A constant value.</span></span>
   * <span data-ttu-id="86381-220">Um objeto `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="86381-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="86381-221">Uma matriz unidimensional do *attribute_argument_expression*s.</span><span class="sxs-lookup"><span data-stu-id="86381-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="86381-222">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86381-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="86381-223">Um *typeof_expression* ([o operador typeof](expressions.md#the-typeof-operator)) usada como uma expressão de argumento de atributo pode referenciar um tipo não genérico, um tipo construído fechado ou um tipo genérico não associado, mas ele não pode fazer referência a um Abra o tipo.</span><span class="sxs-lookup"><span data-stu-id="86381-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="86381-224">Isso é para garantir que a expressão pode ser resolvida em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="86381-225">Instâncias de atributo</span><span class="sxs-lookup"><span data-stu-id="86381-225">Attribute instances</span></span>

<span data-ttu-id="86381-226">Uma ***instância de atributo*** é uma instância que representa um atributo em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86381-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="86381-227">Um atributo é definido com uma classe de atributo, argumentos posicionais e argumentos nomeado.</span><span class="sxs-lookup"><span data-stu-id="86381-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="86381-228">Uma instância do atributo é uma instância da classe de atributo que é inicializado com os argumentos posicionais e nomeados.</span><span class="sxs-lookup"><span data-stu-id="86381-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="86381-229">Recuperação de uma instância do atributo envolve o processamento de tempo de compilação e tempo de execução, conforme descrito nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="86381-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="86381-230">Compilação de um atributo</span><span class="sxs-lookup"><span data-stu-id="86381-230">Compilation of an attribute</span></span>

<span data-ttu-id="86381-231">A compilação de um *atributo* com a classe de atributo `T`, *positional_argument_list* `P` e *named_argument_list* `N`, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="86381-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="86381-232">Siga as etapas de processamento de tempo de compilação para compilar uma *object_creation_expression* do formulário `new T(P)`.</span><span class="sxs-lookup"><span data-stu-id="86381-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="86381-233">Essas etapas resultarem em um erro de tempo de compilação, ou determinar um construtor de instância `C` em `T` que pode ser invocado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="86381-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="86381-234">Se `C` não tem acessibilidade pública, em seguida, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="86381-235">Para cada *named_argument* `Arg` em `N`:</span><span class="sxs-lookup"><span data-stu-id="86381-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="86381-236">Deixe `Name` ser o *identificador* da *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="86381-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="86381-237">`Name` deve identificar um campo público de leitura / gravação não estático ou propriedade em `T`.</span><span class="sxs-lookup"><span data-stu-id="86381-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="86381-238">Se `T` tem não há tal campo ou propriedade, em seguida, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="86381-239">Mantenha as seguintes informações para a instanciação de tempo de execução do atributo: a classe de atributo `T`, o construtor de instância `C` nos `T`, o *positional_argument_list* `P` e o *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="86381-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="86381-240">Recuperação de tempo de execução de uma instância do atributo</span><span class="sxs-lookup"><span data-stu-id="86381-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="86381-241">Compilação de um *atributo* resulta em uma classe de atributo `T`, um construtor de instância `C` na `T`, uma *positional_argument_list* `P`e um *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="86381-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="86381-242">Dadas essas informações, uma instância de atributo pode ser recuperada em tempo de execução usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="86381-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="86381-243">Siga as etapas de processamento de tempo de execução para a execução de um *object_creation_expression* do formulário `new T(P)`, usando o construtor de instância `C` conforme determinado no tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="86381-244">Essas etapas resultarem em uma exceção, ou produzir uma instância `O` de `T`.</span><span class="sxs-lookup"><span data-stu-id="86381-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="86381-245">Para cada *named_argument* `Arg` em `N`, na ordem:</span><span class="sxs-lookup"><span data-stu-id="86381-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="86381-246">Deixe `Name` ser o *identificador* da *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="86381-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="86381-247">Se `Name` não identifica um campo de leitura / gravação público não estático ou propriedade em `O`, em seguida, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="86381-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="86381-248">Deixe `Value` ser o resultado da avaliação de *attribute_argument_expression* de `Arg`.</span><span class="sxs-lookup"><span data-stu-id="86381-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="86381-249">Se `Name` identifica um campo na `O`, em seguida, defina esse campo como `Value`.</span><span class="sxs-lookup"><span data-stu-id="86381-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="86381-250">Caso contrário, `Name` identifica uma propriedade em `O`.</span><span class="sxs-lookup"><span data-stu-id="86381-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="86381-251">Defina essa propriedade como `Value`.</span><span class="sxs-lookup"><span data-stu-id="86381-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="86381-252">O resultado será `O`, uma instância da classe de atributo `T` que foi inicializado com o *positional_argument_list* `P` e o *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="86381-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="86381-253">Atributos reservados</span><span class="sxs-lookup"><span data-stu-id="86381-253">Reserved attributes</span></span>

<span data-ttu-id="86381-254">Um pequeno número de atributos afeta o idioma de alguma forma.</span><span class="sxs-lookup"><span data-stu-id="86381-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="86381-255">Esses atributos incluem:</span><span class="sxs-lookup"><span data-stu-id="86381-255">These attributes include:</span></span>

*  <span data-ttu-id="86381-256">`System.AttributeUsageAttribute` ([Atributo AttributeUsage o](attributes.md#the-attributeusage-attribute)), que é usado para descrever as maneiras em que uma classe de atributo pode ser usada.</span><span class="sxs-lookup"><span data-stu-id="86381-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="86381-257">`System.Diagnostics.ConditionalAttribute` ([Condicional o atributo](attributes.md#the-conditional-attribute)), que é usado para definir métodos condicionais.</span><span class="sxs-lookup"><span data-stu-id="86381-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="86381-258">`System.ObsoleteAttribute` ([Atributo obsoleto o](attributes.md#the-obsolete-attribute)), que é usado para marcar um membro como obsoleto.</span><span class="sxs-lookup"><span data-stu-id="86381-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="86381-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` e `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([atributos de informações do chamador](attributes.md#caller-info-attributes)), que são usados para fornecer informações sobre o contexto de chamada aos parâmetros opcionais.</span><span class="sxs-lookup"><span data-stu-id="86381-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="86381-260">O atributo AttributeUsage</span><span class="sxs-lookup"><span data-stu-id="86381-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="86381-261">O atributo `AttributeUsage` é usado para descrever a maneira na qual a classe de atributo pode ser usada.</span><span class="sxs-lookup"><span data-stu-id="86381-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="86381-262">Uma classe decorada com o `AttributeUsage` atributo deve derivar de `System.Attribute`, direta ou indiretamente.</span><span class="sxs-lookup"><span data-stu-id="86381-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="86381-263">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="86381-264">O atributo Conditional</span><span class="sxs-lookup"><span data-stu-id="86381-264">The Conditional attribute</span></span>

<span data-ttu-id="86381-265">O atributo `Conditional` habilita a definição de ***métodos condicionais*** e ***classes de atributo conditional***.</span><span class="sxs-lookup"><span data-stu-id="86381-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="86381-266">Métodos condicionais</span><span class="sxs-lookup"><span data-stu-id="86381-266">Conditional methods</span></span>

<span data-ttu-id="86381-267">Um método decorado com o `Conditional` atributo é um método condicional.</span><span class="sxs-lookup"><span data-stu-id="86381-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="86381-268">O `Conditional` atributo indica uma condição testando um símbolo de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="86381-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="86381-269">Chamadas para um método condicional são incluídas ou omitidas, dependendo se esse símbolo é definido no ponto de chamada.</span><span class="sxs-lookup"><span data-stu-id="86381-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="86381-270">Se o símbolo é definido, a chamada será incluída; Caso contrário, a chamada (inclusive a avaliação do receptor e parâmetros da chamada) é omitida.</span><span class="sxs-lookup"><span data-stu-id="86381-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="86381-271">Um método condicional está sujeito às seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="86381-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="86381-272">O método condicional deve ser um método em uma *class_declaration* ou *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="86381-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="86381-273">Um erro de tempo de compilação ocorrerá se o `Conditional` atributo é especificado em um método em uma declaração de interface.</span><span class="sxs-lookup"><span data-stu-id="86381-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="86381-274">O método condicional deve ter um tipo de retorno `void`.</span><span class="sxs-lookup"><span data-stu-id="86381-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="86381-275">O método condicional não deve ser marcado com o `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="86381-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="86381-276">Um método condicional pode ser marcado com o `virtual` modificador, no entanto.</span><span class="sxs-lookup"><span data-stu-id="86381-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="86381-277">Substituições desse método são implicitamente condicionais e não devem ser marcadas explicitamente com um `Conditional` atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="86381-278">O método condicional não deve ser uma implementação de um método de interface.</span><span class="sxs-lookup"><span data-stu-id="86381-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="86381-279">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="86381-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="86381-280">Além disso, um erro de tempo de compilação ocorre se um método condicional é usado em uma *delegate_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="86381-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="86381-281">O exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="86381-282">declara `Class1.M` como um método condicional.</span><span class="sxs-lookup"><span data-stu-id="86381-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="86381-283">`Class2`do `Test` método chama esse método.</span><span class="sxs-lookup"><span data-stu-id="86381-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="86381-284">Desde o símbolo de compilação condicional `DEBUG` for definido, se `Class2.Test` é chamado, ele chamará `M`.</span><span class="sxs-lookup"><span data-stu-id="86381-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="86381-285">Se o símbolo `DEBUG` não havia sido definido, em seguida, `Class2.Test` não chamaria `Class1.M`.</span><span class="sxs-lookup"><span data-stu-id="86381-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="86381-286">É importante observar que a inclusão ou exclusão de uma chamada para um método condicional é controlado pelos símbolos de compilação condicional no ponto de chamada.</span><span class="sxs-lookup"><span data-stu-id="86381-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="86381-287">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-287">In the example</span></span>

<span data-ttu-id="86381-288">Arquivo `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="86381-289">Arquivo `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="86381-290">Arquivo `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="86381-291">as classes `Class2` e `Class3` cada conter chamadas para o método condicional `Class1.F`, que é condicional com base em ou não `DEBUG` está definido.</span><span class="sxs-lookup"><span data-stu-id="86381-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="86381-292">Como esse símbolo é definido no contexto de `Class2` , mas não `Class3`, a chamada para `F` na `Class2` está incluído, enquanto a chamada para `F` no `Class3` é omitido.</span><span class="sxs-lookup"><span data-stu-id="86381-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="86381-293">O uso de métodos condicionais em uma cadeia de herança pode ser confuso.</span><span class="sxs-lookup"><span data-stu-id="86381-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="86381-294">Chamadas feitas para um método condicional por meio `base`, no formato `base.M`, estão sujeitas às regras de chamada de método normal de condicional.</span><span class="sxs-lookup"><span data-stu-id="86381-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="86381-295">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-295">In the example</span></span>

<span data-ttu-id="86381-296">Arquivo `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="86381-297">Arquivo `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="86381-298">Arquivo `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="86381-299">`Class2` inclui uma chamada para o `M` definidos em sua classe base.</span><span class="sxs-lookup"><span data-stu-id="86381-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="86381-300">Essa chamada é omitida porque o método base é condicional com base na presença de símbolo `DEBUG`, que é indefinido.</span><span class="sxs-lookup"><span data-stu-id="86381-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="86381-301">Portanto, o método grava no console "`Class2.M executed`" apenas.</span><span class="sxs-lookup"><span data-stu-id="86381-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="86381-302">Uso criterioso de *pp_declaration*s pode eliminar esses problemas.</span><span class="sxs-lookup"><span data-stu-id="86381-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="86381-303">Classes de atributo Conditional</span><span class="sxs-lookup"><span data-stu-id="86381-303">Conditional attribute classes</span></span>

<span data-ttu-id="86381-304">Uma classe de atributo ([classes de atributo](attributes.md#attribute-classes)) decorado com um ou mais `Conditional` atributos é uma ***classe de atributo condicional***.</span><span class="sxs-lookup"><span data-stu-id="86381-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="86381-305">Uma classe de atributo condicional, portanto, é associada com os símbolos de compilação condicional declarados em seu `Conditional` atributos.</span><span class="sxs-lookup"><span data-stu-id="86381-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="86381-306">Neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="86381-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="86381-307">declara `TestAttribute` como uma classe de atributo conditional associada com os símbolos condicionais compilações `ALPHA` e `BETA`.</span><span class="sxs-lookup"><span data-stu-id="86381-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="86381-308">Especificações de atributos ([especificação do atributo](attributes.md#attribute-specification)) de um atributo conditional serão incluídas se um ou mais dos seus símbolos de compilação condicional associado é definido no ponto de especificação, caso contrário, o atributo especificação é omitida.</span><span class="sxs-lookup"><span data-stu-id="86381-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="86381-309">É importante observar que a inclusão ou exclusão de uma especificação de atributo de uma classe de atributo condicional é controlado pelos símbolos de compilação condicional no ponto a especificação.</span><span class="sxs-lookup"><span data-stu-id="86381-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="86381-310">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-310">In the example</span></span>

<span data-ttu-id="86381-311">Arquivo `test.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="86381-312">Arquivo `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="86381-313">Arquivo `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="86381-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="86381-314">as classes `Class1` e `Class2` são cada decorada com o atributo `Test`, que é condicional com base em ou não `DEBUG` é definido.</span><span class="sxs-lookup"><span data-stu-id="86381-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="86381-315">Como esse símbolo é definido no contexto de `Class1` , mas não `Class2`, a especificação da `Test` atributo `Class1` está incluído, enquanto a especificação da `Test` o atributo em `Class2` for omitido.</span><span class="sxs-lookup"><span data-stu-id="86381-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="86381-316">O atributo obsoleto</span><span class="sxs-lookup"><span data-stu-id="86381-316">The Obsolete attribute</span></span>

<span data-ttu-id="86381-317">O atributo `Obsolete` é usado para marcar tipos e membros de tipos que não devem mais ser usados.</span><span class="sxs-lookup"><span data-stu-id="86381-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="86381-318">Se um programa usa um tipo ou membro é decorado com o `Obsolete` atributo, o compilador emite um aviso ou erro.</span><span class="sxs-lookup"><span data-stu-id="86381-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="86381-319">Especificamente, o compilador emite um aviso se nenhum erro de parâmetro for fornecido, ou se o parâmetro de erro é fornecido e tem o valor `false`.</span><span class="sxs-lookup"><span data-stu-id="86381-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="86381-320">O compilador emitirá um erro se o parâmetro de erro for especificado e tem o valor `true`.</span><span class="sxs-lookup"><span data-stu-id="86381-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="86381-321">No exemplo</span><span class="sxs-lookup"><span data-stu-id="86381-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="86381-322">a classe `A` decorada com o `Obsolete` atributo.</span><span class="sxs-lookup"><span data-stu-id="86381-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="86381-323">Cada uso de `A` em `Main` resulta em um aviso de que inclui a mensagem especificada, "Esta classe é obsoleta; Use a classe B."</span><span class="sxs-lookup"><span data-stu-id="86381-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="86381-324">Atributos de informações do chamador</span><span class="sxs-lookup"><span data-stu-id="86381-324">Caller info attributes</span></span>

<span data-ttu-id="86381-325">Para fins, como registro em log e relatórios, às vezes é útil para um membro da função obter determinadas informações de tempo de compilação sobre o código de chamada.</span><span class="sxs-lookup"><span data-stu-id="86381-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="86381-326">Os atributos de informações do chamador fornecem uma maneira para transmitir essas informações de forma transparente.</span><span class="sxs-lookup"><span data-stu-id="86381-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="86381-327">Quando um parâmetro opcional é anotado com um dos atributos de informações do chamador, omitir o argumento correspondente em uma chamada não necessariamente fazer com que o valor do parâmetro padrão a serem substituídos.</span><span class="sxs-lookup"><span data-stu-id="86381-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="86381-328">Em vez disso, se as informações especificadas sobre o contexto de chamada estiverem disponíveis, essas informações serão passadas como o valor do argumento.</span><span class="sxs-lookup"><span data-stu-id="86381-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="86381-329">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="86381-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="86381-330">Uma chamada para `Log()` sem argumentos imprimiria o linha número e caminho do arquivo da chamada, bem como o nome do membro no qual ocorreu a chamada.</span><span class="sxs-lookup"><span data-stu-id="86381-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="86381-331">Atributos de informações do chamador podem ocorrer em parâmetros opcionais em qualquer lugar, inclusive em declarações de delegado.</span><span class="sxs-lookup"><span data-stu-id="86381-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="86381-332">No entanto, os atributos de informações do chamador específico têm restrições sobre os tipos dos parâmetros, que eles podem atributo, para que sempre haverá uma conversão implícita de um valor substituído para o tipo de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86381-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="86381-333">É um erro ter o mesmo atributo de informações do chamador em um parâmetro de tanto a definição e implementação de parte de uma declaração de método parcial.</span><span class="sxs-lookup"><span data-stu-id="86381-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="86381-334">Somente atributos de informações do chamador na parte definição são aplicados, enquanto os atributos de informações do chamador ocorrendo apenas na parte de implementação são ignorados.</span><span class="sxs-lookup"><span data-stu-id="86381-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="86381-335">Informações do chamador não afetam a resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="86381-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="86381-336">Como os parâmetros opcionais são atribuídos ainda são omitidos do código-fonte do chamador, a resolução de sobrecarga ignora esses parâmetros da mesma forma que ela ignora outros parâmetros opcionais omitidos ([resolução de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="86381-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="86381-337">Informações do chamador são substituídas somente quando uma função é invocada explicitamente no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="86381-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="86381-338">Invocações implícitas, tais como chamadas de construtor implícito pai não tem um local de origem e não substituirá as informações do chamador.</span><span class="sxs-lookup"><span data-stu-id="86381-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="86381-339">Além disso, as chamadas que são vinculadas dinamicamente não substituirá as informações do chamador.</span><span class="sxs-lookup"><span data-stu-id="86381-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="86381-340">Quando uma parâmetro atribuído é omitido nesses casos, informações do chamador, o valor padrão especificado do parâmetro é usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="86381-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="86381-341">Uma exceção é expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="86381-341">One exception is query-expressions.</span></span> <span data-ttu-id="86381-342">Esses são considerados expansões sintáticas e se as chamadas elas se expandem para omitir parâmetros opcionais com atributos de informações do chamador, informações do chamador serão substituídas.</span><span class="sxs-lookup"><span data-stu-id="86381-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="86381-343">O local usado é o local da cláusula de consulta que a chamada foi gerada a partir.</span><span class="sxs-lookup"><span data-stu-id="86381-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="86381-344">Se mais de um atributo de informações do chamador é especificado em um determinado parâmetro, elas são preferenciais na seguinte ordem: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="86381-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="86381-345">O atributo CallerLineNumber</span><span class="sxs-lookup"><span data-stu-id="86381-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="86381-346">O `System.Runtime.CompilerServices.CallerLineNumberAttribute` é permitido em parâmetros opcionais quando há uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) do valor de constante `int.MaxValue` para o tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86381-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="86381-347">Isso garante que qualquer número de linha de não negativo até esse valor pode ser passado sem erro.</span><span class="sxs-lookup"><span data-stu-id="86381-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="86381-348">Se uma invocação de função de um local no código-fonte omite um parâmetro opcional com a `CallerLineNumberAttribute`, em seguida, um literal numérico que representa o número da linha do local é usado como um argumento para a invocação em vez do valor de parâmetro padrão.</span><span class="sxs-lookup"><span data-stu-id="86381-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="86381-349">Se a invocação abrange várias linhas, a linha escolhida é dependente de implementação.</span><span class="sxs-lookup"><span data-stu-id="86381-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="86381-350">Observe que o número de linha pode ser afetado pelas `#line` diretivas ([diretivas de linha](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="86381-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="86381-351">O atributo CallerFilePath</span><span class="sxs-lookup"><span data-stu-id="86381-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="86381-352">O `System.Runtime.CompilerServices.CallerFilePathAttribute` é permitido em parâmetros opcionais quando há uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) de `string` para o tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86381-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="86381-353">Se uma invocação de função de um local no código-fonte omite um parâmetro opcional com a `CallerFilePathAttribute`, em seguida, um literal de cadeia de caracteres que representa o caminho do arquivo do local é usado como um argumento para a invocação em vez do valor de parâmetro padrão.</span><span class="sxs-lookup"><span data-stu-id="86381-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="86381-354">O formato do caminho do arquivo é dependente da implementação.</span><span class="sxs-lookup"><span data-stu-id="86381-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="86381-355">Observe que o caminho do arquivo pode ser afetado pelas `#line` diretivas ([diretivas de linha](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="86381-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="86381-356">O atributo CallerMemberName</span><span class="sxs-lookup"><span data-stu-id="86381-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="86381-357">O `System.Runtime.CompilerServices.CallerMemberNameAttribute` é permitido em parâmetros opcionais quando há uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) de `string` para o tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="86381-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="86381-358">Se uma invocação de função de um local dentro do corpo de um membro da função ou dentro de um atributo aplicado ao membro da função em si ou seu tipo de retorno, parâmetros ou parâmetros de tipo no código-fonte omite um parâmetro opcional com a `CallerMemberNameAttribute`, em seguida, um literal de cadeia de caracteres que representa o nome desse membro é usado como um argumento para a invocação em vez do valor de parâmetro padrão.</span><span class="sxs-lookup"><span data-stu-id="86381-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="86381-359">Para invocações que ocorrem dentro de métodos genéricos, apenas o nome do método em si é usado, sem a lista de parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="86381-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="86381-360">Para invocações que ocorrem dentro de implementações de membros de interface explícita, apenas o nome do método em si é usado, sem a qualificação de interface anterior.</span><span class="sxs-lookup"><span data-stu-id="86381-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="86381-361">Para invocações que ocorrem dentro de acessadores de propriedade ou evento, o nome do membro usado é de propriedade ou evento em si.</span><span class="sxs-lookup"><span data-stu-id="86381-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="86381-362">Para invocações que ocorrem dentro de acessadores de indexador, o nome do membro usado é que o fornecido por uma `IndexerNameAttribute` ([atributo IndexerName The](attributes.md#the-indexername-attribute)) sobre o membro de indexador, se presente, ou o nome padrão `Item` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="86381-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="86381-363">Para invocações que ocorrem dentro de declarações de construtores de instância, construtores estáticos, destruidores e operadores, o membro nome usado é dependente da implementação.</span><span class="sxs-lookup"><span data-stu-id="86381-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="86381-364">Atributos para interoperação</span><span class="sxs-lookup"><span data-stu-id="86381-364">Attributes for Interoperation</span></span>

<span data-ttu-id="86381-365">Observação: Esta seção é aplicável somente para a implementação do Microsoft .NET da linguagem c#.</span><span class="sxs-lookup"><span data-stu-id="86381-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="86381-366">Interoperação com componentes COM e Win32</span><span class="sxs-lookup"><span data-stu-id="86381-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="86381-367">O tempo de execução do .NET fornece um grande número de atributos que permitem que programas em c# interoperar com componentes escritos usando COM e DLLs do Win32.</span><span class="sxs-lookup"><span data-stu-id="86381-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="86381-368">Por exemplo, o `DllImport` atributo pode ser usado em um `static extern` método para indicar que a implementação do método deve ser encontrado em uma DLL Win32.</span><span class="sxs-lookup"><span data-stu-id="86381-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="86381-369">Esses atributos são encontrados no `System.Runtime.InteropServices` namespace e a documentação detalhada para esses atributos for encontrado na documentação de tempo de execução do .NET.</span><span class="sxs-lookup"><span data-stu-id="86381-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="86381-370">Interoperação com outras linguagens .NET</span><span class="sxs-lookup"><span data-stu-id="86381-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="86381-371">O atributo IndexerName</span><span class="sxs-lookup"><span data-stu-id="86381-371">The IndexerName attribute</span></span>

<span data-ttu-id="86381-372">Indexadores são implementados no .NET usando as propriedades indexadas e tem um nome nos metadados do .NET.</span><span class="sxs-lookup"><span data-stu-id="86381-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="86381-373">Se nenhum `IndexerName` atributo estiver presente para um indexador e, em seguida, o nome `Item` é usado por padrão.</span><span class="sxs-lookup"><span data-stu-id="86381-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="86381-374">O `IndexerName` atributo permite que um desenvolvedor substituir esse padrão e especifique um nome diferente.</span><span class="sxs-lookup"><span data-stu-id="86381-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
