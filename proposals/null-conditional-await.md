---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484492"
---
# <a name="null-conditional-await"></a>NULL-condicional Await

* [x] proposta
* [] Protótipo: nenhum
* [] Implementação: nenhuma
* [] Especificação: iniciada, abaixo

## <a name="summary"></a>Resumo
[summary]: #summary

Dá suporte a uma expressão do formulário `await? e`, que aguarda `e` se ele não for nulo, caso contrário, ele resultará em `null`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Esse é um padrão de codificação comum, e esse recurso teria uma boa sinergia com os operadores existentes de propagação de nulos e de União nula.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Adicionamos um novo formulário da *await_expression*:

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

O operador de `await` condicional nulo aguardará seu operando somente se esse operando for não nulo. Caso contrário, o resultado da aplicação do operador é NULL.

O tipo do resultado é computado usando as [regras para o operador NULL-Conditional](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).

> **Observação:** Se `e` for do tipo `Task`, `await? e;` não faria nada se `e` for `null`e Await `e` se não for `null`.
>
> Se `e` for do tipo `Task<K>` em que `K` é um tipo de valor, `await? e` resultaria em um valor do tipo `K?`.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Embora exija algum código clichê, os usos desse operador geralmente podem ser substituídos por uma expressão algo como `(e == null) ? null : await e` ou uma instrução como `if (e != null) await e`.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [] Requer revisão LDM

## <a name="design-meetings"></a>Criar reuniões

None.
