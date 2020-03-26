---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281964"
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

## <a name="specification"></a>Especificação
[design]: #detailed-design

Um novo formulário sintático, *target_typed_new* do *object_creation_expression* é aceito, no qual o *tipo* é opcional.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

Uma expressão de *target_typed_new* não tem um tipo. No entanto, há uma nova *conversão de criação de objeto* que é uma conversão implícita de expressão, que existe de um *target_typed_new* a cada tipo.

Dado um tipo de destino `T`, o tipo `T0` é o tipo subjacente de `T`se `T` for uma instância de `System.Nullable`. Caso contrário `T0` é `T`. O significado de uma expressão de *target_typed_new* que é convertida no tipo `T` é igual ao significado de uma *object_creation_expression* correspondente que especifica `T0` como o tipo.

É um erro de tempo de compilação se um *target_typed_new* for usado como um operando de um operador unário ou binário, ou se for usado onde ele não está sujeito a uma *conversão de criação de objeto*.

> **Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?

As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct). Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Diversos

As seguintes são as consequências da especificação:

- `throw new()` é permitido (o tipo de destino é `System.Exception`)
- O `new` de tipo de destino não é permitido com operadores binários.
- Não é permitido quando não há tipo para destino: operadores unários, coleção de um `foreach`, em uma `using`, em uma deconstrução, em uma expressão `await`, como uma propriedade de tipo anônimo (`new { Prop = new() }`), em uma instrução `lock`, em uma `sizeof`, em uma instrução `fixed`, em um acesso de membro (`new().field`), em uma operação expedida dinamicamente (`someDynamic.Method(new())`), em uma consulta LINQ, como o operando do operador `is`, como operando esquerdo do operador `??` ,  ...
- Ele também não é permitido como um `ref`.
- Os tipos de tipos a seguir não são permitidos como destinos da conversão
  - **Tipos de enumeração:** `new()` funcionarão (como `new Enum()` funciona para fornecer o valor padrão), mas `new(1)` não funcionará, pois os tipos de enumeração não têm um construtor.
  - **Tipos de interface:** Isso funcionaria da mesma forma que a expressão de criação correspondente para tipos COM.
  - **Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.    
  - **dinâmico:** não permitimos `new dynamic()`, portanto, não permitimos `new()` com `dynamic` como um tipo de destino.
  - **tuplas:** Eles têm o mesmo significado de uma criação de objeto usando o tipo subjacente.
  - Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.   

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Houve algumas preocupações com o tipo de destino `new` criar novas categorias de alterações significativas, mas já temos isso com `null` e `default`, e isso não foi um problema significativo.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

A maioria das reclamações sobre os tipos muito longos para duplicar na inicialização de campo é sobre os *argumentos de tipo* , não o tipo em si, poderíamos inferir apenas argumentos de tipo como `new Dictionary(...)` (ou semelhantes) e inferir argumentos de tipo localmente a partir de argumentos ou o inicializador de coleção.

## <a name="questions"></a>Perguntas
[questions]: #questions

- Devemos proibir usos em árvores de expressão? foi
- Como o recurso interage com `dynamic` argumentos? (sem tratamento especial)
- Como o IntelliSense deve funcionar com `new()`? (somente quando há um único tipo de destino)

## <a name="design-meetings"></a>Criar reuniões

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [LDM-2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
