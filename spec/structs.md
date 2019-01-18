---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229477"
---
# <a name="structs"></a><span data-ttu-id="b67c7-101">Structs</span><span class="sxs-lookup"><span data-stu-id="b67c7-101">Structs</span></span>

<span data-ttu-id="b67c7-102">Structs são semelhantes às classes que representam estruturas de dados que podem conter membros de dados e os membros da função.</span><span class="sxs-lookup"><span data-stu-id="b67c7-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="b67c7-103">No entanto, diferentemente das classes, structs são tipos de valor e não precisam de alocação de heap.</span><span class="sxs-lookup"><span data-stu-id="b67c7-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="b67c7-104">Uma variável de um tipo de struct diretamente contém os dados do struct, enquanto que uma variável de um tipo de classe contém uma referência aos dados, o último conhecido como um objeto.</span><span class="sxs-lookup"><span data-stu-id="b67c7-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="b67c7-105">Os structs são particularmente úteis para estruturas de dados pequenas que têm semântica de valor.</span><span class="sxs-lookup"><span data-stu-id="b67c7-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="b67c7-106">Números complexos, pontos em um sistema de coordenadas ou pares chave-valor em um dicionário são exemplos de structs.</span><span class="sxs-lookup"><span data-stu-id="b67c7-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="b67c7-107">Chave para essas estruturas de dados é que eles têm alguns membros de dados que eles não exigem o uso de herança ou identidade referencial, e que podem ser convenientemente implementados usando a semântica de valor em que a atribuição copia o valor em vez de referência.</span><span class="sxs-lookup"><span data-stu-id="b67c7-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="b67c7-108">Conforme descrito em [tipos simples](types.md#simple-types), os tipos simples fornecidos por c#, como `int`, `double`, e `bool`, são, na verdade, todos os tipos de struct.</span><span class="sxs-lookup"><span data-stu-id="b67c7-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="b67c7-109">Como esses tipos predefinidos são structs, também é possível usar a sobrecarga de operador para implementar os novos tipos de "primitivos" na linguagem c# e estruturas.</span><span class="sxs-lookup"><span data-stu-id="b67c7-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="b67c7-110">Dois exemplos de tais tipos são fornecidos no final deste capítulo ([exemplos de Struct](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="b67c7-111">Declarações de struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-111">Struct declarations</span></span>

<span data-ttu-id="b67c7-112">Um *struct_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara um novo struct:</span><span class="sxs-lookup"><span data-stu-id="b67c7-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="b67c7-113">Um *struct_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *struct_modifier*s ([modificadores de Struct](structs.md#struct-modifiers)), seguido por um recurso opcional `partial` modificador, seguido da palavra-chave `struct` e uma *identificador* que nomeia o struct, seguido por um opcional *type_parameter_list* especificação ([parâmetros de tipo](classes.md#type-parameters)), seguido por um recurso opcional *struct_interfaces* especificação ([Modificador parcial](structs.md#partial-modifier))), seguido por um recurso opcional *type_parameter_constraints_clause*especificação s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), seguido de um *struct_body* ([corpo de Struct](structs.md#struct-body)), opcionalmente seguido por um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="b67c7-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="b67c7-114">Modificadores de struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-114">Struct modifiers</span></span>

<span data-ttu-id="b67c7-115">Um *struct_declaration* opcionalmente pode incluir uma sequência de modificadores de struct:</span><span class="sxs-lookup"><span data-stu-id="b67c7-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="b67c7-116">Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de struct.</span><span class="sxs-lookup"><span data-stu-id="b67c7-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="b67c7-117">Os modificadores de uma declaração de struct têm o mesmo significado que aquelas de uma declaração de classe ([declarações de classe](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="b67c7-118">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="b67c7-118">Partial modifier</span></span>

<span data-ttu-id="b67c7-119">O `partial` modificador indica que isso *struct_declaration* é uma declaração de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="b67c7-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="b67c7-120">Várias declarações de estrutura parcial com o mesmo nome dentro de uma declaração de namespace ou tipo delimitador se combinam para formar uma declaração de struct, seguindo as regras especificadas na [tipos parciais](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="b67c7-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="b67c7-121">Interfaces de struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-121">Struct interfaces</span></span>

<span data-ttu-id="b67c7-122">Uma declaração de struct pode incluir um *struct_interfaces* especificação, nesse caso o struct é considerado implementar diretamente os tipos de interface fornecido.</span><span class="sxs-lookup"><span data-stu-id="b67c7-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="b67c7-123">Implementações de interface são discutidas mais detalhadamente em [implementações de Interface](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="b67c7-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="b67c7-124">Corpo de struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-124">Struct body</span></span>

<span data-ttu-id="b67c7-125">O *struct_body* de um struct define os membros de struct.</span><span class="sxs-lookup"><span data-stu-id="b67c7-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="b67c7-126">Membros de struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-126">Struct members</span></span>

<span data-ttu-id="b67c7-127">Os membros de um struct consistem em membros introduzidos por seus *struct_member_declaration*s e os membros herdados do tipo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="b67c7-128">Exceto pelas diferenças observadas na [diferenças de classe e struct](structs.md#class-and-struct-differences), as descrições dos membros de classe fornecidos na [membros de classe](classes.md#class-members) por meio do [iteradores](classes.md#iterators) se aplicam a struct membros também.</span><span class="sxs-lookup"><span data-stu-id="b67c7-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="b67c7-129">Diferenças de classe e struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-129">Class and struct differences</span></span>

<span data-ttu-id="b67c7-130">Structs são diferentes das classes de várias maneiras importantes:</span><span class="sxs-lookup"><span data-stu-id="b67c7-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="b67c7-131">Structs são tipos de valor ([semântica de valor](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="b67c7-132">Todos os tipos de structs herdam implicitamente da classe `System.ValueType` ([herança](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="b67c7-133">Atribuição para uma variável de um tipo de struct cria uma cópia do valor que está sendo atribuído ([atribuição](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="b67c7-134">O valor padrão de um struct é o valor produzido pela configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null` ([valores padrão](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="b67c7-135">Operações de conversão boxing e unboxing são usadas para converter entre um tipo de struct e `object` ([conversões Boxing e unboxing](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="b67c7-136">O significado dos `this` é diferente para structs ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="b67c7-137">Declarações de campo de instância para um struct não são permitidas para incluir os inicializadores de variável ([inicializadores de campo](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="b67c7-138">Um struct não é permitido para declarar um construtor de instância sem parâmetros ([construtores](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="b67c7-139">Um struct não é permitido declarar um destruidor ([destruidores](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="b67c7-140">Semântica de valor</span><span class="sxs-lookup"><span data-stu-id="b67c7-140">Value semantics</span></span>

<span data-ttu-id="b67c7-141">Structs são tipos de valor ([tipos de valor](types.md#value-types)) e são considerados como tendo a semântica de valor.</span><span class="sxs-lookup"><span data-stu-id="b67c7-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="b67c7-142">Classes, por outro lado, são tipos de referência ([tipos de referência](types.md#reference-types)) e são considerados como tendo a semântica de referência.</span><span class="sxs-lookup"><span data-stu-id="b67c7-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="b67c7-143">Uma variável de um tipo de struct diretamente contém os dados do struct, enquanto que uma variável de um tipo de classe contém uma referência aos dados, o último conhecido como um objeto.</span><span class="sxs-lookup"><span data-stu-id="b67c7-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="b67c7-144">Quando um struct `B` contém um campo de instância do tipo `A` e `A` é um tipo de struct, ele é um erro de tempo de compilação para `A` depender `B` ou um tipo construído a partir `B`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="b67c7-145">Um struct `X` ***depende diretamente*** um struct `Y` se `X` contém um campo de instância do tipo `Y`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="b67c7-146">Dada esta definição, o conjunto completo de estruturas dos quais depende de um struct é o fechamento transitivo do ***depende diretamente*** relação.</span><span class="sxs-lookup"><span data-stu-id="b67c7-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="b67c7-147">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="b67c7-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="b67c7-148">é um erro porque `Node` contém um campo de instância de seu próprio tipo.</span><span class="sxs-lookup"><span data-stu-id="b67c7-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="b67c7-149">Outro exemplo</span><span class="sxs-lookup"><span data-stu-id="b67c7-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="b67c7-150">é um erro porque cada um dos tipos `A`, `B`, e `C` dependem uns aos outros.</span><span class="sxs-lookup"><span data-stu-id="b67c7-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="b67c7-151">Com classes, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível operações em uma variável afetem o objeto referenciado por outra variável.</span><span class="sxs-lookup"><span data-stu-id="b67c7-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="b67c7-152">Com structs, as variáveis têm sua própria cópia dos dados (exceto no caso de `ref` e `out` variáveis de parâmetro), e não é possível que operações em um afetem o outro.</span><span class="sxs-lookup"><span data-stu-id="b67c7-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="b67c7-153">Além disso, como structs não são tipos de referência, não é possível para valores de um tipo de struct para ser `null`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="b67c7-154">Dada a declaração</span><span class="sxs-lookup"><span data-stu-id="b67c7-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="b67c7-155">o fragmento de código</span><span class="sxs-lookup"><span data-stu-id="b67c7-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="b67c7-156">gera o valor `10`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-156">outputs the value `10`.</span></span> <span data-ttu-id="b67c7-157">A atribuição de `a` à `b` cria uma cópia do valor, e `b` , portanto, não é afetado pela atribuição para `a.x`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="b67c7-158">Tinha `Point` em vez disso, foi declarado como uma classe, a saída seria `100` porque `a` e `b` fará referência ao mesmo objeto.</span><span class="sxs-lookup"><span data-stu-id="b67c7-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="b67c7-159">Herança</span><span class="sxs-lookup"><span data-stu-id="b67c7-159">Inheritance</span></span>

<span data-ttu-id="b67c7-160">Todos os tipos de structs herdam implicitamente da classe `System.ValueType`, que, por sua vez, herda da classe `object`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="b67c7-161">Uma declaração de struct pode especificar uma lista de interfaces implementadas, mas não é possível para uma declaração de struct especificar uma classe base.</span><span class="sxs-lookup"><span data-stu-id="b67c7-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="b67c7-162">Tipos de struct nunca são abstratos e sempre implicitamente são lacrados.</span><span class="sxs-lookup"><span data-stu-id="b67c7-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="b67c7-163">O `abstract` e `sealed` modificadores, portanto, não são permitidos em uma declaração de struct.</span><span class="sxs-lookup"><span data-stu-id="b67c7-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="b67c7-164">Uma vez que não há suporte para herança para structs, a acessibilidade declarada de um membro de struct não pode ser `protected` ou `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="b67c7-165">Membros da função em um struct não podem ser `abstract` ou `virtual`e o `override` modificador é permitido somente para substituir métodos herdados de `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="b67c7-166">Atribuição</span><span class="sxs-lookup"><span data-stu-id="b67c7-166">Assignment</span></span>

<span data-ttu-id="b67c7-167">Atribuição para uma variável de um tipo de struct cria uma cópia do valor que está sendo atribuído.</span><span class="sxs-lookup"><span data-stu-id="b67c7-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="b67c7-168">Isso difere da atribuição a uma variável de um tipo de classe, que copia a referência, mas não o objeto identificado por referência.</span><span class="sxs-lookup"><span data-stu-id="b67c7-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="b67c7-169">Semelhante a uma atribuição, quando um struct é passado como um valor de parâmetro ou retornado como o resultado de um membro da função, uma cópia do struct é criada.</span><span class="sxs-lookup"><span data-stu-id="b67c7-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="b67c7-170">Um struct pode ser passado por referência a um membro da função usando um `ref` ou `out` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b67c7-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="b67c7-171">Quando uma propriedade ou indexador de um struct é o destino de uma atribuição, a expressão de instância associada com o acesso de propriedade ou indexador deve ser classificada como uma variável.</span><span class="sxs-lookup"><span data-stu-id="b67c7-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="b67c7-172">Se a expressão de instância é classificada como um valor, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="b67c7-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="b67c7-173">Isso é descrito mais detalhadamente nas [atribuição simples](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="b67c7-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="b67c7-174">Valores padrão</span><span class="sxs-lookup"><span data-stu-id="b67c7-174">Default values</span></span>

<span data-ttu-id="b67c7-175">Conforme descrito em [valores padrão](variables.md#default-values), vários tipos de variáveis são inicializados automaticamente com seus valores padrão quando eles são criados.</span><span class="sxs-lookup"><span data-stu-id="b67c7-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="b67c7-176">Para variáveis de tipos de classe e outros tipos de referência, o valor padrão é `null`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="b67c7-177">No entanto, como structs são tipos de valor não podem ser `null`, o valor padrão de um struct é o valor produzido pela configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="b67c7-178">Referindo-se para o `Point` struct declarado acima, o exemplo</span><span class="sxs-lookup"><span data-stu-id="b67c7-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="b67c7-179">inicializa cada `Point` na matriz para o valor produzido pela configuração de `x` e `y` campos como zero.</span><span class="sxs-lookup"><span data-stu-id="b67c7-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="b67c7-180">O valor padrão de um struct corresponde ao valor retornado pelo construtor padrão do struct ([1&gt;construtores padrão](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="b67c7-181">Diferentemente de uma classe, um struct não é permitido para declarar um construtor de instância sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="b67c7-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="b67c7-182">Em vez disso, cada struct implicitamente tem um construtor de instância sem parâmetros que sempre retorna o valor resultante da configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="b67c7-183">Estruturas devem ser projetadas para considerar o estado de inicialização padrão em um estado válido.</span><span class="sxs-lookup"><span data-stu-id="b67c7-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="b67c7-184">No exemplo</span><span class="sxs-lookup"><span data-stu-id="b67c7-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="b67c7-185">o construtor de instância definidos pelo usuário protege contra valores nulos apenas onde ele é chamado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="b67c7-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="b67c7-186">Em casos em que um `KeyValuePair` variável está sujeito a inicialização de valor padrão, o `key` e `value` campos será nulos e o struct deve estar preparado para lidar com esse estado.</span><span class="sxs-lookup"><span data-stu-id="b67c7-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="b67c7-187">Conversões boxing e unboxing</span><span class="sxs-lookup"><span data-stu-id="b67c7-187">Boxing and unboxing</span></span>

<span data-ttu-id="b67c7-188">Um valor de um tipo de classe pode ser convertido no tipo `object` ou a um tipo de interface é implementado pela classe simplesmente tratando a referência como outro tipo em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="b67c7-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="b67c7-189">Da mesma forma, um valor do tipo `object` ou um valor de um tipo de interface pode ser convertido para um tipo de classe sem alterar a referência (mas obviamente uma verificação de tipo de tempo de execução é necessária neste caso).</span><span class="sxs-lookup"><span data-stu-id="b67c7-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="b67c7-190">Como structs não são tipos de referência, essas operações são implementadas diferente para tipos de struct.</span><span class="sxs-lookup"><span data-stu-id="b67c7-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="b67c7-191">Quando um valor de um tipo de struct é convertido para o tipo `object` ou para um tipo de interface é implementado pelo struct, uma operação de conversão boxing ocorre.</span><span class="sxs-lookup"><span data-stu-id="b67c7-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="b67c7-192">Da mesma forma, quando um valor do tipo `object` ou um valor de um tipo de interface é convertido para um tipo de struct, uma operação de conversão unboxing.</span><span class="sxs-lookup"><span data-stu-id="b67c7-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="b67c7-193">A principal diferença das mesmas operações em tipos de classe é a conversão boxing e unboxing copia o valor do struct dentro ou fora da instância do box.</span><span class="sxs-lookup"><span data-stu-id="b67c7-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="b67c7-194">Portanto, após uma operação de conversão unboxing ou conversão boxing, as alterações feitas ao struct não Demarcado não são refletidas na estrutura demarcada.</span><span class="sxs-lookup"><span data-stu-id="b67c7-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="b67c7-195">Quando um tipo de struct substitui um método virtual herdado de `System.Object` (como `Equals`, `GetHashCode`, ou `ToString`), a invocação do método virtual por meio de uma instância do tipo struct não faz com que a conversão boxing ocorra.</span><span class="sxs-lookup"><span data-stu-id="b67c7-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="b67c7-196">Isso é verdadeiro mesmo quando o struct é usado como um parâmetro de tipo e a invocação ocorrer através de uma instância do tipo de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="b67c7-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="b67c7-197">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b67c7-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="b67c7-198">A saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="b67c7-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="b67c7-199">Embora seja um estilo ruim para `ToString` para ter efeitos colaterais, o exemplo demonstra que nenhuma conversão boxing ocorreu para as três invocações de `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="b67c7-200">Da mesma forma, a conversão boxing nunca implicitamente ocorre ao acessar um membro em um parâmetro de tipo restrita.</span><span class="sxs-lookup"><span data-stu-id="b67c7-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="b67c7-201">Por exemplo, suponha que uma interface `ICounter` contém um método `Increment` que pode ser usado para modificar um valor.</span><span class="sxs-lookup"><span data-stu-id="b67c7-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="b67c7-202">Se `ICounter` é usado como uma restrição, a implementação do `Increment` método for chamado com uma referência à variável que `Increment` foi chamado em nunca uma cópia demarcada.</span><span class="sxs-lookup"><span data-stu-id="b67c7-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="b67c7-203">A primeira chamada para `Increment` modifica o valor na variável `x`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="b67c7-204">Isso não é equivalente a segunda chamada para `Increment`, que modifica o valor em uma cópia demarcada do `x`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="b67c7-205">Assim, a saída do programa é:</span><span class="sxs-lookup"><span data-stu-id="b67c7-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="b67c7-206">Para obter mais detalhes sobre conversões boxing e unboxing, consulte [conversões Boxing e unboxing](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="b67c7-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="b67c7-207">Significado deste</span><span class="sxs-lookup"><span data-stu-id="b67c7-207">Meaning of this</span></span>

<span data-ttu-id="b67c7-208">Dentro de um construtor de instância ou um membro da função de instância de uma classe, `this` é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="b67c7-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="b67c7-209">Assim, enquanto `this` pode ser usado para fazer referência à instância para o qual o membro da função foi invocado, não é possível atribuir a `this` em um membro da função de uma classe.</span><span class="sxs-lookup"><span data-stu-id="b67c7-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="b67c7-210">Dentro de um construtor de instância de um struct `this` corresponde a um `out` parâmetro do tipo struct e dentro de um membro da função de instância de um struct `this` corresponde a um `ref` parâmetro do tipo struct.</span><span class="sxs-lookup"><span data-stu-id="b67c7-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="b67c7-211">Em ambos os casos `this` é classificado como uma variável, e é possível modificar a estrutura inteira para o qual o membro da função foi invocado, atribuindo a `this` ou passando-o como um `ref` ou `out` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b67c7-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="b67c7-212">Inicializadores de campo</span><span class="sxs-lookup"><span data-stu-id="b67c7-212">Field initializers</span></span>

<span data-ttu-id="b67c7-213">Conforme descrito em [valores padrão](structs.md#default-values), o valor padrão de um struct consiste o valor resultante da configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="b67c7-214">Por esse motivo, um struct não permite que as declarações de campo de instância para incluir os inicializadores de variável.</span><span class="sxs-lookup"><span data-stu-id="b67c7-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="b67c7-215">Essa restrição se aplica somente a campos de instância.</span><span class="sxs-lookup"><span data-stu-id="b67c7-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="b67c7-216">Campos estáticos de um struct podem incluir inicializadores de variável.</span><span class="sxs-lookup"><span data-stu-id="b67c7-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="b67c7-217">O exemplo</span><span class="sxs-lookup"><span data-stu-id="b67c7-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="b67c7-218">é um erro porque as declarações de campo de instância incluem inicializadores de variável.</span><span class="sxs-lookup"><span data-stu-id="b67c7-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="b67c7-219">Construtores</span><span class="sxs-lookup"><span data-stu-id="b67c7-219">Constructors</span></span>

<span data-ttu-id="b67c7-220">Diferentemente de uma classe, um struct não é permitido para declarar um construtor de instância sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="b67c7-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="b67c7-221">Em vez disso, cada struct implicitamente tem um construtor de instância sem parâmetros que sempre retorna o valor resultante da configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo como nulo ([padrão construtores](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="b67c7-222">Um struct pode declarar construtores de instância ter parâmetros.</span><span class="sxs-lookup"><span data-stu-id="b67c7-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="b67c7-223">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="b67c7-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="b67c7-224">Dada a declaração acima, as instruções</span><span class="sxs-lookup"><span data-stu-id="b67c7-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="b67c7-225">ambos criam uma `Point` com `x` e `y` inicializados em zero.</span><span class="sxs-lookup"><span data-stu-id="b67c7-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="b67c7-226">Um construtor de instância struct não podem incluir um inicializador de construtor do formulário `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="b67c7-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="b67c7-227">Se o construtor de instância struct não especificar um inicializador de construtor, o `this` variable corresponde a um `out` parâmetro do tipo struct e é semelhante a um `out` parâmetro, `this` deve ser definitivamente atribuído ( [Atribuição definitiva](variables.md#definite-assignment)) em todos os locais em que o construtor retorna.</span><span class="sxs-lookup"><span data-stu-id="b67c7-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="b67c7-228">Se o construtor de instância struct Especifica um inicializador de construtor, o `this` variable corresponde a um `ref` parâmetro do tipo struct e é semelhante a um `ref` parâmetro, `this` é considerado atribuído definitivamente em entrada para o corpo do construtor.</span><span class="sxs-lookup"><span data-stu-id="b67c7-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="b67c7-229">Considere a implementação do construtor de instância abaixo:</span><span class="sxs-lookup"><span data-stu-id="b67c7-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="b67c7-230">Nenhuma função de membro de instância (incluindo os acessadores set das propriedades `X` e `Y`) pode ser chamado até que todos os campos da estrutura que está sendo construído tiveram sido atribuídos definitivamente.</span><span class="sxs-lookup"><span data-stu-id="b67c7-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="b67c7-231">A única exceção envolve propriedades implementadas automaticamente ([implementadas automaticamente propriedades](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="b67c7-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="b67c7-232">As regras de atribuição definitiva ([expressões de atribuição simples](variables.md#simple-assignment-expressions)) isentar especificamente a atribuição a uma propriedade automática de um tipo de struct dentro de um construtor de instância desse tipo struct: essa atribuição é considerada um definitiva atribuição de campo de suporte ocultos da propriedade automática.</span><span class="sxs-lookup"><span data-stu-id="b67c7-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="b67c7-233">Portanto, o seguinte é permitido:</span><span class="sxs-lookup"><span data-stu-id="b67c7-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="b67c7-234">Destruidores</span><span class="sxs-lookup"><span data-stu-id="b67c7-234">Destructors</span></span>

<span data-ttu-id="b67c7-235">Um struct não é permitido declarar um destruidor.</span><span class="sxs-lookup"><span data-stu-id="b67c7-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="b67c7-236">Construtores estáticos</span><span class="sxs-lookup"><span data-stu-id="b67c7-236">Static constructors</span></span>

<span data-ttu-id="b67c7-237">Construtores estáticos para structs siga com muita frequência as mesmas regras de classes.</span><span class="sxs-lookup"><span data-stu-id="b67c7-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="b67c7-238">A execução de um construtor estático para um tipo de struct é disparada pela primeira dos seguintes eventos ocorra dentro de um domínio de aplicativo:</span><span class="sxs-lookup"><span data-stu-id="b67c7-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="b67c7-239">Um membro estático do tipo struct é referenciado.</span><span class="sxs-lookup"><span data-stu-id="b67c7-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="b67c7-240">Um construtor explicitamente declarado do tipo struct é chamado.</span><span class="sxs-lookup"><span data-stu-id="b67c7-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="b67c7-241">A criação de valores padrão ([valores padrão](structs.md#default-values)) do struct tipos não dispara o construtor estático.</span><span class="sxs-lookup"><span data-stu-id="b67c7-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="b67c7-242">(Um exemplo disso é o valor inicial de elementos em uma matriz.)</span><span class="sxs-lookup"><span data-stu-id="b67c7-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="b67c7-243">Exemplos de struct</span><span class="sxs-lookup"><span data-stu-id="b67c7-243">Struct examples</span></span>

<span data-ttu-id="b67c7-244">A seguir mostra dois exemplos significativos do uso de `struct` tipos para criar tipos que podem ser usados da mesma forma para os tipos predefinidos da linguagem, mas com semântica modificada.</span><span class="sxs-lookup"><span data-stu-id="b67c7-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="b67c7-245">Tipo de inteiro de banco de dados</span><span class="sxs-lookup"><span data-stu-id="b67c7-245">Database integer type</span></span>

<span data-ttu-id="b67c7-246">O `DBInt` abaixo de struct implementa um tipo de inteiro que pode representar o conjunto completo de valores da `int` tipo, além de um estado adicional que indica um valor desconhecido.</span><span class="sxs-lookup"><span data-stu-id="b67c7-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="b67c7-247">Um tipo com essas características é comumente usado em bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="b67c7-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="b67c7-248">Tipo de banco de dados booliano</span><span class="sxs-lookup"><span data-stu-id="b67c7-248">Database boolean type</span></span>

<span data-ttu-id="b67c7-249">O `DBBool` struct abaixo implementa um tipo de lógico de três valores.</span><span class="sxs-lookup"><span data-stu-id="b67c7-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="b67c7-250">Os possíveis valores desse tipo são `DBBool.True`, `DBBool.False`, e `DBBool.Null`, onde o `Null` membro indica um valor desconhecido.</span><span class="sxs-lookup"><span data-stu-id="b67c7-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="b67c7-251">Esses tipos de lógicos de três valores são usados em bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="b67c7-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
