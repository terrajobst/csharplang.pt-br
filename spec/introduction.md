---
ms.openlocfilehash: ab41a3c99f79c4cc70f7d4720f7e53b91a410859
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "49640893"
---
# <a name="introduction"></a><span data-ttu-id="828a4-101">Introdução</span><span class="sxs-lookup"><span data-stu-id="828a4-101">Introduction</span></span>

<span data-ttu-id="828a4-102">O C# (pronuncia-se "C Sharp") é uma linguagem de programação simples, moderna, orientada a objeto e fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="828a4-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="828a4-103">C# tem suas raízes na família de idiomas do C e será imediatamente familiar aos programadores de C, C++ e Java.</span><span class="sxs-lookup"><span data-stu-id="828a4-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="828a4-104">C# é padronizado pelo ECMA International como o ***ECMA-334*** padrão e pela ISO/IEC como o ***ISO/IEC 23270*** padrão.</span><span class="sxs-lookup"><span data-stu-id="828a4-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="828a4-105">Compilador c# da Microsoft para o .NET Framework é uma implementação em conformidade de ambos os padrões.</span><span class="sxs-lookup"><span data-stu-id="828a4-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="828a4-106">O C# é uma linguagem orientada a objeto, mas inclui ainda suporte para programação ***orientada a componentes***.</span><span class="sxs-lookup"><span data-stu-id="828a4-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="828a4-107">O design de software atual depende cada vez mais dos componentes de software na forma de pacotes independentes e autodescritivos de funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="828a4-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="828a4-108">O principal é que esses componentes apresentam um modelo de programação com propriedades, métodos e eventos; eles têm atributos que fornecem informações declarativas sobre o componente; e incorporam sua própria documentação.</span><span class="sxs-lookup"><span data-stu-id="828a4-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="828a4-109">O c# fornece construções de linguagem para dar suporte diretamente a esses conceitos, tornando c# uma linguagem muito natural para criar e usar componentes de software.</span><span class="sxs-lookup"><span data-stu-id="828a4-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="828a4-110">Vários recursos do c# auxiliam na construção de aplicativos robustos e duráveis: ***Coleta de lixo*** recupera automaticamente a memória ocupada por objetos não utilizados; ***tratamento de exceção*** fornece uma abordagem estruturada e extensível para detecção de erros e recuperação; e o ***fortemente tipado*** design da linguagem impossibilita a leitura de variáveis não inicializadas, para indexar matrizes além dos seus limites ou realizar desmarcada conversões de tipos.</span><span class="sxs-lookup"><span data-stu-id="828a4-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="828a4-111">C# tem um ***sistema de tipo unificado***.</span><span class="sxs-lookup"><span data-stu-id="828a4-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="828a4-112">Todos os tipos do C#, incluindo tipos primitivos, como `int` e `double`, herdam de um único tipo de `object` raiz.</span><span class="sxs-lookup"><span data-stu-id="828a4-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="828a4-113">Assim, todos os tipos compartilham um conjunto de operações comuns, e valores de qualquer tipo podem ser armazenados, transportados e operados de maneira consistente.</span><span class="sxs-lookup"><span data-stu-id="828a4-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="828a4-114">Além disso, C# oferece suporte a tipos de referência e tipos de valor definidos pelo usuário, permitindo a alocação dinâmica de objetos, bem como o armazenamento em linha de estruturas leves.</span><span class="sxs-lookup"><span data-stu-id="828a4-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="828a4-115">Para garantir que as bibliotecas e programas em c# podem evoluir ao longo do tempo de uma maneira compatível, muita ênfase foi colocado no ***controle de versão*** no design do #.</span><span class="sxs-lookup"><span data-stu-id="828a4-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="828a4-116">Muitas linguagens de programação prestam pouca atenção a esse problema e, como resultado, programas escritos nessas linguagens quebram com mais frequência do que o necessário quando versões mais recentes das bibliotecas dependentes são introduzidas.</span><span class="sxs-lookup"><span data-stu-id="828a4-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="828a4-117">Aspectos do design do # que foram diretamente influenciados pelas considerações de controle de versão incluem separada `virtual` e `override` modificadores, as regras de resolução de sobrecarga de método e o suporte para declarações de membro de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="828a4-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="828a4-118">O restante deste capítulo descreve os recursos essenciais da linguagem c#.</span><span class="sxs-lookup"><span data-stu-id="828a4-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="828a4-119">Embora os capítulos descrevem regras e exceções de uma maneira orientada em detalhes e, às vezes, matemática, este capítulo se esforça para maior clareza e fins de brevidade às custas da integridade.</span><span class="sxs-lookup"><span data-stu-id="828a4-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="828a4-120">A intenção é fornecer ao leitor uma introdução à linguagem que irá facilitar a escrita dos programas antecipadas e a leitura de capítulos.</span><span class="sxs-lookup"><span data-stu-id="828a4-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="828a4-121">Hello world</span><span class="sxs-lookup"><span data-stu-id="828a4-121">Hello world</span></span>

<span data-ttu-id="828a4-122">O programa "Hello, World" é usado tradicionalmente para introduzir uma linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="828a4-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="828a4-123">Este é para C#:</span><span class="sxs-lookup"><span data-stu-id="828a4-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="828a4-124">Os arquivos de origem em C# normalmente têm a extensão de arquivo `.cs`.</span><span class="sxs-lookup"><span data-stu-id="828a4-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="828a4-125">Supondo que o programa "Hello, World" esteja armazenado no arquivo `hello.cs`, o programa pode ser compilado com o compilador Microsoft c# usando a linha de comando</span><span class="sxs-lookup"><span data-stu-id="828a4-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="828a4-126">que produz um assembly executável denominado `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="828a4-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="828a4-127">É a saída produzida por este aplicativo quando ele é executado</span><span class="sxs-lookup"><span data-stu-id="828a4-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="828a4-128">O programa "Hello, World" começa com uma diretiva `using` que faz referência ao namespace `System`.</span><span class="sxs-lookup"><span data-stu-id="828a4-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="828a4-129">Namespaces fornecem um meio hierárquico de organizar bibliotecas e programas em C#.</span><span class="sxs-lookup"><span data-stu-id="828a4-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="828a4-130">Os namespaces contêm tipos e outros namespaces — por exemplo, o namespace `System` contém uma quantidade de tipos, como a classe `Console` referenciada no programa e diversos outros namespaces, como `IO` e `Collections`.</span><span class="sxs-lookup"><span data-stu-id="828a4-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="828a4-131">A diretiva `using` que faz referência a um determinado namespace permite o uso não qualificado dos tipos que são membros desse namespace.</span><span class="sxs-lookup"><span data-stu-id="828a4-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="828a4-132">Devido à diretiva `using`, o programa pode usar `Console.WriteLine` como um atalho para `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="828a4-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="828a4-133">A classe `Hello` declarada pelo programa "Hello, World" tem um único membro, o método chamado `Main`.</span><span class="sxs-lookup"><span data-stu-id="828a4-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="828a4-134">O `Main` método é declarado com o `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="828a4-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="828a4-135">Embora os métodos de instância possam fazer referência a uma determinada instância de objeto delimitador usando a palavra-chave `this`, métodos estáticos operam sem referência a um objeto específico.</span><span class="sxs-lookup"><span data-stu-id="828a4-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="828a4-136">Por convenção, um método estático denominado `Main` serve como ponto de entrada de um programa.</span><span class="sxs-lookup"><span data-stu-id="828a4-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="828a4-137">A saída do programa é produzida pelo método `WriteLine` da classe `Console` no namespace `System`.</span><span class="sxs-lookup"><span data-stu-id="828a4-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="828a4-138">Essa classe é fornecida pelas bibliotecas de classe do .NET Framework, que, por padrão, são referenciadas automaticamente pelo compilador Microsoft c#.</span><span class="sxs-lookup"><span data-stu-id="828a4-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="828a4-139">Observe que c# em si não tem uma biblioteca de tempo de execução separado.</span><span class="sxs-lookup"><span data-stu-id="828a4-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="828a4-140">Em vez disso, o .NET Framework é a biblioteca de tempo de execução da linguagem c#.</span><span class="sxs-lookup"><span data-stu-id="828a4-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="828a4-141">Estrutura do programa</span><span class="sxs-lookup"><span data-stu-id="828a4-141">Program structure</span></span>

<span data-ttu-id="828a4-142">Os principais conceitos organizacionais em C# são ***programas***, ***namespaces***, ***tipos***, ***membros*** e ***assemblies***.</span><span class="sxs-lookup"><span data-stu-id="828a4-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="828a4-143">Os programas C# consistem em um ou mais arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="828a4-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="828a4-144">Os programas declaram tipos que contêm membros e podem ser organizados em namespaces.</span><span class="sxs-lookup"><span data-stu-id="828a4-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="828a4-145">Classes e interfaces são exemplos de tipos.</span><span class="sxs-lookup"><span data-stu-id="828a4-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="828a4-146">Campos, métodos, propriedades e eventos são exemplos de membros.</span><span class="sxs-lookup"><span data-stu-id="828a4-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="828a4-147">Quando os programas em C# são compilados, eles são empacotados fisicamente em assemblies.</span><span class="sxs-lookup"><span data-stu-id="828a4-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="828a4-148">Os assemblies normalmente têm a extensão de arquivo `.exe` ou `.dll`, dependendo se eles implementam ***aplicativos*** ou ***bibliotecas***.</span><span class="sxs-lookup"><span data-stu-id="828a4-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="828a4-149">O exemplo</span><span class="sxs-lookup"><span data-stu-id="828a4-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="828a4-150">declara uma classe chamada `Stack` em um namespace chamado `Acme.Collections`.</span><span class="sxs-lookup"><span data-stu-id="828a4-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="828a4-151">O nome totalmente qualificado dessa classe é `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="828a4-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="828a4-152">A classe contém vários membros: um campo chamado `top`, dois métodos chamados `Push` e `Pop` e uma classe aninhada chamada `Entry`.</span><span class="sxs-lookup"><span data-stu-id="828a4-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="828a4-153">A classe `Entry` ainda contém três membros: um campo chamado `next`, um campo chamado `data`e um construtor.</span><span class="sxs-lookup"><span data-stu-id="828a4-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="828a4-154">Supondo que o código-fonte do exemplo seja armazenado no arquivo `acme.cs`, a linha de comando</span><span class="sxs-lookup"><span data-stu-id="828a4-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="828a4-155">compila o exemplo como uma biblioteca (o código sem um ponto de entrada `Main`) e produz um assembly denominado `acme.dll`.</span><span class="sxs-lookup"><span data-stu-id="828a4-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="828a4-156">Assemblies contêm código executável na forma de ***Intermediate Language*** instruções de (IL) e informações simbólicas na forma de ***metadados***.</span><span class="sxs-lookup"><span data-stu-id="828a4-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="828a4-157">Antes de ser executado, o código de IL em um assembly é automaticamente convertido em código específico do processador pelo compilador JIT (Just-In-Time) do .NET Common Language Runtime.</span><span class="sxs-lookup"><span data-stu-id="828a4-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="828a4-158">Como um assembly é uma unidade autodescritiva da funcionalidade que contém o código e os metadados, não é necessário de diretivas `#include` e arquivos de cabeçalho no C#.</span><span class="sxs-lookup"><span data-stu-id="828a4-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="828a4-159">Os tipos públicos e os membros contidos em um assembly específico são disponibilizados em um programa C# simplesmente fazendo referência a esse assembly ao compilar o programa.</span><span class="sxs-lookup"><span data-stu-id="828a4-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="828a4-160">Por exemplo, esse programa usa a classe `Acme.Collections.Stack` do assembly `acme.dll`:</span><span class="sxs-lookup"><span data-stu-id="828a4-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="828a4-161">Se o programa é armazenado no arquivo `test.cs`, quando `test.cs` é compilado, o `acme.dll` assembly pode ser referenciado usando o compilador `/r` opção:</span><span class="sxs-lookup"><span data-stu-id="828a4-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="828a4-162">Isso cria um assembly executável denominado `test.exe`, que, quando executado, produz a saída:</span><span class="sxs-lookup"><span data-stu-id="828a4-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="828a4-163">O C# permite que o texto de origem de um programa seja armazenado em vários arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="828a4-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="828a4-164">Quando um programa em C# com vários arquivo é compilado, todos os arquivos de origem são processados juntos e os arquivos de origem podem referenciar livremente uns aos outros. Conceitualmente, é como se todos os arquivos de origem fossem concatenados em um arquivo grande antes de serem processados.</span><span class="sxs-lookup"><span data-stu-id="828a4-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="828a4-165">Declarações de encaminhamento nunca são necessárias em C#, porque, com poucas exceções, a ordem de declaração é insignificante.</span><span class="sxs-lookup"><span data-stu-id="828a4-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="828a4-166">O C# não limita um arquivo de origem para declarar somente um tipo público nem requer o nome do arquivo de origem para corresponder a um tipo declarado no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="828a4-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="828a4-167">Tipos e variáveis</span><span class="sxs-lookup"><span data-stu-id="828a4-167">Types and variables</span></span>

