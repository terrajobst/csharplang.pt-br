---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484499"
---
# <a name="digit-separators"></a><span data-ttu-id="e7886-101">Separadores de dígito</span><span class="sxs-lookup"><span data-stu-id="e7886-101">Digit separators</span></span>

<span data-ttu-id="e7886-102">Ser capaz de agrupar dígitos em grandes literais numéricos teria um grande impacto de legibilidade e nenhuma desvantagem significativa.</span><span class="sxs-lookup"><span data-stu-id="e7886-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="e7886-103">A adição de literais binários (#215) aumentaria a probabilidade de literais numéricos serem longos, de modo que os dois recursos se aprimorem.</span><span class="sxs-lookup"><span data-stu-id="e7886-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="e7886-104">Seguimos o Java e outros e usamos um sublinhado `_` como um separador de dígitos.</span><span class="sxs-lookup"><span data-stu-id="e7886-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="e7886-105">Seria possível ocorrer em todos os lugares em um literal numérico (exceto o primeiro e último caractere), já que diferentes agrupamentos podem fazer sentido em cenários diferentes e, especialmente, para diferentes bases numéricas:</span><span class="sxs-lookup"><span data-stu-id="e7886-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="e7886-106">Qualquer sequência de dígitos pode ser separada por sublinhados, possivelmente mais de um sublinhado entre dois dígitos consecutivos.</span><span class="sxs-lookup"><span data-stu-id="e7886-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="e7886-107">Eles são permitidos em decimais, bem como expoentes, mas após a regra anterior, eles podem não aparecer ao lado do decimal (`10_.0`), ao lado do caractere de expoente (`1.1e_1`) ou ao lado do especificador de tipo (`10_f`).</span><span class="sxs-lookup"><span data-stu-id="e7886-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="e7886-108">Quando usado em literais binários e hexadecimais, eles podem não aparecer imediatamente após o `0x` ou `0b`.</span><span class="sxs-lookup"><span data-stu-id="e7886-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="e7886-109">A sintaxe é simples e os separadores não têm impacto semântico-eles são simplesmente ignorados.</span><span class="sxs-lookup"><span data-stu-id="e7886-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="e7886-110">Isso tem um valor amplo e é fácil de implementar.</span><span class="sxs-lookup"><span data-stu-id="e7886-110">This has broad value and is easy to implement.</span></span>
