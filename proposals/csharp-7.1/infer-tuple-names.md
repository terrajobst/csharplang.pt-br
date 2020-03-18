---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484716"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>Inferir nomes de tupla (também conhecido como. inicializadores de projeção de tupla)

## <a name="summary"></a>Resumo
[summary]: #summary

Em vários casos comuns, esse recurso permite que os nomes de elementos de tupla sejam omitidos e, em vez disso, sejam inferidos. Por exemplo, em vez de digitar `(f1: x.f1, f2: x?.f2)`, os nomes de elemento "F1" e "F2" podem ser inferidos de `(x.f1, x?.f2)`.

Isso paraleliza o comportamento de tipos anônimos, que permitem inferir nomes de membros durante a criação. Por exemplo, `new { x.f1, y?.f2 }` declara os membros "F1" e "F2".

Isso é particularmente útil ao usar tuplas no LINQ:

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Há duas partes para a alteração:

1.  Tente inferir um nome de candidato para cada elemento de tupla que não tem um nome explícito:
    -   Usando as mesmas regras que a inferência de nomes para tipos anônimos.
        - No C#, isso permite três casos: `y` (identificador), `x.y` (acesso de membro simples) e `x?.y` (acesso condicional).
        - No VB, isso permite casos adicionais, como `x.y()`.
    -   Rejeitar nomes de tupla reservada (diferencia maiúsculas C#de minúsculas, não diferencia maiúsculas de minúsculas no VB), pois elas são proibidas ou já implícitas. Por exemplo, como `ItemN`, `Rest`e `ToString`.
    -   Se qualquer nome de candidato for duplicado (diferencia maiúsculas C#de minúsculas, não diferencia maiúsculas de minúsculas no VB) em toda a tupla, descartamos esses candidatos,
2.  Durante as conversões (que verificam e avisam sobre descartar nomes de literais de tupla), nomes deduzidos não produzirão nenhum aviso. Isso evita a interrupção do código de tupla existente.

Observe que a regra para lidar com duplicatas é diferente daquela para tipos anônimos. Por exemplo, `new { x.f1, x.f1 }` produz um erro, mas `(x.f1, x.f1)` ainda seria permitido (apenas sem nenhum nome deduzido). Isso evita a interrupção do código de tupla existente.

Para consistência, o mesmo se aplicaria a tuplas produzidas por degroup-assignments (in C#):

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

O mesmo também se aplicaria a tuplas VB, usando as regras específicas do VB para inferir o nome da expressão e comparações de nome que não diferenciam maiúsculas de minúsculas.

Ao usar o C# compilador 7,1 (ou posterior) com a versão de idioma "7,0", os nomes de elemento serão inferidos (apesar de o recurso não estar disponível), mas haverá um erro de site de uso para tentar acessá-los. Isso limitará adições de novo código que posteriormente enfrentaria o problema de compatibilidade (descrito abaixo).

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

A principal desvantagem é que isso introduz uma interrupção de compatibilidade C# de 7,0:

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

O Conselho de compatibilidade descobriu essa interrupção aceitável, Considerando que ela é limitada e a janela de tempo desde que as tuplas enviadas (em C# 7,0) é curta.

## <a name="references"></a>Referências
- [LDM 4º 2017 de abril](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Discussão do GitHub](https://github.com/dotnet/csharplang/issues/370) (Obrigado @alrz por trazer esse problema para cima)
- [Design de tuplas](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
