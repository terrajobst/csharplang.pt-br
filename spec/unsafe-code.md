---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310366"
---
# <a name="unsafe-code"></a><span data-ttu-id="4daa1-101">Código não seguro</span><span class="sxs-lookup"><span data-stu-id="4daa1-101">Unsafe code</span></span>

<span data-ttu-id="4daa1-102">A linguagem C# principal, conforme definido nos capítulos anteriores, difere notavelmente de C e C++ em sua omissão de ponteiros como um tipo de dados.</span><span class="sxs-lookup"><span data-stu-id="4daa1-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="4daa1-103">Em vez C# disso, o fornece referências e a capacidade de criar objetos gerenciados por um coletor de lixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="4daa1-104">Esse design, juntamente com outros recursos, torna C# uma linguagem muito mais segura do que C ou C++.</span><span class="sxs-lookup"><span data-stu-id="4daa1-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="4daa1-105">No idioma principal C# , simplesmente não é possível ter uma variável não inicializada, um ponteiro "pendente" ou uma expressão que indexe uma matriz além de seus limites.</span><span class="sxs-lookup"><span data-stu-id="4daa1-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="4daa1-106">Categorias inteiras de bugs que, rotineiramente, C++ C e programas são eliminados.</span><span class="sxs-lookup"><span data-stu-id="4daa1-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="4daa1-107">Embora praticamente todos os tipos de ponteiro constructem em C ou C++ tenham um C#tipo de referência equivalente em, no entanto, há situações em que o acesso a tipos de ponteiro se torna uma necessidade.</span><span class="sxs-lookup"><span data-stu-id="4daa1-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="4daa1-108">Por exemplo, a interface com o sistema operacional subjacente, o acesso a um dispositivo mapeado por memória ou a implementação de um algoritmo de tempo crítico pode não ser possível ou prática sem acesso a ponteiros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="4daa1-109">Para atender a essa necessidade C# , o fornece a capacidade de escrever ***código não seguro***.</span><span class="sxs-lookup"><span data-stu-id="4daa1-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="4daa1-110">Em código não seguro, é possível declarar e operar em ponteiros, executar conversões entre ponteiros e tipos integrais, para obter o endereço de variáveis e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4daa1-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="4daa1-111">De certa forma, escrever código inseguro é muito parecido com escrever código C dentro C# de um programa.</span><span class="sxs-lookup"><span data-stu-id="4daa1-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="4daa1-112">O código não seguro é, de fato, um recurso "seguro" da perspectiva de desenvolvedores e usuários.</span><span class="sxs-lookup"><span data-stu-id="4daa1-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="4daa1-113">O código não seguro deve ser claramente marcado com o modificador `unsafe`, de modo que os desenvolvedores não podem possivelmente usar recursos não seguros acidentalmente, e o mecanismo de execução funciona para garantir que o código não seguro não possa ser executado em um ambiente não confiável.</span><span class="sxs-lookup"><span data-stu-id="4daa1-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="4daa1-114">Contextos não seguros</span><span class="sxs-lookup"><span data-stu-id="4daa1-114">Unsafe contexts</span></span>

<span data-ttu-id="4daa1-115">Os recursos não seguros do C# estão disponíveis somente em contextos não seguros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="4daa1-116">Um contexto não seguro é introduzido incluindo um modificador `unsafe` na declaração de um tipo ou membro ou empregando um *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="4daa1-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="4daa1-117">Uma declaração de Class, struct, interface ou delegate pode incluir um modificador `unsafe` e, nesse caso, toda a extensão textual dessa declaração de tipo (incluindo o corpo da classe, struct ou interface) é considerada um contexto não seguro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="4daa1-118">Uma declaração de um campo, método, propriedade, evento, indexador, operador, Construtor de instância, destruidor ou construtor estático pode incluir um modificador `unsafe`, caso em que a extensão textual inteira dessa declaração de membro é considerada um contexto não seguro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="4daa1-119">Um *unsafe_statement* permite o uso de um contexto sem segurança dentro de um *bloco*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="4daa1-120">Toda a extensão textual do *bloco* associado é considerada um contexto não seguro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="4daa1-121">As produções gramaticais associadas são mostradas abaixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="4daa1-122">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="4daa1-123">o modificador `unsafe` especificado na declaração struct faz com que toda a extensão textual da declaração struct se torne um contexto não seguro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="4daa1-124">Portanto, é possível declarar os campos `Left` e `Right` como sendo de um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="4daa1-125">O exemplo acima também poderia ser escrito</span><span class="sxs-lookup"><span data-stu-id="4daa1-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="4daa1-126">Aqui, os modificadores `unsafe` nas declarações de campo fazem com que essas declarações sejam consideradas contextos não seguros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="4daa1-127">Além de estabelecer um contexto não seguro, permitindo, portanto, o uso de tipos de ponteiro, o modificador `unsafe` não tem nenhum efeito sobre um tipo ou um membro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="4daa1-128">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="4daa1-129">o modificador `unsafe` no método `F` em `A` simplesmente faz com que a extensão textual de `F` se torne um contexto não seguro no qual os recursos não seguros do idioma podem ser usados.</span><span class="sxs-lookup"><span data-stu-id="4daa1-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="4daa1-130">Na substituição de `F` em `B`, não é necessário especificar novamente o modificador `unsafe`--a menos que, é claro, o método `F` no `B` em si precisa acessar recursos não seguros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="4daa1-131">A situação é um pouco diferente quando um tipo de ponteiro faz parte da assinatura do método</span><span class="sxs-lookup"><span data-stu-id="4daa1-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="4daa1-132">Aqui, como a assinatura do `F` inclui um tipo de ponteiro, ela só pode ser gravada em um contexto sem segurança.</span><span class="sxs-lookup"><span data-stu-id="4daa1-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="4daa1-133">No entanto, o contexto não seguro pode ser introduzido, tornando a classe inteira insegura, como é o caso no `A`, ou incluindo um modificador `unsafe` na declaração do método, como é o caso no `B`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="4daa1-134">Tipos de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-134">Pointer types</span></span>

