---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484709"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a><span data-ttu-id="ce356-101">Permitir separador de dígitos após 0B ou 0x</span><span class="sxs-lookup"><span data-stu-id="ce356-101">Allow digit separator after 0b or 0x</span></span>

<span data-ttu-id="ce356-102">Em C# 7,2, estendemos o conjunto de lugares que os separadores de dígitos (o caractere de sublinhado) podem aparecer em literais inteiros.</span><span class="sxs-lookup"><span data-stu-id="ce356-102">In C# 7.2, we extend the set of places that digit separators (the underscore character) can appear in integral literals.</span></span> <span data-ttu-id="ce356-103">[A partir C# do 7,0, os separadores são permitidos entre os dígitos de um literal](../csharp-7.0/digit-separators.md).</span><span class="sxs-lookup"><span data-stu-id="ce356-103">[Beginning in C# 7.0, separators are permitted between the digits of a literal](../csharp-7.0/digit-separators.md).</span></span> <span data-ttu-id="ce356-104">Agora, em C# 7,2, também permitimos separadores de dígitos antes do primeiro dígito significativo de um literal binário ou hexadecimal, após o prefixo.</span><span class="sxs-lookup"><span data-stu-id="ce356-104">Now, in C# 7.2, we also permit digit separators before the first significant digit of a binary or hexadecimal literal, after the prefix.</span></span>

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

<span data-ttu-id="ce356-105">Não permitimos que um literal inteiro decimal tenha um sublinhado à esquerda.</span><span class="sxs-lookup"><span data-stu-id="ce356-105">We do not permit a decimal integer literal to have a leading underscore.</span></span> <span data-ttu-id="ce356-106">Um token como `_123` é um identificador.</span><span class="sxs-lookup"><span data-stu-id="ce356-106">A token such as `_123` is an identifier.</span></span>
