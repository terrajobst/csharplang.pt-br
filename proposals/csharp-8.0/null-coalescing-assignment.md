---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484954"
---
# <a name="null-coalescing-assignment"></a>Atribuição de coalescência nula

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: abaixo

## <a name="summary"></a>Resumo
[summary]: #summary

Simplifica um padrão de codificação comum em que uma variável é atribuída a um valor se ela for nula.

Como parte dessa proposta, nós também soltaremos os requisitos de tipo em `??` para permitir uma expressão cujo tipo é um parâmetro de tipo irrestrito a ser usado no lado esquerdo.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

É comum ver o código do formulário

```csharp
if (variable == null)
{
    variable = expression;
}
```

Essa proposta adiciona um operador binário não sobrecarregável à linguagem que executa essa função.

Houve pelo menos oito solicitações de comunidade separadas para esse recurso.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Adicionamos um novo formulário de operador de atribuição

``` antlr
assignment_operator
    : '??='
    ;
```

Que segue as [regras semânticas existentes para operadores de atribuição compostos](../../spec/expressions.md#compound-assignment), exceto que elide a atribuição se o lado esquerdo for não nulo. As regras para esse recurso são as seguintes.

Dado `a ??= b`, em que `A` é o tipo de `a`, `B` é o tipo de `b`e `A0` é o tipo subjacente de `A` se `A` for um tipo de valor anulável:

1. Se `A` não existir ou for um tipo de valor não anulável, ocorrerá um erro em tempo de compilação.
2. Se `B` não for implicitamente conversível para `A` ou `A0` (se `A0` existir), ocorrerá um erro em tempo de compilação.
3. Se `A0` existir e `B` for implicitamente conversível para `A0`e `B` não for dinâmico, o tipo de `a ??= b` será `A0`. `a ??= b` é avaliado em tempo de execução como:
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   Exceto que `a` é avaliada apenas uma vez.
4. Caso contrário, o tipo de `a ??= b` é `A`. `a ??= b` é avaliada em tempo de execução como `a ?? (a = b)`, exceto que `a` é avaliada apenas uma vez.


Para o relaxamento dos requisitos de tipo do `??`, atualizamos a especificação em que ele declara atualmente, dado `a ?? b`, em que `A` é o tipo de `a`:

> 1. Se existir e não for um tipo anulável ou um tipo de referência, ocorrerá um erro em tempo de compilação.

Nós liberamos esse requisito para:

1. Se existir e for um tipo de valor não anulável, ocorrerá um erro em tempo de compilação.

Isso permite que o operador de União nulo funcione em parâmetros de tipo irrestrito, pois o parâmetro de tipo não restrito T existe, não é um tipo anulável e não é um tipo de referência.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

O programador pode gravar `(x = x ?? y)`, `if (x == null) x = y;`ou `x ?? (x = y)` manualmente.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [] Requer revisão LDM
- [] Também há suporte para operadores de `&&=` e `||=`?

## <a name="design-meetings"></a>Criar reuniões

None.
