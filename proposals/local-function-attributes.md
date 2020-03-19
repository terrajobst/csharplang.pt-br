---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509503"
---
# <a name="attributes-on-local-functions"></a>Atributos em funções locais

## <a name="attributes"></a>Atributos

Agora, as declarações de função local têm permissão para ter [atributos](../spec/attributes.md). Parâmetros e parâmetros de tipo em funções locais também podem ter atributos.

Atributos com um significado especificado quando aplicados a um método, seus parâmetros ou seus parâmetros de tipo terão o mesmo significado quando aplicados a uma função local, seus parâmetros ou seus parâmetros de tipo, respectivamente.

Uma função local pode se tornar condicional no mesmo sentido que um [método condicional](../spec/attributes.md#the-conditional-attribute) , decorando-a com um `[ConditionalAttribute]`. Uma função local condicional também deve ser `static`. Todas as restrições em métodos condicionais também se aplicam a funções locais condicionais, incluindo que o tipo de retorno deve ser `void`.

## <a name="extern"></a>Externo

O modificador de `extern` agora é permitido em funções locais. Isso torna a função local externa no mesmo sentido que um [método externo](../spec/classes.md#external-methods).

Da mesma forma que um método externo, o *corpo da função local* de uma função local externa deve ser um ponto-e-vírgula. Um *corpo de função local* de ponto e vírgula só é permitido em uma função local externa. 

Uma função local externa também deve ser `static`.

## <a name="syntax"></a>Sintaxe

A [gramática de funções locais](csharp-7.0/local-functions.md#syntax-grammar) é modificada da seguinte maneira:
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
