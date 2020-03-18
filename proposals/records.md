---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484940"
---
# <a name="records"></a><span data-ttu-id="ec1c9-101">registros</span><span class="sxs-lookup"><span data-stu-id="ec1c9-101">records</span></span>

* <span data-ttu-id="ec1c9-102">[x] proposta</span><span class="sxs-lookup"><span data-stu-id="ec1c9-102">[x] Proposed</span></span>
* <span data-ttu-id="ec1c9-103">[] Protótipo: [concluído](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="ec1c9-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="ec1c9-104">[] Implementação: [em andamento](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="ec1c9-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="ec1c9-105">[] Especificação: especificação de rascunho incluída</span><span class="sxs-lookup"><span data-stu-id="ec1c9-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="ec1c9-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="ec1c9-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ec1c9-107">Os registros são um formulário de declaração novo e C# simplificado para tipos de classe e struct que combinam os benefícios de uma série de recursos mais simples.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="ec1c9-108">Descrevemos os novos recursos (parâmetros do receptor do chamador e *com-Expression*s), fornecemos a sintaxe e a semântica para declarações de registro e, em seguida, fornecem alguns exemplos.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="ec1c9-109">Motivação</span><span class="sxs-lookup"><span data-stu-id="ec1c9-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ec1c9-110">Um número significativo de declarações de tipo C# no é muito mais do que as coleções agregadas de dados digitados.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="ec1c9-111">Infelizmente, a declaração desses tipos requer uma grande quantidade de código clichê.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="ec1c9-112">Os *registros* fornecem um mecanismo para declarar um tipo de dados, descrevendo os membros da agregação junto com código ou desvios adicionais do texto clichê comum, se houver.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="ec1c9-113">Consulte os [exemplos](#examples)abaixo.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ec1c9-114">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="ec1c9-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="ec1c9-115">chamador-parâmetros do receptor</span><span class="sxs-lookup"><span data-stu-id="ec1c9-115">caller-receiver parameters</span></span>

<span data-ttu-id="ec1c9-116">No momento *, o argumento padrão* de um parâmetro de método deve ser</span><span class="sxs-lookup"><span data-stu-id="ec1c9-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="ec1c9-117">uma *expressão de constante*; or</span><span class="sxs-lookup"><span data-stu-id="ec1c9-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="ec1c9-118">uma expressão do formulário `new S()` onde `S` é um tipo de valor; or</span><span class="sxs-lookup"><span data-stu-id="ec1c9-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="ec1c9-119">uma expressão do formulário `default(S)` onde `S` é um tipo de valor</span><span class="sxs-lookup"><span data-stu-id="ec1c9-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="ec1c9-120">Estendemos isso para adicionar o seguinte</span><span class="sxs-lookup"><span data-stu-id="ec1c9-120">We extend this to add the following</span></span>
- <span data-ttu-id="ec1c9-121">uma expressão do formulário `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="ec1c9-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="ec1c9-122">Esse novo formulário é chamado de *argumento-padrão do receptor do chamador*e só é permitido se todos os itens a seguir forem atendidos</span><span class="sxs-lookup"><span data-stu-id="ec1c9-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="ec1c9-123">O método no qual ele aparece é um método de instância; e</span><span class="sxs-lookup"><span data-stu-id="ec1c9-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="ec1c9-124">A expressão `this.Identifier` é associada a um membro de instância do tipo delimitador, que deve ser um campo ou uma propriedade; e</span><span class="sxs-lookup"><span data-stu-id="ec1c9-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="ec1c9-125">O membro ao qual ele é associado (e o acessador de `get`, se for uma propriedade) é pelo menos tão acessível quanto o método; e</span><span class="sxs-lookup"><span data-stu-id="ec1c9-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="ec1c9-126">O tipo de `this.Identifier` é implicitamente conversível por uma identidade ou conversão anulável para o tipo do parâmetro (essa é uma restrição existente no *argumento padrão*).</span><span class="sxs-lookup"><span data-stu-id="ec1c9-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="ec1c9-127">Quando um argumento é omitido de uma invocação de um membro de função para um parâmetro opcional correspondente com um *argumento-padrão de receptor de chamador*, o valor do membro do destinatário é passado implicitamente.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="ec1c9-128">**Observações de design**: o principal motivo para o parâmetro do receptor do chamador é dar suporte a *com a expressão with*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="ec1c9-129">A ideia é que você pode declarar um método como este</span><span class="sxs-lookup"><span data-stu-id="ec1c9-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="ec1c9-130">e, em seguida, usá-lo como este</span><span class="sxs-lookup"><span data-stu-id="ec1c9-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="ec1c9-131">Para criar um novo `Point` assim como um `Point` existente, mas com o valor de `X` alterado.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="ec1c9-132">É uma pergunta aberta se a forma sintática da *expressão with* deve ser adicionada assim que tivermos suporte para parâmetros do receptor do chamador, portanto, é possível que possamos fazer isso em *vez de* *em* vez de *com a expressão with*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="ec1c9-133">[] **Abrir problema**: Qual é a ordem na qual um *argumento-padrão do receptor do chamador* é avaliado com relação a outros argumentos?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="ec1c9-134">Devemos dizer que ele não está especificado?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="ec1c9-135">com expressões</span><span class="sxs-lookup"><span data-stu-id="ec1c9-135">with-expressions</span></span>

<span data-ttu-id="ec1c9-136">Um novo formulário de expressão é proposto:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="ec1c9-137">O token `with` é uma nova palavra-chave sensível ao contexto.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="ec1c9-138">Cada *identificador* à esquerda de um *with_initializer* deve ser associado a um campo de instância acessível ou à propriedade do tipo do *primary_expression* do *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="ec1c9-139">Pode não haver nenhum nome duplicado entre esses identificadores de um determinado *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="ec1c9-140">Uma *with_expression* do formulário</span><span class="sxs-lookup"><span data-stu-id="ec1c9-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="ec1c9-141">*identificador* de `{` de `with` *E1* = *e2*,... `}`</span><span class="sxs-lookup"><span data-stu-id="ec1c9-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="ec1c9-142">é tratado como uma invocação do formulário</span><span class="sxs-lookup"><span data-stu-id="ec1c9-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="ec1c9-143">*e1*`.With(`*identifier2*`:` *e2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="ec1c9-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="ec1c9-144">Em que, para cada método chamado `With` que é um membro de instância acessível de *E1*, selecionamos *identifier2* como o nome do primeiro parâmetro nesse método que tem um parâmetro receptor de chamador que é o mesmo membro que o campo de instância ou a propriedade associada ao *identificador*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="ec1c9-145">Se esse parâmetro não puder ser identificado, o método será eliminado da consideração.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="ec1c9-146">O método a ser invocado é selecionado entre os candidatos restantes pela resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="ec1c9-147">**Observações de design**: os parâmetros do receptor de chamador fornecidos, muitos dos benefícios da *expressão with* estão disponíveis sem esse formulário de sintaxe especial.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="ec1c9-148">Portanto, estamos considerando se é necessário ou não.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="ec1c9-149">Seu principal benefício é permitir que um programe em termos de nomes de campos e propriedades, e não em termos de nomes de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="ec1c9-150">Dessa forma, melhoramos a legibilidade e a qualidade das ferramentas (por exemplo, ir para a definição no identificador de um *with_expression* navegar para a propriedade em vez de um parâmetro de método).</span><span class="sxs-lookup"><span data-stu-id="ec1c9-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="ec1c9-151">[] **Abrir problema**: esta descrição deve ser modificada para dar suporte a métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="ec1c9-152">correspondência padrão</span><span class="sxs-lookup"><span data-stu-id="ec1c9-152">pattern-matching</span></span>

<span data-ttu-id="ec1c9-153">Consulte a [especificação de correspondência de padrões](csharp-8.0/patterns.md#positional-pattern) para uma especificação de `Deconstruct` e sua relação com a correspondência de padrões.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="ec1c9-154">**Observações de design**: em virtude do `Deconstruct` gerado pelo compilador conforme especificado aqui, e a especificação para correspondência de padrões, uma declaração de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="ec1c9-155">oferecerá suporte à correspondência de padrão posicional da seguinte maneira</span><span class="sxs-lookup"><span data-stu-id="ec1c9-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="ec1c9-156">declarações de tipo de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-156">record type declarations</span></span>

<span data-ttu-id="ec1c9-157">A sintaxe para uma declaração de `class` ou `struct` é estendida para dar suporte a parâmetros de valor; os parâmetros tornam-se propriedades do tipo:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="ec1c9-158">**Observações de design**: como os tipos de registro geralmente são úteis sem a necessidade de quaisquer membros explicitamente declarados em um corpo de classe, modificamos a sintaxe da declaração para permitir que um corpo seja simplesmente um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="ec1c9-159">Uma classe (struct) declarada com os *parâmetros de registro* é chamada de *classe de registro* (struct de*registro*), ou seja, um *tipo de registro*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="ec1c9-160">[] **Abrir problema**: precisamos incluir *primary_constructor_body* na gramática para que ela possa aparecer dentro de uma declaração de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="ec1c9-161">[] **Abrir problema**: quais são as regras de conflito de nome para os nomes de parâmetro?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="ec1c9-162">Supostamente, um não tem permissão para entrar em conflito com um parâmetro de tipo ou outro *parâmetro de registro*.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="ec1c9-163">[] **Abrir problema**: precisamos especificar o escopo dos parâmetros de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="ec1c9-164">Onde eles podem ser usados?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-164">Where can they be used?</span></span> <span data-ttu-id="ec1c9-165">Presumivelmente, em inicializadores de campo de instância e *primary_constructor_body* pelo menos.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="ec1c9-166">[] **Abrir problema**: uma declaração de tipo de registro pode ser parcial?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="ec1c9-167">Nesse caso, os parâmetros devem ser repetidos em cada parte?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="ec1c9-168">Membros de um tipo de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-168">Members of a record type</span></span>

<span data-ttu-id="ec1c9-169">Além dos membros declarados no *corpo da classe*, um tipo de registro tem os seguintes membros adicionais:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="ec1c9-170">Construtor principal</span><span class="sxs-lookup"><span data-stu-id="ec1c9-170">Primary Constructor</span></span>

<span data-ttu-id="ec1c9-171">Um tipo de registro tem um Construtor `public` cuja assinatura corresponde aos parâmetros de valor da declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="ec1c9-172">Isso é chamado de *Construtor principal* para o tipo e faz com que o *construtor padrão* declarado implicitamente seja suprimido.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="ec1c9-173">Em tempo de execução, o construtor primário</span><span class="sxs-lookup"><span data-stu-id="ec1c9-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="ec1c9-174">Inicializa os campos de backup gerados pelo compilador para as propriedades correspondentes aos parâmetros de valor (se essas propriedades forem fornecidas pelo compilador; [consulte 1.1.2](#1.1.2)); Clique</span><span class="sxs-lookup"><span data-stu-id="ec1c9-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="ec1c9-175">executa os inicializadores de campo de instância que aparecem no *corpo da classe*; e, em seguida,</span><span class="sxs-lookup"><span data-stu-id="ec1c9-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="ec1c9-176">invoca um construtor de classe base:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="ec1c9-177">Se houver argumentos na *record_base_arguments*, um construtor base selecionado pela resolução de sobrecarga com esses argumentos será invocado;</span><span class="sxs-lookup"><span data-stu-id="ec1c9-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="ec1c9-178">Caso contrário, um construtor base será invocado sem argumentos.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="ec1c9-179">executa o corpo de cada *primary_constructor_body*, se houver, na ordem de origem.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="ec1c9-180">[] **Abrir problema**: precisamos especificar essa ordem, particularmente em unidades de compilação para parciais.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="ec1c9-181">[] **Abrir problema**: precisamos especificar que cada Construtor declarado explicitamente deve encadear o construtor primário.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="ec1c9-182">[] **Abrir problema**: deve ter permissão para alterar o modificador de acesso no construtor primário?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="ec1c9-183">[] **Abrir problema**: em um struct de registro, é um erro para não haver nenhum parâmetro de registro?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="ec1c9-184">Corpo do construtor primário</span><span class="sxs-lookup"><span data-stu-id="ec1c9-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="ec1c9-185">Uma *primary_constructor_body* só pode ser usada dentro de uma declaração de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="ec1c9-186">O *identificador* de um *primary_constructor_body* deve nomear o tipo de registro no qual ele é declarado.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="ec1c9-187">O *primary_constructor_body* não declara um membro por conta própria, mas é uma maneira para o programador fornecer atributos para e especificar o acesso de, um construtor principal de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="ec1c9-188">Ele também permite que o programador forneça código adicional que será executado quando uma instância do tipo de registro for construída.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="ec1c9-189">[] **Abrir problema**: devemos observar que um construtor padrão de struct ignora isso.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="ec1c9-190">[] **Abrir problema**: devemos especificar a ordem de execução da inicialização.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="ec1c9-191">[] **Abrir problema**: devemos permitir algo como um *primary_constructor_body* (presumivelmente sem atributos e modificadores) em uma declaração de tipo sem registro e tratá-lo como seria o código de um inicializador de campo de instância?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="ec1c9-192">{1&gt;Propriedades&lt;1}</span><span class="sxs-lookup"><span data-stu-id="ec1c9-192">Properties</span></span>

<span data-ttu-id="ec1c9-193">Para cada parâmetro de registro de uma declaração de tipo de registro, há um membro de propriedade de `public` correspondente cujo nome e tipo são tirados da declaração de parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="ec1c9-194">Seu nome é o *identificador* do *record_property_name*, se presente, ou o *identificador* do *record_parameter* caso contrário.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="ec1c9-195">Se nenhuma propriedade pública concreta (ou seja, não abstrata) com um acessador `get` e com esse nome e tipo for explicitamente declarada ou herdada, ela será produzida pelo compilador da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="ec1c9-196">Para uma *estrutura de registro* ou uma *classe de registro*de `sealed`:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="ec1c9-197">Um campo de `readonly` `private` é produzido como um campo de apoio para uma propriedade `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="ec1c9-198">Seu valor é inicializado durante a construção com o valor do parâmetro de construtor primário correspondente.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="ec1c9-199">O acessador de `get` da propriedade é implementado para retornar o valor do campo de backup.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="ec1c9-200">Cada acessador de `get` de propriedade virtual herdada "correspondente" é substituído.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="ec1c9-201">**Observações de design**: em outras palavras, se você estender uma classe base ou implementar uma interface que declare uma propriedade abstrata pública com o mesmo nome e tipo como um parâmetro de registro, essa propriedade será substituída ou implementada.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="ec1c9-202">[] **Abrir problema**: deve ser possível alterar o modificador de acesso em uma propriedade quando declarado explicitamente?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="ec1c9-203">[] **Abrir problema**: deve ser possível substituir um campo por uma propriedade?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="ec1c9-204">Métodos de objeto</span><span class="sxs-lookup"><span data-stu-id="ec1c9-204">Object Methods</span></span>

<span data-ttu-id="ec1c9-205">Para uma *estrutura de registro* ou uma `sealed` *classe de registro*, as implementações dos métodos `object.GetHashCode()` e `object.Equals(object)` são produzidas pelo compilador, a menos que sejam fornecidas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="ec1c9-206">[] **Abrir problema**: devemos especificar precisamente sua implementação.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="ec1c9-207">[] **Abrir problema**: também devemos adicionar a interface `IEquatable<T>` para o tipo de registro e especificar que as implementações são fornecidas.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="ec1c9-208">[] **Abrir problema**: também devemos especificar que implementamos cada `IEquatable<T>.Equals`.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="ec1c9-209">[] **Abrir problema**: devemos especificar precisamente como resolvemos o problema de igual à herança de registro: especificamente, como geramos métodos de igualdade, de forma que eles sejam simétricos, transitivos, reflexivo, etc.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="ec1c9-210">[] **Abrir problema**: foi proposto que implementamos `operator ==` e `operator !=` para tipos de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="ec1c9-211">[] **Abrir problema**: devemos gerar automaticamente uma implementação de `object.ToString`?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="ec1c9-212">Um tipo de registro tem um método de `public` gerado pelo compilador `void Deconstruct`, a menos que um com qualquer assinatura seja fornecido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="ec1c9-213">Cada parâmetro é um parâmetro `out` de mesmo nome e tipo que o parâmetro correspondente do tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="ec1c9-214">A implementação fornecida pelo compilador desse método deve atribuir cada parâmetro de `out` com o valor da propriedade correspondente.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="ec1c9-215">Consulte [a especificação de correspondência de padrões](csharp-8.0/patterns.md#positional-pattern) para a semântica de `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="ec1c9-216">Método `With`</span><span class="sxs-lookup"><span data-stu-id="ec1c9-216">`With` method</span></span>

<span data-ttu-id="ec1c9-217">A menos que haja um membro declarado pelo usuário chamado `With` declarado, um tipo de registro tem um método fornecido pelo compilador chamado `With` cujo tipo de retorno é o próprio tipo de registro e que contém um parâmetro de valor correspondente a cada *parâmetro de registro* na mesma ordem em que esses parâmetros aparecem na declaração de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="ec1c9-218">Cada parâmetro deve ter um *argumento de emissor-padrão do receptor* da propriedade correspondente.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="ec1c9-219">Em uma classe de registro `abstract`, o método `With` fornecido pelo compilador é abstrato.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="ec1c9-220">Em uma estrutura de registro, ou uma classe de registro lacrado, o método de `With` fornecido pelo compilador é `sealed`.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="ec1c9-221">Caso contrário, o método de `With` fornecido pelo compilador será ' virtual e sua implementação deverá retornar uma nova instância produzida invocando o Construtor principal com os parâmetros como argumentos para criar uma nova instância a partir dos parâmetros e retornar essa nova instância.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="ec1c9-222">[] **Abrir problema**: também devemos especificar em quais condições substituimos ou implementamos métodos de `With` virtual herdados ou métodos de `With` de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="ec1c9-223">[] **Abrir problema**: devemos dizer o que acontece quando herdamos um método de `With` não virtual.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="ec1c9-224">**Observações de design**: como os tipos de registro são, por padrão, imutáveis, o método `With` fornece uma maneira de criar uma nova instância que seja igual a uma instância existente, mas com as propriedades selecionadas, dadas novos valores.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="ec1c9-225">Por exemplo, dada</span><span class="sxs-lookup"><span data-stu-id="ec1c9-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="ec1c9-226">Há um membro fornecido pelo compilador</span><span class="sxs-lookup"><span data-stu-id="ec1c9-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="ec1c9-227">Que habilita uma variável do tipo de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="ec1c9-228">para ser substituído por uma instância que tenha uma ou mais propriedades diferentes</span><span class="sxs-lookup"><span data-stu-id="ec1c9-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="ec1c9-229">Isso também pode ser expresso usando o *with_expression*:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="ec1c9-230">Exemplos</span><span class="sxs-lookup"><span data-stu-id="ec1c9-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="ec1c9-231">Compatibilidade dos tipos de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-231">Compatibility of record types</span></span>

<span data-ttu-id="ec1c9-232">Como o programador pode adicionar membros a uma declaração de tipo de registro, muitas vezes é possível alterar o conjunto de elementos de registro sem afetar os clientes existentes.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="ec1c9-233">Por exemplo, dada uma versão inicial de um tipo de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="ec1c9-234">Um novo elemento do tipo de registro pode ser dos adicionado na próxima revisão do tipo sem afetar a compatibilidade binária ou de origem:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="ec1c9-235">exemplo de struct do registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-235">record struct example</span></span>

<span data-ttu-id="ec1c9-236">Este struct de registro</span><span class="sxs-lookup"><span data-stu-id="ec1c9-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="ec1c9-237">é traduzido para este código</span><span class="sxs-lookup"><span data-stu-id="ec1c9-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="ec1c9-238">[] **Abrir problema**: a implementação de Equals (par outro) é um membro público do par?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="ec1c9-239">[] **Abrir problema**: essa implementação de `Equals` não é simétrica em face de herança.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="ec1c9-240">[] **Abrir problema**: um registro deve declarar `operator ==` e `operator !=`?</span><span class="sxs-lookup"><span data-stu-id="ec1c9-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="ec1c9-241">**Observações de design**: como um tipo de registro pode herdar de outro, e essa implementação de `Equals` não seria simétrica nesse caso, ela não está correta.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="ec1c9-242">Sugerimos implementar igualdade dessa maneira:</span><span class="sxs-lookup"><span data-stu-id="ec1c9-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="ec1c9-243">Os registros derivados `override EqualityContract`.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="ec1c9-244">A alternativa menos atraente é restringir a herança.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="ec1c9-245">exemplo de registro lacrado</span><span class="sxs-lookup"><span data-stu-id="ec1c9-245">sealed record example</span></span>

<span data-ttu-id="ec1c9-246">Esta classe de registro lacrado</span><span class="sxs-lookup"><span data-stu-id="ec1c9-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="ec1c9-247">é traduzido para este código</span><span class="sxs-lookup"><span data-stu-id="ec1c9-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="ec1c9-248">exemplo de classe de registro abstract</span><span class="sxs-lookup"><span data-stu-id="ec1c9-248">abstract record class example</span></span>

<span data-ttu-id="ec1c9-249">Essa classe de registro abstrato</span><span class="sxs-lookup"><span data-stu-id="ec1c9-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="ec1c9-250">é traduzido para este código</span><span class="sxs-lookup"><span data-stu-id="ec1c9-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="ec1c9-251">combinando registros abstratos e lacrados</span><span class="sxs-lookup"><span data-stu-id="ec1c9-251">combining abstract and sealed records</span></span>

<span data-ttu-id="ec1c9-252">Dada a classe de registro abstrato `Person` acima, essa classe de registro lacrado</span><span class="sxs-lookup"><span data-stu-id="ec1c9-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="ec1c9-253">é traduzido para este código</span><span class="sxs-lookup"><span data-stu-id="ec1c9-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="ec1c9-254">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="ec1c9-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ec1c9-255">Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ec1c9-256">Alternativas</span><span class="sxs-lookup"><span data-stu-id="ec1c9-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ec1c9-257">Consideramos a adição de *construtores primários* em C# 6.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="ec1c9-258">Embora eles ocupem a mesma superfície sintática que essa proposta, descobrimos que eles se resumiram das vantagens oferecidas pelos registros.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ec1c9-259">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="ec1c9-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="ec1c9-260">As perguntas abertas são exibidas no corpo da proposta.</span><span class="sxs-lookup"><span data-stu-id="ec1c9-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ec1c9-261">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="ec1c9-261">Design meetings</span></span>

<span data-ttu-id="ec1c9-262">TBD</span><span class="sxs-lookup"><span data-stu-id="ec1c9-262">TBD</span></span>
