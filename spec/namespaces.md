# <a name="namespaces"></a><span data-ttu-id="4e48b-101">Namespaces</span><span class="sxs-lookup"><span data-stu-id="4e48b-101">Namespaces</span></span>

<span data-ttu-id="4e48b-102">Programas em c# são organizados com o uso de namespaces.</span><span class="sxs-lookup"><span data-stu-id="4e48b-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="4e48b-103">Namespaces são usados como um sistema de organização "interno" para um programa e como um sistema de organização "externa" — uma maneira de apresentar os elementos do programa que são expostos a outros programas.</span><span class="sxs-lookup"><span data-stu-id="4e48b-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="4e48b-104">Diretivas de uso ([diretivas Using](namespaces.md#using-directives)) são fornecidos para facilitar o uso de namespaces.</span><span class="sxs-lookup"><span data-stu-id="4e48b-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="4e48b-105">Unidades de compilação</span><span class="sxs-lookup"><span data-stu-id="4e48b-105">Compilation units</span></span>

<span data-ttu-id="4e48b-106">Um *compilation_unit* define a estrutura geral de um arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="4e48b-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="4e48b-107">Uma unidade de compilação consiste de zero ou mais *using_directive*s seguido por zero ou mais *global_attributes* seguido por zero ou mais *namespace_member_declaration*s .</span><span class="sxs-lookup"><span data-stu-id="4e48b-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="4e48b-108">Um programa c# consiste em uma ou mais unidades de compilação, cada um contido em um arquivo de origem separado.</span><span class="sxs-lookup"><span data-stu-id="4e48b-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="4e48b-109">Quando um programa c# é compilado, todas as unidades de compilação são processadas em conjunto.</span><span class="sxs-lookup"><span data-stu-id="4e48b-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="4e48b-110">Assim, unidades de compilação podem depender entre si, possivelmente de forma circular.</span><span class="sxs-lookup"><span data-stu-id="4e48b-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="4e48b-111">O *using_directive*s de um efeito de unidade de compilação a *global_attributes* e *namespace_member_declaration*s dessa unidade de compilação, mas não têm nenhum efeito outras unidades de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="4e48b-112">O *global_attributes* ([atributos](attributes.md)) de uma unidade de compilação permitem a especificação de atributos para o assembly de destino e o módulo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="4e48b-113">Módulos e assemblies atuam como contêineres físicos para tipos.</span><span class="sxs-lookup"><span data-stu-id="4e48b-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="4e48b-114">Um assembly pode conter vários módulos separados fisicamente.</span><span class="sxs-lookup"><span data-stu-id="4e48b-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="4e48b-115">O *namespace_member_declaration*s de cada unidade de compilação de um programa de colaborar com membros para um espaço de única declaração chamado namespace global.</span><span class="sxs-lookup"><span data-stu-id="4e48b-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="4e48b-116">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-116">For example:</span></span>

<span data-ttu-id="4e48b-117">Arquivo `A.cs`:</span><span class="sxs-lookup"><span data-stu-id="4e48b-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="4e48b-118">Arquivo `B.cs`:</span><span class="sxs-lookup"><span data-stu-id="4e48b-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="4e48b-119">As unidades de dois compilação contribuem para o namespace global único, nesse caso, declarando duas classes com nomes totalmente qualificados `A` e `B`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="4e48b-120">Porque as unidades de dois compilação contribuir com o mesmo espaço de declaração, teria sido um erro se cada continha uma declaração de um membro com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="4e48b-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="4e48b-121">Declarações de namespace</span><span class="sxs-lookup"><span data-stu-id="4e48b-121">Namespace declarations</span></span>

