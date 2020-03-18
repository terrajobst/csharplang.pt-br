---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485150"
---
# <a name="lambda-discard-parameters"></a>Parâmetros de descarte de lambda

## <a name="summary"></a>Resumo

Permitir Descartes (`_`) a serem usados como parâmetros de lambdas e métodos anônimos.
Por exemplo:
- Lambdas: `(_, _) => 0`, `(int _, int _) => 0`
- métodos anônimos: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>Motivação

Os parâmetros não utilizados não precisam ser nomeados. A intenção de Descartes é clara, ou seja, não são usadas/descartadas.

## <a name="detailed-design"></a>Design detalhado

[Parâmetros do método](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) Na lista de parâmetros de um método lambda ou anônimo com mais de um parâmetro chamado `_`, esses parâmetros são parâmetros de descarte.
Observação: se um único parâmetro for nomeado `_`, ele será um parâmetro regular para motivos de compatibilidade com versões anteriores.

Os parâmetros de descarte não introduzem nenhum nome para nenhum escopo.
Observe que isso significa que eles não fazem com que os nomes de `_` (sublinhado) fiquem ocultos.

[Nomes simples](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Se `K` for zero e o *Simple_name* aparecer em um *bloco* e se o espaço da declaração local ([declarações](basic-concepts.md#declarations)) do *bloco*(ou um *bloco*delimitador) contiver uma variável local, um parâmetro (com exceção de descartar parâmetros) ou constante com o nome `I`, o *Simple_name* se refere a essa variável local, parâmetro ou constante e é classificado como uma variável ou valor.

[Escopos](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Com exceção dos parâmetros de descarte, o escopo de um parâmetro declarado em uma *lambda_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) é a *anonymous_function_body* do *lambda_expression* com a exceção de parâmetros de descarte, o escopo de um parâmetro declarado em uma *anonymous_method_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) é o *bloco* desse *anonymous_method_expression*.

## <a name="related-spec-sections"></a>Seções de especificações relacionadas
- [Parâmetros correspondentes](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
