---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484534"
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

- **Qualquer tipo de struct**
- **Qualquer tipo de referência**
- **Qualquer parâmetro de tipo** com um construtor ou uma restrição de `struct`

com as seguintes exceções:

- **Tipos de enumeração:** nem todos os tipos enum contêm a constante zero, portanto, deve ser desejável usar o membro enum explícito.
- **Tipos de interface:** esse é um recurso de nicho e deve ser preferível mencionar explicitamente o tipo.
- **Tipos de matriz:** as matrizes precisam de uma sintaxe especial para fornecer o comprimento.
- **Construtor padrão de struct**: isso regra todos os tipos primitivos e a maioria dos tipos de valor. Se você quisesse usar o valor padrão desses tipos, poderia escrever `default` em vez disso.

Todos os outros tipos que não são permitidos no *object_creation_expression* também são excluídos, por exemplo, tipos de ponteiro.

> **Problema aberto:** devemos permitir delegações e tuplas como o tipo de destino?

As regras acima incluem delegados (um tipo de referência) e tuplas (um tipo de struct). Embora ambos os tipos sejam constructible, se o tipo for inferência, uma função anônima ou um literal de tupla já poderá ser usado.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **Problema aberto:** devemos permitir `throw new()` com `Exception` como o tipo de destino?

Temos `throw null` hoje, mas não `throw default` (embora ele tenha o mesmo efeito). Por outro lado, `throw new()` pode ser realmente útil como uma abreviação para `throw new Exception(...)`. Observe que ele já é permitido pela especificação atual. `Exception` é um tipo de referência, e a especificação para a instrução Throw indica que a expressão é convertida em `Exception`.

> **Problema aberto:** devemos permitir usos de um `new` de tipo de destino com operadores aritméticos e de comparação definidos pelo usuário?

Para comparação, o `default` dá suporte apenas aos operadores de igualdade (definido pelo usuário e interno). Isso fará sentido dar suporte a outros operadores para `new()` também?

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

None.

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
