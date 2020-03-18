---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485080"
---
# <a name="static-lambdas"></a>Lambdas estáticos

## <a name="summary"></a>Resumo

Suporte a lambdas que não permitem capturar o estado do escopo de delimitação.

## <a name="motivation"></a>Motivação

Evite capturar de forma não intencional o estado do contexto delimitador.

## <a name="detailed-design"></a>Design detalhado

Um lambdas com `static` não pode capturar o estado do escopo delimitador.
Como resultado, os locais, os parâmetros e os `this` do escopo delimitador não estão disponíveis em um lambda `static`.

Um `static` lambda não pode referenciar membros de instância de uma referência implícita ou explícita de `this` ou `base`.

Um `static` lambda pode fazer referência `static` membros do escopo delimitador.

Um `static` lambda pode referenciar definições de `constant` do escopo delimitador.

`nameof()` em um `static` lambda pode referenciar locais, parâmetros ou `this` ou `base` do escopo delimitador.

As regras de acessibilidade para membros de `private` no escopo de delimitação são as mesmas para lambdas `static` e não`static`.

Nenhuma garantia é feita como se uma definição de lambda de `static` é emitida como um método de `static` nos metadados. Isso é deixado para a implementação do compilador para otimizar.

Uma função local ou lambda não`static` pode capturar o estado de um `static` lambda delimitador, mas não pode capturar o estado fora do `static` lambda delimitador.

Um `static` lambda pode ser usado em uma árvore de expressão.

Remover o modificador de `static` de um lambda em um programa válido não altera o significado do programa.