<span data-ttu-id="828a4-168">Há dois tipos em C#: ***tipos de referência*** e ***tipos de valor***.</span><span class="sxs-lookup"><span data-stu-id="828a4-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="828a4-169">As variáveis de tipos de valor contêm diretamente seus dados enquanto variáveis de tipos de referência armazenam referências a seus dados, o último sendo conhecido como objetos.</span><span class="sxs-lookup"><span data-stu-id="828a4-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="828a4-170">Com tipos de referência, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível que operações em uma variável afetem o objeto referenciado por outra variável.</span><span class="sxs-lookup"><span data-stu-id="828a4-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="828a4-171">Com tipos de valor, cada variável tem sua própria cópia dos dados e não é possível que operações em uma variável afetem a outra (exceto no caso de variáveis de parâmetros `ref` e `out`).</span><span class="sxs-lookup"><span data-stu-id="828a4-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="828a4-172">Tipos de valor do # são divididos em ***tipos simples***, ***tipos enum***, ***tipos struct***, e ***tipos anuláveis***e a referência do # tipos são divididos em ***tipos de classe***, ***tipos de interface***, ***tipos de matriz***, e ***tipos delegados***.</span><span class="sxs-lookup"><span data-stu-id="828a4-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="828a4-173">A tabela a seguir fornece uma visão geral do sistema de tipos do #.</span><span class="sxs-lookup"><span data-stu-id="828a4-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="828a4-174">__Categoria__</span><span class="sxs-lookup"><span data-stu-id="828a4-174">__Category__</span></span>    |                 | <span data-ttu-id="828a4-175">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="828a4-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="828a4-176">Tipos de valor</span><span class="sxs-lookup"><span data-stu-id="828a4-176">Value types</span></span>     | <span data-ttu-id="828a4-177">Tipos simples</span><span class="sxs-lookup"><span data-stu-id="828a4-177">Simple types</span></span>    | <span data-ttu-id="828a4-178">Integral com sinal: `sbyte`, `short`, `int`,`long`</span><span class="sxs-lookup"><span data-stu-id="828a4-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="828a4-179">Integral sem sinal: `byte`, `ushort`, `uint`,`ulong`</span><span class="sxs-lookup"><span data-stu-id="828a4-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="828a4-180">Caracteres Unicode: `char`</span><span class="sxs-lookup"><span data-stu-id="828a4-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="828a4-181">Ponto flutuante IEEE: `float`, `double`</span><span class="sxs-lookup"><span data-stu-id="828a4-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="828a4-182">Decimal de alta precisão:`decimal`</span><span class="sxs-lookup"><span data-stu-id="828a4-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="828a4-183">Booliano: `bool`</span><span class="sxs-lookup"><span data-stu-id="828a4-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="828a4-184">Tipos enum</span><span class="sxs-lookup"><span data-stu-id="828a4-184">Enum types</span></span>      | <span data-ttu-id="828a4-185">Tipos definidos pelo usuário do formulário `enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="828a4-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="828a4-186">Tipos struct</span><span class="sxs-lookup"><span data-stu-id="828a4-186">Struct types</span></span>    | <span data-ttu-id="828a4-187">Tipos definidos pelo usuário do formulário `struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="828a4-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="828a4-188">Tipos que permitem valor nulo</span><span class="sxs-lookup"><span data-stu-id="828a4-188">Nullable types</span></span>  | <span data-ttu-id="828a4-189">Extensões de todos os outros tipos de valor com um valor `null`</span><span class="sxs-lookup"><span data-stu-id="828a4-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="828a4-190">Tipos de referência</span><span class="sxs-lookup"><span data-stu-id="828a4-190">Reference types</span></span> | <span data-ttu-id="828a4-191">Tipos de classe</span><span class="sxs-lookup"><span data-stu-id="828a4-191">Class types</span></span>     | <span data-ttu-id="828a4-192">Classe base definitiva de todos os outros tipos: `object`</span><span class="sxs-lookup"><span data-stu-id="828a4-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="828a4-193">Cadeia de caracteres Unicode: `string`</span><span class="sxs-lookup"><span data-stu-id="828a4-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="828a4-194">Tipos definidos pelo usuário do formulário `class C {...}`</span><span class="sxs-lookup"><span data-stu-id="828a4-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="828a4-195">Tipos de interface</span><span class="sxs-lookup"><span data-stu-id="828a4-195">Interface types</span></span> | <span data-ttu-id="828a4-196">Tipos definidos pelo usuário do formulário `interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="828a4-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="828a4-197">Tipos de matriz</span><span class="sxs-lookup"><span data-stu-id="828a4-197">Array types</span></span>     | <span data-ttu-id="828a4-198">Unidimensional e multidimensional, por exemplo, `int[]` e `int[,]`</span><span class="sxs-lookup"><span data-stu-id="828a4-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="828a4-199">Tipos delegados</span><span class="sxs-lookup"><span data-stu-id="828a4-199">Delegate types</span></span>  | <span data-ttu-id="828a4-200">Tipos definidos pelo usuário do formulário, por exemplo, `delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="828a4-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="828a4-201">Os tipos integrais oito dão suporte a valores de 8 bits, 16 bits, 32 bits e 64 bits no formulário com ou sem sinal.</span><span class="sxs-lookup"><span data-stu-id="828a4-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="828a4-202">De tipos, os dois flutuante ponto `float` e `double`, são representados usando os formatos 32 bits de precisão simples e 64 bits de precisão dupla IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="828a4-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="828a4-203">O tipo `decimal` é um tipo de dados de 128 bits adequado para cálculos financeiros e monetários.</span><span class="sxs-lookup"><span data-stu-id="828a4-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="828a4-204">Do # `bool` tipo é usado para representar valores boolianos — valores que são `true` ou `false`.</span><span class="sxs-lookup"><span data-stu-id="828a4-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="828a4-205">O processamento de cadeia de caracteres e caracteres em C# usa codificação Unicode.</span><span class="sxs-lookup"><span data-stu-id="828a4-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="828a4-206">O tipo `char` representa uma unidade de código UTF-16 e o tipo `string` representa uma sequência de unidades de código UTF-16.</span><span class="sxs-lookup"><span data-stu-id="828a4-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="828a4-207">A tabela a seguir resume os tipos numéricos do #.</span><span class="sxs-lookup"><span data-stu-id="828a4-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="828a4-208">__Categoria__</span><span class="sxs-lookup"><span data-stu-id="828a4-208">__Category__</span></span>      | <span data-ttu-id="828a4-209">__Bits__</span><span class="sxs-lookup"><span data-stu-id="828a4-209">__Bits__</span></span> | <span data-ttu-id="828a4-210">__Tipo__</span><span class="sxs-lookup"><span data-stu-id="828a4-210">__Type__</span></span>  | <span data-ttu-id="828a4-211">__Intervalo de precisão__</span><span class="sxs-lookup"><span data-stu-id="828a4-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="828a4-212">Integral assinado</span><span class="sxs-lookup"><span data-stu-id="828a4-212">Signed integral</span></span>   | <span data-ttu-id="828a4-213">8</span><span class="sxs-lookup"><span data-stu-id="828a4-213">8</span></span>        | `sbyte`   | <span data-ttu-id="828a4-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="828a4-214">-128...127</span></span> |
|                   | <span data-ttu-id="828a4-215">16</span><span class="sxs-lookup"><span data-stu-id="828a4-215">16</span></span>       | `short`   | <span data-ttu-id="828a4-216">-32,768...32,767</span><span class="sxs-lookup"><span data-stu-id="828a4-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="828a4-217">32</span><span class="sxs-lookup"><span data-stu-id="828a4-217">32</span></span>       | `int`     | <span data-ttu-id="828a4-218">-2,147,483,648...2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="828a4-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="828a4-219">64</span><span class="sxs-lookup"><span data-stu-id="828a4-219">64</span></span>       | `long`    | <span data-ttu-id="828a4-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="828a4-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="828a4-221">Integral sem sinal</span><span class="sxs-lookup"><span data-stu-id="828a4-221">Unsigned integral</span></span> | <span data-ttu-id="828a4-222">8</span><span class="sxs-lookup"><span data-stu-id="828a4-222">8</span></span>        | `byte`    | <span data-ttu-id="828a4-223">0...255</span><span class="sxs-lookup"><span data-stu-id="828a4-223">0...255</span></span> |
|                   | <span data-ttu-id="828a4-224">16</span><span class="sxs-lookup"><span data-stu-id="828a4-224">16</span></span>       | `ushort`  | <span data-ttu-id="828a4-225">0...65,535</span><span class="sxs-lookup"><span data-stu-id="828a4-225">0...65,535</span></span> |
|                   | <span data-ttu-id="828a4-226">32</span><span class="sxs-lookup"><span data-stu-id="828a4-226">32</span></span>       | `uint`    | <span data-ttu-id="828a4-227">0...4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="828a4-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="828a4-228">64</span><span class="sxs-lookup"><span data-stu-id="828a4-228">64</span></span>       | `ulong`   | <span data-ttu-id="828a4-229">0...18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="828a4-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="828a4-230">Ponto flutuante</span><span class="sxs-lookup"><span data-stu-id="828a4-230">Floating point</span></span>    | <span data-ttu-id="828a4-231">32</span><span class="sxs-lookup"><span data-stu-id="828a4-231">32</span></span>       | `float`   | <span data-ttu-id="828a4-232">1,5 × 10 ^ − 45 a 3,4 × 10 ^ 38, precisão de 7 dígitos</span><span class="sxs-lookup"><span data-stu-id="828a4-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="828a4-233">64</span><span class="sxs-lookup"><span data-stu-id="828a4-233">64</span></span>       | `double`  | <span data-ttu-id="828a4-234">5,0 × 10 ^ −324 a 1,7 × 10 ^ 308, precisão de 15 dígitos</span><span class="sxs-lookup"><span data-stu-id="828a4-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="828a4-235">Decimal</span><span class="sxs-lookup"><span data-stu-id="828a4-235">Decimal</span></span>           | <span data-ttu-id="828a4-236">128</span><span class="sxs-lookup"><span data-stu-id="828a4-236">128</span></span>      | `decimal` | <span data-ttu-id="828a4-237">1.0 × 10 ^ − 28 a 7,9 × 10 ^ 28, precisão de 28 dígitos</span><span class="sxs-lookup"><span data-stu-id="828a4-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="828a4-238">Os programas em C# usam ***declarações de tipos*** para criar novos tipos.</span><span class="sxs-lookup"><span data-stu-id="828a4-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="828a4-239">Uma declaração de tipo especifica o nome e os membros do novo tipo.</span><span class="sxs-lookup"><span data-stu-id="828a4-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="828a4-240">Cinco categorias do # de tipos são definidos pelo usuário: classe tipos, tipos struct, tipos de interface, tipos enum e tipos de delegado.</span><span class="sxs-lookup"><span data-stu-id="828a4-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="828a4-241">Um tipo de classe define uma estrutura de dados que contém membros de dados (campos) e os membros da função (métodos, propriedades e outros).</span><span class="sxs-lookup"><span data-stu-id="828a4-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="828a4-242">Os tipos de classe dão suporte à herança única e ao polimorfismo, mecanismos nos quais as classes derivadas podem estender e especializar as classes base.</span><span class="sxs-lookup"><span data-stu-id="828a4-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="828a4-243">Um tipo de estrutura é semelhante a um tipo de classe que representa uma estrutura com membros de dados e os membros da função.</span><span class="sxs-lookup"><span data-stu-id="828a4-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="828a4-244">No entanto, diferentemente das classes, structs são tipos de valor e não precisam de alocação de heap.</span><span class="sxs-lookup"><span data-stu-id="828a4-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="828a4-245">Os tipos de estrutura não dão suporte à herança especificada pelo usuário, e todos os tipos de structs são herdados implicitamente do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="828a4-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="828a4-246">Um tipo de interface define um contrato como um conjunto nomeado de membros da função pública.</span><span class="sxs-lookup"><span data-stu-id="828a4-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="828a4-247">Uma classe ou struct que implementa uma interface deve fornecer implementações da interface membros da função.</span><span class="sxs-lookup"><span data-stu-id="828a4-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="828a4-248">Uma interface pode herdar de várias interfaces base e uma classe ou struct pode implementar várias interfaces.</span><span class="sxs-lookup"><span data-stu-id="828a4-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="828a4-249">Um tipo de delegado representa referências aos métodos com uma lista de parâmetro específico e o tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="828a4-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="828a4-250">Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="828a4-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="828a4-251">Os delegados são parecidos com o conceito de ponteiros de função em outras linguagens, mas ao contrário dos ponteiros de função, os delegados são orientados a objetos e fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="828a4-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="828a4-252">Tipos dão suporte a genéricos, podem ser parametrizados com outros tipos de classe, struct, interface e delegado.</span><span class="sxs-lookup"><span data-stu-id="828a4-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="828a4-253">Um tipo de enumeração é um tipo distinto com constantes nomeadas.</span><span class="sxs-lookup"><span data-stu-id="828a4-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="828a4-254">Cada tipo de enumeração tem um tipo subjacente, que deve ser um dos oito tipos integrais.</span><span class="sxs-lookup"><span data-stu-id="828a4-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="828a4-255">O conjunto de valores de um tipo de enumeração é o mesmo que o conjunto de valores do tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="828a4-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="828a4-256">O C# dá suporte a matrizes uni e multidimensionais de qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="828a4-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="828a4-257">Ao contrário dos tipos listados acima, os tipos de matriz não precisam ser declarados antes de serem usados.</span><span class="sxs-lookup"><span data-stu-id="828a4-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="828a4-258">Em vez disso, os tipos de matriz são construídos seguindo um nome de tipo entre colchetes.</span><span class="sxs-lookup"><span data-stu-id="828a4-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="828a4-259">Por exemplo, `int[]` é uma matriz unidimensional de `int`, `int[,]` é uma matriz bidimensional de `int`, e `int[][]` é uma matriz unidimensional de matrizes unidimensionais de `int`.</span><span class="sxs-lookup"><span data-stu-id="828a4-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="828a4-260">Tipos anuláveis também não precisa ser declarada antes que possam ser usados.</span><span class="sxs-lookup"><span data-stu-id="828a4-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="828a4-261">Para cada tipo de valor não anulável `T` há um tipo que permite valor nulo correspondente `T?`, que pode conter um valor adicional `null`.</span><span class="sxs-lookup"><span data-stu-id="828a4-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="828a4-262">Por exemplo, `int?` é um tipo que pode conter qualquer número inteiro de 32 bits ou o valor `null`.</span><span class="sxs-lookup"><span data-stu-id="828a4-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="828a4-263">O sistema de tipos do # é unificado, de modo que um valor de qualquer tipo pode ser tratado como um objeto.</span><span class="sxs-lookup"><span data-stu-id="828a4-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="828a4-264">Cada tipo no C#, direta ou indiretamente, deriva do tipo de classe `object`, e `object` é a classe base definitiva de todos os tipos.</span><span class="sxs-lookup"><span data-stu-id="828a4-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="828a4-265">Os valores de tipos de referência são tratados como objetos simplesmente exibindo os valores como tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="828a4-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="828a4-266">Valores de tipos de valor são tratados como objetos, executando ***conversão boxing*** e ***unboxing*** operações.</span><span class="sxs-lookup"><span data-stu-id="828a4-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="828a4-267">No exemplo a seguir, um valor `int` é convertido em `object` e volta novamente ao `int`.</span><span class="sxs-lookup"><span data-stu-id="828a4-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="828a4-268">Quando um valor de um tipo de valor é convertido para o tipo `object`, uma instância do objeto, também chamada de "caixa", é alocada para armazenar o valor e o valor é copiado na caixa.</span><span class="sxs-lookup"><span data-stu-id="828a4-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="828a4-269">Por outro lado, quando um `object` referência é convertida para um tipo de valor, é feita uma verificação que o objeto referenciado é uma caixa do tipo do valor correto e, se a verificação for bem-sucedida, o valor na caixa será copiado.</span><span class="sxs-lookup"><span data-stu-id="828a4-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="828a4-270">O sistema de tipo unificado do # significa que os tipos de valor podem se tornar objetos "sob"demanda.</span><span class="sxs-lookup"><span data-stu-id="828a4-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="828a4-271">Devido à unificação, as bibliotecas de finalidade geral que usam o tipo `object` podem ser usadas com os tipos de referência e os tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="828a4-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="828a4-272">Existem vários tipos de ***variáveis*** no C#, incluindo campos, elementos de matriz, variáveis locais e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="828a4-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="828a4-273">As variáveis representam os locais de armazenamento, e cada variável tem um tipo que determina quais valores podem ser armazenados na variável, conforme mostrado pela tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="828a4-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="828a4-274">__Tipo de variável__</span><span class="sxs-lookup"><span data-stu-id="828a4-274">__Type of Variable__</span></span>    | <span data-ttu-id="828a4-275">__Conteúdo possível__</span><span class="sxs-lookup"><span data-stu-id="828a4-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="828a4-276">Tipo de valor não nulo</span><span class="sxs-lookup"><span data-stu-id="828a4-276">Non-nullable value type</span></span> | <span data-ttu-id="828a4-277">Um valor de tipo exato</span><span class="sxs-lookup"><span data-stu-id="828a4-277">A value of that exact type</span></span> |
| <span data-ttu-id="828a4-278">Tipos de valor anulável</span><span class="sxs-lookup"><span data-stu-id="828a4-278">Nullable value type</span></span>     | <span data-ttu-id="828a4-279">Um valor nulo ou um valor de tipo exato</span><span class="sxs-lookup"><span data-stu-id="828a4-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="828a4-280">Uma referência nula, uma referência a um objeto de qualquer tipo de referência ou uma referência a um valor demarcado de qualquer tipo de valor</span><span class="sxs-lookup"><span data-stu-id="828a4-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="828a4-281">Tipo de classe</span><span class="sxs-lookup"><span data-stu-id="828a4-281">Class type</span></span>              | <span data-ttu-id="828a4-282">Uma referência nula, uma referência a uma instância desse tipo de classe ou uma referência a uma instância de uma classe derivada de tipo de classe</span><span class="sxs-lookup"><span data-stu-id="828a4-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="828a4-283">Tipo de interface</span><span class="sxs-lookup"><span data-stu-id="828a4-283">Interface type</span></span>          | <span data-ttu-id="828a4-284">Uma referência nula, uma referência a uma instância de um tipo de classe que implementa esse tipo de interface ou uma referência a um valor demarcado de um tipo de valor que implementa esse tipo de interface</span><span class="sxs-lookup"><span data-stu-id="828a4-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="828a4-285">Tipo de matriz</span><span class="sxs-lookup"><span data-stu-id="828a4-285">Array type</span></span>              | <span data-ttu-id="828a4-286">Uma referência nula, uma referência a uma instância desse tipo de matriz ou uma referência a uma instância de um tipo de matriz compatível</span><span class="sxs-lookup"><span data-stu-id="828a4-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="828a4-287">Tipo delegado</span><span class="sxs-lookup"><span data-stu-id="828a4-287">Delegate type</span></span>           | <span data-ttu-id="828a4-288">Uma referência nula ou uma referência a uma instância do tipo delegado</span><span class="sxs-lookup"><span data-stu-id="828a4-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="828a4-289">Expressões</span><span class="sxs-lookup"><span data-stu-id="828a4-289">Expressions</span></span>