<span data-ttu-id="4daa1-135">Em um contexto não seguro, um *tipo* ([tipos](types.md)) pode ser um *pointer_type* , bem como um *value_type* ou um *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="4daa1-136">No entanto, um *pointer_type* também pode ser usado em uma expressão `typeof` ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) fora de um contexto sem segurança, pois tal uso não é seguro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="4daa1-137">Um *pointer_type* é escrito como um *unmanaged_type* ou a palavra-chave `void`, seguido por um token `*`:</span><span class="sxs-lookup"><span data-stu-id="4daa1-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="4daa1-138">O tipo especificado antes do `*` em um tipo de ponteiro é chamado de ***tipo Referent*** do tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="4daa1-139">Representa o tipo da variável para a qual um valor do tipo de ponteiro aponta.</span><span class="sxs-lookup"><span data-stu-id="4daa1-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="4daa1-140">Ao contrário das referências (valores de tipos de referência), os ponteiros não são rastreados pelo coletor de lixo – o coletor de lixo não tem conhecimento de ponteiros e dos dados para os quais eles apontam.</span><span class="sxs-lookup"><span data-stu-id="4daa1-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="4daa1-141">Por esse motivo, um ponteiro não tem permissão para apontar para uma referência ou para uma struct que contém referências, e o tipo Referent de um ponteiro deve ser um *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="4daa1-142">Um *unmanaged_type* é qualquer tipo que não seja um tipo *reference_type* ou construído e não contém *reference_type* ou campos de tipo construído em qualquer nível de aninhamento.</span><span class="sxs-lookup"><span data-stu-id="4daa1-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="4daa1-143">Em outras palavras, um *unmanaged_type* é um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="4daa1-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="4daa1-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 ou 2.</span><span class="sxs-lookup"><span data-stu-id="4daa1-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="4daa1-145">Qualquer *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="4daa1-146">Qualquer *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="4daa1-147">Qualquer *struct_type* definida pelo usuário que não seja um tipo construído e contém campos de *unmanaged_type*s.</span><span class="sxs-lookup"><span data-stu-id="4daa1-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="4daa1-148">A regra intuitiva para a combinação de ponteiros e referências é que referents de referências (objetos) têm permissão para conter ponteiros, mas referents de ponteiros não tem permissão para conter referências.</span><span class="sxs-lookup"><span data-stu-id="4daa1-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="4daa1-149">Alguns exemplos de tipos de ponteiro são fornecidos na tabela a seguir:</span><span class="sxs-lookup"><span data-stu-id="4daa1-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="4daa1-150">__Exemplo__</span><span class="sxs-lookup"><span data-stu-id="4daa1-150">__Example__</span></span> | <span data-ttu-id="4daa1-151">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="4daa1-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="4daa1-152">Ponteiro para `byte`</span><span class="sxs-lookup"><span data-stu-id="4daa1-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="4daa1-153">Ponteiro para `char`</span><span class="sxs-lookup"><span data-stu-id="4daa1-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="4daa1-154">Ponteiro para ponteiro para `int`</span><span class="sxs-lookup"><span data-stu-id="4daa1-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="4daa1-155">Matriz unidimensional de ponteiros para `int`</span><span class="sxs-lookup"><span data-stu-id="4daa1-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="4daa1-156">Ponteiro para tipo desconhecido</span><span class="sxs-lookup"><span data-stu-id="4daa1-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="4daa1-157">Para uma determinada implementação, todos os tipos de ponteiro devem ter o mesmo tamanho e representação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="4daa1-158">Ao contrário de C++C e, quando vários ponteiros são declarados na C# mesma declaração, no `*` é gravado juntamente com o tipo subjacente, não como um pontuador de prefixo em cada nome de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="4daa1-159">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="4daa1-160">O valor de um ponteiro com o tipo `T*` representa o endereço de uma variável do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="4daa1-161">O operador de indireção de ponteiro `*` ([indireção de ponteiro](unsafe-code.md#pointer-indirection)) pode ser usado para acessar essa variável.</span><span class="sxs-lookup"><span data-stu-id="4daa1-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="4daa1-162">Por exemplo, dada uma variável `P` do tipo `int*`, a expressão `*P` denota que a variável `int` foi encontrada no endereço contido em `P`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="4daa1-163">Como uma referência de objeto, um ponteiro pode ser `null`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="4daa1-164">A aplicação do operador de indireção a um ponteiro `null` resulta em comportamento definido pela implementação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="4daa1-165">Um ponteiro com o valor `null` é representado por todos os bits-zero.</span><span class="sxs-lookup"><span data-stu-id="4daa1-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="4daa1-166">O tipo `void*` representa um ponteiro para um tipo desconhecido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="4daa1-167">Como o tipo Referent é desconhecido, o operador de indireção não pode ser aplicado a um ponteiro do tipo `void*`, nem qualquer aritmética pode ser executada nesse ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="4daa1-168">No entanto, um ponteiro do tipo `void*` pode ser convertido em qualquer outro tipo de ponteiro (e vice-versa).</span><span class="sxs-lookup"><span data-stu-id="4daa1-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="4daa1-169">Os tipos de ponteiro são uma categoria separada de tipos.</span><span class="sxs-lookup"><span data-stu-id="4daa1-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="4daa1-170">Diferentemente dos tipos de referência e tipos de valor, os tipos de ponteiro não herdam de `object` e nenhuma conversões existe entre os tipos de ponteiro e `object`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="4daa1-171">Em particular, boxing e unboxing ([boxing e unboxing](types.md#boxing-and-unboxing)) não têm suporte para ponteiros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="4daa1-172">No entanto, as conversões são permitidas entre diferentes tipos de ponteiro e entre os tipos de ponteiro e os tipos integrais.</span><span class="sxs-lookup"><span data-stu-id="4daa1-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="4daa1-173">Isso é descrito em [conversões de ponteiro](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="4daa1-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="4daa1-174">Um *pointer_type* não pode ser usado como um argumento de tipo ([tipos construídos](types.md#constructed-types)), e a inferência de tipos ([inferência](expressions.md#type-inference)de tipos) falha em chamadas de método genérico que teriam inferido um argumento de tipo para ser um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="4daa1-175">Um *pointer_type* pode ser usado como o tipo de um campo volátil ([campos voláteis](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="4daa1-176">Embora os ponteiros possam ser passados como parâmetros `ref` ou `out`, fazer isso pode causar um comportamento indefinido, já que o ponteiro pode ser bem definido para apontar para uma variável local que não existe mais quando o método chamado retorna ou o objeto fixo ao qual ele costumava apontar , não é mais fixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="4daa1-177">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4daa1-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="4daa1-178">Um método pode retornar um valor de algum tipo, e esse tipo pode ser um ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="4daa1-179">Por exemplo, quando um ponteiro é fornecido para uma sequência contígua de `int`s, a contagem de elementos dessa sequência e algum outro valor de `int`, o método a seguir retorna o endereço desse valor nessa sequência, se ocorrer uma correspondência; caso contrário, ele retornará `null`:</span><span class="sxs-lookup"><span data-stu-id="4daa1-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="4daa1-180">Em um contexto não seguro, várias construções estão disponíveis para operar em ponteiros:</span><span class="sxs-lookup"><span data-stu-id="4daa1-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="4daa1-181">O operador `*` pode ser usado para executar o indireção de ponteiro ([indireção de ponteiro](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="4daa1-182">O operador `->` pode ser usado para acessar um membro de uma struct por meio de um ponteiro ([acesso de membro de ponteiro](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="4daa1-183">O operador `[]` pode ser usado para indexar um ponteiro ([acesso de elemento de ponteiro](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="4daa1-184">O operador `&` pode ser usado para obter o endereço de uma variável ([o operador address-of](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="4daa1-185">Os operadores `++` e `--` podem ser usados para incrementar e decrementar ponteiros ([incremento de ponteiro e decréscimo](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="4daa1-186">Os operadores `+` e `-` podem ser usados para executar aritmética de ponteiro ([aritmética de ponteiro](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="4daa1-187">Os operadores `==`, `!=`, `<`, `>`, `<=` e `=>` podem ser usados para comparar ponteiros ([comparação de ponteiros](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="4daa1-188">O operador `stackalloc` pode ser usado para alocar memória da pilha de chamadas ([buffers de tamanho fixo](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="4daa1-189">A instrução `fixed` pode ser usada para corrigir temporariamente uma variável para que seu endereço possa ser obtido ([a instrução Fixed](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="4daa1-190">Variáveis fixas e móveis</span><span class="sxs-lookup"><span data-stu-id="4daa1-190">Fixed and moveable variables</span></span>

