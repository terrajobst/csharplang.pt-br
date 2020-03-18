---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485136"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>Permitir `stackalloc` em contextos aninhados

### <a name="stack-allocation"></a>Alocação de pilha

Modificamos a [*alocação da pilha*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) de seções C# da especificação da linguagem para relaxar os locais quando uma expressão `stackalloc` pode aparecer. Nós excluímos

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

e substitua-os por

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

Observe que a adição de um *array_initializer* para *stackalloc_initializer* (e a criação da expressão de índice opcional) era uma [extensão C# em 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) e não é descrita aqui.

O *tipo de elemento* da expressão de `stackalloc` é o *unmanaged_type* nomeado na expressão stackalloc, se houver, ou o tipo comum entre os elementos da *array_initializer* caso contrário.

O tipo de *stackalloc_initializer* com o *tipo de elemento* `K` depende de seu contexto sintático:
- Se o *stackalloc_initializer* aparecer diretamente como o *local_variable_initializer* de uma instrução *local_variable_declaration* ou uma *for_initializer*, seu tipo será `K*`.
- Caso contrário, seu tipo será `System.Span<K>`.

### <a name="stackalloc-conversion"></a>Conversão de stackalloc

A *conversão stackalloc* é uma nova conversão interna implícita da expressão. Quando o tipo de um *stackalloc_initializer* é `K*`, há uma conversão implícita de *stackalloc* do *stackalloc_initializer* para o tipo `System.Span<K>`.