<span data-ttu-id="828a4-290">***Expressões*** são construídas a partir de ***operandos*** e ***operadores***.</span><span class="sxs-lookup"><span data-stu-id="828a4-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="828a4-291">Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos.</span><span class="sxs-lookup"><span data-stu-id="828a4-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="828a4-292">Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`.</span><span class="sxs-lookup"><span data-stu-id="828a4-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="828a4-293">Exemplos de operandos incluem literais, campos, variáveis locais e expressões.</span><span class="sxs-lookup"><span data-stu-id="828a4-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="828a4-294">Quando uma expressão contiver vários operadores, a ***precedência*** dos operadores controla a ordem na qual os operadores individuais são avaliados.</span><span class="sxs-lookup"><span data-stu-id="828a4-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="828a4-295">Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador `*` tem precedência maior do que o operador `+`.</span><span class="sxs-lookup"><span data-stu-id="828a4-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="828a4-296">A maioria dos operadores pode ser ***sobrecarregada***.</span><span class="sxs-lookup"><span data-stu-id="828a4-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="828a4-297">A sobrecarga de operador permite que implementações de operador definidas pelo usuário sejam especificadas para operações em que um ou ambos os operandos são de um tipo struct ou de classe definida pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="828a4-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="828a4-298">A tabela a seguir resume os operadores do #, listando as categorias de operador em ordem de precedência da mais alta para a mais baixa.</span><span class="sxs-lookup"><span data-stu-id="828a4-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="828a4-299">Operadores na mesma categoria têm a mesma precedência.</span><span class="sxs-lookup"><span data-stu-id="828a4-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="828a4-300">__Categoria__</span><span class="sxs-lookup"><span data-stu-id="828a4-300">__Category__</span></span>                     | <span data-ttu-id="828a4-301">__Expressão__</span><span class="sxs-lookup"><span data-stu-id="828a4-301">__Expression__</span></span>    | <span data-ttu-id="828a4-302">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="828a4-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="828a4-303">Primária</span><span class="sxs-lookup"><span data-stu-id="828a4-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="828a4-304">Acesso de membros</span><span class="sxs-lookup"><span data-stu-id="828a4-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="828a4-305">Invocação de método e delegado</span><span class="sxs-lookup"><span data-stu-id="828a4-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="828a4-306">Acesso de matriz e indexador</span><span class="sxs-lookup"><span data-stu-id="828a4-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="828a4-307">Pós-incremento</span><span class="sxs-lookup"><span data-stu-id="828a4-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="828a4-308">Pós-decremento</span><span class="sxs-lookup"><span data-stu-id="828a4-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="828a4-309">Criação de objeto e delegado</span><span class="sxs-lookup"><span data-stu-id="828a4-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="828a4-310">Criação de objeto com inicializador</span><span class="sxs-lookup"><span data-stu-id="828a4-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="828a4-311">Inicializador de objeto anônimo</span><span class="sxs-lookup"><span data-stu-id="828a4-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="828a4-312">Criação de matriz</span><span class="sxs-lookup"><span data-stu-id="828a4-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="828a4-313">Obter `System.Type` de objeto para `T`</span><span class="sxs-lookup"><span data-stu-id="828a4-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="828a4-314">Avalia expressão no contexto selecionado</span><span class="sxs-lookup"><span data-stu-id="828a4-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="828a4-315">Avalia expressão no contexto desmarcado</span><span class="sxs-lookup"><span data-stu-id="828a4-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="828a4-316">Obter o valor padrão de tipo `T`</span><span class="sxs-lookup"><span data-stu-id="828a4-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="828a4-317">Função anônima (método anônimo)</span><span class="sxs-lookup"><span data-stu-id="828a4-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="828a4-318">Unário</span><span class="sxs-lookup"><span data-stu-id="828a4-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="828a4-319">Identidade</span><span class="sxs-lookup"><span data-stu-id="828a4-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="828a4-320">Negação</span><span class="sxs-lookup"><span data-stu-id="828a4-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="828a4-321">Negação lógica</span><span class="sxs-lookup"><span data-stu-id="828a4-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="828a4-322">Negação bit a bit</span><span class="sxs-lookup"><span data-stu-id="828a4-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="828a4-323">Pré-incremento</span><span class="sxs-lookup"><span data-stu-id="828a4-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="828a4-324">Pré-decremento</span><span class="sxs-lookup"><span data-stu-id="828a4-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="828a4-325">Converter explicitamente `x` digitar `T`</span><span class="sxs-lookup"><span data-stu-id="828a4-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="828a4-326">Aguardar de forma assíncrona `x` para concluir</span><span class="sxs-lookup"><span data-stu-id="828a4-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="828a4-327">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="828a4-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="828a4-328">Multiplicação</span><span class="sxs-lookup"><span data-stu-id="828a4-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="828a4-329">Divisão</span><span class="sxs-lookup"><span data-stu-id="828a4-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="828a4-330">Restante</span><span class="sxs-lookup"><span data-stu-id="828a4-330">Remainder</span></span> |
| <span data-ttu-id="828a4-331">Aditivo</span><span class="sxs-lookup"><span data-stu-id="828a4-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="828a4-332">Adição, concatenação de cadeia de caracteres, combinação de delegados</span><span class="sxs-lookup"><span data-stu-id="828a4-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="828a4-333">Subtração, remoção de delegado</span><span class="sxs-lookup"><span data-stu-id="828a4-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="828a4-334">Shift</span><span class="sxs-lookup"><span data-stu-id="828a4-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="828a4-335">Shift esquerdo</span><span class="sxs-lookup"><span data-stu-id="828a4-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="828a4-336">Shift direito</span><span class="sxs-lookup"><span data-stu-id="828a4-336">Shift right</span></span> |
| <span data-ttu-id="828a4-337">Teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="828a4-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="828a4-338">Menor que</span><span class="sxs-lookup"><span data-stu-id="828a4-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="828a4-339">Maior que</span><span class="sxs-lookup"><span data-stu-id="828a4-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="828a4-340">Menor que ou igual a</span><span class="sxs-lookup"><span data-stu-id="828a4-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="828a4-341">Maior que ou igual a</span><span class="sxs-lookup"><span data-stu-id="828a4-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="828a4-342">Retornar `true` se `x` é um `T`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="828a4-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="828a4-343">Retornar `x` tipada como `T`, ou `null` se `x` não é um `T`</span><span class="sxs-lookup"><span data-stu-id="828a4-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="828a4-344">Igualdade</span><span class="sxs-lookup"><span data-stu-id="828a4-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="828a4-345">Igual</span><span class="sxs-lookup"><span data-stu-id="828a4-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="828a4-346">Não é igual a</span><span class="sxs-lookup"><span data-stu-id="828a4-346">Not equal</span></span> |
| <span data-ttu-id="828a4-347">AND lógico</span><span class="sxs-lookup"><span data-stu-id="828a4-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="828a4-348">Inteiro bit a bit, AND lógico booliano</span><span class="sxs-lookup"><span data-stu-id="828a4-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="828a4-349">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="828a4-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="828a4-350">XOR bit a bit inteiro, XOR lógico booliano</span><span class="sxs-lookup"><span data-stu-id="828a4-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="828a4-351">OR lógico</span><span class="sxs-lookup"><span data-stu-id="828a4-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="828a4-352">OR bit a bit inteiro, OR lógico booliano</span><span class="sxs-lookup"><span data-stu-id="828a4-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="828a4-353">AND condicional</span><span class="sxs-lookup"><span data-stu-id="828a4-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="828a4-354">Avalia `y` somente se `x` é `true`</span><span class="sxs-lookup"><span data-stu-id="828a4-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="828a4-355">OR condicional</span><span class="sxs-lookup"><span data-stu-id="828a4-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="828a4-356">Avalia `y` somente se `x` é `false`</span><span class="sxs-lookup"><span data-stu-id="828a4-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="828a4-357">Coalescência nula</span><span class="sxs-lookup"><span data-stu-id="828a4-357">Null coalescing</span></span>                  | `X ?? y`          | <span data-ttu-id="828a4-358">É avaliada como `y` se `x` é `null`, para `x` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="828a4-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="828a4-359">Condicional</span><span class="sxs-lookup"><span data-stu-id="828a4-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="828a4-360">Avalia `y` se `x` é `true`, `z` se `x` é `false`</span><span class="sxs-lookup"><span data-stu-id="828a4-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="828a4-361">Atribuição ou função anônima</span><span class="sxs-lookup"><span data-stu-id="828a4-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="828a4-362">Atribuição</span><span class="sxs-lookup"><span data-stu-id="828a4-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="828a4-363">Atribuição composta; operadores com suporte são `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="828a4-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="828a4-364">Função anônima (expressão lambda)</span><span class="sxs-lookup"><span data-stu-id="828a4-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="828a4-365">Instruções</span><span class="sxs-lookup"><span data-stu-id="828a4-365">Statements</span></span>