<span data-ttu-id="4e48b-122">Um *namespace_declaration* consiste na palavra-chave `namespace`, seguido por um nome de namespace e o corpo, opcionalmente seguido por um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="4e48b-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="4e48b-123">Um *namespace_declaration* pode ocorrer como uma declaração de nível superior em um *compilation_unit* ou como uma declaração de membro dentro de outra *namespace_declaration*.</span><span class="sxs-lookup"><span data-stu-id="4e48b-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="4e48b-124">Quando um *namespace_declaration* ocorre como uma declaração de nível superior em um *compilation_unit*, o namespace se torna um membro do namespace global.</span><span class="sxs-lookup"><span data-stu-id="4e48b-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="4e48b-125">Quando um *namespace_declaration* ocorre dentro de outra *namespace_declaration*, o namespace interno se torna membro o namespace externo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="4e48b-126">Em ambos os casos, o nome de um namespace deve ser exclusivo dentro do namespace que contém.</span><span class="sxs-lookup"><span data-stu-id="4e48b-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="4e48b-127">Namespaces são implicitamente `public` e a declaração de um namespace não pode incluir qualquer modificador de acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="4e48b-128">Dentro de um *namespace_body*, opcional *using_directive*s importar os nomes de outros namespaces, tipos e membros, permitindo que elas sejam referenciadas diretamente em vez de por meio de nomes qualificados.</span><span class="sxs-lookup"><span data-stu-id="4e48b-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="4e48b-129">Opcional *namespace_member_declaration*s contribuir com os membros do espaço de declaração do namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="4e48b-130">Observe que todos os *using_directive*s deve aparecer antes de qualquer declaração de membro.</span><span class="sxs-lookup"><span data-stu-id="4e48b-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="4e48b-131">O *qualified_identifier* de uma *namespace_declaration* pode ser um identificador único ou uma sequência de identificadores separados por "`.`" tokens.</span><span class="sxs-lookup"><span data-stu-id="4e48b-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="4e48b-132">A segunda forma permite que um programa para definir um namespace aninhado sem aninhamento lexicalmente várias declarações de namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="4e48b-133">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="4e48b-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="4e48b-134">é semanticamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="4e48b-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="4e48b-135">Namespaces são abertos e contribuir com duas declarações de namespace com o mesmo nome totalmente qualificado para o mesmo espaço de declaração ([declarações](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="4e48b-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="4e48b-136">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="4e48b-137">As duas declarações de namespace acima contribuem para o mesmo espaço de declaração, nesse caso, declarando duas classes com nomes totalmente qualificados `N1.N2.A` e `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="4e48b-138">Porque as duas declarações a contribuir com o mesmo espaço de declaração, teria sido um erro se cada continha uma declaração de um membro com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="4e48b-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="4e48b-139">Aliases extern</span><span class="sxs-lookup"><span data-stu-id="4e48b-139">Extern aliases</span></span>

<span data-ttu-id="4e48b-140">Uma *extern_alias_directive* apresenta um identificador que serve como um alias para um namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="4e48b-141">A especificação do namespace com alias é externa ao código-fonte do programa e também se aplica a namespaces aninhados do namespace com alias.</span><span class="sxs-lookup"><span data-stu-id="4e48b-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="4e48b-142">O escopo de um *extern_alias_directive* se estende pela *using_directive*s, *global_attributes* e *namespace_member_declaration*s de seu corpo de namespace ou unidade de compilação imediatamente contido.</span><span class="sxs-lookup"><span data-stu-id="4e48b-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="4e48b-143">Dentro de um corpo de namespace ou unidade de compilação que contém um *extern_alias_directive*, o identificador introduzido pelo *extern_alias_directive* pode ser usado para referenciar o namespace com alias.</span><span class="sxs-lookup"><span data-stu-id="4e48b-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="4e48b-144">É um erro de tempo de compilação para o *identificador* para ser a palavra `global`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="4e48b-145">Uma *extern_alias_directive* disponibiliza um alias dentro de um corpo de namespace ou unidade de compilação específica, mas não contribui com os novos membros para o espaço de declaração subjacente.</span><span class="sxs-lookup"><span data-stu-id="4e48b-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="4e48b-146">Em outras palavras, uma *extern_alias_directive* não é transitiva, mas, em vez disso, afeta apenas o compilação unidade namespace corpo ou nas quais ele ocorre.</span><span class="sxs-lookup"><span data-stu-id="4e48b-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="4e48b-147">O programa a seguir declara e usa dois alias extern, `X` e `Y`, cada do que representam a raiz de uma hierarquia de namespace distinto:</span><span class="sxs-lookup"><span data-stu-id="4e48b-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="4e48b-148">O programa declara a existência da extern aliases `X` e `Y`, mas as reais definições de aliases são externas ao programa.</span><span class="sxs-lookup"><span data-stu-id="4e48b-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="4e48b-149">O nome idêntico `N.B` classes agora podem ser referenciadas como `X.N.B` e `Y.N.B`, ou, usando o qualificador alias de namespace `X::N.B` e `Y::N.B`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="4e48b-150">Um erro ocorrerá se um programa declara um alias externo para o qual é fornecida nenhuma definição de externa.</span><span class="sxs-lookup"><span data-stu-id="4e48b-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="4e48b-151">usando diretivas</span><span class="sxs-lookup"><span data-stu-id="4e48b-151">Using directives</span></span>

<span data-ttu-id="4e48b-152">***Usando diretivas*** facilitar o uso de namespaces e tipos definidos em outros namespaces.</span><span class="sxs-lookup"><span data-stu-id="4e48b-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="4e48b-153">Usando o processo de resolução de nome do impacto de diretivas *namespace_or_type_name*s ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) e *simple_name*s ([nomes simples ](expressions.md#simple-names)), mas, ao contrário de declarações, diretivas de uso não contribuem novos membros para os espaços de declaração subjacente do unidades de compilação ou namespaces dentro do qual eles são usados.</span><span class="sxs-lookup"><span data-stu-id="4e48b-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="4e48b-154">Um *using_alias_directive* ([diretivas de alias Using](namespaces.md#using-alias-directives)) apresenta um alias para um namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="4e48b-155">Um *using_namespace_directive* ([usando diretivas de namespace](namespaces.md#using-namespace-directives)) importa os membros de tipo de um namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="4e48b-156">Um *using_static_directive* ([diretivas estáticas de uso](namespaces.md#using-static-directives)) importa os tipos aninhados e membros estáticos de um tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="4e48b-157">O escopo de um *using_directive* se estende pela *namespace_member_declaration*s de seu corpo de namespace ou unidade de compilação imediatamente contido.</span><span class="sxs-lookup"><span data-stu-id="4e48b-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="4e48b-158">O escopo de um *using_directive* especificamente não inclui seu par *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="4e48b-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="4e48b-159">Assim, emparelhar *using_directive*s não afetam uns aos outros e a ordem na qual elas são gravadas é insignificante.</span><span class="sxs-lookup"><span data-stu-id="4e48b-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="4e48b-160">Usando diretivas de alias</span><span class="sxs-lookup"><span data-stu-id="4e48b-160">Using alias directives</span></span>

<span data-ttu-id="4e48b-161">Um *using_alias_directive* apresenta um identificador que serve como um alias para um namespace ou tipo dentro do corpo de namespace ou unidade de compilação imediatamente delimitador.</span><span class="sxs-lookup"><span data-stu-id="4e48b-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="4e48b-162">Dentro de declarações de membro em um corpo de namespace ou unidade de compilação que contém um *using_alias_directive*, o identificador introduzido pelo *using_alias_directive* pode ser usado para referência a determinado o namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="4e48b-163">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="4e48b-164">Acima, dentro de declarações de membro na `N3` namespace, `A` é um alias para `N1.N2.A`e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="4e48b-165">O mesmo efeito pode ser obtido pela criação de um alias `R` para `N1.N2` e, em seguida, referenciar `R.A`:</span><span class="sxs-lookup"><span data-stu-id="4e48b-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="4e48b-166">O *identificador* de uma *using_alias_directive* deve ser exclusivo dentro do espaço de declaração da unidade de compilação ou namespace que contém imediatamente o *using_alias_directive* .</span><span class="sxs-lookup"><span data-stu-id="4e48b-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="4e48b-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="4e48b-168">Acima, `N3` já contém um membro `A`, portanto, é um erro de tempo de compilação para um *using_alias_directive* para usar esse identificador.</span><span class="sxs-lookup"><span data-stu-id="4e48b-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="4e48b-169">Da mesma forma, ele é um erro de tempo de compilação para dois ou mais *using_alias_directive*s na mesma compilação unidade ou namespace corpo declarar aliases com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="4e48b-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="4e48b-170">Um *using_alias_directive* disponibiliza um alias dentro de um corpo de namespace ou unidade de compilação específica, mas não contribui com os novos membros para o espaço de declaração subjacente.</span><span class="sxs-lookup"><span data-stu-id="4e48b-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="4e48b-171">Em outras palavras, uma *using_alias_directive* não é transitiva, mas em vez disso, afeta apenas o compilação unidade namespace corpo ou nas quais ele ocorre.</span><span class="sxs-lookup"><span data-stu-id="4e48b-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="4e48b-172">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="4e48b-173">o escopo do *using_alias_directive* que apresenta `R` só se estende às declarações de membro no corpo do namespace no qual ele está contido, portanto, `R` é desconhecido na segunda declaração de namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="4e48b-174">No entanto, colocar o *using_alias_directive* na compilação do recipiente unidade faz com que o alias para se tornar disponível em ambas as declarações de namespace:</span><span class="sxs-lookup"><span data-stu-id="4e48b-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="4e48b-175">Assim como os membros regulares, os nomes introduzidos por *using_alias_directive*s ficam ocultos por membros nomeados da mesma forma em escopos aninhados.</span><span class="sxs-lookup"><span data-stu-id="4e48b-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="4e48b-176">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="4e48b-177">a referência ao `R.A` na declaração de `B` causa um erro de tempo de compilação porque `R` refere-se ao `N3.R`, e não `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="4e48b-178">A ordem na qual *using_alias_directive*s são gravados não tem nenhum significado e a resolução da *namespace_or_type_name* referenciado por um *using_alias_directive*não é afetado pela *using_alias_directive* em si ou por outras *using_directive*s em que o corpo de namespace ou unidade de compilação imediatamente contido.</span><span class="sxs-lookup"><span data-stu-id="4e48b-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="4e48b-179">Em outras palavras, o *namespace_or_type_name* de uma *using_alias_directive* for resolvida como se o corpo de namespace ou unidade de compilação imediatamente contido tivesse não *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="4e48b-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="4e48b-180">Um *using_alias_directive* Entretanto pode ser afetado pela *extern_alias_directive*s em que o corpo de namespace ou unidade de compilação imediatamente contido.</span><span class="sxs-lookup"><span data-stu-id="4e48b-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="4e48b-181">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="4e48b-182">a última *using_alias_directive* resulta em um erro de tempo de compilação porque ela não é afetada pela primeira *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="4e48b-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="4e48b-183">A primeira *using_alias_directive* não resulta em um erro desde o escopo do alias extern `E` inclui as *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="4e48b-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="4e48b-184">Um *using_alias_directive* pode criar um alias para qualquer namespace ou tipo, incluindo o namespace no qual ele é exibido e qualquer namespace ou tipo aninhado dentro desse namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="4e48b-185">Acessando um namespace ou tipo por meio de um alias produz exatamente o mesmo resultado e o acesso a esse namespace ou tipo por meio de seu nome declarado.</span><span class="sxs-lookup"><span data-stu-id="4e48b-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="4e48b-186">Por exemplo, dada</span><span class="sxs-lookup"><span data-stu-id="4e48b-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="4e48b-187">os nomes `N1.N2.A`, `R1.N2.A`, e `R2.A` são equivalentes e todos se referir à classe cujo nome totalmente qualificado é `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="4e48b-188">Usando aliases pode nome de um tipo construído fechado, mas não o nome de uma declaração de tipo genérico não associado sem fornecer argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="4e48b-189">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="4e48b-190">Usando diretivas de namespace</span><span class="sxs-lookup"><span data-stu-id="4e48b-190">Using namespace directives</span></span>

