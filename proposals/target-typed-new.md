---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108358"
---

# <a name="target-typed-new-expressions"></a>Expressões de `new` de tipo de destino

* [x] proposta
* [x] [protótipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] Implementação
* [] Especificação

## <a name="summary"></a>Resumo
[summary]: #summary

Não exigir especificação de tipo para construtores quando o tipo é conhecido. 

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Permitir inicialização de campo sem duplicar o tipo.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

Permitir a omissão do tipo quando ele puder ser inferido do uso.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

Crie uma instância de um objeto sem soletrar o tipo.
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

A sintaxe de *object_creation_expression* seria modificada para tornar o *tipo* opcional quando parênteses estiverem presentes. Isso é necessário para resolver a ambiguidade com *anonymous_object_creation_expression*.
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

Um `new` com tipo de destino é conversível para qualquer tipo. Como resultado, ele não contribui para a resolução de sobrecarga. Isso é principalmente para evitar alterações significativas imprevisíveis.

A lista de argumentos e as expressões de inicializador serão associadas depois que o tipo for determinado.

O tipo da expressão seria inferido do tipo de destino que seria necessário para ser um dos seguintes:

- **Qualquer tipo de struct** (incluindo tipos de tupla)
- **Qualquer tipo de referência** (incluindo tipos delegados)
- **Qualquer parâmetro de tipo** com um construtor ou uma restrição de `struct`

com as seguintes exceções:

- **Tipos de enumeração:** nem todos os tipos enum contêm a constante zero, portanto, deve ser desejável usar o membro enum explícito.
- **Tipos de interface:** esse é um recurso de nicho e deve ser preferível mencionar explicitamente o tipo.
- **Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.
- **dinâmico:** não permitimos `new dynamic()`, portanto, não permitimos `new()` com `dynamic` como um tipo de destino.

Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.

Quando o tipo de destino for um tipo de valor anulável, o `new` de tipo de destino será convertido para o tipo subjacente em vez do tipo anulável.

> **Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?

As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct). Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
