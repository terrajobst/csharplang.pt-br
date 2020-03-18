---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484506"
---
# <a name="out-variable-declarations"></a>Declarações de variável out

O recurso de *declaração de variável out* permite que uma variável seja declarada no local que está sendo passado como um argumento `out`.

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

Uma variável declarada dessa forma é chamada de uma *variável out*. Você pode usar a palavra-chave contextual `var` para o tipo da variável. O escopo será o mesmo para uma variável de *padrão* introduzida por meio de correspondência de padrões.

De acordo com a especificação da linguagem (seção acesso ao elemento 7.6.7), a lista de argumentos de um acesso de elemento (expressão de indexação) não contém argumentos ref ou out. No entanto, eles são permitidos pelo compilador para vários cenários, por exemplo indexadores declarados em metadados que aceitam `out`.

Dentro do escopo de uma variável local introduzida por um argument_value, é um erro de tempo de compilação para se referir a essa variável local em uma posição textual que precede sua declaração.

Também é um erro fazer referência a uma variável de saída de tipo implícito (§ 8.5.1) na mesma lista de argumentos que contém imediatamente sua declaração.

A resolução de sobrecarga é modificada da seguinte maneira:

Adicionamos uma nova conversão:

> Há uma *conversão de expressão* de uma declaração de variável digitada implicitamente para cada tipo.

Também

> O tipo de um argumento de variável explicitamente digitado é o tipo declarado.

e

> Um argumento de variável de tipo implícito não tem nenhum tipo.

A *conversão de expressão* de uma declaração de variável de tipo implícito não é considerada melhor do que qualquer outra *conversão da expressão*.

O tipo de uma variável de digitação implícita é o tipo do parâmetro correspondente na assinatura do método selecionado pela resolução de sobrecarga.

O novo nó de sintaxe `DeclarationExpressionSyntax` é adicionado para representar a declaração em um argumento var out.
