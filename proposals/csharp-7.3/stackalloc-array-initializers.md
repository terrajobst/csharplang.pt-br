---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484604"
---
# <a name="stackalloc-array-initializers"></a>Inicializadores de matriz stackalloc

## <a name="summary"></a>Resumo
[summary]: #summary

Permitir que a sintaxe do inicializador de matriz seja usada com `stackalloc`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Matrizes comuns podem ter seus elementos inicializados no momento da criação. Parece razoável permitir isso em `stackalloc` caso.

A pergunta sobre por que essa sintaxe não é permitida com `stackalloc` surge com muita frequência.  
Consulte, por exemplo, [#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>Design detalhado

Matrizes comuns podem ser criadas por meio da seguinte sintaxe:

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

Devemos permitir que matrizes alocadas da pilha sejam criadas por meio de:  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

A semântica de todos os casos é praticamente a mesma que com matrizes.  
Por exemplo: no último caso, o tipo de elemento é inferido do inicializador e deve ser um tipo "não gerenciado".

Observação: o recurso não depende do destino ser um `Span<T>`. Ele é tão aplicável em `T*` caso, portanto, não parece razoável para predicar a ti em `Span<T>` caso.  

## <a name="translation"></a>{1&gt;Tradução&lt;1}

A implementação ingênua poderia simplesmente inicializar a matriz logo após a criação por meio de uma série de atribuições de elemento.  

De maneira semelhante ao caso com matrizes, pode ser possível e desejável detectar casos em que todos ou a maioria dos elementos são tipos blittable e usar técnicas mais eficientes copiando o estado pré-criado de todos os elementos constantes. 

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Esse é um recurso de conveniência. É possível simplesmente não fazer nada.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Criar reuniões

Nenhum ainda. 
