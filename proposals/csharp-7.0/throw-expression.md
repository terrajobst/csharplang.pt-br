---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484541"
---
# <a name="throw-expression"></a>Expressão throw

Estendemos o conjunto de formulários de expressões para incluir

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

As regras de tipo são as seguintes:

- Um *throw_expression* não tem nenhum tipo.
- Uma *throw_expression* é conversível para cada tipo por uma conversão implícita.

Uma *expressão throw* gera o valor produzido avaliando o *null_coalescing_expression*, que deve indicar um valor do tipo de classe `System.Exception`, de um tipo de classe que deriva de `System.Exception` ou de um tipo de parâmetro de tipo que tem `System.Exception` (ou uma subclasse) como sua classe base efetiva. Se a avaliação da expressão produzir `null`, uma `System.NullReferenceException` será lançada em vez disso.

O comportamento em tempo de execução da avaliação de uma *expressão throw* é o mesmo [especificado para uma *instrução Throw*](../../spec/statements.md#the-throw-statement).

As regras de análise de fluxo são as seguintes:

- Para cada variável *v*, a *v* é definitivamente atribuída antes da *null_coalescing_expression* de um *throw_expression* IFF ele é definitivamente atribuído antes da *throw_expression*.
- Para cada variável *v*, *v* é definitivamente atribuído após *throw_expression*.

Uma *expressão throw* é permitida somente nos seguintes contextos sintáticos:
- Como o segundo ou terceiro operando de um operador condicional Ternário `?:`
- Como o segundo operando de um operador de União nulo `??`
- Como o corpo de um lambda ou método apto para de expressão.
