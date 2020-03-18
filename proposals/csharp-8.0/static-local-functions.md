---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485094"
---
# <a name="static-local-functions"></a>Funções locais estáticas

## <a name="summary"></a>Resumo

Suporte a funções locais que não permitem capturar o estado do escopo delimitador.

## <a name="motivation"></a>Motivação

Evite capturar de forma não intencional o estado do contexto delimitador.
Permita que as funções locais sejam usadas em cenários em que um método de `static` é necessário.

## <a name="detailed-design"></a>Design detalhado

Uma função local declarada `static` não pode capturar o estado do escopo delimitador.
Como resultado, os locais, os parâmetros e os `this` do escopo delimitador não estão disponíveis em uma função local `static`.

Uma `static` função local não pode referenciar membros de instância de uma referência implícita ou explícita `this` ou `base`.

Uma `static` função local pode referenciar `static` membros do escopo delimitador.

Uma `static` função local pode referenciar `constant` definições do escopo delimitador.

`nameof()` em uma função local `static` pode referenciar locais, parâmetros ou `this` ou `base` do escopo delimitador.

As regras de acessibilidade para membros de `private` no escopo delimitador são as mesmas para funções locais `static` e não`static`.

Uma definição de função local `static` é emitida como um método `static` nos metadados, mesmo que seja usada apenas em um delegado.

Uma função local ou lambda não`static` pode capturar o estado de uma função local de `static` delimitadora, mas não pode capturar o estado fora da função local `static` de circunscrição.

Uma função local `static` não pode ser invocada em uma árvore de expressão.

Uma chamada para uma função local é emitida como `call` em vez de `callvirt`, independentemente de a função local ser `static`.

A resolução de sobrecarga de uma chamada em uma função local não afetada pelo fato de a função local ser `static`.

Remover o modificador de `static` de uma função local em um programa válido não altera o significado do programa.

## <a name="design-meetings"></a>Criar reuniões

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
