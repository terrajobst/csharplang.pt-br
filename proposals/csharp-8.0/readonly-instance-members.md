---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485514"
---
# <a name="readonly-instance-members"></a>Membros da instância ReadOnly

Problema defensor: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>Resumo
[summary]: #summary

Forneça uma maneira de especificar membros de instância individuais em uma struct não modificar o estado, da mesma maneira que `readonly struct` especifica que nenhum membro de instância modifica o estado.

Vale a pena observar que `readonly instance member`! = `pure instance member`. Um membro de instância `pure` garante que nenhum Estado será modificado. Um membro da instância `readonly` só garante que o estado da instância não será modificado.

Todos os membros de instância em um `readonly struct` podem ser considerados implicitamente `readonly instance members`. A `readonly instance members` explícita declarada em structs não ReadOnly se comportaria da mesma maneira. Por exemplo, eles ainda criaria cópias ocultas se você chamou um membro de instância (na instância atual ou em um campo da instância) que, por si só, não é ReadOnly.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Hoje, os usuários têm a capacidade de criar `readonly struct` tipos que o compilador impõe que todos os campos sejam ReadOnly (e por extensão, que nenhum membro de instância modifique o estado). No entanto, há alguns cenários em que você tem uma API existente que expõe campos acessíveis ou que tem uma combinação de membros que fazem mutação e não mutação. Sob essas circunstâncias, você não pode marcar o tipo como `readonly` (seria uma alteração significativa).

Isso normalmente não tem muito impacto, exceto no caso de `in` parâmetros. Com parâmetros de `in` para structs não ReadOnly, o compilador fará uma cópia do parâmetro para cada invocação de membro de instância, pois não pode garantir que a invocação não modifique o estado interno. Isso pode levar a uma infinidade de cópias e pior desempenho geral do que se você acabou de passar o struct diretamente por valor. Para obter um exemplo, consulte este código em [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)

Alguns outros cenários em que as cópias ocultas podem ocorrer incluem `static readonly fields` e `literals`. Se eles tiverem suporte no futuro, `blittable constants` terminaria no mesmo barco; Isso é que, atualmente, todos precisam de uma cópia completa (em invocação de membro de instância) se a estrutura não estiver marcada `readonly`.

## <a name="design"></a>Design
[design]: #design

Permitir que um usuário especifique que um membro de instância é, ele mesmo, `readonly` e não modifica o estado da instância (com toda a verificação apropriada feita pelo compilador, é claro). Por exemplo:

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

ReadOnly pode ser aplicado a acessadores de propriedade para indicar que `this` não serão modificadas no acessador. Os exemplos a seguir têm setters ReadOnly porque esses acessadores modificam o estado do campo de membro, mas não modificam o valor desse campo de membro.

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

Quando `readonly` é aplicado à sintaxe de propriedade, isso significa que todos os acessadores são `readonly`.

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

ReadOnly só pode ser aplicado a acessadores que não permutam o tipo recipiente.

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

ReadOnly pode ser aplicado a algumas propriedades implementadas automaticamente, mas não terá um efeito significativo. O compilador tratará todos os getters autoimplementados como ReadOnly se a palavra-chave `readonly` estiver presente ou não.

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

ReadOnly pode ser aplicado a eventos implementados manualmente, mas não a eventos do tipo campo. ReadOnly não pode ser aplicado a acessadores de evento individuais (Adicionar/remover).

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

Alguns exemplos de sintaxe:

* Membros de apto para de expressão: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* Restrições genéricas: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

O compilador emitiria o membro da instância, como de costume, e também emitiria um atributo reconhecido do compilador indicando que o membro da instância não modifica o estado. Isso efetivamente faz com que o parâmetro oculto `this` se torne `in T` em vez de `ref T`.

Isso permitiria que o usuário chamasse o método de instância dito com segurança sem que o compilador precise fazer uma cópia.

As restrições incluem:

* O modificador `readonly` não pode ser aplicado a métodos estáticos, construtores ou destruidores.
* O modificador de `readonly` não pode ser aplicado a delegados.
* Não é possível aplicar o modificador de `readonly` a membros da classe ou interface.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

As mesmas desvantagens que existem com os métodos de `readonly struct` hoje. Determinado código ainda pode causar cópias ocultas.

## <a name="notes"></a>{1&gt;Observações&lt;1}
[notes]: #notes

Usar um atributo ou outra palavra-chave também pode ser possível.

Essa proposta está um pouco relacionada a (mas é mais um subconjunto de) `functional purity` e/ou `constant expressions`, ambas com propostas existentes.