<span data-ttu-id="828a4-366">As ações de um programa são expressas usando ***instruções***.</span><span class="sxs-lookup"><span data-stu-id="828a4-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="828a4-367">O C# oferece suporte a vários tipos diferentes de instruções, algumas delas definidas em termos de instruções inseridas.</span><span class="sxs-lookup"><span data-stu-id="828a4-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="828a4-368">Um ***bloco*** permite a produção de várias instruções em contextos nos quais uma única instrução é permitida.</span><span class="sxs-lookup"><span data-stu-id="828a4-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="828a4-369">Um bloco é composto por uma lista de instruções escritas entre os delimitadores `{` e `}`.</span><span class="sxs-lookup"><span data-stu-id="828a4-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="828a4-370">***Instruções de declaração*** são usadas para declarar constantes e variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="828a4-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="828a4-371">***Instruções de expressão*** são usadas para avaliar expressões.</span><span class="sxs-lookup"><span data-stu-id="828a4-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="828a4-372">Expressões que podem ser usadas como instruções incluem chamadas de método, alocações de objeto usando o `new` operador, atribuições usando `=` e os operadores de atribuição composta, operações de incremento e decremento usando o `++`e `--` operadores e expressões await.</span><span class="sxs-lookup"><span data-stu-id="828a4-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="828a4-373">***Instruções de seleção*** são usadas para selecionar uma dentre várias instruções possíveis para execução com base no valor de alguma expressão.</span><span class="sxs-lookup"><span data-stu-id="828a4-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="828a4-374">Neste grupo estão as instruções `if` e `switch`.</span><span class="sxs-lookup"><span data-stu-id="828a4-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="828a4-375">***Instruções de iteração*** são usados para executar repetidamente uma instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="828a4-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="828a4-376">Neste grupo estão as instruções `while`, `do`, `for` e `foreach`.</span><span class="sxs-lookup"><span data-stu-id="828a4-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="828a4-377">***Instruções de salto*** são usadas para transferir o controle.</span><span class="sxs-lookup"><span data-stu-id="828a4-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="828a4-378">Neste grupo estão as instruções `break`, `continue`, `goto`, `throw`, `return` e `yield`.</span><span class="sxs-lookup"><span data-stu-id="828a4-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="828a4-379">A instrução `try`... `catch` é usada para capturar exceções que ocorrem durante a execução de um bloco, e a instrução `try`... `finally` é usada para especificar o código de finalização que é executado sempre, se uma exceção ocorrer ou não.</span><span class="sxs-lookup"><span data-stu-id="828a4-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="828a4-380">O `checked` e `unchecked` instruções são usadas para controlar o contexto para operações aritméticas de tipo integral e conversões de verificação de estouro.</span><span class="sxs-lookup"><span data-stu-id="828a4-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="828a4-381">A instrução `lock` é usada para obter o bloqueio de exclusão mútua para um determinado objeto, executar uma instrução e, em seguida, liberar o bloqueio.</span><span class="sxs-lookup"><span data-stu-id="828a4-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="828a4-382">A instrução `using` é usada para obter um recurso, executar uma instrução e, em seguida, descartar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="828a4-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="828a4-383">A seguir estão exemplos de cada tipo de instrução</span><span class="sxs-lookup"><span data-stu-id="828a4-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="828a4-384">__Declarações de variável local__</span><span class="sxs-lookup"><span data-stu-id="828a4-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="828a4-385">__Declaração de constante local__</span><span class="sxs-lookup"><span data-stu-id="828a4-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="828a4-386">__Instrução de expressão__</span><span class="sxs-lookup"><span data-stu-id="828a4-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="828a4-387">__`if` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-387">__`if` statement__</span></span>

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


<span data-ttu-id="828a4-388">__`switch` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-388">__`switch` statement__</span></span>

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

<span data-ttu-id="828a4-389">__`while` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="828a4-390">__`do` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="828a4-391">__`for` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="828a4-392">__`foreach` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="828a4-393">__`break` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="828a4-394">__`continue` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="828a4-395">__`goto` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-395">__`goto` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

<span data-ttu-id="828a4-396">__`return` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="828a4-397">__`yield` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-397">__`yield` statement__</span></span>

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="828a4-398">__`throw` e `try` instruções__</span><span class="sxs-lookup"><span data-stu-id="828a4-398">__`throw` and `try` statements__</span></span>

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

<span data-ttu-id="828a4-399">__`checked` e `unchecked` instruções__</span><span class="sxs-lookup"><span data-stu-id="828a4-399">__`checked` and `unchecked` statements__</span></span>

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

<span data-ttu-id="828a4-400">__`lock` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-400">__`lock` statement__</span></span>

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

<span data-ttu-id="828a4-401">__`using` statement__</span><span class="sxs-lookup"><span data-stu-id="828a4-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="828a4-402">Classes e objetos</span><span class="sxs-lookup"><span data-stu-id="828a4-402">Classes and objects</span></span>

<span data-ttu-id="828a4-403">As ***classes*** são os tipos do C# mais fundamentais.</span><span class="sxs-lookup"><span data-stu-id="828a4-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="828a4-404">Uma classe é uma estrutura de dados que combina ações (métodos e outros membros da função) e estado (campos) em uma única unidade.</span><span class="sxs-lookup"><span data-stu-id="828a4-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="828a4-405">Uma classe fornece uma definição para ***instâncias*** da classe criadas dinamicamente, também conhecidas como ***objetos***.</span><span class="sxs-lookup"><span data-stu-id="828a4-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="828a4-406">As classes dão suporte à ***herança*** e ***polimorfismo***, mecanismos nos quais ***classes derivadas*** podem estender e especializar ***classes base***.</span><span class="sxs-lookup"><span data-stu-id="828a4-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="828a4-407">Novas classes são criadas usando declarações de classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-407">New classes are created using class declarations.</span></span> <span data-ttu-id="828a4-408">Uma declaração de classe inicia com um cabeçalho que especifica os atributos e modificadores de classe, o nome da classe, a classe base (se fornecida) e as interfaces implementadas pela classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="828a4-409">O cabeçalho é seguido pelo corpo da classe, que consiste em uma lista de declarações de membro escrita entre os delimitadores `{` e `}`.</span><span class="sxs-lookup"><span data-stu-id="828a4-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="828a4-410">A seguir está uma declaração de uma classe simples chamada `Point`:</span><span class="sxs-lookup"><span data-stu-id="828a4-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="828a4-411">Instâncias de classes são criadas usando o operador `new`, que aloca memória para uma nova instância, chama um construtor para inicializar a instância e retorna uma referência à instância.</span><span class="sxs-lookup"><span data-stu-id="828a4-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="828a4-412">As instruções a seguir criam dois `Point` objetos e armazenam referências a esses objetos em duas variáveis:</span><span class="sxs-lookup"><span data-stu-id="828a4-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="828a4-413">A memória ocupada por um objeto é recuperada automaticamente quando o objeto não está mais em uso.</span><span class="sxs-lookup"><span data-stu-id="828a4-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="828a4-414">Não é necessário nem possível desalocar explicitamente os objetos em C#.</span><span class="sxs-lookup"><span data-stu-id="828a4-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="828a4-415">Membros</span><span class="sxs-lookup"><span data-stu-id="828a4-415">Members</span></span>

<span data-ttu-id="828a4-416">Os membros de uma classe são ***membros estáticos*** ou ***membros de instância***.</span><span class="sxs-lookup"><span data-stu-id="828a4-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="828a4-417">Os membros estáticos pertencem às classes e os membros de instância pertencem aos objetos (instâncias de classes).</span><span class="sxs-lookup"><span data-stu-id="828a4-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="828a4-418">A tabela a seguir fornece uma visão geral dos tipos de membros de que uma classe pode conter.</span><span class="sxs-lookup"><span data-stu-id="828a4-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="828a4-419">__Membro__</span><span class="sxs-lookup"><span data-stu-id="828a4-419">__Member__</span></span>   | <span data-ttu-id="828a4-420">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="828a4-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="828a4-421">Constantes</span><span class="sxs-lookup"><span data-stu-id="828a4-421">Constants</span></span>    | <span data-ttu-id="828a4-422">Valores constantes associados à classe</span><span class="sxs-lookup"><span data-stu-id="828a4-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="828a4-423">Campos</span><span class="sxs-lookup"><span data-stu-id="828a4-423">Fields</span></span>       | <span data-ttu-id="828a4-424">Variáveis de classe</span><span class="sxs-lookup"><span data-stu-id="828a4-424">Variables of the class</span></span> |
| <span data-ttu-id="828a4-425">Métodos</span><span class="sxs-lookup"><span data-stu-id="828a4-425">Methods</span></span>      | <span data-ttu-id="828a4-426">Os cálculos e as ações que podem ser executados pela classe</span><span class="sxs-lookup"><span data-stu-id="828a4-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="828a4-427">Propriedades</span><span class="sxs-lookup"><span data-stu-id="828a4-427">Properties</span></span>   | <span data-ttu-id="828a4-428">Ações associadas à leitura e à gravação de propriedades nomeadas da classe</span><span class="sxs-lookup"><span data-stu-id="828a4-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="828a4-429">Indexadores</span><span class="sxs-lookup"><span data-stu-id="828a4-429">Indexers</span></span>     | <span data-ttu-id="828a4-430">Ações associadas a instâncias de indexação da classe como uma matriz</span><span class="sxs-lookup"><span data-stu-id="828a4-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="828a4-431">Eventos</span><span class="sxs-lookup"><span data-stu-id="828a4-431">Events</span></span>       | <span data-ttu-id="828a4-432">Notificações que podem ser geradas pela classe</span><span class="sxs-lookup"><span data-stu-id="828a4-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="828a4-433">Operadores</span><span class="sxs-lookup"><span data-stu-id="828a4-433">Operators</span></span>    | <span data-ttu-id="828a4-434">Operadores de conversões e expressão com suporte da classe</span><span class="sxs-lookup"><span data-stu-id="828a4-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="828a4-435">Construtores</span><span class="sxs-lookup"><span data-stu-id="828a4-435">Constructors</span></span> | <span data-ttu-id="828a4-436">Ações necessárias para inicializar instâncias da classe ou a própria classe</span><span class="sxs-lookup"><span data-stu-id="828a4-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="828a4-437">Destruidores</span><span class="sxs-lookup"><span data-stu-id="828a4-437">Destructors</span></span>  | <span data-ttu-id="828a4-438">Ações a serem executadas antes de as instâncias da classe serem descartadas permanentemente</span><span class="sxs-lookup"><span data-stu-id="828a4-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="828a4-439">Tipos</span><span class="sxs-lookup"><span data-stu-id="828a4-439">Types</span></span>        | <span data-ttu-id="828a4-440">Tipos aninhados declarados pela classe</span><span class="sxs-lookup"><span data-stu-id="828a4-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="828a4-441">Acessibilidade</span><span class="sxs-lookup"><span data-stu-id="828a4-441">Accessibility</span></span>

<span data-ttu-id="828a4-442">Cada membro de uma classe tem uma acessibilidade associada, que controla as regiões de texto do programa que são capazes de acessar o membro.</span><span class="sxs-lookup"><span data-stu-id="828a4-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="828a4-443">Há cinco formas possíveis de acessibilidade.</span><span class="sxs-lookup"><span data-stu-id="828a4-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="828a4-444">Esses métodos estão resumidos na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="828a4-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="828a4-445">__Acessibilidade__</span><span class="sxs-lookup"><span data-stu-id="828a4-445">__Accessibility__</span></span>    | <span data-ttu-id="828a4-446">__Significado__</span><span class="sxs-lookup"><span data-stu-id="828a4-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="828a4-447">Acesso não limitado</span><span class="sxs-lookup"><span data-stu-id="828a4-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="828a4-448">Acesso limitado a essa classe ou classes derivadas dessa classe</span><span class="sxs-lookup"><span data-stu-id="828a4-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="828a4-449">Acesso limitado a este programa</span><span class="sxs-lookup"><span data-stu-id="828a4-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="828a4-450">Acesso limitado a esse programa ou a classes derivadas dessa classe</span><span class="sxs-lookup"><span data-stu-id="828a4-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="828a4-451">Acesso limitado a essa classe</span><span class="sxs-lookup"><span data-stu-id="828a4-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="828a4-452">Parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="828a4-452">Type parameters</span></span>