<span data-ttu-id="4e48b-191">Um *using_namespace_directive* importa os tipos contidos em um namespace para o fechamento imediato compilação unidade ou namespace corpo, permitindo que o identificador de cada tipo a ser usado sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="4e48b-192">Dentro de declarações de membro em um corpo de namespace ou unidade de compilação que contém um *using_namespace_directive*, os tipos contidos no namespace específico podem ser referenciados diretamente.</span><span class="sxs-lookup"><span data-stu-id="4e48b-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="4e48b-193">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="4e48b-194">Acima, dentro de declarações de membro na `N3` namespace, os membros do tipo `N1.N2` estão diretamente disponíveis e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="4e48b-195">Um *using_namespace_directive* importa os tipos contidos no namespace específico, mas especificamente não importa namespaces aninhados.</span><span class="sxs-lookup"><span data-stu-id="4e48b-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="4e48b-196">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="4e48b-197">o *using_namespace_directive* importa os tipos contidos em `N1`, mas não os namespaces aninhados em `N1`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="4e48b-198">Portanto, a referência ao `N2.A` na declaração de `B` resulta em um erro de tempo de compilação porque nenhum membro denominado `N2` estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="4e48b-199">Ao contrário de um *using_alias_directive*, um *using_namespace_directive* pode importar os tipos cujos identificadores já estão definidos dentro do corpo de namespace ou unidade de compilação delimitador.</span><span class="sxs-lookup"><span data-stu-id="4e48b-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="4e48b-200">Na verdade, os nomes são importados por um *using_namespace_directive* ficam ocultos por membros nomeados da mesma forma no corpo de namespace ou unidade de compilação delimitador.</span><span class="sxs-lookup"><span data-stu-id="4e48b-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="4e48b-201">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="4e48b-202">Aqui, dentro de declarações de membro na `N3` namespace, `A` refere-se ao `N3.A` em vez de `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="4e48b-203">Quando mais de um namespace ou tipo importado pelo *using_namespace_directive*s ou *using_static_directive*s em que o corpo de namespace ou unidade de compilação mesmo conter tipos com o mesmo nome, as referências a Esse nome como um *type_name* são considerados ambíguos.</span><span class="sxs-lookup"><span data-stu-id="4e48b-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="4e48b-204">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="4e48b-205">ambos `N1` e `N2` conter um membro `A`e porque `N3` importa ambos, referenciando `A` em `N3` é um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="4e48b-206">Nessa situação, o conflito pode ser resolvido por meio da qualificação de referências a `A`, ou com a introdução de um *using_alias_directive* que seleciona um determinado `A`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="4e48b-207">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="4e48b-208">Além disso, quando mais de um namespace ou tipo importado pelo *using_namespace_directive*s ou *using_static_directive*s em que o corpo de namespace ou unidade de compilação mesmo conter tipos ou membros pela mesmo nome, as referências a esse nome como um *simple_name* são considerados ambíguos.</span><span class="sxs-lookup"><span data-stu-id="4e48b-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="4e48b-209">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="4e48b-210">`N1` contém um membro de tipo `A`, e `C` contém um campo estático `A`e porque `N2` importa ambos, referenciando `A` como uma *simple_name* é ambígua e um tempo de compilação erro.</span><span class="sxs-lookup"><span data-stu-id="4e48b-210">`N1` contains a type member `A`, and `C` contains a static field `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="4e48b-211">Como uma *using_alias_directive*, um *using_namespace_directive* não contribui com os novos membros para o espaço de declaração subjacente da unidade de compilação ou namespace, mas em vez disso, só afeta o compilação de unidade ou namespace corpo na qual ele aparece.</span><span class="sxs-lookup"><span data-stu-id="4e48b-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="4e48b-212">O *namespace_name* referenciado por uma *using_namespace_directive* for resolvida da mesma maneira como o *namespace_or_type_name* referenciado por um  *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="4e48b-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="4e48b-213">Portanto, *using_namespace_directive*s em que o mesmo corpo de namespace ou unidade de compilação não afetam uns aos outros e podem ser escritos em qualquer ordem.</span><span class="sxs-lookup"><span data-stu-id="4e48b-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="4e48b-214">Diretivas estáticas de uso</span><span class="sxs-lookup"><span data-stu-id="4e48b-214">Using static directives</span></span>

