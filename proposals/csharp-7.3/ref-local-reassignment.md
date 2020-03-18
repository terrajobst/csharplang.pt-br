---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484597"
---
# <a name="ref-local-reassignment"></a>Reatribuição local de referência

Em C# 7,3, adicionamos suporte para reassociar o Referent de uma variável local ref ou um parâmetro ref.

Adicionamos o seguinte ao conjunto de `assignment_operator`s.

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

O operador de `=ref` é chamado de ***operador de atribuição de referência***. Não é um *operador de atribuição composta*. O operando esquerdo deve ser uma expressão que seja associada a uma variável local ref, um parâmetro ref (diferente de `this`) ou um parâmetro out. O operando à direita deve ser uma expressão que produz um lvalue que designa um valor do mesmo tipo que o operando esquerdo.

O operando à direita deve ser definitivamente atribuído no ponto da atribuição de referência.

Quando o operando esquerdo for associado a um parâmetro `out`, será um erro se esse parâmetro de `out` não tiver sido atribuído definitivamente no início do operador de atribuição de referência.

Se o operando esquerdo for uma ref gravável (ou seja, ele designa algo diferente de um `ref readonly` local ou `in` parâmetro), o operando à direita deverá ser um lvalue gravável.

O operador ref Assignment produz um lvalue do tipo atribuído. É gravável se o operando esquerdo é gravável (ou seja, não `ref readonly` ou `in`).

As regras de segurança para esse operador são:

- Para uma `e1 = ref e2`de reatribuição de referência, o *ref-safe-to-escape* de `e2` deve ser pelo menos tão amplo quanto o escopo de *ref-safe-to-escape* de `e1`.

Onde *ref-safe-to-escape* é definido em [segurança para tipos do tipo ref](../csharp-7.2/span-safety.md)
