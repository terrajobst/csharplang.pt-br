---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484919"
---
# <a name="expression-variables-in-initializers"></a>Variáveis de expressão em inicializadores

## <a name="summary"></a>Resumo
[summary]: #summary

Estendemos os recursos introduzidos em C# 7 para permitir expressões que contêm variáveis de expressão (saída de declarações de variável e padrões de declaração) em inicializadores de campo, inicializadores de propriedade, Construtor-inicializadores e cláusulas de consulta.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Isso conclui algumas das bordas aproximadas restantes no C# idioma devido à falta de tempo.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Removemos a restrição que impede a declaração de variáveis de expressão (saída de declarações de variável e padrões de declaração) em um inicializador de construtor. Tal variável declarada está no escopo ao longo do corpo do construtor.

Removemos a restrição que impede a declaração de variáveis de expressão (saída de declarações de variável e padrões de declaração) em um inicializador de campo ou propriedade. Essa variável declarada está no escopo durante a expressão de inicialização.

Removemos a restrição que impede a declaração de variáveis de expressão (saída de declarações de variável e padrões de declaração) em uma cláusula de expressão de consulta que é convertida no corpo de um lambda. Essa variável declarada está no escopo por toda a expressão da cláusula de consulta.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

None.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

O escopo apropriado para variáveis de expressão declaradas nesses contextos não é óbvio e merece mais discussão LDM.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [] Qual é o escopo apropriado para essas variáveis?

## <a name="design-meetings"></a>Criar reuniões

None.
