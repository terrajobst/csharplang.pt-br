---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485178"
---
# <a name="default-interface-methods"></a><span data-ttu-id="21dde-101">métodos de interface padrão</span><span class="sxs-lookup"><span data-stu-id="21dde-101">default interface methods</span></span>

* <span data-ttu-id="21dde-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="21dde-102">[x] Proposed</span></span>
* <span data-ttu-id="21dde-103">[] Protótipo: [em andamento](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="21dde-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="21dde-104">[] Implementação: nenhuma</span><span class="sxs-lookup"><span data-stu-id="21dde-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="21dde-105">[] Especificação: em andamento, abaixo</span><span class="sxs-lookup"><span data-stu-id="21dde-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="21dde-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="21dde-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="21dde-107">Adicionar suporte para _métodos de extensão virtual_ – métodos em interfaces com implementações concretas.</span><span class="sxs-lookup"><span data-stu-id="21dde-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="21dde-108">Uma classe ou estrutura que implementa tal interface é necessária para ter uma única implementação _mais específica_ para o método de interface, implementada pela classe ou struct, ou herdada de suas classes ou interfaces base.</span><span class="sxs-lookup"><span data-stu-id="21dde-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="21dde-109">Os métodos de extensão virtual permitem que um autor de API adicione métodos a uma interface em versões futuras sem perder a origem ou a compatibilidade binária com as implementações existentes dessa interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="21dde-110">Eles são semelhantes aos ["métodos padrão"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)do Java.</span><span class="sxs-lookup"><span data-stu-id="21dde-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="21dde-111">(Com base na provável técnica de implementação), esse recurso requer suporte correspondente no CLI/CLR.</span><span class="sxs-lookup"><span data-stu-id="21dde-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="21dde-112">Os programas que tiram proveito desse recurso não podem ser executados em versões anteriores da plataforma.</span><span class="sxs-lookup"><span data-stu-id="21dde-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="21dde-113">Motivação</span><span class="sxs-lookup"><span data-stu-id="21dde-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="21dde-114">As principais motivações para esse recurso são</span><span class="sxs-lookup"><span data-stu-id="21dde-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="21dde-115">Os métodos de interface padrão permitem que um autor de API adicione métodos a uma interface em versões futuras sem perder a origem ou a compatibilidade binária com as implementações existentes dessa interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="21dde-116">O recurso permite C# interoperar com APIs voltadas para [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) e [Ios (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), que dão suporte a recursos semelhantes.</span><span class="sxs-lookup"><span data-stu-id="21dde-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="21dde-117">Como acontece, adicionar implementações de interface padrão fornece os elementos do recurso de linguagem "características" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span><span class="sxs-lookup"><span data-stu-id="21dde-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="21dde-118">As características comprovaram ser uma técnica de programação avançada (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span><span class="sxs-lookup"><span data-stu-id="21dde-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="21dde-119">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="21dde-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="21dde-120">A sintaxe de uma interface é estendida para permitir</span><span class="sxs-lookup"><span data-stu-id="21dde-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="21dde-121">declarações de membro que declaram constantes, operadores, construtores estáticos e tipos aninhados;</span><span class="sxs-lookup"><span data-stu-id="21dde-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="21dde-122">um *corpo* para um método, um indexador, uma propriedade ou um acessador de evento (ou seja, uma implementação "padrão");</span><span class="sxs-lookup"><span data-stu-id="21dde-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="21dde-123">declarações de membro que declaram campos estáticos, métodos, propriedades, indexadores e eventos;</span><span class="sxs-lookup"><span data-stu-id="21dde-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="21dde-124">declarações de membro usando a sintaxe de implementação de interface explícita; e</span><span class="sxs-lookup"><span data-stu-id="21dde-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="21dde-125">Modificadores de acesso explícitos (o acesso padrão é `public`).</span><span class="sxs-lookup"><span data-stu-id="21dde-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="21dde-126">Membros com corpos permitem que a interface forneça uma implementação "padrão" para o método em classes e estruturas que não fornecem uma implementação de substituição.</span><span class="sxs-lookup"><span data-stu-id="21dde-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="21dde-127">As interfaces não podem conter o estado da instância.</span><span class="sxs-lookup"><span data-stu-id="21dde-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="21dde-128">Embora os campos estáticos agora sejam permitidos, os campos de instância não são permitidos em interfaces.</span><span class="sxs-lookup"><span data-stu-id="21dde-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="21dde-129">Não há suporte para propriedades automáticas de instância em interfaces, pois elas declarariam implicitamente um campo oculto.</span><span class="sxs-lookup"><span data-stu-id="21dde-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="21dde-130">Os métodos estáticos e privados permitem a refatoração útil e a organização do código usado para implementar a API pública da interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="21dde-131">Uma substituição de método em uma interface deve usar a sintaxe de implementação de interface explícita.</span><span class="sxs-lookup"><span data-stu-id="21dde-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="21dde-132">É um erro declarar um tipo de classe, tipo de struct ou tipo de enumeração dentro do escopo de um parâmetro de tipo que foi declarado com um *variance_annotation*.</span><span class="sxs-lookup"><span data-stu-id="21dde-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="21dde-133">Por exemplo, a declaração de `C` abaixo é um erro.</span><span class="sxs-lookup"><span data-stu-id="21dde-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="21dde-134">Métodos concretos em interfaces</span><span class="sxs-lookup"><span data-stu-id="21dde-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="21dde-135">A forma mais simples desse recurso é a capacidade de declarar um *método concreto* em uma interface, que é um método com um corpo.</span><span class="sxs-lookup"><span data-stu-id="21dde-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="21dde-136">Uma classe que implementa essa interface não precisa implementar seu método concreto.</span><span class="sxs-lookup"><span data-stu-id="21dde-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="21dde-137">A substituição final para `IA.M` na classe `C` é o método concreto `M` declarado em `IA`.</span><span class="sxs-lookup"><span data-stu-id="21dde-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="21dde-138">Observe que uma classe não herda membros de suas interfaces; Isso não é alterado por esse recurso:</span><span class="sxs-lookup"><span data-stu-id="21dde-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="21dde-139">Dentro de um membro de instância de uma interface, `this` tem o tipo da interface delimitadora.</span><span class="sxs-lookup"><span data-stu-id="21dde-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="21dde-140">Modificadores em interfaces</span><span class="sxs-lookup"><span data-stu-id="21dde-140">Modifiers in interfaces</span></span>

<span data-ttu-id="21dde-141">A sintaxe de uma interface é relaxada para permitir modificadores em seus membros.</span><span class="sxs-lookup"><span data-stu-id="21dde-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="21dde-142">Os itens a seguir são permitidos: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`e `partial`.</span><span class="sxs-lookup"><span data-stu-id="21dde-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="21dde-143">***Todo***: Verifique quais outros modificadores existem.</span><span class="sxs-lookup"><span data-stu-id="21dde-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="21dde-144">Um membro de interface cuja declaração inclui um corpo é um membro `virtual`, a menos que o modificador `sealed` ou `private` seja usado.</span><span class="sxs-lookup"><span data-stu-id="21dde-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="21dde-145">O modificador de `virtual` pode ser usado em um membro de função que, de outra forma, seria implicitamente `virtual`.</span><span class="sxs-lookup"><span data-stu-id="21dde-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="21dde-146">Da mesma forma, embora `abstract` seja o padrão em membros de interface sem corpos, esse modificador pode ser fornecido explicitamente.</span><span class="sxs-lookup"><span data-stu-id="21dde-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="21dde-147">Um membro não virtual pode ser declarado usando a palavra-chave `sealed`.</span><span class="sxs-lookup"><span data-stu-id="21dde-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="21dde-148">É um erro para um membro de função `private` ou `sealed` de uma interface não ter corpo.</span><span class="sxs-lookup"><span data-stu-id="21dde-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="21dde-149">Um membro da função `private` pode não ter o modificador `sealed`.</span><span class="sxs-lookup"><span data-stu-id="21dde-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="21dde-150">Os modificadores de acesso podem ser usados em membros de interface de todos os tipos de membros que são permitidos.</span><span class="sxs-lookup"><span data-stu-id="21dde-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="21dde-151">O nível de acesso `public` é o padrão, mas pode ser fornecido explicitamente.</span><span class="sxs-lookup"><span data-stu-id="21dde-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="21dde-152">***Abrir problema:*** Precisamos especificar o significado preciso dos modificadores de acesso, como `protected` e `internal`, e quais declarações fazem e não os substituem (em uma interface derivada) ou os implementam (em uma classe que implementa a interface).</span><span class="sxs-lookup"><span data-stu-id="21dde-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="21dde-153">As interfaces podem declarar `static` Membros, incluindo tipos aninhados, métodos, indexadores, propriedades, eventos e construtores estáticos.</span><span class="sxs-lookup"><span data-stu-id="21dde-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="21dde-154">O nível de acesso padrão para todos os membros da interface é `public`.</span><span class="sxs-lookup"><span data-stu-id="21dde-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="21dde-155">As interfaces não podem declarar construtores, destruidores ou campos de instância.</span><span class="sxs-lookup"><span data-stu-id="21dde-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="21dde-156">***Problema fechado:*** As declarações de operador devem ser permitidas em uma interface?</span><span class="sxs-lookup"><span data-stu-id="21dde-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="21dde-157">Provavelmente não há operadores de conversão, mas e quanto a outros?</span><span class="sxs-lookup"><span data-stu-id="21dde-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="21dde-158">***Decisão***: operadores são permitidos *, exceto* para operadores de conversão, igualdade e desigualdade.</span><span class="sxs-lookup"><span data-stu-id="21dde-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="21dde-159">***Problema fechado:*** Deve `new` ser permitido em declarações de membro de interface que ocultam membros de interfaces base?</span><span class="sxs-lookup"><span data-stu-id="21dde-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="21dde-160">***Decisão***: Sim.</span><span class="sxs-lookup"><span data-stu-id="21dde-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="21dde-161">***Problema fechado:*** No momento, não permitimos `partial` em uma interface ou seus membros.</span><span class="sxs-lookup"><span data-stu-id="21dde-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="21dde-162">Isso exigiria uma proposta separada.</span><span class="sxs-lookup"><span data-stu-id="21dde-162">That would require a separate proposal.</span></span> <span data-ttu-id="21dde-163">***Decisão***: Sim.</span><span class="sxs-lookup"><span data-stu-id="21dde-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="21dde-164">Substituições em interfaces</span><span class="sxs-lookup"><span data-stu-id="21dde-164">Overrides in interfaces</span></span>

<span data-ttu-id="21dde-165">As declarações de substituição (ou seja, aquelas que contêm o modificador de `override`) permitem que o programador forneça uma implementação mais específica de um membro virtual em uma interface em que o compilador ou tempo de execução não teria, de outra forma, encontrar um.</span><span class="sxs-lookup"><span data-stu-id="21dde-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="21dde-166">Ele também permite transformar um membro abstrato de uma superinterface em um membro padrão em uma interface derivada.</span><span class="sxs-lookup"><span data-stu-id="21dde-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="21dde-167">Uma declaração de substituição tem permissão para substituir *explicitamente* um método de interface base específico qualificando a declaração com o nome da interface (nenhum modificador de acesso é permitido nesse caso).</span><span class="sxs-lookup"><span data-stu-id="21dde-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="21dde-168">Substituições implícitas não são permitidas.</span><span class="sxs-lookup"><span data-stu-id="21dde-168">Implicit overrides are not permitted.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

<span data-ttu-id="21dde-169">Declarações de substituição em interfaces não podem ser declaradas `sealed`.</span><span class="sxs-lookup"><span data-stu-id="21dde-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="21dde-170">Os membros da função pública `virtual` em uma interface podem ser substituídos em uma interface derivada explicitamente (qualificando o nome na declaração de substituição com o tipo de interface que declarou originalmente o método e omitindo um modificador de acesso).</span><span class="sxs-lookup"><span data-stu-id="21dde-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="21dde-171">`virtual` membros de função em uma interface só podem ser substituídos explicitamente (não implicitamente) em interfaces derivadas e os membros que não são `public` só podem ser implementados em uma classe ou estrutura explicitamente (não implicitamente).</span><span class="sxs-lookup"><span data-stu-id="21dde-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="21dde-172">Em ambos os casos, o membro substituído ou implementado deve estar *acessível* onde é substituído.</span><span class="sxs-lookup"><span data-stu-id="21dde-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="21dde-173">Reabstração</span><span class="sxs-lookup"><span data-stu-id="21dde-173">Reabstraction</span></span>

<span data-ttu-id="21dde-174">Um método virtual (concreto) declarado em uma interface pode ser substituído para ser abstrato em uma interface derivada</span><span class="sxs-lookup"><span data-stu-id="21dde-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

<span data-ttu-id="21dde-175">O modificador de `abstract` não é necessário na declaração de `IB.M` (que é o padrão nas interfaces), mas provavelmente é uma boa prática ser explícita em uma declaração de substituição.</span><span class="sxs-lookup"><span data-stu-id="21dde-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="21dde-176">Isso é útil em interfaces derivadas em que a implementação padrão de um método é inadequada e uma implementação mais apropriada deve ser fornecida pela implementação de classes.</span><span class="sxs-lookup"><span data-stu-id="21dde-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="21dde-177">***Abrir problema:*** A reabstração deve ser permitida?</span><span class="sxs-lookup"><span data-stu-id="21dde-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="21dde-178">A regra de substituição mais específica</span><span class="sxs-lookup"><span data-stu-id="21dde-178">The most specific override rule</span></span>

<span data-ttu-id="21dde-179">Exigimos que cada interface e classe tenham uma *substituição mais específica* para cada membro virtual entre as substituições que aparecem no tipo ou suas interfaces diretas e indiretas.</span><span class="sxs-lookup"><span data-stu-id="21dde-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="21dde-180">A *substituição mais específica* é uma substituição exclusiva que é mais específica do que todas as outras substituições.</span><span class="sxs-lookup"><span data-stu-id="21dde-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="21dde-181">Se não houver nenhuma substituição, o membro em si será considerado a substituição mais específica.</span><span class="sxs-lookup"><span data-stu-id="21dde-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="21dde-182">Uma substituição `M1` é considerada *mais específica* do que outra `M2` de substituição se `M1` é declarado no tipo `T1`, `M2` é declarado no tipo `T2`e qualquer um</span><span class="sxs-lookup"><span data-stu-id="21dde-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="21dde-183">`T1` contém `T2` entre suas interfaces diretas ou indiretas, ou</span><span class="sxs-lookup"><span data-stu-id="21dde-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="21dde-184">`T2` é um tipo de interface, mas `T1` não é um tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="21dde-185">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="21dde-185">For example:</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

<span data-ttu-id="21dde-186">A regra de substituição mais específica garante que um conflito (ou seja, uma ambiguidade resultante da herança de losango) seja resolvido explicitamente pelo programador no ponto em que surge o conflito.</span><span class="sxs-lookup"><span data-stu-id="21dde-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="21dde-187">Como damos suporte a substituições abstratas explícitas em interfaces, poderíamos fazer isso em classes também</span><span class="sxs-lookup"><span data-stu-id="21dde-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="21dde-188">***Problema aberto***: devemos dar suporte a substituições abstratas de interface explícita em classes?</span><span class="sxs-lookup"><span data-stu-id="21dde-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="21dde-189">Além disso, é um erro se, em uma declaração de classe, a substituição mais específica de algum método de interface for uma substituição abstrata que foi declarada em uma interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="21dde-190">Esta é uma regra existente renovada usando a nova terminologia.</span><span class="sxs-lookup"><span data-stu-id="21dde-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="21dde-191">É possível que uma propriedade virtual declarada em uma interface tenha uma substituição mais específica para seu acessador de `get` em uma interface e uma substituição mais específica para seu acessador de `set` em uma interface diferente.</span><span class="sxs-lookup"><span data-stu-id="21dde-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="21dde-192">Isso é considerado uma violação da regra de *substituição mais específica* .</span><span class="sxs-lookup"><span data-stu-id="21dde-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="21dde-193">Métodos `static` e `private`</span><span class="sxs-lookup"><span data-stu-id="21dde-193">`static` and `private` methods</span></span>

<span data-ttu-id="21dde-194">Como as interfaces agora podem conter código executável, é útil abstrair o código comum em métodos privados e estáticos.</span><span class="sxs-lookup"><span data-stu-id="21dde-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="21dde-195">Agora, permitimos isso em interfaces.</span><span class="sxs-lookup"><span data-stu-id="21dde-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="21dde-196">***Problema fechado***: devemos dar suporte a métodos privados?</span><span class="sxs-lookup"><span data-stu-id="21dde-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="21dde-197">Devemos dar suporte a métodos estáticos?</span><span class="sxs-lookup"><span data-stu-id="21dde-197">Should we support static methods?</span></span> <span data-ttu-id="21dde-198">**Decisão: Sim**</span><span class="sxs-lookup"><span data-stu-id="21dde-198">**Decision: YES**</span></span>

> <span data-ttu-id="21dde-199">***Problema de abertura***: devemos permitir que os métodos de interface sejam `protected` ou `internal` ou outro acesso?</span><span class="sxs-lookup"><span data-stu-id="21dde-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="21dde-200">Em caso afirmativo, quais são as semânticas?</span><span class="sxs-lookup"><span data-stu-id="21dde-200">If so, what are the semantics?</span></span> <span data-ttu-id="21dde-201">Eles são `virtual` por padrão?</span><span class="sxs-lookup"><span data-stu-id="21dde-201">Are they `virtual` by default?</span></span> <span data-ttu-id="21dde-202">Nesse caso, há uma maneira de torná-los não virtuais?</span><span class="sxs-lookup"><span data-stu-id="21dde-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="21dde-203">***Problema aberto***: se damos suporte a métodos estáticos, devemos dar suporte a operadores (estáticos)?</span><span class="sxs-lookup"><span data-stu-id="21dde-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="21dde-204">Invocações de interface base</span><span class="sxs-lookup"><span data-stu-id="21dde-204">Base interface invocations</span></span>

<span data-ttu-id="21dde-205">O código em um tipo derivado de uma interface com um método padrão pode invocar explicitamente a implementação de "base" dessa interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

<span data-ttu-id="21dde-206">Um método de instância (não estático) é permitido para invocar a implementação de um método de instância acessível em uma interface base direta não virtualmente, nomeando-a usando a sintaxe `base(Type).M`.</span><span class="sxs-lookup"><span data-stu-id="21dde-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="21dde-207">Isso é útil quando uma substituição necessária para ser fornecida devido à herança de losango é resolvida pela delegação a uma implementação de base específica.</span><span class="sxs-lookup"><span data-stu-id="21dde-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

<span data-ttu-id="21dde-208">Quando um membro de `virtual` ou `abstract` é acessado usando a sintaxe `base(Type).M`, é necessário que `Type` contenha uma *substituição exclusiva mais específica* para `M`.</span><span class="sxs-lookup"><span data-stu-id="21dde-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="21dde-209">Vinculando cláusulas base</span><span class="sxs-lookup"><span data-stu-id="21dde-209">Binding base clauses</span></span>

<span data-ttu-id="21dde-210">Agora, as interfaces contêm tipos.</span><span class="sxs-lookup"><span data-stu-id="21dde-210">Interfaces now contain types.</span></span>  <span data-ttu-id="21dde-211">Esses tipos podem ser usados na cláusula base como interfaces base.</span><span class="sxs-lookup"><span data-stu-id="21dde-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="21dde-212">Ao ligar uma cláusula base, talvez seja necessário saber o conjunto de interfaces base para associar esses tipos (por exemplo, para pesquisar neles e resolver o acesso protegido).</span><span class="sxs-lookup"><span data-stu-id="21dde-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="21dde-213">O significado da cláusula base de uma interface é, portanto, definido circularmente.</span><span class="sxs-lookup"><span data-stu-id="21dde-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="21dde-214">Para interromper o ciclo, adicionamos novas regras de idioma correspondentes a uma regra semelhante já em vigor para classes.</span><span class="sxs-lookup"><span data-stu-id="21dde-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="21dde-215">Ao determinar o significado da *interface_base* de uma interface, as interfaces base são temporariamente consideradas vazias.</span><span class="sxs-lookup"><span data-stu-id="21dde-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="21dde-216">Intuitivamente, isso garante que o significado de uma cláusula base não possa ser recursivamente dependente de si mesma.</span><span class="sxs-lookup"><span data-stu-id="21dde-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="21dde-217">**Nós usamos as seguintes regras:**</span><span class="sxs-lookup"><span data-stu-id="21dde-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="21dde-218">"Quando uma classe B deriva de uma classe A, é um erro de tempo de compilação para um para depender de B. Uma classe **depende diretamente** de sua classe base direta (se houver) e **depende diretamente** da ~~**classe**~~ na qual ela é imediatamente aninhada (se houver).</span><span class="sxs-lookup"><span data-stu-id="21dde-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="21dde-219">Dada essa definição, o conjunto completo de ~~**classes**~~ sobre as quais uma classe depende é o fechamento reflexivo e transitivo da relação **depende diretamente** de.</span><span class="sxs-lookup"><span data-stu-id="21dde-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="21dde-220">É um erro de tempo de compilação para uma interface herdar direta ou indiretamente de si mesma.</span><span class="sxs-lookup"><span data-stu-id="21dde-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="21dde-221">As **interfaces base** de uma interface são as interfaces base explícitas e suas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="21dde-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="21dde-222">Em outras palavras, o conjunto de interfaces base é o fechamento transitivo completo das interfaces base explícitas, suas interfaces base explícitas e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="21dde-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="21dde-223">**Estamos ajustando-os da seguinte maneira:**</span><span class="sxs-lookup"><span data-stu-id="21dde-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="21dde-224">Quando uma classe B deriva de uma classe A, é um erro de tempo de compilação para um para depender de B. Uma classe **depende diretamente** de sua classe base direta (se houver) e **depende diretamente** do _**tipo**_ no qual ele é imediatamente aninhado (se houver).</span><span class="sxs-lookup"><span data-stu-id="21dde-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="21dde-225">Quando uma interface IB estende uma interface IA, é um erro de tempo de compilação para IA depender da IB.</span><span class="sxs-lookup"><span data-stu-id="21dde-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="21dde-226">Uma interface **depende diretamente** de suas interfaces base diretas (se houver) e **depende diretamente** do tipo no qual ele é imediatamente aninhado (se houver).</span><span class="sxs-lookup"><span data-stu-id="21dde-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="21dde-227">Dadas essas definições, o conjunto completo de **tipos** sobre os quais um tipo depende é o fechamento reflexivo e transitivo da relação **depende diretamente** de.</span><span class="sxs-lookup"><span data-stu-id="21dde-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="21dde-228">Efeito em programas existentes</span><span class="sxs-lookup"><span data-stu-id="21dde-228">Effect on existing programs</span></span>

<span data-ttu-id="21dde-229">As regras apresentadas aqui destinam-se a não afetar o significado dos programas existentes.</span><span class="sxs-lookup"><span data-stu-id="21dde-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="21dde-230">Exemplo 1:</span><span class="sxs-lookup"><span data-stu-id="21dde-230">Example 1:</span></span>

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

<span data-ttu-id="21dde-231">Exemplo 2:</span><span class="sxs-lookup"><span data-stu-id="21dde-231">Example 2:</span></span>

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

<span data-ttu-id="21dde-232">As mesmas regras fornecem resultados semelhantes à situação análoga que envolve métodos de interface padrão:</span><span class="sxs-lookup"><span data-stu-id="21dde-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> <span data-ttu-id="21dde-233">***Problema fechado***: Confirme se essa é uma conseqüência pretendida da especificação.</span><span class="sxs-lookup"><span data-stu-id="21dde-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="21dde-234">**Decisão: Sim**</span><span class="sxs-lookup"><span data-stu-id="21dde-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="21dde-235">Resolução do método de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="21dde-235">Runtime method resolution</span></span>

> <span data-ttu-id="21dde-236">***Problema fechado:*** A especificação deve descrever o algoritmo de resolução do método de tempo de execução na face dos métodos padrão de interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="21dde-237">Precisamos garantir que a semântica seja consistente com a semântica da linguagem, por exemplo, quais métodos declarados fazem e não substituem ou implementam um método `internal`.</span><span class="sxs-lookup"><span data-stu-id="21dde-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="21dde-238">API de suporte CLR</span><span class="sxs-lookup"><span data-stu-id="21dde-238">CLR support API</span></span>

<span data-ttu-id="21dde-239">Para que os compiladores detectem quando estão compilando para um tempo de execução que dá suporte a esse recurso, as bibliotecas para tais tempos de execução são modificadas para anunciar esse fato por meio da API discutida em <https://github.com/dotnet/corefx/issues/17116>.</span><span class="sxs-lookup"><span data-stu-id="21dde-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="21dde-240">Adicionamos</span><span class="sxs-lookup"><span data-stu-id="21dde-240">We add</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> <span data-ttu-id="21dde-241">***Abrir problema***: é o melhor nome para o recurso *CLR* ?</span><span class="sxs-lookup"><span data-stu-id="21dde-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="21dde-242">O recurso CLR faz muito mais do que apenas isso (por exemplo, libera as restrições de proteção, dá suporte a substituições em interfaces, etc).</span><span class="sxs-lookup"><span data-stu-id="21dde-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="21dde-243">Talvez ele deva ser chamado de algo como "métodos concretos em interfaces" ou "características"?</span><span class="sxs-lookup"><span data-stu-id="21dde-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="21dde-244">Outras áreas a serem especificadas</span><span class="sxs-lookup"><span data-stu-id="21dde-244">Further areas to be specified</span></span>

- <span data-ttu-id="21dde-245">[] Seria útil catalogar os tipos de efeitos de compatibilidade binária e de origem causados pela adição de métodos de interface padrão e substituições a interfaces existentes.</span><span class="sxs-lookup"><span data-stu-id="21dde-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="21dde-246">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="21dde-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="21dde-247">Essa proposta requer uma atualização coordenada para a especificação do CLR (para dar suporte a métodos concretos em interfaces e resolução de métodos).</span><span class="sxs-lookup"><span data-stu-id="21dde-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="21dde-248">Portanto, é razoavelmente "caro" e pode valer a pena fazer isso em combinação com outros recursos que também antecipamos a necessidade de alterações no CLR.</span><span class="sxs-lookup"><span data-stu-id="21dde-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="21dde-249">Alternativas</span><span class="sxs-lookup"><span data-stu-id="21dde-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="21dde-250">None.</span><span class="sxs-lookup"><span data-stu-id="21dde-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="21dde-251">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="21dde-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="21dde-252">As perguntas abertas são chamadas em toda a proposta, acima.</span><span class="sxs-lookup"><span data-stu-id="21dde-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="21dde-253">Consulte também <https://github.com/dotnet/csharplang/issues/406> para obter uma lista de perguntas abertas.</span><span class="sxs-lookup"><span data-stu-id="21dde-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="21dde-254">A especificação detalhada deve descrever o mecanismo de resolução usado em tempo de execução para selecionar o método preciso a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="21dde-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="21dde-255">A interação dos metadados produzidos por novos compiladores e consumidos por compiladores mais antigos precisa ser realizada em detalhes.</span><span class="sxs-lookup"><span data-stu-id="21dde-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="21dde-256">Por exemplo, precisamos garantir que a representação de metadados que usamos não cause a adição de uma implementação padrão em uma interface para interromper uma classe existente que implementa essa interface quando compilada por um compilador mais antigo.</span><span class="sxs-lookup"><span data-stu-id="21dde-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="21dde-257">Isso pode afetar a representação de metadados que podemos usar.</span><span class="sxs-lookup"><span data-stu-id="21dde-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="21dde-258">O design deve considerar a interoperação com outras linguagens e compiladores existentes para outras linguagens.</span><span class="sxs-lookup"><span data-stu-id="21dde-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="21dde-259">Perguntas resolvidas</span><span class="sxs-lookup"><span data-stu-id="21dde-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="21dde-260">Substituição abstrata</span><span class="sxs-lookup"><span data-stu-id="21dde-260">Abstract Override</span></span>

<span data-ttu-id="21dde-261">A especificação de rascunho anterior continha a capacidade de "reabstrair" um método herdado:</span><span class="sxs-lookup"><span data-stu-id="21dde-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

<span data-ttu-id="21dde-262">Minhas notas para 2017-03-20 mostraram que decidimos não permitir isso.</span><span class="sxs-lookup"><span data-stu-id="21dde-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="21dde-263">No entanto, há pelo menos dois casos de uso para ele:</span><span class="sxs-lookup"><span data-stu-id="21dde-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="21dde-264">As APIs do Java, com as quais alguns usuários desse recurso esperamos interoperar dependem dessa instalação.</span><span class="sxs-lookup"><span data-stu-id="21dde-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="21dde-265">A programação com *características* se beneficia disso.</span><span class="sxs-lookup"><span data-stu-id="21dde-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="21dde-266">A reabstração é um dos elementos do recurso de linguagem "características" (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span><span class="sxs-lookup"><span data-stu-id="21dde-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="21dde-267">O seguinte é permitido com classes:</span><span class="sxs-lookup"><span data-stu-id="21dde-267">The following is permitted with classes:</span></span>

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

<span data-ttu-id="21dde-268">Infelizmente, esse código não pode ser refatorado como um conjunto de interfaces (características), a menos que isso seja permitido.</span><span class="sxs-lookup"><span data-stu-id="21dde-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="21dde-269">Pelo *princípio de Jared de ganância*, ele deve ser permitido.</span><span class="sxs-lookup"><span data-stu-id="21dde-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="21dde-270">***Problema fechado:*** A reabstração deve ser permitida?</span><span class="sxs-lookup"><span data-stu-id="21dde-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="21dde-271">Ok Minhas anotações estavam erradas.</span><span class="sxs-lookup"><span data-stu-id="21dde-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="21dde-272">As [observações do LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) dizem que a reabstração é permitida em uma interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="21dde-273">Não está em uma classe.</span><span class="sxs-lookup"><span data-stu-id="21dde-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="21dde-274">Modificador e modificador virtual vs lacrado</span><span class="sxs-lookup"><span data-stu-id="21dde-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="21dde-275">De [Aleksey Tsingauz](https://github.com/AlekseyTs):</span><span class="sxs-lookup"><span data-stu-id="21dde-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="21dde-276">Decidimos permitir que modificadores explicitamente declarados em membros de interface, a menos que haja um motivo para não permitir alguns deles.</span><span class="sxs-lookup"><span data-stu-id="21dde-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="21dde-277">Isso traz uma pergunta interessante sobre o modificador virtual.</span><span class="sxs-lookup"><span data-stu-id="21dde-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="21dde-278">Eles devem ser necessários em membros com implementação padrão?</span><span class="sxs-lookup"><span data-stu-id="21dde-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="21dde-279">Poderíamos dizer que:</span><span class="sxs-lookup"><span data-stu-id="21dde-279">We could say that:</span></span>
>
> - <span data-ttu-id="21dde-280">Se não houver nenhuma implementação e nenhum virtual, nem lacrado for especificado, supomos que o membro é abstrato.</span><span class="sxs-lookup"><span data-stu-id="21dde-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="21dde-281">Se houver uma implementação e nenhum resumo, nem lacrado for especificado, supomos que o membro é virtual.</span><span class="sxs-lookup"><span data-stu-id="21dde-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="21dde-282">o modificador lacrado é necessário para tornar um método não virtual nem abstrato.</span><span class="sxs-lookup"><span data-stu-id="21dde-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="21dde-283">Como alternativa, poderíamos dizer que o modificador virtual é necessário para um membro virtual.</span><span class="sxs-lookup"><span data-stu-id="21dde-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="21dde-284">Ou seja, se houver um membro com implementação não explicitamente marcado com modificador virtual, ele não será virtual nem abstrato.</span><span class="sxs-lookup"><span data-stu-id="21dde-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="21dde-285">Essa abordagem pode proporcionar uma melhor experiência quando um método é movido de uma classe para uma interface:</span><span class="sxs-lookup"><span data-stu-id="21dde-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="21dde-286">um método abstrato permanece abstrato.</span><span class="sxs-lookup"><span data-stu-id="21dde-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="21dde-287">um método virtual permanece virtual.</span><span class="sxs-lookup"><span data-stu-id="21dde-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="21dde-288">um método sem nenhum modificador não permanece virtual nem abstract.</span><span class="sxs-lookup"><span data-stu-id="21dde-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="21dde-289">o modificador lacrado não pode ser aplicado a um método que não seja uma substituição.</span><span class="sxs-lookup"><span data-stu-id="21dde-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="21dde-290">O que você acha?</span><span class="sxs-lookup"><span data-stu-id="21dde-290">What do you think?</span></span>

> <span data-ttu-id="21dde-291">***Problema fechado:*** Um método concreto (com implementação) deve ser implicitamente `virtual`?</span><span class="sxs-lookup"><span data-stu-id="21dde-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="21dde-292">Ok</span><span class="sxs-lookup"><span data-stu-id="21dde-292">[YES]</span></span>

<span data-ttu-id="21dde-293">***Decisões:*** Feitas no LDM 2017-04-05:</span><span class="sxs-lookup"><span data-stu-id="21dde-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="21dde-294">Não virtual deve ser expresso explicitamente por meio de `sealed` ou `private`.</span><span class="sxs-lookup"><span data-stu-id="21dde-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="21dde-295">`sealed` é a palavra-chave para tornar membros da instância da interface com corpos não virtuais</span><span class="sxs-lookup"><span data-stu-id="21dde-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="21dde-296">Queremos permitir todos os modificadores em interfaces</span><span class="sxs-lookup"><span data-stu-id="21dde-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="21dde-297">A acessibilidade padrão para membros de interface é pública, incluindo tipos aninhados</span><span class="sxs-lookup"><span data-stu-id="21dde-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="21dde-298">Membros de função privada em interfaces são lacrados implicitamente e `sealed` não são permitidos neles.</span><span class="sxs-lookup"><span data-stu-id="21dde-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="21dde-299">As classes privadas (em interfaces) são permitidas e podem ser seladas, e isso significa lacrado na classe sensação de sealed.</span><span class="sxs-lookup"><span data-stu-id="21dde-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="21dde-300">Uma boa proposta está ausente, parcial ainda não é permitida em interfaces ou seus membros.</span><span class="sxs-lookup"><span data-stu-id="21dde-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="21dde-301">Compatibilidade binária 1</span><span class="sxs-lookup"><span data-stu-id="21dde-301">Binary Compatibility 1</span></span>

<span data-ttu-id="21dde-302">Quando uma biblioteca fornece uma implementação padrão</span><span class="sxs-lookup"><span data-stu-id="21dde-302">When a library provides a default implementation</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

<span data-ttu-id="21dde-303">Entendemos que a implementação de `I1.M` no `C` é `I1.M`.</span><span class="sxs-lookup"><span data-stu-id="21dde-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="21dde-304">E se o assembly que contém `I2` for alterado da seguinte maneira e recompilada</span><span class="sxs-lookup"><span data-stu-id="21dde-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="21dde-305">Mas `C` não é recompilado.</span><span class="sxs-lookup"><span data-stu-id="21dde-305">but `C` is not recompiled.</span></span> <span data-ttu-id="21dde-306">O que acontece quando o programa é executado?</span><span class="sxs-lookup"><span data-stu-id="21dde-306">What happens when the program is run?</span></span> <span data-ttu-id="21dde-307">Uma invocação de `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="21dde-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="21dde-308">Executa `I1.M`</span><span class="sxs-lookup"><span data-stu-id="21dde-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="21dde-309">Executa `I2.M`</span><span class="sxs-lookup"><span data-stu-id="21dde-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="21dde-310">Gera algum tipo de erro de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="21dde-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="21dde-311">***Decisão:*** Tornou-se 2017-04-11: executa `I2.M`, que é a substituição mais específica, sem ambigüidade, no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="21dde-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="21dde-312">Acessadores de evento (fechados)</span><span class="sxs-lookup"><span data-stu-id="21dde-312">Event accessors (closed)</span></span>

> <span data-ttu-id="21dde-313">***Problema fechado:*** Um evento pode ser substituído "piecewise"?</span><span class="sxs-lookup"><span data-stu-id="21dde-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="21dde-314">Considere este caso:</span><span class="sxs-lookup"><span data-stu-id="21dde-314">Consider this case:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

<span data-ttu-id="21dde-315">Essa implementação "parcial" do evento não é permitida porque, como em uma classe, a sintaxe de uma declaração de evento não permite apenas um acessador; ambos (ou nenhum) devem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="21dde-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="21dde-316">Você poderia realizar a mesma coisa permitindo que o acessador remove abstract na sintaxe seja implicitamente abstrato pela ausência de um corpo:</span><span class="sxs-lookup"><span data-stu-id="21dde-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

<span data-ttu-id="21dde-317">Observe que *essa é uma sintaxe nova (proposta)* .</span><span class="sxs-lookup"><span data-stu-id="21dde-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="21dde-318">Na gramática atual, os acessadores de evento têm um corpo obrigatório.</span><span class="sxs-lookup"><span data-stu-id="21dde-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="21dde-319">***Problema fechado:*** Um acessador de evento pode ser (implicitamente) abstrato pela omissão de um corpo, da mesma forma que os métodos em interfaces e acessadores de propriedade são (implicitamente) abstratos pela omissão de um corpo?</span><span class="sxs-lookup"><span data-stu-id="21dde-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="21dde-320">***Decisão:*** (2017-04-18) não, as declarações de evento exigem tanto acessadores concretos (ou nenhum).</span><span class="sxs-lookup"><span data-stu-id="21dde-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="21dde-321">Reabstração em uma classe (fechada)</span><span class="sxs-lookup"><span data-stu-id="21dde-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="21dde-322">***Problema fechado:*** Devemos confirmar que isso é permitido (caso contrário, adicionar uma implementação padrão seria uma alteração significativa):</span><span class="sxs-lookup"><span data-stu-id="21dde-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

<span data-ttu-id="21dde-323">***Decisão:*** (2017-04-18) Sim, a adição de um corpo a uma declaração de membro de interface não deve quebrar C.</span><span class="sxs-lookup"><span data-stu-id="21dde-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="21dde-324">Substituição lacrada (fechada)</span><span class="sxs-lookup"><span data-stu-id="21dde-324">Sealed Override (closed)</span></span>

<span data-ttu-id="21dde-325">A pergunta anterior pressupõe implicitamente que o modificador de `sealed` possa ser aplicado a um `override` em uma interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="21dde-326">Isso contradiz a especificação de rascunho.</span><span class="sxs-lookup"><span data-stu-id="21dde-326">This contradicts the draft specification.</span></span> <span data-ttu-id="21dde-327">Queremos permitir o lacre de uma substituição?</span><span class="sxs-lookup"><span data-stu-id="21dde-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="21dde-328">Devem ser considerados efeitos de compatibilidade de origem e binário de lacre.</span><span class="sxs-lookup"><span data-stu-id="21dde-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="21dde-329">***Problema fechado:*** Devemos permitir lacrar uma substituição?</span><span class="sxs-lookup"><span data-stu-id="21dde-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="21dde-330">***Decisão:*** (2017-04-18) não é permitido `sealed` em substituições em interfaces.</span><span class="sxs-lookup"><span data-stu-id="21dde-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="21dde-331">O único uso de `sealed` em membros de interface é torná-los não virtuais em sua declaração inicial.</span><span class="sxs-lookup"><span data-stu-id="21dde-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="21dde-332">Herança de diamante e classes (fechadas)</span><span class="sxs-lookup"><span data-stu-id="21dde-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="21dde-333">O rascunho da proposta prefere substituições de classe a substituições de interface em cenários de herança em losango:</span><span class="sxs-lookup"><span data-stu-id="21dde-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="21dde-334">Exigimos que cada interface e classe tenham uma *substituição mais específica* para cada método de interface entre as substituições que aparecem no tipo ou suas interfaces diretas e indiretas.</span><span class="sxs-lookup"><span data-stu-id="21dde-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="21dde-335">A *substituição mais específica* é uma substituição exclusiva que é mais específica do que todas as outras substituições.</span><span class="sxs-lookup"><span data-stu-id="21dde-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="21dde-336">Se não houver nenhuma substituição, o próprio método será considerado a substituição mais específica.</span><span class="sxs-lookup"><span data-stu-id="21dde-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="21dde-337">Uma substituição `M1` é considerada *mais específica* do que outra `M2` de substituição se `M1` é declarado no tipo `T1`, `M2` é declarado no tipo `T2`e qualquer um</span><span class="sxs-lookup"><span data-stu-id="21dde-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="21dde-338">`T1` contém `T2` entre suas interfaces diretas ou indiretas, ou</span><span class="sxs-lookup"><span data-stu-id="21dde-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="21dde-339">`T2` é um tipo de interface, mas `T1` não é um tipo de interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="21dde-340">O cenário é este</span><span class="sxs-lookup"><span data-stu-id="21dde-340">The scenario is this</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

<span data-ttu-id="21dde-341">Devemos confirmar esse comportamento (ou decidir de outra forma)</span><span class="sxs-lookup"><span data-stu-id="21dde-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="21dde-342">***Problema fechado:*** Confirme a especificação de rascunho, acima, para a *substituição mais específica* , pois ela se aplica a classes e interfaces mistas (uma classe tem prioridade sobre uma interface).</span><span class="sxs-lookup"><span data-stu-id="21dde-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="21dde-343">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span><span class="sxs-lookup"><span data-stu-id="21dde-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="21dde-344">Métodos de interface vs structs (fechados)</span><span class="sxs-lookup"><span data-stu-id="21dde-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="21dde-345">Há algumas interações de infeliz entre as estruturas e os métodos de interface padrão.</span><span class="sxs-lookup"><span data-stu-id="21dde-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="21dde-346">Observe que os membros da interface não são herdados:</span><span class="sxs-lookup"><span data-stu-id="21dde-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="21dde-347">Consequentemente, o cliente deve caixar a estrutura para invocar métodos de interface</span><span class="sxs-lookup"><span data-stu-id="21dde-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="21dde-348">A Boxing dessa forma derrota os principais benefícios de um tipo de `struct`.</span><span class="sxs-lookup"><span data-stu-id="21dde-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="21dde-349">Além disso, qualquer método de mutação não terá efeito aparente, pois eles estão operando em uma *cópia em caixa* da estrutura:</span><span class="sxs-lookup"><span data-stu-id="21dde-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> <span data-ttu-id="21dde-350">***Problema fechado:*** O que podemos fazer a respeito:</span><span class="sxs-lookup"><span data-stu-id="21dde-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="21dde-351">Proíba uma `struct` de herdar uma implementação padrão.</span><span class="sxs-lookup"><span data-stu-id="21dde-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="21dde-352">Todos os métodos de interface seriam tratados como abstratos em um `struct`.</span><span class="sxs-lookup"><span data-stu-id="21dde-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="21dde-353">Em seguida, poderemos levar tempo mais tarde para decidir como fazê-lo funcionar melhor.</span><span class="sxs-lookup"><span data-stu-id="21dde-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="21dde-354">Surgirão com algum tipo de estratégia de geração de código que evita boxing.</span><span class="sxs-lookup"><span data-stu-id="21dde-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="21dde-355">Dentro de um método como `IB.Increment`, o tipo de `this` talvez seria semelhante a um parâmetro de tipo restrito a `IB`.</span><span class="sxs-lookup"><span data-stu-id="21dde-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="21dde-356">Em conjunto com isso, para evitar boxing no chamador, os métodos não abstratos seriam herdados de interfaces.</span><span class="sxs-lookup"><span data-stu-id="21dde-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="21dde-357">Isso pode aumentar o trabalho de implementação do compilador e do CLR.</span><span class="sxs-lookup"><span data-stu-id="21dde-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="21dde-358">Não se preocupe e simplesmente deixe-o como um imperfeição.</span><span class="sxs-lookup"><span data-stu-id="21dde-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="21dde-359">Outras ideias?</span><span class="sxs-lookup"><span data-stu-id="21dde-359">Other ideas?</span></span>

<span data-ttu-id="21dde-360">***Decisão:*** Não se preocupe e simplesmente deixe-o como um imperfeição.</span><span class="sxs-lookup"><span data-stu-id="21dde-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="21dde-361">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span><span class="sxs-lookup"><span data-stu-id="21dde-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="21dde-362">Invocações de interface base (fechadas)</span><span class="sxs-lookup"><span data-stu-id="21dde-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="21dde-363">A especificação de rascunho sugere uma sintaxe para invocações de interface base inspiradas por Java: `Interface.base.M()`.</span><span class="sxs-lookup"><span data-stu-id="21dde-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="21dde-364">Precisamos selecionar uma sintaxe, pelo menos para o protótipo inicial.</span><span class="sxs-lookup"><span data-stu-id="21dde-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="21dde-365">Meu favorito é `base<Interface>.M()`.</span><span class="sxs-lookup"><span data-stu-id="21dde-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="21dde-366">***Problema fechado:*** Qual é a sintaxe para uma invocação de membro base?</span><span class="sxs-lookup"><span data-stu-id="21dde-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="21dde-367">***Decisão:*** A sintaxe é `base(Interface).M()`.</span><span class="sxs-lookup"><span data-stu-id="21dde-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="21dde-368">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span><span class="sxs-lookup"><span data-stu-id="21dde-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="21dde-369">A interface, portanto, denominada deve ser uma interface base, mas não precisa ser uma interface base direta.</span><span class="sxs-lookup"><span data-stu-id="21dde-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="21dde-370">***Abrir problema:*** As invocações de interface base devem ser permitidas em membros de classe?</span><span class="sxs-lookup"><span data-stu-id="21dde-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="21dde-371">***Decisão***: Sim.</span><span class="sxs-lookup"><span data-stu-id="21dde-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="21dde-372">Substituindo membros de interface não-pública (fechado)</span><span class="sxs-lookup"><span data-stu-id="21dde-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="21dde-373">Em uma interface, membros não públicos de interfaces base são substituídos usando o modificador de `override`.</span><span class="sxs-lookup"><span data-stu-id="21dde-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="21dde-374">Se for uma substituição "explícita" que nomeia a interface que contém o membro, o modificador de acesso será omitido.</span><span class="sxs-lookup"><span data-stu-id="21dde-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="21dde-375">***Problema fechado:*** Se for uma substituição "implícita" que não nomeie a interface, o modificador de acesso precisará corresponder?</span><span class="sxs-lookup"><span data-stu-id="21dde-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="21dde-376">***Decisão:*** Somente membros públicos podem ser substituídos implicitamente e o acesso deve corresponder.</span><span class="sxs-lookup"><span data-stu-id="21dde-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="21dde-377">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="21dde-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="21dde-378">***Abrir problema:*** O modificador de acesso é necessário, opcional ou omitido em uma substituição explícita, como `override void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="21dde-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="21dde-379">***Abrir problema:*** É `override` necessário, opcional ou omitido em uma substituição explícita, como `void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="21dde-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="21dde-380">Como uma implementação de um membro de interface não-pública em uma classe?</span><span class="sxs-lookup"><span data-stu-id="21dde-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="21dde-381">Talvez ele deva ser feito explicitamente?</span><span class="sxs-lookup"><span data-stu-id="21dde-381">Perhaps it must be done explicitly?</span></span>

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> <span data-ttu-id="21dde-382">***Problema fechado:*** Como uma implementação de um membro de interface não-pública em uma classe?</span><span class="sxs-lookup"><span data-stu-id="21dde-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="21dde-383">***Decisão:*** Você só pode implementar membros de interface não pública explicitamente.</span><span class="sxs-lookup"><span data-stu-id="21dde-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="21dde-384">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="21dde-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="21dde-385">***Decisão***: nenhuma palavra-chave `override` permitida em membros de interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="21dde-386">Compatibilidade binária 2 (fechado)</span><span class="sxs-lookup"><span data-stu-id="21dde-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="21dde-387">Considere o seguinte código no qual cada tipo está em um assembly separado</span><span class="sxs-lookup"><span data-stu-id="21dde-387">Consider the following code in which each type is in a separate assembly</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

<span data-ttu-id="21dde-388">Entendemos que a implementação de `I1.M` no `C` é `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="21dde-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="21dde-389">E se o assembly que contém `I3` for alterado da seguinte maneira e recompilada</span><span class="sxs-lookup"><span data-stu-id="21dde-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="21dde-390">Mas `C` não é recompilado.</span><span class="sxs-lookup"><span data-stu-id="21dde-390">but `C` is not recompiled.</span></span> <span data-ttu-id="21dde-391">O que acontece quando o programa é executado?</span><span class="sxs-lookup"><span data-stu-id="21dde-391">What happens when the program is run?</span></span> <span data-ttu-id="21dde-392">Uma invocação de `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="21dde-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="21dde-393">Executa `I1.M`</span><span class="sxs-lookup"><span data-stu-id="21dde-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="21dde-394">Executa `I2.M`</span><span class="sxs-lookup"><span data-stu-id="21dde-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="21dde-395">Executa `I3.M`</span><span class="sxs-lookup"><span data-stu-id="21dde-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="21dde-396">2 ou 3, de forma determinista</span><span class="sxs-lookup"><span data-stu-id="21dde-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="21dde-397">Gera algum tipo de exceção de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="21dde-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="21dde-398">***Decisão***: lançar uma exceção (5).</span><span class="sxs-lookup"><span data-stu-id="21dde-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="21dde-399">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span><span class="sxs-lookup"><span data-stu-id="21dde-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="21dde-400">Permitir `partial` na interface?</span><span class="sxs-lookup"><span data-stu-id="21dde-400">Permit `partial` in interface?</span></span> <span data-ttu-id="21dde-401">Legenda</span><span class="sxs-lookup"><span data-stu-id="21dde-401">(closed)</span></span>

<span data-ttu-id="21dde-402">Considerando que as interfaces podem ser usadas de maneiras análogas à forma como as classes abstratas são usadas, pode ser útil declará-las `partial`.</span><span class="sxs-lookup"><span data-stu-id="21dde-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="21dde-403">Isso seria particularmente útil na face de geradores.</span><span class="sxs-lookup"><span data-stu-id="21dde-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="21dde-404">***Proposta:*** Remova a restrição de idioma que as interfaces e os membros das interfaces não podem ser declarados `partial`.</span><span class="sxs-lookup"><span data-stu-id="21dde-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="21dde-405">***Decisão***: Sim.</span><span class="sxs-lookup"><span data-stu-id="21dde-405">***Decision***: Yes.</span></span> <span data-ttu-id="21dde-406">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span><span class="sxs-lookup"><span data-stu-id="21dde-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="21dde-407">`Main` em uma interface?</span><span class="sxs-lookup"><span data-stu-id="21dde-407">`Main` in an interface?</span></span> <span data-ttu-id="21dde-408">Legenda</span><span class="sxs-lookup"><span data-stu-id="21dde-408">(closed)</span></span>

> <span data-ttu-id="21dde-409">***Abrir problema:*** Um método `static Main` em uma interface que é candidato a ser o ponto de entrada do programa?</span><span class="sxs-lookup"><span data-stu-id="21dde-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="21dde-410">***Decisão***: Sim.</span><span class="sxs-lookup"><span data-stu-id="21dde-410">***Decision***: Yes.</span></span> <span data-ttu-id="21dde-411">Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span><span class="sxs-lookup"><span data-stu-id="21dde-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="21dde-412">Confirmar a intenção de dar suporte a métodos não virtuais públicos (fechados)</span><span class="sxs-lookup"><span data-stu-id="21dde-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="21dde-413">Podemos confirmar (ou reverter) nossa decisão de permitir métodos públicos não virtuais em uma interface?</span><span class="sxs-lookup"><span data-stu-id="21dde-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="21dde-414">***Problema semifechado:*** (2017-04-18) achamos que ele será útil, mas voltará a ele.</span><span class="sxs-lookup"><span data-stu-id="21dde-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="21dde-415">Esse é um bloco mental de blocos de modelo.</span><span class="sxs-lookup"><span data-stu-id="21dde-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="21dde-416">***Decisão***: Sim.</span><span class="sxs-lookup"><span data-stu-id="21dde-416">***Decision***: Yes.</span></span> <span data-ttu-id="21dde-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods></span><span class="sxs-lookup"><span data-stu-id="21dde-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="21dde-418">Um `override` em uma interface introduz um novo membro?</span><span class="sxs-lookup"><span data-stu-id="21dde-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="21dde-419">Legenda</span><span class="sxs-lookup"><span data-stu-id="21dde-419">(closed)</span></span>

<span data-ttu-id="21dde-420">Há algumas maneiras de observar se uma declaração de substituição introduz um novo membro ou não.</span><span class="sxs-lookup"><span data-stu-id="21dde-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> <span data-ttu-id="21dde-421">***Abrir problema:*** Uma declaração de substituição em uma interface introduz um novo membro?</span><span class="sxs-lookup"><span data-stu-id="21dde-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="21dde-422">Legenda</span><span class="sxs-lookup"><span data-stu-id="21dde-422">(closed)</span></span>

<span data-ttu-id="21dde-423">Em uma classe, um método de substituição é "visível" em alguns sentidos.</span><span class="sxs-lookup"><span data-stu-id="21dde-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="21dde-424">Por exemplo, os nomes de seus parâmetros têm precedência sobre os nomes dos parâmetros no método substituído.</span><span class="sxs-lookup"><span data-stu-id="21dde-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="21dde-425">Pode ser possível duplicar esse comportamento em interfaces, pois sempre há uma substituição mais específica.</span><span class="sxs-lookup"><span data-stu-id="21dde-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="21dde-426">Mas queremos duplicar esse comportamento?</span><span class="sxs-lookup"><span data-stu-id="21dde-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="21dde-427">Além disso, é possível "substituir" um método de substituição?</span><span class="sxs-lookup"><span data-stu-id="21dde-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="21dde-428">Sentido</span><span class="sxs-lookup"><span data-stu-id="21dde-428">[Moot]</span></span>

<span data-ttu-id="21dde-429">***Decisão***: nenhuma palavra-chave `override` permitida em membros de interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="21dde-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member></span><span class="sxs-lookup"><span data-stu-id="21dde-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="21dde-431">Propriedades com um acessador privado (fechado)</span><span class="sxs-lookup"><span data-stu-id="21dde-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="21dde-432">Dizemos que os membros privados não são virtuais e a combinação de virtual e privada não é permitida.</span><span class="sxs-lookup"><span data-stu-id="21dde-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="21dde-433">Mas e quanto a uma propriedade com um acessador particular?</span><span class="sxs-lookup"><span data-stu-id="21dde-433">But what about a property with a private accessor?</span></span>

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

<span data-ttu-id="21dde-434">Isso é permitido?</span><span class="sxs-lookup"><span data-stu-id="21dde-434">Is this allowed?</span></span> <span data-ttu-id="21dde-435">O acessador de `set` é `virtual` ou não?</span><span class="sxs-lookup"><span data-stu-id="21dde-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="21dde-436">Ele pode ser substituído onde estiver acessível?</span><span class="sxs-lookup"><span data-stu-id="21dde-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="21dde-437">O seguinte implementa implicitamente o acessador `get`?</span><span class="sxs-lookup"><span data-stu-id="21dde-437">Does the following implicitly implement only the `get` accessor?</span></span>

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="21dde-438">O seguinte é supostamente um erro porque IA. P. set não é virtual e também porque não está acessível?</span><span class="sxs-lookup"><span data-stu-id="21dde-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="21dde-439">***Decisão***: o primeiro exemplo é válido, enquanto o último não.</span><span class="sxs-lookup"><span data-stu-id="21dde-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="21dde-440">Isso é resolvido de forma análoga em como ele já funciona C#no.</span><span class="sxs-lookup"><span data-stu-id="21dde-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="21dde-441">Invocações de interface base, redondo 2 (fechado)</span><span class="sxs-lookup"><span data-stu-id="21dde-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="21dde-442">Nossa "resolução" anterior de como lidar com invocações de base não oferece, na verdade, uma expressividade suficiente.</span><span class="sxs-lookup"><span data-stu-id="21dde-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="21dde-443">Acontece que, no C# e no CLR, ao contrário do Java, você precisa especificar a interface que contém a declaração do método e o local da implementação que você deseja invocar.</span><span class="sxs-lookup"><span data-stu-id="21dde-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="21dde-444">Eu propondo a sintaxe a seguir para chamadas base em interfaces.</span><span class="sxs-lookup"><span data-stu-id="21dde-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="21dde-445">Não estou de amor, mas ilustra o que qualquer sintaxe deve ser capaz de expressar:</span><span class="sxs-lookup"><span data-stu-id="21dde-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

<span data-ttu-id="21dde-446">Se não houver nenhuma ambiguidade, você poderá escrevê-la mais simplesmente</span><span class="sxs-lookup"><span data-stu-id="21dde-446">If there is no ambiguity, you can write it more simply</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

<span data-ttu-id="21dde-447">Ou</span><span class="sxs-lookup"><span data-stu-id="21dde-447">Or</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

<span data-ttu-id="21dde-448">Ou</span><span class="sxs-lookup"><span data-stu-id="21dde-448">Or</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

<span data-ttu-id="21dde-449">***Decisão***: decidida em `base(N.I1<T>).M(s)`, convinculando que, se tivermos uma associação de invocação, pode haver um problema aqui mais tarde.</span><span class="sxs-lookup"><span data-stu-id="21dde-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="21dde-450">Aviso para struct não implementar o método padrão?</span><span class="sxs-lookup"><span data-stu-id="21dde-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="21dde-451">Legenda</span><span class="sxs-lookup"><span data-stu-id="21dde-451">(closed)</span></span>

<span data-ttu-id="21dde-452">@vancem afirma que devemos considerar seriamente a criação de um aviso se uma declaração de tipo de valor não substituir algum método de interface, mesmo que herde uma implementação desse método de uma interface.</span><span class="sxs-lookup"><span data-stu-id="21dde-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="21dde-453">Porque faz com que a boxing e a subminam chamadas restritas.</span><span class="sxs-lookup"><span data-stu-id="21dde-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="21dde-454">***Decisão***: isso parece algo mais adequado para um analisador.</span><span class="sxs-lookup"><span data-stu-id="21dde-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="21dde-455">Também parece que esse aviso pode ser barulhento, pois ele será disparado mesmo se o método de interface padrão nunca for chamado e nenhuma Boxing ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="21dde-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="21dde-456">Construtores estáticos de interface (fechados)</span><span class="sxs-lookup"><span data-stu-id="21dde-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="21dde-457">Quando os construtores estáticos da interface são executados?</span><span class="sxs-lookup"><span data-stu-id="21dde-457">When are interface static constructors run?</span></span>  <span data-ttu-id="21dde-458">O rascunho da CLI atual propõe que ele ocorre quando o primeiro método ou campo estático é acessado.</span><span class="sxs-lookup"><span data-stu-id="21dde-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="21dde-459">Se não houver nenhuma delas, ela pode nunca ser executada?</span><span class="sxs-lookup"><span data-stu-id="21dde-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="21dde-460">[2018-10-09 a equipe do CLR propõe "vai espelhar o que fazemos para valuetypes (verificação de cctor no acesso a cada método de instância)"]</span><span class="sxs-lookup"><span data-stu-id="21dde-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="21dde-461">***Decisão***: construtores estáticos também são executados na entrada para métodos de instância, se o construtor estático não tiver sido `beforefieldinit`, caso em que construtores estáticos são executados antes do acesso ao primeiro campo estático.</span><span class="sxs-lookup"><span data-stu-id="21dde-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="21dde-462">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="21dde-462">Design meetings</span></span>

<span data-ttu-id="21dde-463">[2017-03-08 notas de reunião do ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 as notas de reunião do LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 Meeting "comportamento CLR para métodos de Interface padrão"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05
LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md) notas de reunião
2017-04-11 as notas de reunião do LDM [2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
2017-04-18 as notas de [reunião](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) do LDM
2017-04-19 [2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) [2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) [2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) [LDM Notas de reunião](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 de reuniões LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[notas de reunião do LDM 2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)

</span><span class="sxs-lookup"><span data-stu-id="21dde-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
