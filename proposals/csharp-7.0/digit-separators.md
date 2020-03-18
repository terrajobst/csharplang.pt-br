---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484499"
---
# <a name="digit-separators"></a>Separadores de dígito

Ser capaz de agrupar dígitos em grandes literais numéricos teria um grande impacto de legibilidade e nenhuma desvantagem significativa. 

A adição de literais binários (#215) aumentaria a probabilidade de literais numéricos serem longos, de modo que os dois recursos se aprimorem. 

Seguimos o Java e outros e usamos um sublinhado `_` como um separador de dígitos. Seria possível ocorrer em todos os lugares em um literal numérico (exceto o primeiro e último caractere), já que diferentes agrupamentos podem fazer sentido em cenários diferentes e, especialmente, para diferentes bases numéricas:

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

Qualquer sequência de dígitos pode ser separada por sublinhados, possivelmente mais de um sublinhado entre dois dígitos consecutivos. Eles são permitidos em decimais, bem como expoentes, mas após a regra anterior, eles podem não aparecer ao lado do decimal (`10_.0`), ao lado do caractere de expoente (`1.1e_1`) ou ao lado do especificador de tipo (`10_f`). Quando usado em literais binários e hexadecimais, eles podem não aparecer imediatamente após o `0x` ou `0b`.

A sintaxe é simples e os separadores não têm impacto semântico-eles são simplesmente ignorados.

Isso tem um valor amplo e é fácil de implementar.