<span data-ttu-id="828a4-453">Uma definição de classe pode especificar um conjunto de parâmetros de tipo seguindo o nome da classe com colchetes angulares com uma lista de nomes de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="828a4-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="828a4-454">Os parâmetros de tipo podem a ser usado no corpo das declarações de classe para definir os membros da classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="828a4-455">No exemplo a seguir, os parâmetros de tipo de `Pair` são `TFirst` e `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="828a4-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="828a4-456">Um tipo de classe é declarado para pegar parâmetros de tipo é chamado de um tipo de classe genérica.</span><span class="sxs-lookup"><span data-stu-id="828a4-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="828a4-457">Tipos de struct, de interface e de delegado também podem ser genéricos.</span><span class="sxs-lookup"><span data-stu-id="828a4-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="828a4-458">Quando a classe genérica é usada, os argumentos de tipo devem ser fornecidos para cada um dos parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="828a4-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="828a4-459">Um tipo genérico com argumentos de tipo fornecidos, como `Pair<int,string>
    ` acima, é chamado um tipo construído.</span><span class="sxs-lookup"><span data-stu-id="828a4-459">A generic type with type arguments provided, like `Pair<int,string>
    ` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="828a4-460">Classes base</span><span class="sxs-lookup"><span data-stu-id="828a4-460">Base classes</span></span>

<span data-ttu-id="828a4-461">Uma declaração de classe pode especificar uma classe base, seguindo os parâmetros de nome da classe e tipo com dois-pontos e o nome da classe base.</span><span class="sxs-lookup"><span data-stu-id="828a4-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="828a4-462">Omitir uma especificação de classe base é o mesmo que derivar do `object` de tipo.</span><span class="sxs-lookup"><span data-stu-id="828a4-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="828a4-463">No exemplo a seguir, a classe base de `Point3D` é `Point` e a classe base de `Point` é `object`:</span><span class="sxs-lookup"><span data-stu-id="828a4-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="828a4-464">Uma classe herda os membros de sua classe base.</span><span class="sxs-lookup"><span data-stu-id="828a4-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="828a4-465">Herança significa que uma classe contém implicitamente todos os membros de sua classe base, exceto para a instância e construtores estáticos e os destruidores da classe base.</span><span class="sxs-lookup"><span data-stu-id="828a4-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="828a4-466">Uma classe derivada pode adicionar novos membros aos que ela herda, mas ela não pode remover a definição de um membro herdado.</span><span class="sxs-lookup"><span data-stu-id="828a4-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="828a4-467">No exemplo anterior, `Point3D` herda os campos `x` e `y` de `Point` e cada instância `Point3D` contém três campos: `x`, `y` e `z`.</span><span class="sxs-lookup"><span data-stu-id="828a4-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="828a4-468">Existe uma conversão implícita de um tipo de classe para qualquer um de seus tipos de classe base.</span><span class="sxs-lookup"><span data-stu-id="828a4-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="828a4-469">Portanto, uma variável de um tipo de classe pode referenciar uma instância dessa classe ou uma instância de qualquer classe derivada.</span><span class="sxs-lookup"><span data-stu-id="828a4-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="828a4-470">Por exemplo, dadas as declarações de classe anteriores, uma variável do tipo `Point` podem referenciar um `Point` ou um `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="828a4-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="828a4-471">Campos</span><span class="sxs-lookup"><span data-stu-id="828a4-471">Fields</span></span>

<span data-ttu-id="828a4-472">Um campo é uma variável que está associada a uma classe ou uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="828a4-473">Um campo declarado com o `static` modificador define uma ***campo estático***.</span><span class="sxs-lookup"><span data-stu-id="828a4-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="828a4-474">Um campo estático identifica exatamente um local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="828a4-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="828a4-475">Não importa quantas instâncias de uma classe são criadas, há sempre apenas uma cópia de um campo estático.</span><span class="sxs-lookup"><span data-stu-id="828a4-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="828a4-476">Um campo declarado sem o `static` modificador define uma ***campo de instância***.</span><span class="sxs-lookup"><span data-stu-id="828a4-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="828a4-477">Cada instância de uma classe contém uma cópia separada de todos os campos de instância dessa classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="828a4-478">No exemplo a seguir, cada instância da classe `Color` tem uma cópia separada dos campos de instância `r`, `g` e `b`, mas há apenas uma cópia dos campos estáticos `Black`, `White`, `Red`, `Green` e `Blue`:</span><span class="sxs-lookup"><span data-stu-id="828a4-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="828a4-479">Conforme mostrado no exemplo anterior, os ***campos somente leitura*** podem ser declarados com um modificador `readonly`.</span><span class="sxs-lookup"><span data-stu-id="828a4-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="828a4-480">Atribuição a um `readonly` campo só pode ocorrer como parte da declaração do campo ou em um construtor na mesma classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="828a4-481">Métodos</span><span class="sxs-lookup"><span data-stu-id="828a4-481">Methods</span></span>

<span data-ttu-id="828a4-482">Um ***método*** é um membro que implementa um cálculo ou uma ação que pode ser executada por um objeto ou classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="828a4-483">Os ***métodos estáticos*** são acessados pela classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="828a4-484">Os ***métodos de instância*** são acessados pelas instâncias da classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="828a4-485">Métodos têm uma lista (possivelmente vazia) de ***parâmetros***, que representam valores ou referências de variável passadas para o método e uma ***tipo de retorno***, que especifica o tipo do valor calculado e retornado pelo o método.</span><span class="sxs-lookup"><span data-stu-id="828a4-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="828a4-486">Tipo de retorno do método é `void` se ele não retorna um valor.</span><span class="sxs-lookup"><span data-stu-id="828a4-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="828a4-487">Como os tipos, os métodos também podem ter um conjunto de parâmetros de tipo, para que os quais os argumentos de tipo devem ser especificados quando o método é chamado.</span><span class="sxs-lookup"><span data-stu-id="828a4-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="828a4-488">Ao contrário dos tipos, os argumentos de tipo geralmente podem ser inferidos de argumentos de uma chamada de método e não precisam ser fornecidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="828a4-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="828a4-489">A ***assinatura*** de um método deve ser exclusiva na classe na qual o método é declarado.</span><span class="sxs-lookup"><span data-stu-id="828a4-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="828a4-490">A assinatura de um método consiste no nome do método, número de parâmetros de tipo e número, modificadores e tipos de seus parâmetros.</span><span class="sxs-lookup"><span data-stu-id="828a4-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="828a4-491">A assinatura de um método não inclui o tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="828a4-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="828a4-492">Parâmetros</span><span class="sxs-lookup"><span data-stu-id="828a4-492">Parameters</span></span>

<span data-ttu-id="828a4-493">Os parâmetros são usados para passar valores ou referências de variável aos métodos.</span><span class="sxs-lookup"><span data-stu-id="828a4-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="828a4-494">Os parâmetros de um método obtêm seus valores reais de ***argumentos*** que são especificados quando o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="828a4-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="828a4-495">Há quatro tipos de parâmetros: parâmetros de valor, parâmetros de referência, parâmetros de saída e matrizes de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="828a4-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="828a4-496">Um ***parâmetro de valor*** é usado para passar o parâmetro de entrada.</span><span class="sxs-lookup"><span data-stu-id="828a4-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="828a4-497">Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento passado para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="828a4-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="828a4-498">As modificações em um parâmetro de valor não afetam o argumento passado para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="828a4-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="828a4-499">Os parâmetros de valor podem ser opcionais, especificando um valor padrão para que os argumentos correspondentes possam ser omitidos.</span><span class="sxs-lookup"><span data-stu-id="828a4-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="828a4-500">Um ***parâmetro de referência*** é usado para passar os parâmetros de entrada e saída.</span><span class="sxs-lookup"><span data-stu-id="828a4-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="828a4-501">O argumento passado para um parâmetro de referência deve ser uma variável e, durante a execução do método, o parâmetro de referência representa o mesmo local de armazenamento como a variável de argumento.</span><span class="sxs-lookup"><span data-stu-id="828a4-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="828a4-502">Um parâmetro de referência é declarado com o modificador `ref`.</span><span class="sxs-lookup"><span data-stu-id="828a4-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="828a4-503">O exemplo a seguir mostra o uso de parâmetros `ref`.</span><span class="sxs-lookup"><span data-stu-id="828a4-503">The following example shows the use of `ref` parameters.</span></span>

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
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="828a4-504">Um ***parâmetro de saída*** é usado para passar o parâmetro de entrada.</span><span class="sxs-lookup"><span data-stu-id="828a4-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="828a4-505">Um parâmetro de saída é semelhante a um parâmetro de referência, exceto que o valor inicial do argumento fornecido não é importante.</span><span class="sxs-lookup"><span data-stu-id="828a4-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="828a4-506">Um parâmetro de saída é declarado com o modificador `out`.</span><span class="sxs-lookup"><span data-stu-id="828a4-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="828a4-507">O exemplo a seguir mostra o uso de parâmetros `out`.</span><span class="sxs-lookup"><span data-stu-id="828a4-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="828a4-508">Uma ***matriz de parâmetros*** permite que um número variável de argumentos sejam passados para um método.</span><span class="sxs-lookup"><span data-stu-id="828a4-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="828a4-509">Uma matriz de parâmetro é declarada com o modificador `params`.</span><span class="sxs-lookup"><span data-stu-id="828a4-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="828a4-510">Somente o último parâmetro de um método pode ser uma matriz de parâmetros e o tipo de uma matriz de parâmetros deve ser um tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="828a4-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="828a4-511">O `Write` e `WriteLine` métodos do `System.Console` classe são bons exemplos de uso da matriz de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="828a4-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="828a4-512">Eles são declarados como segue.</span><span class="sxs-lookup"><span data-stu-id="828a4-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="828a4-513">Dentro de um método que usa uma matriz de parâmetros, a matriz de parâmetros se comporta exatamente como um parâmetro regular de um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="828a4-514">No entanto, em uma invocação de um método com uma matriz de parâmetros, é possível passar um único argumento do tipo da matriz de parâmetro ou qualquer número de argumentos do tipo de elemento da matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="828a4-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="828a4-515">No último caso, uma instância de matriz é automaticamente criada e inicializada com os argumentos determinados.</span><span class="sxs-lookup"><span data-stu-id="828a4-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="828a4-516">Esse exemplo</span><span class="sxs-lookup"><span data-stu-id="828a4-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="828a4-517">é equivalente ao escrito a seguir.</span><span class="sxs-lookup"><span data-stu-id="828a4-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="828a4-518">Corpo do método e variáveis locais</span><span class="sxs-lookup"><span data-stu-id="828a4-518">Method body and local variables</span></span>

<span data-ttu-id="828a4-519">Um corpo do método Especifica as instruções para executar quando o método é invocado.</span><span class="sxs-lookup"><span data-stu-id="828a4-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="828a4-520">Um corpo de método pode declarar variáveis que são específicas para a invocação do método.</span><span class="sxs-lookup"><span data-stu-id="828a4-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="828a4-521">Essas variáveis são chamadas de ***variáveis locais***.</span><span class="sxs-lookup"><span data-stu-id="828a4-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="828a4-522">Uma declaração de variável local especifica um nome de tipo, um nome de variável e, possivelmente, um valor inicial.</span><span class="sxs-lookup"><span data-stu-id="828a4-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="828a4-523">O exemplo a seguir declara uma variável local `i` com um valor inicial de zero e uma variável local `j` sem valor inicial.</span><span class="sxs-lookup"><span data-stu-id="828a4-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="828a4-524">O C# requer que uma variável local seja ***atribuída definitivamente*** antes de seu valor poder ser obtido.</span><span class="sxs-lookup"><span data-stu-id="828a4-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="828a4-525">Por exemplo, se a declaração do `i` anterior não incluísse um valor inicial, o compilador relataria um erro para usos subsequentes de `i` porque `i` não seria definitivamente atribuído a esses pontos do programa.</span><span class="sxs-lookup"><span data-stu-id="828a4-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="828a4-526">Um método pode usar instruções `return` para retornar o controle é pelo chamador.</span><span class="sxs-lookup"><span data-stu-id="828a4-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="828a4-527">Em um método que retorna `void`, as instruções `return` não podem especificar uma expressão.</span><span class="sxs-lookup"><span data-stu-id="828a4-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="828a4-528">Em um método de retorno não -`void`, `return` instruções devem incluir uma expressão que calcula o valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="828a4-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="828a4-529">Métodos estáticos e de instância</span><span class="sxs-lookup"><span data-stu-id="828a4-529">Static and instance methods</span></span>