<span data-ttu-id="4e48b-215">Um *using_static_directive* importa os tipos aninhados e membros estáticos contidos diretamente em uma declaração de tipo para o fechamento imediato compilação unidade ou namespace corpo, permitindo que o identificador de cada membro e o tipo para ser usados sem qualificação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="4e48b-216">Dentro de declarações de membro em um corpo de namespace ou unidade de compilação que contém um *using_static_directive*, aninhado a acessível, tipos e membros estáticos (exceto os métodos de extensão) contidos diretamente na declaração da tipo fornecido pode ser referenciado diretamente.</span><span class="sxs-lookup"><span data-stu-id="4e48b-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="4e48b-217">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e48b-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="4e48b-218">Acima, dentro de declarações de membro na `N2` namespace, os membros estáticos e tipos aninhados de `N1.A` estão diretamente disponíveis e, portanto, o método `N` é capaz de fazer referência a ambos os `B` e `M` membros `N1.A`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="4e48b-219">Um *using_static_directive* especificamente não importa os métodos de extensão diretamente como métodos estáticos, mas torna-os disponíveis para invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="4e48b-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="4e48b-220">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="4e48b-221">o *using_static_directive* importa o método de extensão `M` contidos no `N1.A`, mas somente como um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="4e48b-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="4e48b-222">Portanto, a primeira referência às `M` no corpo da `B.N` resulta em um erro de tempo de compilação porque nenhum membro denominado `M` estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="4e48b-223">Um *using_static_directive* importa somente os membros e tipos declarados diretamente no tipo especificado, não os tipos e membros declarados em classes base.</span><span class="sxs-lookup"><span data-stu-id="4e48b-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="4e48b-224">TODO: Exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-224">TODO: Example</span></span>