<span data-ttu-id="4daa1-191">O operador address-of ([o operador address-of](unsafe-code.md#the-address-of-operator)) e a instrução `fixed` ([a instrução Fixed](unsafe-code.md#the-fixed-statement)) dividem variáveis em duas categorias: ***Variáveis fixas*** e ***variáveis móveis***.</span><span class="sxs-lookup"><span data-stu-id="4daa1-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="4daa1-192">As variáveis fixas residem em locais de armazenamento que não são afetados pela operação do coletor de lixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="4daa1-193">(Exemplos de variáveis fixas incluem variáveis locais, parâmetros de valor e variáveis criadas pela desreferenciação de ponteiros.) Por outro lado, as variáveis móveis residem em locais de armazenamento que estão sujeitos a realocação ou descarte pelo coletor de lixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="4daa1-194">(Exemplos de variáveis móveis incluem campos em objetos e elementos de matrizes.)</span><span class="sxs-lookup"><span data-stu-id="4daa1-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="4daa1-195">O operador `&` ([o operador address-of](unsafe-code.md#the-address-of-operator)) permite que o endereço de uma variável fixa seja obtido sem restrições.</span><span class="sxs-lookup"><span data-stu-id="4daa1-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="4daa1-196">No entanto, como uma variável móvel está sujeita a realocação ou alienação pelo coletor de lixo, o endereço de uma variável móvel só pode ser obtido usando uma instrução `fixed` ([a instrução Fixed](unsafe-code.md#the-fixed-statement)) e esse endereço permanece válido somente para o duração da instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="4daa1-197">Em termos precisos, uma variável fixa é uma das seguintes:</span><span class="sxs-lookup"><span data-stu-id="4daa1-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="4daa1-198">Uma variável resultante de um *Simple_name* ([nomes simples](expressions.md#simple-names)) que se refere a uma variável local ou a um parâmetro de valor, a menos que a variável seja capturada por uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="4daa1-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="4daa1-199">Uma variável resultante de um *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `V.I`, em que `V` é uma variável fixa de um *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="4daa1-200">Uma variável resultante de um *pointer_indirection_expression* ([indireção de ponteiro](unsafe-code.md#pointer-indirection)) do formulário `*P`, um *pointer_member_access* (acesso de[membro de ponteiro](unsafe-code.md#pointer-member-access)) do formato `P->I` ou um *pointer_element_access* ( [Acesso de elemento de ponteiro](unsafe-code.md#pointer-element-access)) do formulário `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="4daa1-201">Todas as outras variáveis são classificadas como variáveis móveis.</span><span class="sxs-lookup"><span data-stu-id="4daa1-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="4daa1-202">Observe que um campo estático é classificado como uma variável móvel.</span><span class="sxs-lookup"><span data-stu-id="4daa1-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="4daa1-203">Observe também que um parâmetro `ref` ou `out` é classificado como uma variável móvel, mesmo que o argumento fornecido para o parâmetro seja uma variável fixa.</span><span class="sxs-lookup"><span data-stu-id="4daa1-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="4daa1-204">Por fim, observe que uma variável produzida por desreferenciar um ponteiro é sempre classificada como uma variável fixa.</span><span class="sxs-lookup"><span data-stu-id="4daa1-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="4daa1-205">Conversões de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-205">Pointer conversions</span></span>

<span data-ttu-id="4daa1-206">Em um contexto sem segurança, o conjunto de conversões implícitas disponíveis ([conversões implícitas](conversions.md#implicit-conversions)) é estendido para incluir as seguintes conversões de ponteiro implícitas:</span><span class="sxs-lookup"><span data-stu-id="4daa1-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="4daa1-207">De qualquer *pointer_type* para o tipo `void*`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="4daa1-208">Do `null` literal para qualquer *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="4daa1-209">Além disso, em um contexto não seguro, o conjunto de conversões explícitas disponíveis ([conversões explícitas](conversions.md#explicit-conversions)) é estendido para incluir as seguintes conversões de ponteiro explícitas:</span><span class="sxs-lookup"><span data-stu-id="4daa1-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="4daa1-210">De qualquer *pointer_type* para qualquer outro *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="4daa1-211">De `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong` para qualquer *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="4daa1-212">De qualquer *pointer_type* para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="4daa1-213">Por fim, em um contexto não seguro, o conjunto de conversões implícitas padrão ([conversões implícitas padrão](conversions.md#standard-implicit-conversions)) inclui a seguinte conversão de ponteiro:</span><span class="sxs-lookup"><span data-stu-id="4daa1-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="4daa1-214">De qualquer *pointer_type* para o tipo `void*`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="4daa1-215">As conversões entre dois tipos de ponteiro nunca alteram o valor real do ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="4daa1-216">Em outras palavras, uma conversão de um tipo de ponteiro para outro não tem nenhum efeito sobre o endereço subjacente fornecido pelo ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="4daa1-217">Quando um tipo de ponteiro é convertido em outro, se o ponteiro resultante não estiver alinhado corretamente para o tipo apontado, o comportamento será indefinido se o resultado for de referência.</span><span class="sxs-lookup"><span data-stu-id="4daa1-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="4daa1-218">Em geral, o conceito "alinhado corretamente" é transitiva: se um ponteiro para digitar `A` estiver corretamente alinhado para um ponteiro para digitar `B`, que, por sua vez, está alinhado corretamente para um ponteiro para digitar `C`, então um ponteiro para digitar `A` está alinhado corretamente para um Ponteiro para o tipo `C`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="4daa1-219">Considere o seguinte caso em que uma variável com um tipo é acessada por meio de um ponteiro para um tipo diferente:</span><span class="sxs-lookup"><span data-stu-id="4daa1-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="4daa1-220">Quando um tipo de ponteiro é convertido em um ponteiro para byte, o resultado aponta para o byte de endereçamento mais baixo da variável.</span><span class="sxs-lookup"><span data-stu-id="4daa1-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="4daa1-221">Incrementos sucessivos do resultado, até o tamanho da variável, geram ponteiros para os bytes restantes dessa variável.</span><span class="sxs-lookup"><span data-stu-id="4daa1-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="4daa1-222">Por exemplo, o método a seguir exibe cada um dos oito bytes em um duplo como um valor hexadecimal:</span><span class="sxs-lookup"><span data-stu-id="4daa1-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="4daa1-223">É claro que a saída produzida depende da ordenação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="4daa1-224">Os mapeamentos entre ponteiros e inteiros são definidos pela implementação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="4daa1-225">No entanto, em arquiteturas de CPU de 32 \* e 64 bits com um espaço de endereço linear, conversões de ponteiros para ou de tipos integrais normalmente se comportam exatamente como conversões de valores `uint` ou `ulong`, respectivamente, de ou para esses tipos integral.</span><span class="sxs-lookup"><span data-stu-id="4daa1-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="4daa1-226">Matrizes de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-226">Pointer arrays</span></span>

<span data-ttu-id="4daa1-227">Em um contexto não seguro, matrizes de ponteiros podem ser construídas.</span><span class="sxs-lookup"><span data-stu-id="4daa1-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="4daa1-228">Somente algumas das conversões que se aplicam a outros tipos de matriz são permitidas em matrizes de ponteiro:</span><span class="sxs-lookup"><span data-stu-id="4daa1-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="4daa1-229">A conversão de referência implícita ([conversões de referência implícitas](conversions.md#implicit-reference-conversions)) de qualquer *array_type* para `System.Array` e as interfaces que ele implementa também se aplica a matrizes de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="4daa1-230">No entanto, qualquer tentativa de acessar os elementos da matriz por meio de `System.Array` ou das interfaces que ele implementa resultará em uma exceção em tempo de execução, pois os tipos de ponteiro não são conversíveis para `object`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="4daa1-231">As conversões de referência implícita e explícita (conversões de[referência implícita](conversions.md#implicit-reference-conversions), [conversões de referência explícitas](conversions.md#explicit-reference-conversions)) de um tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` e suas interfaces base genéricas nunca se aplicam a matrizes de ponteiros, como os tipos de ponteiro não podem ser usados como argumentos de tipo, e não há conversões de tipos de ponteiro para tipos que não sejam ponteiros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="4daa1-232">A conversão de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) do `System.Array` e das interfaces que ele implementa para qualquer *array_type* se aplica a matrizes de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="4daa1-233">As conversões de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) de `System.Collections.Generic.IList<S>` e suas interfaces base para um tipo de matriz unidimensional `T[]` nunca se aplicam a matrizes de ponteiro, pois tipos de ponteiro não podem ser usados como argumentos de tipo, e há Não há conversões de tipos de ponteiro para tipos sem ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="4daa1-234">Essas restrições significam que a expansão para a instrução `foreach` sobre matrizes descritas na [instrução foreach](statements.md#the-foreach-statement) não pode ser aplicada a matrizes de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="4daa1-235">Em vez disso, uma instrução foreach do formulário</span><span class="sxs-lookup"><span data-stu-id="4daa1-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="4daa1-236">onde o tipo de `x` é um tipo de matriz do formulário `T[,,...,]`, `N` é o número de dimensões menos 1 e `T` ou `V` é um tipo de ponteiro, é expandido usando loops for aninhado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4daa1-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="4daa1-237">As variáveis `a`, `i0`, `i1`,..., `iN` não são visíveis ou acessíveis para `x` ou *embedded_statement* ou qualquer outro código-fonte do programa.</span><span class="sxs-lookup"><span data-stu-id="4daa1-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="4daa1-238">A variável `v` é somente leitura na instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="4daa1-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="4daa1-239">Se não houver uma conversão explícita ([conversões de ponteiro](unsafe-code.md#pointer-conversions)) de `T` (o tipo de elemento) para `V`, um erro será produzido e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="4daa1-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="4daa1-240">Se `x` tiver o valor `null`, um `System.NullReferenceException` será lançado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4daa1-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="4daa1-241">Ponteiros em expressões</span><span class="sxs-lookup"><span data-stu-id="4daa1-241">Pointers in expressions</span></span>

<span data-ttu-id="4daa1-242">Em um contexto não seguro, uma expressão pode gerar um resultado de um tipo de ponteiro, mas fora de um contexto sem segurança, é um erro de tempo de compilação para uma expressão ser de um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="4daa1-243">Em termos precisos, fora de um contexto sem segurança, um erro de tempo de compilação ocorrerá se qualquer *Simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro](expressions.md#member-access)), *invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)) ou  *element_access* ([acesso de elemento](expressions.md#element-access)) é de um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="4daa1-244">Em um contexto sem segurança, as produções *primary_no_array_creation_expression* ([expressões primárias](expressions.md#primary-expressions)) e *unary_expression* ([operadores unários](expressions.md#unary-operators)) permitem as seguintes construções adicionais:</span><span class="sxs-lookup"><span data-stu-id="4daa1-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="4daa1-245">Essas construções são descritas nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="4daa1-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="4daa1-246">A precedência e a Associação dos operadores inseguros é implícita pela gramática.</span><span class="sxs-lookup"><span data-stu-id="4daa1-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="4daa1-247">Indireção de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-247">Pointer indirection</span></span>

<span data-ttu-id="4daa1-248">Um *pointer_indirection_expression* consiste em um asterisco (`*`) seguido por um *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="4daa1-249">O operador unário `*` denota indireção de ponteiro e é usado para obter a variável para a qual um ponteiro aponta.</span><span class="sxs-lookup"><span data-stu-id="4daa1-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="4daa1-250">O resultado da avaliação de `*P`, em que `P` é uma expressão de um tipo de ponteiro `T*`, é uma variável do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="4daa1-251">É um erro de tempo de compilação para aplicar o operador unário `*` a uma expressão do tipo `void*` ou a uma expressão que não é de um tipo de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="4daa1-252">O efeito de aplicar o operador `*` unário a um ponteiro `null` é definido pela implementação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="4daa1-253">Em particular, não há nenhuma garantia de que essa operação gere um `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="4daa1-254">Se um valor inválido tiver sido atribuído ao ponteiro, o comportamento do operador unário `*` será indefinido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="4daa1-255">Entre os valores inválidos para desreferenciar um ponteiro pelo operador `*` unário são um endereço alinhado incorretamente para o tipo apontado (consulte o exemplo em [conversões de ponteiro](unsafe-code.md#pointer-conversions)) e o endereço de uma variável após o término de seu tempo de vida.</span><span class="sxs-lookup"><span data-stu-id="4daa1-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="4daa1-256">Para fins de análise de atribuição definitiva, uma variável produzida pela avaliação de uma expressão do formulário `*P` é considerada inicialmente atribuída ([variáveis inicialmente atribuídas](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="4daa1-257">Acesso de membro de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-257">Pointer member access</span></span>

<span data-ttu-id="4daa1-258">Um *pointer_member_access* consiste em um *primary_expression*, seguido por um token "`->`", seguido por um *identificador* e um *type_argument_list*opcional.</span><span class="sxs-lookup"><span data-stu-id="4daa1-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="4daa1-259">Em um membro de ponteiro, o acesso do formulário `P->I`, `P` deve ser uma expressão de um tipo de ponteiro diferente de `void*` e `I` deve indicar um membro acessível do tipo para o qual `P` aponta.</span><span class="sxs-lookup"><span data-stu-id="4daa1-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="4daa1-260">Um acesso de membro de ponteiro do formulário `P->I` é avaliado exatamente como `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="4daa1-261">Para obter uma descrição do operador de indireção de ponteiro (`*`), consulte [indireção de ponteiro](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="4daa1-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="4daa1-262">Para obter uma descrição do operador de acesso de membro (`.`), consulte [acesso de membro](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="4daa1-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="4daa1-263">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="4daa1-264">o operador `->` é usado para acessar campos e invocar um método de uma struct por meio de um ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="4daa1-265">Como a operação `P->I` é precisamente equivalente a `(*P).I`, o método `Main` poderia igualmente ter sido escrito:</span><span class="sxs-lookup"><span data-stu-id="4daa1-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="4daa1-266">Acesso de elemento de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-266">Pointer element access</span></span>

<span data-ttu-id="4daa1-267">Um *pointer_element_access* consiste em um *primary_no_array_creation_expression* seguido por uma expressão colocada em "`[`" e "`]`".</span><span class="sxs-lookup"><span data-stu-id="4daa1-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="4daa1-268">Em um elemento de ponteiro, o acesso ao formulário `P[E]`, `P` deve ser uma expressão de um tipo de ponteiro diferente de `void*`, e `E` deve ser uma expressão que possa ser convertida implicitamente em `int`, `uint`, `long` ou `ulong`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="4daa1-269">Um elemento de ponteiro acesso ao formulário `P[E]` é avaliado exatamente como `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="4daa1-270">Para obter uma descrição do operador de indireção de ponteiro (`*`), consulte [indireção de ponteiro](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="4daa1-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="4daa1-271">Para obter uma descrição do operador de adição de ponteiro (`+`), consulte [aritmética de ponteiro](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="4daa1-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="4daa1-272">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="4daa1-273">um acesso de elemento de ponteiro é usado para inicializar o buffer de caracteres em um loop `for`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="4daa1-274">Como a operação `P[E]` é precisamente equivalente a `*(P + E)`, o exemplo poderia igualmente ter sido escrito:</span><span class="sxs-lookup"><span data-stu-id="4daa1-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="4daa1-275">O operador de acesso de elemento de ponteiro não verifica erros fora de limites e o comportamento ao acessar um elemento fora dos limites é indefinido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="4daa1-276">Isso é o mesmo que C e C++.</span><span class="sxs-lookup"><span data-stu-id="4daa1-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="4daa1-277">O operador address-of</span><span class="sxs-lookup"><span data-stu-id="4daa1-277">The address-of operator</span></span>

<span data-ttu-id="4daa1-278">Um *addressof_expression* consiste em um e comercial (`&`) seguido por um *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="4daa1-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="4daa1-279">Dada uma expressão `E` que é de um tipo `T` e é classificado como uma variável fixa ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)), a construção `&E` computa o endereço da variável fornecida pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="4daa1-280">O tipo do resultado é `T*` e é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="4daa1-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="4daa1-281">Ocorrerá um erro de tempo de compilação se `E` não for classificado como uma variável, se `E` for classificado como uma variável local somente leitura ou se `E` denotar uma variável móvel.</span><span class="sxs-lookup"><span data-stu-id="4daa1-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="4daa1-282">No último caso, uma instrução fixa ([a instrução Fixed](unsafe-code.md#the-fixed-statement)) pode ser usada para "corrigir" temporariamente a variável antes de obter seu endereço.</span><span class="sxs-lookup"><span data-stu-id="4daa1-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="4daa1-283">Conforme indicado no [acesso de membro](expressions.md#member-access), fora de um construtor de instância ou construtor estático para uma struct ou classe que define um campo `readonly`, esse campo é considerado um valor, e não uma variável.</span><span class="sxs-lookup"><span data-stu-id="4daa1-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="4daa1-284">Como tal, seu endereço não pode ser obtido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="4daa1-285">Da mesma forma, o endereço de uma constante não pode ser obtido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="4daa1-286">O operador `&` não exige que seu argumento seja definitivamente atribuído, mas após uma operação `&`, a variável à qual o operador é aplicado é considerada definitivamente atribuída no caminho de execução no qual a operação ocorre.</span><span class="sxs-lookup"><span data-stu-id="4daa1-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="4daa1-287">É de responsabilidade do programador garantir que a inicialização correta da variável realmente ocorra nessa situação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="4daa1-288">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="4daa1-289">`i` é considerado definitivamente atribuído após a operação de `&i` usada para inicializar `p`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="4daa1-290">A atribuição para `*p` em vigor Inicializa `i`, mas a inclusão dessa inicialização é responsabilidade do programador e nenhum erro de tempo de compilação ocorrerá se a atribuição tiver sido removida.</span><span class="sxs-lookup"><span data-stu-id="4daa1-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="4daa1-291">As regras de atribuição definitiva para o operador `&` existem, de modo que a inicialização redundante de variáveis locais pode ser evitada.</span><span class="sxs-lookup"><span data-stu-id="4daa1-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="4daa1-292">Por exemplo, muitas APIs externas usam um ponteiro para uma estrutura que é preenchida pela API.</span><span class="sxs-lookup"><span data-stu-id="4daa1-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="4daa1-293">Chamadas para essas APIs normalmente passam o endereço de uma variável de struct local e sem a regra, a inicialização redundante da variável de struct seria necessária.</span><span class="sxs-lookup"><span data-stu-id="4daa1-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="4daa1-294">Incrementar e decrementar ponteiros</span><span class="sxs-lookup"><span data-stu-id="4daa1-294">Pointer increment and decrement</span></span>

<span data-ttu-id="4daa1-295">Em um contexto sem segurança, os operadores `++` e `--` ([incremento de sufixo e diminuir](expressions.md#postfix-increment-and-decrement-operators) os operadores e [incremento de prefixo e decrementar](expressions.md#prefix-increment-and-decrement-operators)) podem ser aplicados a variáveis de ponteiro de todos os tipos, exceto `void*`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="4daa1-296">Portanto, para cada tipo de ponteiro `T*`, os seguintes operadores são definidos implicitamente:</span><span class="sxs-lookup"><span data-stu-id="4daa1-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="4daa1-297">Os operadores produzem os mesmos resultados que `x + 1` e `x - 1`, respectivamente ([aritmética de ponteiro](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="4daa1-298">Em outras palavras, para uma variável de ponteiro do tipo `T*`, o operador `++` adiciona `sizeof(T)` ao endereço contido na variável, e o operador `--` subtrai `sizeof(T)` do endereço contido na variável.</span><span class="sxs-lookup"><span data-stu-id="4daa1-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="4daa1-299">Se uma operação de incremento ou diminuição de ponteiro estourar o domínio do tipo de ponteiro, o resultado será definido pela implementação, mas nenhuma exceção será produzida.</span><span class="sxs-lookup"><span data-stu-id="4daa1-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="4daa1-300">Aritmética do ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-300">Pointer arithmetic</span></span>

<span data-ttu-id="4daa1-301">Em um contexto sem segurança, os operadores `+` e `-` ([operador de adição](expressions.md#addition-operator) e [subtração](expressions.md#subtraction-operator)) podem ser aplicados a valores de todos os tipos de ponteiro, exceto `void*`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="4daa1-302">Portanto, para cada tipo de ponteiro `T*`, os seguintes operadores são definidos implicitamente:</span><span class="sxs-lookup"><span data-stu-id="4daa1-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="4daa1-303">Dada uma expressão `P` de um tipo de ponteiro `T*` e uma expressão `N` do tipo `int`, `uint`, `long` ou `ulong`, as expressões `P + N` e `N + P` calculam o valor de ponteiro do tipo `T*` que resulta da adição de 0 ao endereço fornecido por 1.</span><span class="sxs-lookup"><span data-stu-id="4daa1-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="4daa1-304">Da mesma forma, a expressão `P - N` computa o valor do ponteiro do tipo `T*` resultante da subtração `N * sizeof(T)` do endereço fornecido pelo `P`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="4daa1-305">Dadas duas expressões, `P` e `Q`, de um tipo de ponteiro `T*`, a expressão `P - Q` computa a diferença entre os endereços fornecidos por `P` e `Q` e, em seguida, divide essa diferença por `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="4daa1-306">O tipo do resultado é sempre `long`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-306">The type of the result is always `long`.</span></span> <span data-ttu-id="4daa1-307">Na verdade, `P - Q` é computado como `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="4daa1-308">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4daa1-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="4daa1-309">que produz a saída:</span><span class="sxs-lookup"><span data-stu-id="4daa1-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="4daa1-310">Se uma operação aritmética de ponteiro estoura o domínio do tipo de ponteiro, o resultado é truncado de maneira definida pela implementação, mas nenhuma exceção é produzida.</span><span class="sxs-lookup"><span data-stu-id="4daa1-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="4daa1-311">Comparação de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4daa1-311">Pointer comparison</span></span>

<span data-ttu-id="4daa1-312">Em um contexto sem segurança, os operadores `==`, `!=`, `<`, `>`, `<=` e `=>` ([operadores relacionais e de teste de tipo](expressions.md#relational-and-type-testing-operators)) podem ser aplicados a valores de todos os tipos de ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="4daa1-313">Os operadores de comparação de ponteiro são:</span><span class="sxs-lookup"><span data-stu-id="4daa1-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="4daa1-314">Como existe uma conversão implícita de qualquer tipo de ponteiro para o tipo `void*`, os operandos de qualquer tipo de ponteiro podem ser comparados usando esses operadores.</span><span class="sxs-lookup"><span data-stu-id="4daa1-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="4daa1-315">Os operadores de comparação comparam os endereços fornecidos pelos dois operandos como se fossem inteiros sem sinal.</span><span class="sxs-lookup"><span data-stu-id="4daa1-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="4daa1-316">O operador sizeof</span><span class="sxs-lookup"><span data-stu-id="4daa1-316">The sizeof operator</span></span>

<span data-ttu-id="4daa1-317">O operador `sizeof` retorna o número de bytes ocupados por uma variável de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="4daa1-318">O tipo especificado como um operando para `sizeof` deve ser um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="4daa1-319">O resultado do operador `sizeof` é um valor do tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="4daa1-320">Para determinados tipos predefinidos, o operador `sizeof` produz um valor constante, conforme mostrado na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="4daa1-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="4daa1-321">__Expressão__</span><span class="sxs-lookup"><span data-stu-id="4daa1-321">__Expression__</span></span>   | <span data-ttu-id="4daa1-322">__Result__</span><span class="sxs-lookup"><span data-stu-id="4daa1-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="4daa1-323">Para todos os outros tipos, o resultado do operador `sizeof` é definido pela implementação e é classificado como um valor, não uma constante.</span><span class="sxs-lookup"><span data-stu-id="4daa1-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="4daa1-324">A ordem na qual os membros são empacotados em um struct não é especificada.</span><span class="sxs-lookup"><span data-stu-id="4daa1-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="4daa1-325">Para fins de alinhamento, pode haver um preenchimento sem nome no início de uma struct, dentro de uma struct e no final da estrutura.</span><span class="sxs-lookup"><span data-stu-id="4daa1-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="4daa1-326">O conteúdo dos bits usados como preenchimento é indeterminado.</span><span class="sxs-lookup"><span data-stu-id="4daa1-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="4daa1-327">Quando aplicado a um operando que tem o tipo struct, o resultado é o número total de bytes em uma variável desse tipo, incluindo qualquer preenchimento.</span><span class="sxs-lookup"><span data-stu-id="4daa1-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="4daa1-328">A instrução Fixed</span><span class="sxs-lookup"><span data-stu-id="4daa1-328">The fixed statement</span></span>

<span data-ttu-id="4daa1-329">Em um contexto não seguro, a produção *embedded_statement* ([instruções](statements.md)) permite um constructo adicional, a instrução `fixed`, que é usada para "corrigir" uma variável móvel, de modo que seu endereço permaneça constante durante a instrução .</span><span class="sxs-lookup"><span data-stu-id="4daa1-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="4daa1-330">Cada *fixed_pointer_declarator* declara uma variável local do *pointer_type* especificado e inicializa essa variável local com o endereço computado pelo *fixed_pointer_initializer*correspondente.</span><span class="sxs-lookup"><span data-stu-id="4daa1-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="4daa1-331">Uma variável local declarada em uma instrução `fixed` é acessível em qualquer *fixed_pointer_initializer*s que ocorre à direita da declaração dessa variável e no *embedded_statement* da instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="4daa1-332">Uma variável local declarada por uma instrução `fixed` é considerada somente leitura.</span><span class="sxs-lookup"><span data-stu-id="4daa1-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="4daa1-333">Ocorrerá um erro de tempo de compilação se a instrução inserida tentar modificar essa variável local (por meio de atribuição ou os operadores `++` e `--`) ou passá-la como um parâmetro `ref` ou `out`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="4daa1-334">Um *fixed_pointer_initializer* pode ser um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="4daa1-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="4daa1-335">O token "`&`" seguido por um *variable_reference* ([regras precisas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) para uma variável móvel ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)) de um tipo não gerenciado `T`, desde que o tipo `T*` seja implicitamente conversível para o tipo de ponteiro fornecido na instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="4daa1-336">Nesse caso, o inicializador computa o endereço da variável fornecida, e a variável é garantida para permanecer em um endereço fixo durante a instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="4daa1-337">Uma expressão de um *array_type* com elementos de um tipo não gerenciado `T`, desde que o tipo `T*` seja implicitamente conversível no tipo de ponteiro fornecido na instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="4daa1-338">Nesse caso, o inicializador computa o endereço do primeiro elemento na matriz e toda a matriz é garantida para permanecer em um endereço fixo durante a instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="4daa1-339">Se a expressão de matriz for nula ou se a matriz tiver zero elementos, o inicializador computará um endereço igual a zero.</span><span class="sxs-lookup"><span data-stu-id="4daa1-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="4daa1-340">Uma expressão do tipo `string`, desde que o tipo `char*` seja implicitamente conversível para o tipo de ponteiro fornecido na instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="4daa1-341">Nesse caso, o inicializador computa o endereço do primeiro caractere na cadeia de caracteres e toda a cadeia de caracteres é garantida para permanecer em um endereço fixo durante a instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="4daa1-342">O comportamento da instrução `fixed` será definido pela implementação se a expressão de cadeia de caracteres for nula.</span><span class="sxs-lookup"><span data-stu-id="4daa1-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="4daa1-343">Um *Simple_name* ou *member_access* que faz referência a um membro de buffer de tamanho fixo de uma variável móvel, desde que o tipo do membro de buffer de tamanho fixo seja implicitamente conversível para o tipo de ponteiro fornecido na instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="4daa1-344">Nesse caso, o inicializador computa um ponteiro para o primeiro elemento do buffer de tamanho fixo ([buffers de tamanho fixo em expressões](unsafe-code.md#fixed-size-buffers-in-expressions)) e o buffer de tamanho fixo é garantido para permanecer em um endereço fixo durante a instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="4daa1-345">Para cada endereço calculado por um *fixed_pointer_initializer* , a instrução `fixed` garante que a variável referenciada pelo endereço não esteja sujeita a realocação ou descarte pelo coletor de lixo pela duração da instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="4daa1-346">Por exemplo, se o endereço computado por um *fixed_pointer_initializer* referenciar um campo de um objeto ou um elemento de uma instância de matriz, a instrução `fixed` garante que a instância de objeto recipiente não seja realocada ou descartada durante o tempo de vida da instrução.</span><span class="sxs-lookup"><span data-stu-id="4daa1-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="4daa1-347">É responsabilidade do programador garantir que os ponteiros criados pelas instruções `fixed` não sobrevivem além da execução dessas instruções.</span><span class="sxs-lookup"><span data-stu-id="4daa1-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="4daa1-348">Por exemplo, quando os ponteiros criados pelas instruções `fixed` são passados para APIs externas, é responsabilidade do programador garantir que as APIs não mantenham memória desses ponteiros.</span><span class="sxs-lookup"><span data-stu-id="4daa1-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="4daa1-349">Os objetos fixos podem causar a fragmentação do heap (porque eles não podem ser movidos).</span><span class="sxs-lookup"><span data-stu-id="4daa1-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="4daa1-350">Por esse motivo, os objetos devem ser corrigidos somente quando absolutamente necessário e, em seguida, apenas pelo menor tempo possível.</span><span class="sxs-lookup"><span data-stu-id="4daa1-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="4daa1-351">O exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="4daa1-352">demonstra vários usos da instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="4daa1-353">A primeira instrução corrige e Obtém o endereço de um campo estático, a segunda instrução corrige e Obtém o endereço de um campo de instância, e a terceira instrução corrige e Obtém o endereço de um elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="4daa1-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="4daa1-354">Em cada caso, seria um erro para usar o operador regular `&`, uma vez que as variáveis são todas classificadas como variáveis móveis.</span><span class="sxs-lookup"><span data-stu-id="4daa1-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="4daa1-355">A quarta instrução `fixed` no exemplo acima produz um resultado semelhante ao terceiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="4daa1-356">Este exemplo da instrução `fixed` usa `string`:</span><span class="sxs-lookup"><span data-stu-id="4daa1-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="4daa1-357">Em um contexto não seguro, os elementos de matrizes unidimensionais são armazenados em ordem de índice crescente, começando com o índice `0` e terminando com o índice `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="4daa1-358">Para matrizes multidimensionais, os elementos de matriz são armazenados de modo que os índices da dimensão mais à direita sejam aumentados primeiro, depois a dimensão à esquerda e assim por diante à esquerda.</span><span class="sxs-lookup"><span data-stu-id="4daa1-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="4daa1-359">Dentro de uma instrução `fixed` que obtém um ponteiro `p` para uma instância de matriz `a`, os valores de ponteiro que variam de `p` a `p + a.Length - 1` representam endereços dos elementos na matriz.</span><span class="sxs-lookup"><span data-stu-id="4daa1-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="4daa1-360">Da mesma forma, as variáveis que variam de `p[0]` a `p[a.Length - 1]` representam os elementos reais da matriz.</span><span class="sxs-lookup"><span data-stu-id="4daa1-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="4daa1-361">Devido à maneira como as matrizes são armazenadas, podemos tratar uma matriz de qualquer dimensão como se ela fosse linear.</span><span class="sxs-lookup"><span data-stu-id="4daa1-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="4daa1-362">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4daa1-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="4daa1-363">que produz a saída:</span><span class="sxs-lookup"><span data-stu-id="4daa1-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="4daa1-364">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="4daa1-365">uma instrução `fixed` é usada para corrigir uma matriz para que seu endereço possa ser passado para um método que usa um ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="4daa1-366">No exemplo:</span><span class="sxs-lookup"><span data-stu-id="4daa1-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="4daa1-367">uma instrução Fixed é usada para corrigir um buffer de tamanho fixo de uma struct para que seu endereço possa ser usado como um ponteiro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="4daa1-368">Um valor `char*` produzido pela correção de uma instância de cadeia de caracteres sempre aponta para uma cadeia de caracteres terminada em nulo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="4daa1-369">Dentro de uma instrução fixa que obtém um ponteiro `p` a uma instância de cadeia de caracteres `s`, os valores de ponteiro que variam de `p` a `p + s.Length - 1` representam endereços dos caracteres na cadeia de caracteres e o valor do ponteiro `p + s.Length` sempre aponta para um caractere nulo (o caractere com valor `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="4daa1-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="4daa1-370">Modificar objetos de tipo gerenciado por meio de ponteiros fixos pode resultar em um comportamento indefinido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="4daa1-371">Por exemplo, como as cadeias de caracteres são imutáveis, é responsabilidade do programador garantir que o caractere referenciado por um ponteiro para uma cadeia de caracteres fixa não seja modificado.</span><span class="sxs-lookup"><span data-stu-id="4daa1-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="4daa1-372">A terminação nula automática de cadeias de caracteres é particularmente conveniente ao chamar APIs externas que esperam cadeias de caracteres "estilo C".</span><span class="sxs-lookup"><span data-stu-id="4daa1-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="4daa1-373">Observe, no entanto, que uma instância de cadeia de caracteres tem permissão para conter caracteres nulos.</span><span class="sxs-lookup"><span data-stu-id="4daa1-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="4daa1-374">Se esses caracteres nulos estiverem presentes, a cadeia de caracteres aparecerá truncada quando tratada como um `char*` terminada em nulo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="4daa1-375">Buffers de tamanho fixo</span><span class="sxs-lookup"><span data-stu-id="4daa1-375">Fixed size buffers</span></span>

<span data-ttu-id="4daa1-376">Buffers de tamanho fixo são usados para declarar matrizes em linha "C style" como membros de structs e são principalmente úteis para a interface com APIs não gerenciadas.</span><span class="sxs-lookup"><span data-stu-id="4daa1-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="4daa1-377">Declarações de buffer de tamanho fixo</span><span class="sxs-lookup"><span data-stu-id="4daa1-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="4daa1-378">Um ***buffer de tamanho fixo*** é um membro que representa o armazenamento de um buffer de comprimento fixo de variáveis de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="4daa1-379">Uma declaração de buffer de tamanho fixo apresenta um ou mais buffers de tamanho fixo de um determinado tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="4daa1-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="4daa1-380">Buffers de tamanho fixo só são permitidos em declarações struct e só podem ocorrer em contextos não seguros ([contextos não seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="4daa1-381">Uma declaração de buffer de tamanho fixo pode incluir um conjunto de atributos ([atributos](attributes.md)), um modificador `new` ([modificadores](classes.md#modifiers)), uma combinação válida dos quatro modificadores de acesso ([parâmetros de tipo e restrições](classes.md#type-parameters-and-constraints)) e um modificador de `unsafe` ([não seguro contextos](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="4daa1-382">Os atributos e os modificadores se aplicam a todos os membros declarados pela declaração de buffer de tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="4daa1-383">É um erro para que o mesmo modificador apareça várias vezes em uma declaração de buffer de tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="4daa1-384">Uma declaração de buffer de tamanho fixo não tem permissão para incluir o modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="4daa1-385">O tipo de elemento de buffer de uma declaração de buffer de tamanho fixo especifica o tipo de elemento dos buffers introduzidos pela declaração.</span><span class="sxs-lookup"><span data-stu-id="4daa1-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="4daa1-386">O tipo de elemento de buffer deve ser um dos tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 ou 1.</span><span class="sxs-lookup"><span data-stu-id="4daa1-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="4daa1-387">O tipo de elemento buffer é seguido por uma lista de declaradores de buffer de tamanho fixo, e cada um deles introduz um novo membro.</span><span class="sxs-lookup"><span data-stu-id="4daa1-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="4daa1-388">Um Declarador de buffer de tamanho fixo consiste em um identificador que nomeia o membro, seguido por uma expressão constante entre os tokens `[` e `]`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="4daa1-389">A expressão constante denota o número de elementos no membro introduzidos pelo Declarador de buffer de tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="4daa1-390">O tipo da expressão constante deve ser conversível implicitamente no tipo `int`, e o valor deve ser um inteiro positivo diferente de zero.</span><span class="sxs-lookup"><span data-stu-id="4daa1-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="4daa1-391">Os elementos de um buffer de tamanho fixo têm garantia de serem dispostos em sequência na memória.</span><span class="sxs-lookup"><span data-stu-id="4daa1-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="4daa1-392">Uma declaração de buffer de tamanho fixo que declara vários buffers de tamanho fixo é equivalente a várias declarações de uma única declaração de buffer de tamanho fixo com os mesmos atributos e tipos de elemento.</span><span class="sxs-lookup"><span data-stu-id="4daa1-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="4daa1-393">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="4daa1-394">equivale a</span><span class="sxs-lookup"><span data-stu-id="4daa1-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="4daa1-395">Buffers de tamanho fixo em expressões</span><span class="sxs-lookup"><span data-stu-id="4daa1-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="4daa1-396">A pesquisa de Membros ([operadores](expressions.md#operators)) de um membro de buffer de tamanho fixo prossegue exatamente como a pesquisa de membros de um campo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="4daa1-397">Um buffer de tamanho fixo pode ser referenciado em uma expressão usando um *Simple_name* ([inferência de tipos](expressions.md#type-inference)) ou um *member_access* ([verificação de tempo de compilação de resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="4daa1-398">Quando um membro de buffer de tamanho fixo é referenciado como um nome simples, o efeito é o mesmo que um membro de acesso do formulário `this.I`, em que `I` é o membro do buffer de tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="4daa1-399">Em um acesso de membro do formulário `E.I`, se `E` for de um tipo struct e uma pesquisa de membro de `I` nesse tipo struct identificar um membro de tamanho fixo, `E.I` será avaliado como classificado a seguir:</span><span class="sxs-lookup"><span data-stu-id="4daa1-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="4daa1-400">Se a expressão `E.I` não ocorrer em um contexto sem segurança, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="4daa1-401">Se `E` for classificado como um valor, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="4daa1-402">Caso contrário, se `E` for uma variável móvel ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)) e a expressão `E.I` não for um *fixed_pointer_initializer* ([a instrução fixa](unsafe-code.md#the-fixed-statement)), ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="4daa1-403">Caso contrário, `E` faz referência a uma variável fixa e o resultado da expressão é um ponteiro para o primeiro elemento do membro de buffer de tamanho fixo `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="4daa1-404">O resultado é do tipo `S*`, em que `S` é o tipo de elemento de `I` e é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="4daa1-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="4daa1-405">Os elementos subsequentes do buffer de tamanho fixo podem ser acessados usando operações de ponteiro do primeiro elemento.</span><span class="sxs-lookup"><span data-stu-id="4daa1-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="4daa1-406">Ao contrário do acesso a matrizes, o acesso aos elementos de um buffer de tamanho fixo é uma operação não segura e não é verificado.</span><span class="sxs-lookup"><span data-stu-id="4daa1-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="4daa1-407">O exemplo a seguir declara e usa uma struct com um membro de buffer de tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="4daa1-408">Verificação de atribuição definitiva</span><span class="sxs-lookup"><span data-stu-id="4daa1-408">Definite assignment checking</span></span>

<span data-ttu-id="4daa1-409">Os buffers de tamanho fixo não estão sujeitos à verificação de atribuição definitiva ([atribuição definitiva](variables.md#definite-assignment)) e os membros do buffer de tamanho fixo são ignorados para fins de verificação de atribuição definitiva de variáveis de tipo struct.</span><span class="sxs-lookup"><span data-stu-id="4daa1-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="4daa1-410">Quando o que contém a variável de struct mais externa de um membro de buffer de tamanho fixo é uma variável estática, uma variável de instância de uma instância de classe ou um elemento de matriz, os elementos do buffer de tamanho fixo são inicializados automaticamente para seus valores padrão ([padrão valores](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="4daa1-411">Em todos os outros casos, o conteúdo inicial de um buffer de tamanho fixo é indefinido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="4daa1-412">Alocação de pilha</span><span class="sxs-lookup"><span data-stu-id="4daa1-412">Stack allocation</span></span>

<span data-ttu-id="4daa1-413">Em um contexto não seguro, uma declaração de variável local ([declarações de variável local](statements.md#local-variable-declarations)) pode incluir um inicializador de alocação de pilha que aloca memória da pilha de chamadas.</span><span class="sxs-lookup"><span data-stu-id="4daa1-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="4daa1-414">O *unmanaged_type* indica o tipo dos itens que serão armazenados no local alocado recentemente e a *expressão* indica o número desses itens.</span><span class="sxs-lookup"><span data-stu-id="4daa1-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="4daa1-415">Em conjunto, eles especificam o tamanho de alocação necessário.</span><span class="sxs-lookup"><span data-stu-id="4daa1-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="4daa1-416">Como o tamanho de uma alocação de pilha não pode ser negativo, é um erro de tempo de compilação para especificar o número de itens como um *constant_expression* que é avaliado como um valor negativo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="4daa1-417">Um inicializador de alocação de pilha do formulário `stackalloc T[E]` requer que `T` seja um tipo não gerenciado ([tipos de ponteiro](unsafe-code.md#pointer-types)) e `E` seja uma expressão do tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="4daa1-418">A construção aloca `E * sizeof(T)` bytes da pilha de chamadas e retorna um ponteiro, do tipo `T*`, para o bloco recentemente alocado.</span><span class="sxs-lookup"><span data-stu-id="4daa1-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="4daa1-419">Se `E` for um valor negativo, o comportamento será indefinido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="4daa1-420">Se `E` for zero, nenhuma alocação será feita e o ponteiro retornado será definido pela implementação.</span><span class="sxs-lookup"><span data-stu-id="4daa1-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="4daa1-421">Se não houver memória suficiente disponível para alocar um bloco de determinado tamanho, um `System.StackOverflowException` será lançado.</span><span class="sxs-lookup"><span data-stu-id="4daa1-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="4daa1-422">O conteúdo da memória recém-alocada é indefinido.</span><span class="sxs-lookup"><span data-stu-id="4daa1-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="4daa1-423">Inicializadores de alocação de pilha não são permitidos em blocos `catch` ou `finally` ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="4daa1-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="4daa1-424">Não há nenhuma maneira de liberar explicitamente a memória alocada usando `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="4daa1-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="4daa1-425">Todos os blocos de memória alocada na pilha criados durante a execução de um membro de função são automaticamente descartados quando esse membro de função retorna.</span><span class="sxs-lookup"><span data-stu-id="4daa1-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="4daa1-426">Isso corresponde à função `alloca`, uma extensão normalmente encontrada em C e C++ implementações.</span><span class="sxs-lookup"><span data-stu-id="4daa1-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="4daa1-427">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4daa1-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="4daa1-428">um inicializador `stackalloc` é usado no método `IntToString` para alocar um buffer de 16 caracteres na pilha.</span><span class="sxs-lookup"><span data-stu-id="4daa1-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="4daa1-429">O buffer é descartado automaticamente quando o método retorna.</span><span class="sxs-lookup"><span data-stu-id="4daa1-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="4daa1-430">Alocação de memória dinâmica</span><span class="sxs-lookup"><span data-stu-id="4daa1-430">Dynamic memory allocation</span></span>

<span data-ttu-id="4daa1-431">Exceto pelo operador `stackalloc`, C# o não fornece construções predefinidas para gerenciar memória não coletada pelo lixo.</span><span class="sxs-lookup"><span data-stu-id="4daa1-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="4daa1-432">Esses serviços normalmente são fornecidos pelo suporte a bibliotecas de classes ou importados diretamente do sistema operacional subjacente.</span><span class="sxs-lookup"><span data-stu-id="4daa1-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="4daa1-433">Por exemplo, a classe `Memory` abaixo ilustra como as funções de heap de um sistema operacional subjacente podem ser acessadas de C#:</span><span class="sxs-lookup"><span data-stu-id="4daa1-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

<span data-ttu-id="4daa1-434">Um exemplo que usa a classe `Memory` é fornecido abaixo:</span><span class="sxs-lookup"><span data-stu-id="4daa1-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

<span data-ttu-id="4daa1-435">O exemplo aloca 256 bytes de memória por meio de `Memory.Alloc` e inicializa o bloco de memória com valores que aumentam de 0 a 255.</span><span class="sxs-lookup"><span data-stu-id="4daa1-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="4daa1-436">Em seguida, ele aloca uma matriz de bytes do elemento 256 e usa `Memory.Copy` para copiar o conteúdo do bloco de memória para a matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="4daa1-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="4daa1-437">Por fim, o bloco de memória é liberado usando `Memory.Free` e o conteúdo da matriz de bytes é apresentado no console.</span><span class="sxs-lookup"><span data-stu-id="4daa1-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