<span data-ttu-id="828a4-530">Um método declarado com um `static` modificador é uma ***método estático***.</span><span class="sxs-lookup"><span data-stu-id="828a4-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="828a4-531">Um método estático não funciona em uma instância específica e pode acessar diretamente apenas membros estáticos.</span><span class="sxs-lookup"><span data-stu-id="828a4-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="828a4-532">Um método declarado sem um `static` modificador é um ***método de instância***.</span><span class="sxs-lookup"><span data-stu-id="828a4-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="828a4-533">Um método de instância opera em uma instância específica e pode acessar membros estáticos e de instância.</span><span class="sxs-lookup"><span data-stu-id="828a4-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="828a4-534">A instância em que um método de instância foi invocado pode ser explicitamente acessada como `this`.</span><span class="sxs-lookup"><span data-stu-id="828a4-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="828a4-535">É um erro se referir a `this` em um método estático.</span><span class="sxs-lookup"><span data-stu-id="828a4-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="828a4-536">A seguinte classe `Entity` tem membros estáticos e de instância.</span><span class="sxs-lookup"><span data-stu-id="828a4-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="828a4-537">Cada instância `Entity` contém um número de série (e, possivelmente, outras informações que não são mostradas aqui).</span><span class="sxs-lookup"><span data-stu-id="828a4-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="828a4-538">O construtor `Entity` (que é como um método de instância) inicializa a nova instância com o próximo número de série disponível.</span><span class="sxs-lookup"><span data-stu-id="828a4-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="828a4-539">Como o construtor é um membro de instância, ele tem permissão para acessar tanto o campo de instância `serialNo` e o campo estático `nextSerialNo`.</span><span class="sxs-lookup"><span data-stu-id="828a4-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="828a4-540">Os métodos estáticos `GetNextSerialNo` e `SetNextSerialNo` podem acessar o campo estático `nextSerialNo`, mas seria um erro para eles acessar diretamente o campo de instância `serialNo`.</span><span class="sxs-lookup"><span data-stu-id="828a4-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="828a4-541">O exemplo a seguir mostra o uso da `Entity` classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="828a4-542">Observe que os métodos estáticos `SetNextSerialNo` e `GetNextSerialNo` são invocados na classe enquanto o método de instância `GetSerialNo` é chamado em instâncias da classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="828a4-543">Métodos abstratos, virtuais e de substituição</span><span class="sxs-lookup"><span data-stu-id="828a4-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="828a4-544">Quando uma declaração de método de instância inclui um modificador `virtual`, o método deve ser um ***método virtual***.</span><span class="sxs-lookup"><span data-stu-id="828a4-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="828a4-545">Quando nenhum `virtual` modificador estiver presente, o método deve ser um ***método não virtual***.</span><span class="sxs-lookup"><span data-stu-id="828a4-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="828a4-546">Quando um método virtual é invocado, o ***tipo de tempo de execução*** da instância para o qual essa invocação ocorre determina a implementação real do método para invocar.</span><span class="sxs-lookup"><span data-stu-id="828a4-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="828a4-547">Em uma invocação de método não virtual, o ***tipo de tempo de compilação*** da instância é o fator determinante.</span><span class="sxs-lookup"><span data-stu-id="828a4-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="828a4-548">Um método virtual pode ser ***substituído*** em uma classe derivada.</span><span class="sxs-lookup"><span data-stu-id="828a4-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="828a4-549">Quando uma declaração de método de instância inclui um `override` modificador, o método substitui um método virtual herdado com a mesma assinatura.</span><span class="sxs-lookup"><span data-stu-id="828a4-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="828a4-550">Enquanto uma declaração de método virtual apresenta um novo método, uma declaração de método de substituição restringe um método virtual herdado existente fornecendo uma nova implementação do método.</span><span class="sxs-lookup"><span data-stu-id="828a4-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="828a4-551">Uma ***abstrata*** é um método virtual sem implementação.</span><span class="sxs-lookup"><span data-stu-id="828a4-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="828a4-552">Um método abstrato é declarado com o `abstract` modificador e é permitido somente em uma classe que também é declarada `abstract`.</span><span class="sxs-lookup"><span data-stu-id="828a4-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="828a4-553">Um método abstrato deve ser substituído em cada classe derivada não abstrata.</span><span class="sxs-lookup"><span data-stu-id="828a4-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="828a4-554">O exemplo a seguir declara uma classe abstrata, `Expression`, que representa um nó de árvore de expressão e três classes derivadas, `Constant`, `VariableReference` e `Operation`, que implementam nós de árvore de expressão para operações aritméticas, referências de variável e constantes.</span><span class="sxs-lookup"><span data-stu-id="828a4-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="828a4-555">(Isso é semelhante, mas não deve para ser confundido com os tipos de árvore de expressão introduzido no [tipos de árvore de expressão](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="828a4-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="828a4-556">As quatro classes anteriores podem ser usadas para modelar expressões aritméticas.</span><span class="sxs-lookup"><span data-stu-id="828a4-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="828a4-557">Por exemplo, usando instâncias dessas classes, a expressão `x + 3` pode ser representada da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="828a4-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="828a4-558">O método `Evaluate` de uma instância `Expression` é chamado para avaliar a expressão especificada e produzir um valor `double`.</span><span class="sxs-lookup"><span data-stu-id="828a4-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="828a4-559">O método recebe como argumento um `Hashtable` que contém nomes de variáveis (como chaves das entradas) e valores (como valores das entradas).</span><span class="sxs-lookup"><span data-stu-id="828a4-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="828a4-560">O `Evaluate` é um método abstrato virtual, que significa que classes derivadas não abstratas devem o substituir para fornecer uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="828a4-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="828a4-561">Uma implementação de `Evaluate` do `Constant` retorna apenas a constante armazenada.</span><span class="sxs-lookup"><span data-stu-id="828a4-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="828a4-562">Um `VariableReference`retorna o valor resultante e da implementação procura o nome da variável da tabela de hash.</span><span class="sxs-lookup"><span data-stu-id="828a4-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="828a4-563">Uma implementação de `Operation` primeiro avalia os operandos esquerdo e direito (chamando recursivamente seus métodos `Evaluate`) e, em seguida, executa a operação aritmética determinada.</span><span class="sxs-lookup"><span data-stu-id="828a4-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="828a4-564">O seguinte programa usa as classes `Expression` para avaliar a expressão `x * (y + 2)` para valores diferentes de `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="828a4-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="828a4-565">Sobrecarga de método</span><span class="sxs-lookup"><span data-stu-id="828a4-565">Method overloading</span></span>

<span data-ttu-id="828a4-566">A ***sobrecarga*** de método permite que vários métodos na mesma classe tenham o mesmo nome, contanto que tenham assinaturas exclusivas.</span><span class="sxs-lookup"><span data-stu-id="828a4-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="828a4-567">Ao compilar uma invocação de um método sobrecarregado, o compilador usa a ***resolução de sobrecarga*** para determinar o método específico para invocar.</span><span class="sxs-lookup"><span data-stu-id="828a4-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="828a4-568">A resolução de sobrecarga localizará o método que melhor corresponde aos argumentos ou relatará um erro se nenhuma correspondência for encontrada.</span><span class="sxs-lookup"><span data-stu-id="828a4-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="828a4-569">O exemplo a seguir mostra a resolução de sobrecarga em vigor.</span><span class="sxs-lookup"><span data-stu-id="828a4-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="828a4-570">O comentário para cada invocação no método `Main` mostra qual método é realmente chamado.</span><span class="sxs-lookup"><span data-stu-id="828a4-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="828a4-571">Conforme mostrado no exemplo, um determinado método sempre pode ser selecionado ao converter explicitamente os argumentos para os tipos de parâmetro exatos e/ou fornecendo explicitamente os argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="828a4-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="828a4-572">Outros membros da função</span><span class="sxs-lookup"><span data-stu-id="828a4-572">Other function members</span></span>

<span data-ttu-id="828a4-573">Os membros que contêm código executável são conhecidos coletivamente como ***membros de função*** de uma classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="828a4-574">A seção anterior descreve os métodos, que são o tipo principal de membros da função.</span><span class="sxs-lookup"><span data-stu-id="828a4-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="828a4-575">Esta seção descreve os outros tipos de membros da função comportados pelo c#: construtores, propriedades, indexadores, eventos, operadores e destruidores.</span><span class="sxs-lookup"><span data-stu-id="828a4-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="828a4-576">O código a seguir mostra uma classe genérica chamada `List<T>`, que implementa uma lista crescente de objetos.</span><span class="sxs-lookup"><span data-stu-id="828a4-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="828a4-577">A classe contém vários exemplos dos tipos mais comuns de membros da função.</span><span class="sxs-lookup"><span data-stu-id="828a4-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="828a4-578">Construtores</span><span class="sxs-lookup"><span data-stu-id="828a4-578">Constructors</span></span>

<span data-ttu-id="828a4-579">O C# dá suporte aos construtores estáticos e de instância.</span><span class="sxs-lookup"><span data-stu-id="828a4-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="828a4-580">Um ***construtor de instância*** é um membro que implementa as ações necessárias para inicializar uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="828a4-581">Um ***construtor estático*** é um membro que implementa as ações necessárias para inicializar uma classe quando ele for carregado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="828a4-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="828a4-582">Um construtor é declarado como um método sem nenhum tipo de retorno e o mesmo nome que a classe continente.</span><span class="sxs-lookup"><span data-stu-id="828a4-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="828a4-583">Se uma declaração de construtor inclui um `static` modificador, ele declara um construtor estático.</span><span class="sxs-lookup"><span data-stu-id="828a4-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="828a4-584">Caso contrário, ela declara um construtor de instância.</span><span class="sxs-lookup"><span data-stu-id="828a4-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="828a4-585">Construtores de instância podem ser sobrecarregados.</span><span class="sxs-lookup"><span data-stu-id="828a4-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="828a4-586">Por exemplo, a classe `List<T>
` declara dois construtores de instância, um sem parâmetros e um que utiliza um parâmetro `int`.</span><span class="sxs-lookup"><span data-stu-id="828a4-586">For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="828a4-587">Os construtores de instância são invocados usando o operador `new`.</span><span class="sxs-lookup"><span data-stu-id="828a4-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="828a4-588">As seguintes instruções alocam duas `List<string>
` instâncias usando cada um dos construtores do `List` classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-588">The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="828a4-589">Diferentemente de outros membros, construtores de instância não são herdados e uma classe não tem nenhum construtor de instância que não os que são realmente declarados na classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="828a4-590">Se nenhum construtor de instância for fornecido para uma classe, então um construtor vazio sem parâmetros será fornecido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="828a4-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="828a4-591">Propriedades</span><span class="sxs-lookup"><span data-stu-id="828a4-591">Properties</span></span>

<span data-ttu-id="828a4-592">As ***propriedades*** são uma extensão natural dos campos.</span><span class="sxs-lookup"><span data-stu-id="828a4-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="828a4-593">Elas são denominadas membros com tipos associados, e a sintaxe para acessar os campos e as propriedades é a mesma.</span><span class="sxs-lookup"><span data-stu-id="828a4-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="828a4-594">No entanto, diferentemente dos campos, as propriedades não denotam locais de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="828a4-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="828a4-595">Em vez disso, as propriedades têm ***acessadores*** que especificam as instruções a serem executadas quando os valores forem lidos ou gravados.</span><span class="sxs-lookup"><span data-stu-id="828a4-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="828a4-596">Uma propriedade é declarada como um campo, exceto que a declaração termina com um `get` acessador e/ou um `set` escrito entre os delimitadores de acessador `{` e `}` em vez de terminar em um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="828a4-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="828a4-597">Uma propriedade que tem ambos um `get` acessador e um `set` acessador é uma ***propriedade de leitura / gravação***, uma propriedade que tem apenas um `get` acessador é uma ***propriedade somente leitura***e um propriedade que tem apenas um `set` acessador é uma ***propriedade somente gravação***.</span><span class="sxs-lookup"><span data-stu-id="828a4-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="828a4-598">Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="828a4-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="828a4-599">Exceto como o destino de uma atribuição, quando uma propriedade é referenciada em uma expressão, o `get` acessador da propriedade é invocado para calcular o valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="828a4-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="828a4-600">Um `set` acessador corresponde a um método com um parâmetro único chamado `value` e nenhum tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="828a4-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="828a4-601">Quando uma propriedade é referenciada como o destino de uma atribuição ou como o operando da `++` ou `--`, o `set` acessador é invocado com um argumento que fornece o novo valor.</span><span class="sxs-lookup"><span data-stu-id="828a4-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="828a4-602">O `List<T>
` classe declara duas propriedades, `Count` e `Capacity`, que são somente leitura e leitura / gravação, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="828a4-602">The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="828a4-603">A seguir está um exemplo de uso dessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="828a4-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="828a4-604">Como nos campos e métodos, o C# dá suporte a propriedades de instância e a propriedades estáticas.</span><span class="sxs-lookup"><span data-stu-id="828a4-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="828a4-605">Propriedades estáticas são declaradas com o `static` modificador e as propriedades de instância são declaradas sem ele.</span><span class="sxs-lookup"><span data-stu-id="828a4-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="828a4-606">Os acessadores de uma propriedade podem ser virtuais.</span><span class="sxs-lookup"><span data-stu-id="828a4-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="828a4-607">Quando uma declaração de propriedade inclui um modificador `virtual`, `abstract` ou `override`, ela se aplica aos acessadores da propriedade.</span><span class="sxs-lookup"><span data-stu-id="828a4-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="828a4-608">Indexadores</span><span class="sxs-lookup"><span data-stu-id="828a4-608">Indexers</span></span>

<span data-ttu-id="828a4-609">Um ***indexador*** é um membro que permite que objetos sejam indexados da mesma forma que uma matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="828a4-610">Um indexador é declarado como uma propriedade, exceto que é o nome do membro `this` seguido por uma lista de parâmetros escrita entre os delimitadores `[` e `]`.</span><span class="sxs-lookup"><span data-stu-id="828a4-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="828a4-611">Os parâmetros estão disponíveis nos acessadores do indexador.</span><span class="sxs-lookup"><span data-stu-id="828a4-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="828a4-612">Semelhante às propriedades, os indexadores podem ser de leitura-gravação, somente leitura e somente gravação, e os acessadores de um indexador pode ser virtuais.</span><span class="sxs-lookup"><span data-stu-id="828a4-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="828a4-613">A classe `List` declara um indexador único de leitura-gravação que usa um parâmetro `int`.</span><span class="sxs-lookup"><span data-stu-id="828a4-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="828a4-614">O indexador possibilita indexar instâncias `List` com valores `int`.</span><span class="sxs-lookup"><span data-stu-id="828a4-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="828a4-615">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="828a4-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="828a4-616">Os indexadores podem ser sobrecarregados, o que significa que uma classe pode declarar vários indexadores, desde que o número ou os tipos de seus parâmetros sejam diferentes.</span><span class="sxs-lookup"><span data-stu-id="828a4-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="828a4-617">Eventos</span><span class="sxs-lookup"><span data-stu-id="828a4-617">Events</span></span>

<span data-ttu-id="828a4-618">Um ***evento*** é um membro que permite que uma classe ou objeto forneça notificações.</span><span class="sxs-lookup"><span data-stu-id="828a4-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="828a4-619">Um evento é declarado como um campo, exceto que a declaração inclui um `event` palavra-chave e o tipo devem ser um tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="828a4-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="828a4-620">Em uma classe que declara um membro de evento, o evento se comporta exatamente como um campo de um tipo delegado (desde que o evento não seja abstrato e não declare acessadores).</span><span class="sxs-lookup"><span data-stu-id="828a4-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="828a4-621">O campo armazena uma referência a um delegado que representa os manipuladores de eventos que foram adicionados ao evento.</span><span class="sxs-lookup"><span data-stu-id="828a4-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="828a4-622">Se nenhum identificador de evento estiver presente, o campo é `null`.</span><span class="sxs-lookup"><span data-stu-id="828a4-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="828a4-623">A classe `List<T>
` declara um membro único de evento chamado `Changed`, que indica que um novo item foi adicionado à lista.</span><span class="sxs-lookup"><span data-stu-id="828a4-623">The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="828a4-624">O `Changed` é gerado pela `OnChanged` método virtual, que primeiro verifica se o evento é `null` (o que significa que nenhum manipulador está presente).</span><span class="sxs-lookup"><span data-stu-id="828a4-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="828a4-625">A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento — assim, não há constructos de linguagem especial para gerar eventos.</span><span class="sxs-lookup"><span data-stu-id="828a4-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="828a4-626">Os clientes reagem a eventos por meio de ***manipuladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="828a4-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="828a4-627">Os manipuladores de eventos são conectados usando o operador `+=` e removidos usando o operador `-=`.</span><span class="sxs-lookup"><span data-stu-id="828a4-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="828a4-628">O exemplo a seguir anexa um manipulador de eventos para o evento `Changed` de um `List<string>
`.</span><span class="sxs-lookup"><span data-stu-id="828a4-628">The following example attaches an event handler to the `Changed` event of a `List<string>
`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="828a4-629">Para cenários avançados nos quais o controle do armazenamento subjacente de um evento é desejado, uma declaração de evento pode fornecer explicitamente acessadores `add` e `remove`, que são um pouco semelhantes ao acessador `set` de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="828a4-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="828a4-630">Operadores</span><span class="sxs-lookup"><span data-stu-id="828a4-630">Operators</span></span>

<span data-ttu-id="828a4-631">Um ***operador*** é um membro que define o significado da aplicação de um operador de expressão específico para instâncias de uma classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="828a4-632">Três tipos de operadores podem ser definidos: operadores unários, operadores binários e operadores de conversão.</span><span class="sxs-lookup"><span data-stu-id="828a4-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="828a4-633">Todos os operadores devem ser declarados como `public` e `static`.</span><span class="sxs-lookup"><span data-stu-id="828a4-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="828a4-634">A classe `List<T>
` declara dois operadores, `operator==` e `operator!=` e, portanto, dá um novo significado para as expressões que aplicam esses operadores a instâncias `List`.</span><span class="sxs-lookup"><span data-stu-id="828a4-634">The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="828a4-635">Especificamente, os operadores de definem a igualdade de duas `List<T>
` instâncias como comparar cada um dos objetos contidos usando seus `Equals` métodos.</span><span class="sxs-lookup"><span data-stu-id="828a4-635">Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="828a4-636">O exemplo a seguir usa o operador `==` para comparar duas instâncias `List<int>
`.</span><span class="sxs-lookup"><span data-stu-id="828a4-636">The following example uses the `==` operator to compare two `List<int>
` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="828a4-637">O primeiro `Console.WriteLine` gera `True` porque as duas listas contêm o mesmo número de objetos com os mesmos valores na mesma ordem.</span><span class="sxs-lookup"><span data-stu-id="828a4-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="828a4-638">Como `List<T>
` não definiu `operator==`, o primeiro `Console.WriteLine` geraria `False` porque `a` e `b` referenciam diferentes instâncias `List<int>
`.</span><span class="sxs-lookup"><span data-stu-id="828a4-638">Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="828a4-639">Destruidores</span><span class="sxs-lookup"><span data-stu-id="828a4-639">Destructors</span></span>

<span data-ttu-id="828a4-640">Um ***destruidor*** é um membro que implementa as ações necessárias para destruir uma instância de uma classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="828a4-641">Destruidores não podem ter parâmetros, eles não podem ter modificadores de acessibilidade e não pode ser invocados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="828a4-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="828a4-642">O destruidor para uma instância é invocado automaticamente durante a coleta de lixo.</span><span class="sxs-lookup"><span data-stu-id="828a4-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="828a4-643">O coletor de lixo tem latitude ampla ao decidir quando coletar objetos e executar os destruidores.</span><span class="sxs-lookup"><span data-stu-id="828a4-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="828a4-644">Especificamente, o tempo de invocações de destruidor não é determinístico e destruidores podem ser executados em qualquer thread.</span><span class="sxs-lookup"><span data-stu-id="828a4-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="828a4-645">Para esses e outros motivos, as classes devem implementar os destruidores somente quando não houver outras soluções viáveis.</span><span class="sxs-lookup"><span data-stu-id="828a4-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="828a4-646">A instrução `using` fornece uma abordagem melhor para a destruição de objetos.</span><span class="sxs-lookup"><span data-stu-id="828a4-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="828a4-647">Structs</span><span class="sxs-lookup"><span data-stu-id="828a4-647">Structs</span></span>

<span data-ttu-id="828a4-648">Como classes, os ***structs*** são estruturas de dados que podem conter membros de dados e os membros da função, mas, ao contrário das classes, as estruturas são tipos de valor e não precisam de alocação de heap.</span><span class="sxs-lookup"><span data-stu-id="828a4-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="828a4-649">Uma variável de um tipo struct armazena diretamente os dados do struct, enquanto que uma variável de um tipo de classe armazena uma referência a um objeto alocado dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="828a4-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="828a4-650">Os tipos de estrutura não dão suporte à herança especificada pelo usuário, e todos os tipos de structs são herdados implicitamente do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="828a4-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="828a4-651">Os structs são particularmente úteis para estruturas de dados pequenas que têm semântica de valor.</span><span class="sxs-lookup"><span data-stu-id="828a4-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="828a4-652">Números complexos, pontos em um sistema de coordenadas ou pares chave-valor em um dicionário são exemplos de structs.</span><span class="sxs-lookup"><span data-stu-id="828a4-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="828a4-653">O uso de structs, em vez de classes para estruturas de dados pequenas, pode fazer uma grande diferença no número de alocações de memória que um aplicativo executa.</span><span class="sxs-lookup"><span data-stu-id="828a4-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="828a4-654">Por exemplo, o programa a seguir cria e inicializa uma matriz de 100 pontos.</span><span class="sxs-lookup"><span data-stu-id="828a4-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="828a4-655">Com `Point` implementado como uma classe, 101 objetos separados são instanciados — um para a matriz e um para os elementos de 100.</span><span class="sxs-lookup"><span data-stu-id="828a4-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="828a4-656">Uma alternativa é fazer `Point` um struct.</span><span class="sxs-lookup"><span data-stu-id="828a4-656">An alternative is to make `Point` a struct.</span></span>

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
<span data-ttu-id="828a4-657">Agora, somente um objeto é instanciado — um para a matriz — e as instâncias `Point` são armazenadas em linha na matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="828a4-658">Construtores de struct são invocados com o operador `new`, mas isso não significa que a memória está sendo alocada.</span><span class="sxs-lookup"><span data-stu-id="828a4-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="828a4-659">Em vez de alocar dinamicamente um objeto e retornar uma referência a ele, um construtor de struct simplesmente retorna o valor do struct (normalmente em um local temporário na pilha), e esse valor é, então, copiado conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="828a4-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="828a4-660">Com classes, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível que operações em uma variável afetem o objeto referenciado por outra variável.</span><span class="sxs-lookup"><span data-stu-id="828a4-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="828a4-661">Com structs, as variáveis têm sua própria cópia dos dados e não é possível que as operações em um afetem o outro.</span><span class="sxs-lookup"><span data-stu-id="828a4-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="828a4-662">Por exemplo, a saída produzida pelo seguinte fragmento de código depende de se `Point` é uma classe ou estrutura.</span><span class="sxs-lookup"><span data-stu-id="828a4-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="828a4-663">Se `Point` é uma classe, a saída é `20` porque `a` e `b` referenciam o mesmo objeto.</span><span class="sxs-lookup"><span data-stu-id="828a4-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="828a4-664">Se `Point` é um struct, a saída é `10` porque a atribuição de `a` à `b` cria uma cópia do valor, e essa cópia não é afetada pela atribuição subsequente para `a.x`.</span><span class="sxs-lookup"><span data-stu-id="828a4-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="828a4-665">O exemplo anterior destaca duas das limitações dos structs.</span><span class="sxs-lookup"><span data-stu-id="828a4-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="828a4-666">Primeiro, copiar um struct inteiro é, geralmente, menos eficiente do que copiar uma referência de objeto, então a passagem de atribuição e de valor do parâmetro pode ser mais custosa com structs que com tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="828a4-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="828a4-667">Segundo, com exceção para parâmetros `ref` e `out`, não é possível criar referências para structs, o que rege o uso em diversas situações.</span><span class="sxs-lookup"><span data-stu-id="828a4-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="828a4-668">Matrizes</span><span class="sxs-lookup"><span data-stu-id="828a4-668">Arrays</span></span>

<span data-ttu-id="828a4-669">Uma ***matriz*** é uma estrutura de dados que contém algumas variáveis acessadas por meio de índices calculados.</span><span class="sxs-lookup"><span data-stu-id="828a4-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="828a4-670">As variáveis contidas em uma matriz, também chamadas de ***elementos*** da matriz, são todas do mesmo tipo, e esse tipo é chamado de ***tipo de elemento*** da matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="828a4-671">Os tipos de matriz são tipos de referência, e a declaração de uma variável de matriz simplesmente reserva espaço para uma referência a uma instância de matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="828a4-672">Instâncias reais da matriz são criadas dinamicamente em tempo de execução usando o `new` operador.</span><span class="sxs-lookup"><span data-stu-id="828a4-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="828a4-673">O `new` operação Especifica a ***comprimento*** da nova instância de matriz, que depois fica fixa para o tempo de vida da instância.</span><span class="sxs-lookup"><span data-stu-id="828a4-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="828a4-674">Os índices dos elementos de uma matriz variam de `0` a `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="828a4-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="828a4-675">O operador `new` inicializa automaticamente os elementos de uma matriz usando o valor padrão, que, por exemplo, é zero para todos os tipos numéricos e `null` para todos os tipos de referência.</span><span class="sxs-lookup"><span data-stu-id="828a4-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="828a4-676">O exemplo a seguir cria uma matriz de elementos `int`, inicializa a matriz e imprime o conteúdo da matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="828a4-677">Este exemplo cria e opera em uma ***matriz unidimensional***.</span><span class="sxs-lookup"><span data-stu-id="828a4-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="828a4-678">O C# também oferece suporte a ***matrizes multidimensionais***.</span><span class="sxs-lookup"><span data-stu-id="828a4-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="828a4-679">O número de dimensões de um tipo de matriz, também conhecido como ***classificação*** do tipo de matriz, é o número um mais o número de vírgulas escrito entre os colchetes do tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="828a4-680">O exemplo a seguir aloca um unidimensional, bidimensional e uma matriz tridimensional.</span><span class="sxs-lookup"><span data-stu-id="828a4-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="828a4-681">A matriz `a1` contém 10 elementos, a matriz `a2` contém 50 (10 × 5) elementos e a matriz `a3` contém 100 (10 × 5 × 2) elementos.</span><span class="sxs-lookup"><span data-stu-id="828a4-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="828a4-682">O tipo do elemento de uma matriz pode ser qualquer tipo, incluindo um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="828a4-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="828a4-683">Uma matriz com elementos de um tipo de matriz é chamada às vezes de ***matriz denteada***, pois os tamanhos das matrizes do elemento nem sempre precisam ser iguais.</span><span class="sxs-lookup"><span data-stu-id="828a4-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="828a4-684">O exemplo a seguir aloca uma matriz de matrizes de `int`:</span><span class="sxs-lookup"><span data-stu-id="828a4-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="828a4-685">A primeira linha cria uma matriz com três elementos, cada um do tipo `int[]`, e cada um com um valor inicial de `null`.</span><span class="sxs-lookup"><span data-stu-id="828a4-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="828a4-686">As linhas subsequentes inicializam os três elementos com referências às instâncias individuais da matriz de tamanhos variados.</span><span class="sxs-lookup"><span data-stu-id="828a4-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="828a4-687">O `new` operador permite que os valores iniciais dos elementos da matriz seja especificada com um ***inicializador de matriz***, que é uma lista de expressões escritas entre os delimitadores `{` e `}`.</span><span class="sxs-lookup"><span data-stu-id="828a4-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="828a4-688">O exemplo a seguir aloca e inicializa um `int[]` com três elementos.</span><span class="sxs-lookup"><span data-stu-id="828a4-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="828a4-689">Observe que o comprimento da matriz é inferido do número de expressões entre `{` e `}`.</span><span class="sxs-lookup"><span data-stu-id="828a4-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="828a4-690">A variável local e declarações de campo podem ser reduzidas ainda mais, de modo que o tipo de matriz não precise ser redefinido.</span><span class="sxs-lookup"><span data-stu-id="828a4-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="828a4-691">Os dois exemplos anteriores são equivalentes ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="828a4-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="828a4-692">Interfaces</span><span class="sxs-lookup"><span data-stu-id="828a4-692">Interfaces</span></span>

