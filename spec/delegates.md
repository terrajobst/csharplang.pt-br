---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704094"
---
# <a name="delegates"></a><span data-ttu-id="83d0a-101">Delegados</span><span class="sxs-lookup"><span data-stu-id="83d0a-101">Delegates</span></span>

<span data-ttu-id="83d0a-102">Delegados habilitam cenários que outras linguagens — C++como, Pascal e modula--foram abordadas com ponteiros de função.</span><span class="sxs-lookup"><span data-stu-id="83d0a-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="83d0a-103">No C++ entanto, diferentemente dos ponteiros de função, os delegados C++ são totalmente orientados a objeto e, ao contrário de ponteiros para funções de membro, os delegados encapsulam uma instância de objeto e um método.</span><span class="sxs-lookup"><span data-stu-id="83d0a-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="83d0a-104">Uma declaração delegate define uma classe que é derivada da classe `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="83d0a-105">Uma instância de delegado encapsula uma lista de invocação, que é uma lista de um ou mais métodos, cada um dos quais é conhecido como uma entidade que possa ser chamada.</span><span class="sxs-lookup"><span data-stu-id="83d0a-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="83d0a-106">Para métodos de instância, uma entidade que possa ser chamada consiste em uma instância e um método nessa instância.</span><span class="sxs-lookup"><span data-stu-id="83d0a-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="83d0a-107">Para métodos estáticos, uma entidade que possa ser chamada consiste apenas em um método.</span><span class="sxs-lookup"><span data-stu-id="83d0a-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="83d0a-108">Invocar uma instância de delegado com um conjunto apropriado de argumentos faz com que cada uma das entidades chamáveis do delegado seja invocada com o conjunto de argumentos fornecido.</span><span class="sxs-lookup"><span data-stu-id="83d0a-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="83d0a-109">Uma propriedade interessante e útil de uma instância de delegado é que ela não sabe ou se preocupa com as classes dos métodos que encapsula; Tudo o que importa é que esses métodos sejam compatíveis ([delegar declarações](delegates.md#delegate-declarations)) com o tipo do delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="83d0a-110">Isso torna os delegados perfeitamente adequados para invocação "anônima".</span><span class="sxs-lookup"><span data-stu-id="83d0a-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="83d0a-111">Delegar declarações</span><span class="sxs-lookup"><span data-stu-id="83d0a-111">Delegate declarations</span></span>

<span data-ttu-id="83d0a-112">Um *delegate_declaration* é um *type_declaration* ([declarações de tipo](namespaces.md#type-declarations)) que declara um novo tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="83d0a-113">É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="83d0a-114">O modificador `new` só é permitido em delegados declarados dentro de outro tipo; nesse caso, ele especifica que esse delegado oculta um membro herdado com o mesmo nome, conforme descrito no [novo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="83d0a-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="83d0a-115">Os modificadores `public`, `protected`, `internal` e `private` controlam a acessibilidade do tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="83d0a-116">Dependendo do contexto no qual a declaração delegate ocorre, alguns desses modificadores podem não ser permitidos ([acessibilidade declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="83d0a-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="83d0a-117">O nome do tipo do delegado é *identificador*.</span><span class="sxs-lookup"><span data-stu-id="83d0a-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="83d0a-118">O *formal_parameter_list* opcional especifica os parâmetros do delegado e *return_type* indica o tipo de retorno do delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="83d0a-119">O *variant_type_parameter_list* opcional ([listas de parâmetros de tipo de variante](interfaces.md#variant-type-parameter-lists)) especifica os parâmetros de tipo para o próprio delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="83d0a-120">O tipo de retorno de um tipo delegado deve ser `void` ou de saída (segurança de[variação](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="83d0a-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="83d0a-121">Todos os tipos de parâmetro formais de um tipo delegado devem ser de entrada segura.</span><span class="sxs-lookup"><span data-stu-id="83d0a-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="83d0a-122">Além disso, qualquer tipo de parâmetro `out` ou `ref` também deve ser de saída segura.</span><span class="sxs-lookup"><span data-stu-id="83d0a-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="83d0a-123">Observe que até mesmo os parâmetros `out` devem ser seguros de entrada, devido a uma limitação da plataforma de execução subjacente.</span><span class="sxs-lookup"><span data-stu-id="83d0a-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="83d0a-124">Tipos delegados em C# são equivalentes a nome, não é estruturalmente equivalente.</span><span class="sxs-lookup"><span data-stu-id="83d0a-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="83d0a-125">Especificamente, dois tipos delegados diferentes que têm as mesmas listas de parâmetros e tipos de retorno são considerados tipos delegados diferentes.</span><span class="sxs-lookup"><span data-stu-id="83d0a-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="83d0a-126">No entanto, as instâncias de dois tipos delegados distintos, mas estruturalmente equivalentes podem ser comparadas como iguais ([operadores de igualdade de delegado](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="83d0a-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="83d0a-127">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="83d0a-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="83d0a-128">Os métodos `A.M1` e `B.M1` são compatíveis com os tipos delegados `D1` e `D2`, pois eles têm o mesmo tipo de retorno e lista de parâmetros; no entanto, esses tipos delegados são dois tipos diferentes, portanto, não são intercambiáveis.</span><span class="sxs-lookup"><span data-stu-id="83d0a-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="83d0a-129">Os métodos `B.M2`, `B.M3` e `B.M4` são incompatíveis com os tipos de delegado `D1` e `D2`, pois têm diferentes tipos de retorno ou listas de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="83d0a-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="83d0a-130">Assim como outras declarações de tipo genérico, argumentos de tipo devem ser fornecidos para criar um tipo de delegado construído.</span><span class="sxs-lookup"><span data-stu-id="83d0a-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="83d0a-131">Os tipos de parâmetro e o tipo de retorno de um tipo de delegado construído são criados pela substituição, para cada parâmetro de tipo na declaração de delegado, o argumento de tipo correspondente do tipo de delegado construído.</span><span class="sxs-lookup"><span data-stu-id="83d0a-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="83d0a-132">O tipo de retorno resultante e os tipos de parâmetro são usados para determinar quais métodos são compatíveis com um tipo de delegado construído.</span><span class="sxs-lookup"><span data-stu-id="83d0a-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="83d0a-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="83d0a-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="83d0a-134">O método `X.F` é compatível com o tipo delegado `Predicate<int>` e o método `X.G` é compatível com o tipo delegado `Predicate<string>`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="83d0a-135">A única maneira de declarar um tipo delegate é por meio de um *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="83d0a-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="83d0a-136">Um tipo delegado é um tipo de classe que é derivado de `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="83d0a-137">Os tipos delegados são implicitamente `sealed`, portanto não é permitido derivar qualquer tipo de um tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="83d0a-138">Também não é permitido derivar um tipo de classe não-delegado de `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="83d0a-139">Observe que `System.Delegate` não é, em si, um tipo delegado; é um tipo de classe do qual todos os tipos delegados são derivados.</span><span class="sxs-lookup"><span data-stu-id="83d0a-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="83d0a-140">C#fornece uma sintaxe especial para a instanciação e a invocação de delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="83d0a-141">Exceto para instanciação, qualquer operação que possa ser aplicada a uma classe ou instância de classe também pode ser aplicada a uma classe ou instância de delegado, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="83d0a-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="83d0a-142">Em particular, é possível acessar membros do tipo `System.Delegate` por meio da sintaxe de acesso de membro usual.</span><span class="sxs-lookup"><span data-stu-id="83d0a-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="83d0a-143">O conjunto de métodos encapsulado por uma instância de delegado é chamado de lista de invocação.</span><span class="sxs-lookup"><span data-stu-id="83d0a-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="83d0a-144">Quando uma instância de delegado é criada ([compatibilidade de delegado](delegates.md#delegate-compatibility)) de um único método, ela encapsula esse método e sua lista de invocação contém apenas uma entrada.</span><span class="sxs-lookup"><span data-stu-id="83d0a-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="83d0a-145">No entanto, quando duas instâncias delegadas não nulas são combinadas, suas listas de invocação são concatenadas – no operando de ordem esquerda e, em seguida, operando à direita – para formar uma nova lista de invocação, que contém duas ou mais entradas.</span><span class="sxs-lookup"><span data-stu-id="83d0a-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="83d0a-146">Os delegados são combinados usando o binário `+` ([operador de adição](expressions.md#addition-operator)) e os operadores de `+=` ([atribuição composta](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="83d0a-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="83d0a-147">Um delegado pode ser removido de uma combinação de delegados, usando o binário `-` ([operador de subtração](expressions.md#subtraction-operator)) e os operadores de `-=` ([atribuição composta](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="83d0a-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="83d0a-148">Os delegados podem ser comparados para igualdade ([operadores de igualdade de delegado](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="83d0a-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="83d0a-149">O exemplo a seguir mostra a instanciação de vários delegados e suas listas de invocação correspondentes:</span><span class="sxs-lookup"><span data-stu-id="83d0a-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="83d0a-150">Quando `cd1` e `cd2` são instanciados, cada um encapsula um método.</span><span class="sxs-lookup"><span data-stu-id="83d0a-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="83d0a-151">Quando `cd3` é instanciado, ele tem uma lista de invocação de dois métodos, `M1` e `M2`, nessa ordem.</span><span class="sxs-lookup"><span data-stu-id="83d0a-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="83d0a-152">a lista de invocação do `cd4` contém `M1`, `M2` e `M1`, nessa ordem.</span><span class="sxs-lookup"><span data-stu-id="83d0a-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="83d0a-153">Finalmente, a lista de invocação do `cd5` contém `M1`, `M2`, `M1`, `M1` e `M2`, nessa ordem.</span><span class="sxs-lookup"><span data-stu-id="83d0a-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="83d0a-154">Para obter mais exemplos de como combinar (bem como remover) delegados, consulte [delegar invocação](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="83d0a-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="83d0a-155">Delegar compatibilidade</span><span class="sxs-lookup"><span data-stu-id="83d0a-155">Delegate compatibility</span></span>

<span data-ttu-id="83d0a-156">Um método ou delegado `M` é ***compatível*** com um tipo delegado `D` se todas as seguintes opções forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="83d0a-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="83d0a-157">`D` e `M` têm o mesmo número de parâmetros, e cada parâmetro em `D` tem os mesmos modificadores `ref` ou `out` que o parâmetro correspondente em `M`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="83d0a-158">Para cada parâmetro de valor (um parâmetro sem `ref` ou `out` modificador), existe uma conversão de identidade ([conversão de identidade](conversions.md#identity-conversion)) ou conversão de referência implícita ([conversões de referência implícitas](conversions.md#implicit-reference-conversions)) do tipo de parâmetro em `D` para o tipo de parâmetro correspondente no `M`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="83d0a-159">Para cada parâmetro `ref` ou `out`, o tipo de parâmetro em `D` é o mesmo que o tipo de parâmetro em `M`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="83d0a-160">Existe uma conversão de identidade ou de referência implícita do tipo de retorno de `M` para o tipo de retorno de `D`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="83d0a-161">Delegar instanciação</span><span class="sxs-lookup"><span data-stu-id="83d0a-161">Delegate instantiation</span></span>

<span data-ttu-id="83d0a-162">Uma instância de um delegado é criada por um *delegate_creation_expression* ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)) ou uma conversão para um tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="83d0a-163">A instância delegada recém-criada então se refere a:</span><span class="sxs-lookup"><span data-stu-id="83d0a-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="83d0a-164">O método estático referenciado no *delegate_creation_expression*ou</span><span class="sxs-lookup"><span data-stu-id="83d0a-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="83d0a-165">O objeto de destino (que não pode ser `null`) e o método de instância referenciado no *delegate_creation_expression*ou</span><span class="sxs-lookup"><span data-stu-id="83d0a-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="83d0a-166">Outro delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-166">Another delegate.</span></span>

<span data-ttu-id="83d0a-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="83d0a-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="83d0a-168">Depois de instanciadas, as instâncias delegadas sempre se referem ao mesmo objeto e método de destino.</span><span class="sxs-lookup"><span data-stu-id="83d0a-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="83d0a-169">Lembre-se de que, quando dois delegados são combinados, ou um é removido de outro, um novo delegado resulta em sua própria lista de invocação; as listas de invocação dos delegados combinados ou removidos permanecem inalteradas.</span><span class="sxs-lookup"><span data-stu-id="83d0a-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="83d0a-170">Invocação de delegado</span><span class="sxs-lookup"><span data-stu-id="83d0a-170">Delegate invocation</span></span>

<span data-ttu-id="83d0a-171">C#fornece uma sintaxe especial para invocar um delegado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="83d0a-172">Quando uma instância de delegado não nula cuja lista de invocação contém uma entrada é invocada, ela invoca um método com os mesmos argumentos que foi fornecido e retorna o mesmo valor que o método referenciado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="83d0a-173">(Confira as [invocações de representante](expressions.md#delegate-invocations) para obter informações detalhadas sobre a invocação de delegado.) Se ocorrer uma exceção durante a invocação de tal delegado e essa exceção não for detectada dentro do método que foi invocado, a pesquisa de uma cláusula catch de exceção continuará no método que chamou o delegado, como se esse método tivesse chamado diretamente de método ao qual o delegado é referenciado.</span><span class="sxs-lookup"><span data-stu-id="83d0a-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="83d0a-174">A invocação de uma instância delegada cuja lista de invocação contém várias entradas continua invocando cada um dos métodos na lista de invocação, de forma síncrona, na ordem.</span><span class="sxs-lookup"><span data-stu-id="83d0a-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="83d0a-175">Cada método chamado é passado como o mesmo conjunto de argumentos, como foi dado à instância delegada.</span><span class="sxs-lookup"><span data-stu-id="83d0a-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="83d0a-176">Se essa invocação de delegado incluir parâmetros de referência ([parâmetros de referência](classes.md#reference-parameters)), cada invocação de método ocorrerá com uma referência à mesma variável; as alterações nessa variável por um método na lista de invocação ficarão visíveis para os métodos mais adiante na lista de invocação.</span><span class="sxs-lookup"><span data-stu-id="83d0a-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="83d0a-177">Se a invocação de delegado incluir parâmetros de saída ou um valor de retorno, o valor final será proveniente da invocação do último delegado na lista.</span><span class="sxs-lookup"><span data-stu-id="83d0a-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="83d0a-178">Se ocorrer uma exceção durante o processamento da invocação de tal delegado, e essa exceção não for detectada dentro do método que foi invocado, a pesquisa de uma cláusula catch de exceção continuará no método que chamou o delegado e todos os métodos abaixo a lista de invocação não é invocada.</span><span class="sxs-lookup"><span data-stu-id="83d0a-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="83d0a-179">A tentativa de invocar uma instância delegada cujo valor é nulo resulta em uma exceção do tipo `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="83d0a-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="83d0a-180">O exemplo a seguir mostra como criar uma instância, combinar, remover e invocar delegados:</span><span class="sxs-lookup"><span data-stu-id="83d0a-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="83d0a-181">Conforme mostrado na instrução `cd3 += cd1;`, um delegado pode estar presente em uma lista de invocação várias vezes.</span><span class="sxs-lookup"><span data-stu-id="83d0a-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="83d0a-182">Nesse caso, ele é simplesmente invocado uma vez por ocorrência.</span><span class="sxs-lookup"><span data-stu-id="83d0a-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="83d0a-183">Em uma lista de invocação como essa, quando esse delegado é removido, a última ocorrência na lista de invocação é a que foi realmente removida.</span><span class="sxs-lookup"><span data-stu-id="83d0a-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="83d0a-184">Imediatamente antes da execução da instrução final, `cd3 -= cd1;`, o delegado `cd3` se refere a uma lista de invocação vazia.</span><span class="sxs-lookup"><span data-stu-id="83d0a-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="83d0a-185">A tentativa de remover um delegado de uma lista vazia (ou de remover um delegado não existente de uma lista não vazia) não é um erro.</span><span class="sxs-lookup"><span data-stu-id="83d0a-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="83d0a-186">A saída produzida é:</span><span class="sxs-lookup"><span data-stu-id="83d0a-186">The output produced is:</span></span>

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
