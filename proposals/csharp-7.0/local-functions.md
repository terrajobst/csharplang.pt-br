---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484436"
---
# <a name="local-functions"></a>Funções locais

Estendemos C# para dar suporte à declaração de funções no escopo de bloco. As funções locais podem usar variáveis (capturar) do escopo delimitador.

O compilador usa a análise de fluxo para detectar quais variáveis uma função local usa antes de atribuir um valor a ela. Cada chamada da função requer que essas variáveis sejam definitivamente atribuídas. Da mesma forma, o compilador determina quais variáveis são definitivamente atribuídas no retorno. Essas variáveis são consideradas definitivamente atribuídas depois que a função local é invocada.

As funções locais podem ser chamadas de um ponto léxico antes de sua definição. As instruções de declaração de função local não causam um aviso quando não estão acessíveis.

TODO: _criar especificação_

## <a name="syntax-grammar"></a>Gramática de sintaxe

Essa gramática é representada como uma comparação da gramática de especificações atual.

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

As funções locais podem usar variáveis definidas no escopo delimitador. A implementação atual requer que todas as variáveis lidas dentro de uma função local sejam definitivamente atribuídas, como se estiver executando a função local em seu ponto de definição. Além disso, a definição da função local deve ter sido "executada" em qualquer ponto de uso.

Depois de experimentar esse bit (por exemplo, não é possível definir duas funções locais recursivas mutuamente), já que revisamos como queremos que a atribuição definitiva funcione. A revisão (ainda não implementada) é que todas as variáveis locais lidas em uma função local devem ser definitivamente atribuídas em cada invocação da função local. Isso é, na verdade, mais sutil do que parece, e há um monte de trabalho restante para fazê-lo funcionar. Depois de fazer isso, você poderá mover suas funções locais para o final de seu bloco delimitador.

As novas regras de atribuição definitivas são incompatíveis com a inferência do tipo de retorno de uma função local, portanto, provavelmente, vamos remover o suporte para inferir o tipo de retorno.

A menos que você converta uma função local em um delegado, a captura é feita em quadros que são tipos de valor. Isso significa que você não obtém nenhuma pressão de GC do uso de funções locais com a captura.

### <a name="reachability"></a>Acessibilidade

Adicionamos à especificação

> O corpo de uma expressão lambda apto para de instrução ou função local é considerado acessível.