<span data-ttu-id="828a4-693">Uma ***interface*** define um contrato que pode ser implementado por classes e estruturas.</span><span class="sxs-lookup"><span data-stu-id="828a4-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="828a4-694">Uma interface pode conter métodos, propriedades, eventos e indexadores.</span><span class="sxs-lookup"><span data-stu-id="828a4-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="828a4-695">Uma interface não fornece implementações dos membros que define — ela simplesmente especifica os membros que devem ser fornecidos por classes ou estruturas que implementam a interface.</span><span class="sxs-lookup"><span data-stu-id="828a4-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="828a4-696">As interfaces podem empregar a ***herança múltipla***.</span><span class="sxs-lookup"><span data-stu-id="828a4-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="828a4-697">No exemplo a seguir, a interface `IComboBox` herda de `ITextBox` e `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="828a4-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="828a4-698">Classes e structs podem implementar várias interfaces.</span><span class="sxs-lookup"><span data-stu-id="828a4-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="828a4-699">No exemplo a seguir, a classe `EditBox` implementa `IControl` e `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="828a4-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="828a4-700">Quando uma classe ou struct implementa uma interface específica, as instâncias dessa classe ou struct podem ser convertidas implicitamente para esse tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="828a4-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="828a4-701">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="828a4-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="828a4-702">Em casos nos quais uma instância não é conhecida por ser estática para implementar uma interface específica, é possível usar conversões de tipo dinâmico.</span><span class="sxs-lookup"><span data-stu-id="828a4-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="828a4-703">Por exemplo, as instruções a seguir usam conversões de tipo dinâmico para obter um objeto `IControl` e `IDataBound` implementações de interface.</span><span class="sxs-lookup"><span data-stu-id="828a4-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="828a4-704">Porque o tipo real do objeto é `EditBox`, as conversões são bem-sucedidas.</span><span class="sxs-lookup"><span data-stu-id="828a4-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="828a4-705">Anteriormente na `EditBox` classe, o `Paint` método da `IControl` interface e o `Bind` método da `IDataBound` interface são implementados usando `public` membros.</span><span class="sxs-lookup"><span data-stu-id="828a4-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="828a4-706">C# também dá suporte a ***implementações de membros de interface explícita***, com o qual a classe ou struct pode evitar a tornar os membros `public`.</span><span class="sxs-lookup"><span data-stu-id="828a4-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="828a4-707">Uma implementação de membro de interface explícita é escrita usando o nome do membro de interface totalmente qualificado.</span><span class="sxs-lookup"><span data-stu-id="828a4-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="828a4-708">Por exemplo, a classe `EditBox` pode implementar os métodos `IControl.Paint` e `IDataBound.Bind` usando implementações de membros de interface explícita da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="828a4-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="828a4-709">Os membros de interface explícita só podem ser acessados por meio do tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="828a4-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="828a4-710">Por exemplo, a implementação de `IControl.Paint` fornecidos pelo anterior `EditBox` classe só pode ser chamada convertendo primeiro o `EditBox` de referência para o `IControl` tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="828a4-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="828a4-711">Enums</span><span class="sxs-lookup"><span data-stu-id="828a4-711">Enums</span></span>

