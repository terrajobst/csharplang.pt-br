---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217197"
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
```

### <a name="miscellaneous"></a>Diversos

`throw new()` não é permitido.

O `new` de tipo de destino não é permitido com operadores binários.

Não é permitido quando não há tipo para destino: operadores unários, coleção de um `foreach`, em uma `using`, em uma deconstrução, em uma expressão `await`, como uma propriedade de tipo anônimo (`new { Prop = new() }`), em uma instrução `lock`, em uma `sizeof`, em uma instrução `fixed`, em um acesso de membro (`new().field`), em uma operação expedida dinamicamente (`someDynamic.Method(new())`), em uma consulta LINQ, como o operando do operador `is`, como operando esquerdo do operador `??` ,  ...

Ele também não é permitido como um `ref`.

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