<span data-ttu-id="4e48b-225">Ambiguidades entre vários *using_namespace_directives* e *using_static_directives* são discutidos na [usando diretivas de namespace](namespaces.md#using-namespace-directives).</span><span class="sxs-lookup"><span data-stu-id="4e48b-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="4e48b-226">Membros do Namespace</span><span class="sxs-lookup"><span data-stu-id="4e48b-226">Namespace members</span></span>

<span data-ttu-id="4e48b-227">Um *namespace_member_declaration* é um *namespace_declaration* ([declarações de Namespace](namespaces.md#namespace-declarations)) ou um *type_declaration* ( [As declarações de tipo](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="4e48b-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="4e48b-228">Uma unidade de compilação ou um corpo de namespace pode conter *namespace_member_declaration*s e essas declarações contribuir com novos membros para o espaço de declaração subjacente do corpo que contém compilação unidade ou namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="4e48b-229">Declarações de tipo</span><span class="sxs-lookup"><span data-stu-id="4e48b-229">Type declarations</span></span>

<span data-ttu-id="4e48b-230">Um *type_declaration* é um *class_declaration* ([declarações de classe](classes.md#class-declarations)), um *struct_declaration* ([Struct declarações](structs.md#struct-declarations)), uma *interface_declaration* ([declarações de Interface](interfaces.md#interface-declarations)), uma *enum_declaration* ([Enum declarações](enums.md#enum-declarations)), ou um *delegate_declaration* ([declarações de delegado](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="4e48b-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="4e48b-231">Um *type_declaration* pode ocorrer como uma declaração de nível superior em uma unidade de compilação ou como uma declaração de membro dentro de um namespace, classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="4e48b-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="4e48b-232">Quando uma declaração de tipo para um tipo `T` ocorre como uma declaração de nível superior em uma unidade de compilação, o nome totalmente qualificado do tipo declarado recentemente é simplesmente `T`.</span><span class="sxs-lookup"><span data-stu-id="4e48b-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="4e48b-233">Quando uma declaração de tipo para um tipo `T` ocorre dentro de um namespace, classe ou struct, o nome totalmente qualificado do tipo declarado recentemente é `N.T`, onde `N` é o nome totalmente qualificado do namespace, classe ou struct recipiente.</span><span class="sxs-lookup"><span data-stu-id="4e48b-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="4e48b-234">Um tipo declarado dentro de uma classe ou struct é chamado um tipo aninhado ([tipos aninhados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="4e48b-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="4e48b-235">Os modificadores de acesso permitido e o acesso padrão para uma declaração de tipo dependem do contexto no qual a declaração ocorre ([declarado acessibilidade](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="4e48b-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="4e48b-236">Tipos declarados em unidades de compilação ou namespaces podem ter `public` ou `internal` acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="4e48b-237">O padrão é `internal` acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="4e48b-238">Tipos declarados em classes podem ter `public`, `protected internal`, `protected`, `internal`, ou `private` acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="4e48b-239">O padrão é `private` acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-239">The default is `private` access.</span></span>
*  <span data-ttu-id="4e48b-240">Tipos declarados em estruturas podem ter `public`, `internal`, ou `private` acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="4e48b-241">O padrão é `private` acesso.</span><span class="sxs-lookup"><span data-stu-id="4e48b-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="4e48b-242">Namespace alias qualifiers</span><span class="sxs-lookup"><span data-stu-id="4e48b-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="4e48b-243">O ***qualificador alias de namespace*** `::` torna possível garantir que as pesquisas de nome de tipo não são afetadas pela introdução de novos tipos e membros.</span><span class="sxs-lookup"><span data-stu-id="4e48b-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="4e48b-244">O qualificador alias de namespace sempre aparece entre dois identificadores, conhecidos como os identificadores do lado esquerdos e direito.</span><span class="sxs-lookup"><span data-stu-id="4e48b-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="4e48b-245">Ao contrário de normal `.` qualificador, o identificador à esquerda do `::` qualificador é pesquisado up apenas como um externo ou usando o alias.</span><span class="sxs-lookup"><span data-stu-id="4e48b-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="4e48b-246">Um *qualified_alias_member* é definido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4e48b-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="4e48b-247">Um *qualified_alias_member* pode ser usado como um *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) ou como o operando esquerdo em uma *member_access* ([Acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="4e48b-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="4e48b-248">Um *qualified_alias_member* tem uma das duas formas:</span><span class="sxs-lookup"><span data-stu-id="4e48b-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="4e48b-249">`N::I<A1, ..., Ak>`, onde `N` e `I` representam identificadores, e `<A1, ..., Ak>` é uma lista de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="4e48b-250">(`K` é sempre pelo menos um.)</span><span class="sxs-lookup"><span data-stu-id="4e48b-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="4e48b-251">`N::I`, onde `N` e `I` representam identificadores.</span><span class="sxs-lookup"><span data-stu-id="4e48b-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="4e48b-252">(Nesse caso, `K` é considerado como zero.)</span><span class="sxs-lookup"><span data-stu-id="4e48b-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="4e48b-253">Usando essa notação, o significado de um *qualified_alias_member* é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4e48b-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="4e48b-254">Se `N` é o identificador `global`, em seguida, o namespace global é pesquisado `I`:</span><span class="sxs-lookup"><span data-stu-id="4e48b-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="4e48b-255">Se o namespace global contém um namespace chamado `I` e `K` for zero, em seguida, a *qualified_alias_member* refere-se ao namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="4e48b-256">Caso contrário, se o namespace global contém um tipo não genérico chamado `I` e `K` for zero, em seguida, a *qualified_alias_member* refere-se a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="4e48b-257">Caso contrário, se o namespace global contém um tipo chamado `I` que tem `K`  parâmetros de tipo, o *qualified_alias_member* refere-se ao tipo construído com os argumentos de tipo em questão.</span><span class="sxs-lookup"><span data-stu-id="4e48b-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="4e48b-258">Caso contrário, o *qualified_alias_member* é indefinido e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="4e48b-259">Caso contrário, começando com a declaração de namespace ([declarações de Namespace](namespaces.md#namespace-declarations)) imediatamente que contém o *qualified_alias_member* (se houver), continuando com cada declaração de namespace delimitador (se houver) e terminando com a unidade de compilação que contém o *qualified_alias_member*, as etapas a seguir são avaliadas até que uma entidade está localizada:</span><span class="sxs-lookup"><span data-stu-id="4e48b-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="4e48b-260">Se a unidade de compilação ou de declaração de namespace contém uma *using_alias_directive* que associa `N` com um tipo, em seguida, o *qualified_alias_member* é indefinido e um tempo de compilação erro ocorre.</span><span class="sxs-lookup"><span data-stu-id="4e48b-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="4e48b-261">Caso contrário, se a unidade de compilação ou de declaração de namespace contém um *extern_alias_directive* ou *using_alias_directive* que associa `N` com um namespace, então:</span><span class="sxs-lookup"><span data-stu-id="4e48b-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="4e48b-262">Se o namespace associado `N` contém um namespace chamado `I` e `K` for zero, o *qualified_alias_member* refere-se ao namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="4e48b-263">Caso contrário, se o namespace associado `N` contém um tipo não genérico chamado `I` e `K` for zero, o *qualified_alias_member* refere-se a esse tipo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="4e48b-264">Caso contrário, se o namespace associado `N` contém um tipo denominado `I` que tem `K`  parâmetros de tipo, o *qualified_alias_member* refere-se ao que o tipo construído com os argumentos de tipo em questão.</span><span class="sxs-lookup"><span data-stu-id="4e48b-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="4e48b-265">Caso contrário, o *qualified_alias_member* é indefinido e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="4e48b-266">Caso contrário, o *qualified_alias_member* é indefinido e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="4e48b-267">Observe que usando o qualificador alias de namespace com um alias que faz referência a um tipo causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="4e48b-268">Observe também que, se o identificador `N` é `global`, em seguida, a pesquisa é executada no namespace global, mesmo se houver um alias associando usando `global` com um tipo ou namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="4e48b-269">Exclusividade de aliases</span><span class="sxs-lookup"><span data-stu-id="4e48b-269">Uniqueness of aliases</span></span>

<span data-ttu-id="4e48b-270">Cada corpo de unidade e o namespace de compilação tem um espaço de declaração separada para aliases extern e uso de aliases.</span><span class="sxs-lookup"><span data-stu-id="4e48b-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="4e48b-271">Assim, enquanto o nome de um alias externo ou usar o alias deve ser exclusivo dentro do conjunto de aliases extern e aliases using declaradas no corpo imediatamente contido compilação unidade ou namespace, um alias é permitido ter o mesmo nome que um tipo ou namespace, desde que eu t é usado apenas com o `::` qualificador.</span><span class="sxs-lookup"><span data-stu-id="4e48b-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="4e48b-272">No exemplo</span><span class="sxs-lookup"><span data-stu-id="4e48b-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="4e48b-273">o nome `A` tem dois significados possíveis no corpo de namespace da segunda porque os dois é a classe `A` e o uso de alias `A` estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="4e48b-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="4e48b-274">Por esse motivo, uso de `A` no nome qualificado `A.Stream` é ambíguo e faz com que ocorra um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="4e48b-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="4e48b-275">No entanto, usar `A` com o `::` qualificador não é um erro porque `A` é pesquisado apenas como um alias de namespace.</span><span class="sxs-lookup"><span data-stu-id="4e48b-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