<span data-ttu-id="828a4-712">Um ***tipo enum*** é um tipo de valor diferente com um conjunto de constantes nomeadas.</span><span class="sxs-lookup"><span data-stu-id="828a4-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="828a4-713">O exemplo a seguir declara e usa um tipo de enumeração denominado `Color` com três valores de constantes `Red`, `Green`, e `Blue`.</span><span class="sxs-lookup"><span data-stu-id="828a4-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="828a4-714">Cada tipo de enumeração tem um tipo integral correspondente chamado de ***tipo subjacente*** o tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="828a4-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="828a4-715">Um tipo de enumeração que não declara explicitamente um tipo subjacente tem um tipo subjacente de `int`.</span><span class="sxs-lookup"><span data-stu-id="828a4-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="828a4-716">O formato de armazenamento e intervalo de valores possíveis de um tipo de enumeração são determinados pelo seu tipo subjacente.</span><span class="sxs-lookup"><span data-stu-id="828a4-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="828a4-717">O conjunto de valores que pode assumir um tipo de enumeração não é limitado por seus membros de enum.</span><span class="sxs-lookup"><span data-stu-id="828a4-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="828a4-718">Em particular, qualquer valor do tipo subjacente de uma enumeração pode ser convertido para o tipo de enumeração e é um valor válido diferente daquele tipo enum.</span><span class="sxs-lookup"><span data-stu-id="828a4-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="828a4-719">O exemplo a seguir declara um tipo de enumeração denominado `Alignment` com um tipo subjacente de `sbyte`.</span><span class="sxs-lookup"><span data-stu-id="828a4-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="828a4-720">Conforme mostrado no exemplo anterior, uma declaração de membro de enumeração pode incluir uma expressão constante que especifica o valor do membro.</span><span class="sxs-lookup"><span data-stu-id="828a4-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="828a4-721">O valor da constante para cada membro de enumeração deve ser no intervalo do tipo subjacente da enumeração.</span><span class="sxs-lookup"><span data-stu-id="828a4-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="828a4-722">Quando uma declaração de membro de enumeração não especifica explicitamente um valor, o membro recebe o valor zero (se ele for o primeiro membro no tipo enum) ou o valor do membro enum textualmente precedente mais um.</span><span class="sxs-lookup"><span data-stu-id="828a4-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="828a4-723">Valores de enumeração podem ser convertidos em valores integrais e vice-versa usando conversões de tipo.</span><span class="sxs-lookup"><span data-stu-id="828a4-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="828a4-724">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="828a4-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="828a4-725">O valor padrão de qualquer tipo de enumeração é o valor integral zero convertido para o tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="828a4-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="828a4-726">Em casos em que as variáveis são inicializadas automaticamente como um valor padrão, isso é o valor fornecido para variáveis de tipos enum.</span><span class="sxs-lookup"><span data-stu-id="828a4-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="828a4-727">Para que o valor padrão de um tipo de enumeração fique facilmente disponível, o literal `0` converte implicitamente para qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="828a4-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="828a4-728">Dessa forma, o seguinte é permitido.</span><span class="sxs-lookup"><span data-stu-id="828a4-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="828a4-729">Delegados</span><span class="sxs-lookup"><span data-stu-id="828a4-729">Delegates</span></span>

<span data-ttu-id="828a4-730">Um ***delegado*** é um tipo que representa referências aos métodos com uma lista de parâmetros e tipo de retorno específicos.</span><span class="sxs-lookup"><span data-stu-id="828a4-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="828a4-731">Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="828a4-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="828a4-732">Os delegados são parecidos com o conceito de ponteiros de função em outras linguagens, mas ao contrário dos ponteiros de função, os delegados são orientados a objetos e fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="828a4-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="828a4-733">O exemplo a seguir declara e usa um tipo delegado chamado `Function`.</span><span class="sxs-lookup"><span data-stu-id="828a4-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="828a4-734">Uma instância do tipo delegado `Function` pode fazer referência a qualquer método que usa um argumento `double` e retorna um valor `double`.</span><span class="sxs-lookup"><span data-stu-id="828a4-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="828a4-735">O `Apply` método aplica-se um determinado `Function` para os elementos de uma `double[]`, retornando um `double[]` com os resultados.</span><span class="sxs-lookup"><span data-stu-id="828a4-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="828a4-736">No método `Main`, `Apply` é usado para aplicar três funções diferentes para um `double[]`.</span><span class="sxs-lookup"><span data-stu-id="828a4-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="828a4-737">Um delegado pode referenciar um método estático (como `Square` ou `Math.Sin` no exemplo anterior) ou um método de instância (como `m.Multiply` no exemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="828a4-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="828a4-738">Um delegado que referencia um método de instância também referencia um objeto específico, e quando o método de instância é invocado por meio do delegado, esse objeto se torna `this` na invocação.</span><span class="sxs-lookup"><span data-stu-id="828a4-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="828a4-739">Os delegados podem ser criados usando funções anônimas, que são "métodos embutidos" criados dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="828a4-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="828a4-740">As funções anônimas podem ver as variáveis locais dos métodos ao redor.</span><span class="sxs-lookup"><span data-stu-id="828a4-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="828a4-741">Portanto, o exemplo de multiplicador acima pode ser gravado mais facilmente sem usar um `Multiplier` classe:</span><span class="sxs-lookup"><span data-stu-id="828a4-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="828a4-742">Uma propriedade interessante e útil de um delegado é que ele não sabe ou se importa com a classe do método que referencia; o que importa é que o método referenciado tem os mesmos parâmetros e o tipo de retorno do delegado.</span><span class="sxs-lookup"><span data-stu-id="828a4-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="828a4-743">Atributos</span><span class="sxs-lookup"><span data-stu-id="828a4-743">Attributes</span></span>

<span data-ttu-id="828a4-744">Tipos, membros e outras entidades em um programa C# dão suporte a modificadores que controlam determinados aspectos de seu comportamento.</span><span class="sxs-lookup"><span data-stu-id="828a4-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="828a4-745">Por exemplo, a acessibilidade de um método é controlada usando os modificadores `public`, `protected`, `internal` e `private`.</span><span class="sxs-lookup"><span data-stu-id="828a4-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="828a4-746">O C# generaliza essa funcionalidade, de modo que os tipos definidos pelo usuário de informações declarativas podem ser anexados a entidades de programa e recuperados no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="828a4-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="828a4-747">Os programas especificam essas informações declarativas adicionais, definindo e usando os ***atributos***.</span><span class="sxs-lookup"><span data-stu-id="828a4-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="828a4-748">O exemplo a seguir declara um atributo `HelpAttribute` que pode ser colocado em entidades de programa para fornecem links para a documentação associada.</span><span class="sxs-lookup"><span data-stu-id="828a4-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="828a4-749">Todas as classes de atributo derivam o `System.Attribute` fornecido pelo .NET Framework de classe base.</span><span class="sxs-lookup"><span data-stu-id="828a4-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="828a4-750">Os atributos podem ser aplicados, fornecendo seu nome, junto com quaisquer argumentos, dentro dos colchetes pouco antes da declaração associada.</span><span class="sxs-lookup"><span data-stu-id="828a4-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="828a4-751">Se o nome de um atributo termina em `Attribute`, essa parte do nome pode ser omitida quando o atributo é referenciado.</span><span class="sxs-lookup"><span data-stu-id="828a4-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="828a4-752">Por exemplo, o atributo `HelpAttribute` pode ser usado da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="828a4-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="828a4-753">Este exemplo anexa um `HelpAttribute` para o `Widget` classe e outro `HelpAttribute` para o `Display` método na classe.</span><span class="sxs-lookup"><span data-stu-id="828a4-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="828a4-754">Os construtores públicos de uma classe de atributo controlam as informações que devem ser fornecidas quando o atributo é anexado a uma entidade de programa.</span><span class="sxs-lookup"><span data-stu-id="828a4-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="828a4-755">As informações adicionais podem ser fornecidas ao referenciar propriedades públicas de leitura-gravação da classe de atributo (como a referência anterior à propriedade `Topic`).</span><span class="sxs-lookup"><span data-stu-id="828a4-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="828a4-756">O exemplo a seguir mostra como as informações de atributo para uma entidade de determinado programa podem ser recuperadas em tempo de execução usando reflexão.</span><span class="sxs-lookup"><span data-stu-id="828a4-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="828a4-757">Quando um atributo específico for solicitado por meio de reflexão, o construtor para a classe de atributo será invocado com as informações fornecidas na origem do programa e a instância do atributo resultante será retornada.</span><span class="sxs-lookup"><span data-stu-id="828a4-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="828a4-758">Se forem fornecidas informações adicionais por meio de propriedades, essas propriedades serão definidas para os valores fornecidos antes que a instância do atributo seja retornada.</span><span class="sxs-lookup"><span data-stu-id="828a4-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
