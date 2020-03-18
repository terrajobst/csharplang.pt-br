---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485514"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="77de3-101">Membros da instância ReadOnly</span><span class="sxs-lookup"><span data-stu-id="77de3-101">Readonly Instance Members</span></span>

<span data-ttu-id="77de3-102">Problema defensor: <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="77de3-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="77de3-103">Resumo</span><span class="sxs-lookup"><span data-stu-id="77de3-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="77de3-104">Forneça uma maneira de especificar membros de instância individuais em uma struct não modificar o estado, da mesma maneira que `readonly struct` especifica que nenhum membro de instância modifica o estado.</span><span class="sxs-lookup"><span data-stu-id="77de3-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="77de3-105">Vale a pena observar que `readonly instance member`! = `pure instance member`.</span><span class="sxs-lookup"><span data-stu-id="77de3-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="77de3-106">Um membro de instância `pure` garante que nenhum Estado será modificado.</span><span class="sxs-lookup"><span data-stu-id="77de3-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="77de3-107">Um membro da instância `readonly` só garante que o estado da instância não será modificado.</span><span class="sxs-lookup"><span data-stu-id="77de3-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="77de3-108">Todos os membros de instância em um `readonly struct` podem ser considerados implicitamente `readonly instance members`.</span><span class="sxs-lookup"><span data-stu-id="77de3-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="77de3-109">A `readonly instance members` explícita declarada em structs não ReadOnly se comportaria da mesma maneira.</span><span class="sxs-lookup"><span data-stu-id="77de3-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="77de3-110">Por exemplo, eles ainda criaria cópias ocultas se você chamou um membro de instância (na instância atual ou em um campo da instância) que, por si só, não é ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="77de3-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="77de3-111">Motivação</span><span class="sxs-lookup"><span data-stu-id="77de3-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="77de3-112">Hoje, os usuários têm a capacidade de criar `readonly struct` tipos que o compilador impõe que todos os campos sejam ReadOnly (e por extensão, que nenhum membro de instância modifique o estado).</span><span class="sxs-lookup"><span data-stu-id="77de3-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="77de3-113">No entanto, há alguns cenários em que você tem uma API existente que expõe campos acessíveis ou que tem uma combinação de membros que fazem mutação e não mutação.</span><span class="sxs-lookup"><span data-stu-id="77de3-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="77de3-114">Sob essas circunstâncias, você não pode marcar o tipo como `readonly` (seria uma alteração significativa).</span><span class="sxs-lookup"><span data-stu-id="77de3-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="77de3-115">Isso normalmente não tem muito impacto, exceto no caso de `in` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="77de3-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="77de3-116">Com parâmetros de `in` para structs não ReadOnly, o compilador fará uma cópia do parâmetro para cada invocação de membro de instância, pois não pode garantir que a invocação não modifique o estado interno.</span><span class="sxs-lookup"><span data-stu-id="77de3-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="77de3-117">Isso pode levar a uma infinidade de cópias e pior desempenho geral do que se você acabou de passar o struct diretamente por valor.</span><span class="sxs-lookup"><span data-stu-id="77de3-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="77de3-118">Para obter um exemplo, consulte este código em [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span><span class="sxs-lookup"><span data-stu-id="77de3-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="77de3-119">Alguns outros cenários em que as cópias ocultas podem ocorrer incluem `static readonly fields` e `literals`.</span><span class="sxs-lookup"><span data-stu-id="77de3-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="77de3-120">Se eles tiverem suporte no futuro, `blittable constants` terminaria no mesmo barco; Isso é que, atualmente, todos precisam de uma cópia completa (em invocação de membro de instância) se a estrutura não estiver marcada `readonly`.</span><span class="sxs-lookup"><span data-stu-id="77de3-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="77de3-121">Design</span><span class="sxs-lookup"><span data-stu-id="77de3-121">Design</span></span>
[design]: #design

<span data-ttu-id="77de3-122">Permitir que um usuário especifique que um membro de instância é, ele mesmo, `readonly` e não modifica o estado da instância (com toda a verificação apropriada feita pelo compilador, é claro).</span><span class="sxs-lookup"><span data-stu-id="77de3-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="77de3-123">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="77de3-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="77de3-124">ReadOnly pode ser aplicado a acessadores de propriedade para indicar que `this` não serão modificadas no acessador.</span><span class="sxs-lookup"><span data-stu-id="77de3-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="77de3-125">Os exemplos a seguir têm setters ReadOnly porque esses acessadores modificam o estado do campo de membro, mas não modificam o valor desse campo de membro.</span><span class="sxs-lookup"><span data-stu-id="77de3-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="77de3-126">Quando `readonly` é aplicado à sintaxe de propriedade, isso significa que todos os acessadores são `readonly`.</span><span class="sxs-lookup"><span data-stu-id="77de3-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="77de3-127">ReadOnly só pode ser aplicado a acessadores que não permutam o tipo recipiente.</span><span class="sxs-lookup"><span data-stu-id="77de3-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="77de3-128">ReadOnly pode ser aplicado a algumas propriedades implementadas automaticamente, mas não terá um efeito significativo.</span><span class="sxs-lookup"><span data-stu-id="77de3-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="77de3-129">O compilador tratará todos os getters autoimplementados como ReadOnly se a palavra-chave `readonly` estiver presente ou não.</span><span class="sxs-lookup"><span data-stu-id="77de3-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="77de3-130">ReadOnly pode ser aplicado a eventos implementados manualmente, mas não a eventos do tipo campo.</span><span class="sxs-lookup"><span data-stu-id="77de3-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="77de3-131">ReadOnly não pode ser aplicado a acessadores de evento individuais (Adicionar/remover).</span><span class="sxs-lookup"><span data-stu-id="77de3-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="77de3-132">Alguns exemplos de sintaxe:</span><span class="sxs-lookup"><span data-stu-id="77de3-132">Some other syntax examples:</span></span>

* <span data-ttu-id="77de3-133">Membros de apto para de expressão: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="77de3-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="77de3-134">Restrições genéricas: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="77de3-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="77de3-135">O compilador emitiria o membro da instância, como de costume, e também emitiria um atributo reconhecido do compilador indicando que o membro da instância não modifica o estado.</span><span class="sxs-lookup"><span data-stu-id="77de3-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="77de3-136">Isso efetivamente faz com que o parâmetro oculto `this` se torne `in T` em vez de `ref T`.</span><span class="sxs-lookup"><span data-stu-id="77de3-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="77de3-137">Isso permitiria que o usuário chamasse o método de instância dito com segurança sem que o compilador precise fazer uma cópia.</span><span class="sxs-lookup"><span data-stu-id="77de3-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="77de3-138">As restrições incluem:</span><span class="sxs-lookup"><span data-stu-id="77de3-138">The restrictions would include:</span></span>

* <span data-ttu-id="77de3-139">O modificador `readonly` não pode ser aplicado a métodos estáticos, construtores ou destruidores.</span><span class="sxs-lookup"><span data-stu-id="77de3-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="77de3-140">O modificador de `readonly` não pode ser aplicado a delegados.</span><span class="sxs-lookup"><span data-stu-id="77de3-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="77de3-141">Não é possível aplicar o modificador de `readonly` a membros da classe ou interface.</span><span class="sxs-lookup"><span data-stu-id="77de3-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="77de3-142">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="77de3-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="77de3-143">As mesmas desvantagens que existem com os métodos de `readonly struct` hoje.</span><span class="sxs-lookup"><span data-stu-id="77de3-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="77de3-144">Determinado código ainda pode causar cópias ocultas.</span><span class="sxs-lookup"><span data-stu-id="77de3-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="77de3-145">{1&gt;Observações&lt;1}</span><span class="sxs-lookup"><span data-stu-id="77de3-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="77de3-146">Usar um atributo ou outra palavra-chave também pode ser possível.</span><span class="sxs-lookup"><span data-stu-id="77de3-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="77de3-147">Essa proposta está um pouco relacionada a (mas é mais um subconjunto de) `functional purity` e/ou `constant expressions`, ambas com propostas existentes.</span><span class="sxs-lookup"><span data-stu-id="77de3-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
