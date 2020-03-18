---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484702"
---
# <a name="pattern-matching-with-generics"></a>correspondência de padrões com genéricos

* [x] proposta
* [] Protótipo:
* [] Implementação:
* [] Especificação:

## <a name="summary"></a>Resumo
[summary]: #summary

A especificação para o [operador C# as existente](../../spec/expressions.md#the-as-operator) permite que não haja nenhuma conversão entre o tipo do operando e o tipo especificado quando um é um tipo aberto. No entanto C# , no 7, o padrão de `Type identifier` requer que haja uma conversão entre o tipo de entrada e o tipo fornecido.

Sugerimos relaxar isso e alterar `expression is Type identifier`, além de serem permitidas nas condições, quando permitido em C# 7, também serão permitidas quando `expression as Type` seriam permitidas. Especificamente, os novos casos são casos em que o tipo da expressão ou o tipo especificado é um tipo aberto. 

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Casos em que a correspondência de padrões deve ser permitida no momento com falha na compilação. Consulte, por exemplo, https://github.com/dotnet/roslyn/issues/16195.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Alteramos o parágrafo na especificação de correspondência de padrões (a adição proposta é mostrada em negrito):

> Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação. Um valor de tipo estático `E` é chamado de *padrão compatível* com o tipo `T` se houver uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão unboxing de `E` para `T`**ou se `E` ou `T` for um tipo aberto**. É um erro de tempo de compilação se uma expressão do tipo `E` não for compatível com o tipo em um padrão de tipo com o qual ele é correspondido.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

None.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

None.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

None.

## <a name="design-meetings"></a>Criar reuniões

O LDM considerou essa pergunta e sentiu que era uma alteração no nível de correção de bug. Estamos tratando-o como um recurso de idioma separado porque apenas fazer a alteração depois que o idioma foi liberado introduzirá uma incompatibilidade posterior. O uso da alteração proposta requer que o programador especifique a versão de idioma 7,1.
