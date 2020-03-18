---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484709"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>Permitir separador de dígitos após 0B ou 0x

Em C# 7,2, estendemos o conjunto de lugares que os separadores de dígitos (o caractere de sublinhado) podem aparecer em literais inteiros. [A partir C# do 7,0, os separadores são permitidos entre os dígitos de um literal](../csharp-7.0/digit-separators.md). Agora, em C# 7,2, também permitimos separadores de dígitos antes do primeiro dígito significativo de um literal binário ou hexadecimal, após o prefixo.

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

Não permitimos que um literal inteiro decimal tenha um sublinhado à esquerda. Um token como `_123` é um identificador.
