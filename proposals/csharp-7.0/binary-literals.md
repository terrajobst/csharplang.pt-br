---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484422"
---
# <a name="binary-literals"></a><span data-ttu-id="a380b-101">Literais binários</span><span class="sxs-lookup"><span data-stu-id="a380b-101">Binary literals</span></span>

<span data-ttu-id="a380b-102">Há uma solicitação relativamente comum para adicionar literais binários a C# e VB.</span><span class="sxs-lookup"><span data-stu-id="a380b-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="a380b-103">Para bitmasks (por exemplo, enumerações de sinalizador), isso parece autênticomente útil, mas também seria ótimo apenas para fins educacionais.</span><span class="sxs-lookup"><span data-stu-id="a380b-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="a380b-104">Os literais binários teriam a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="a380b-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="a380b-105">Sintaticamente e semanticamente eles são idênticos a literais hexadecimais, exceto pelo uso de `b`/`B` em vez de `x`/`X`, tendo apenas dígitos `0` e `1` e sendo interpretados na base 2 em vez de 16.</span><span class="sxs-lookup"><span data-stu-id="a380b-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="a380b-106">Há pouco custo para implementá-los e pouca sobrecarga conceitual para os usuários do idioma.</span><span class="sxs-lookup"><span data-stu-id="a380b-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="a380b-107">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="a380b-107">Syntax</span></span>

<span data-ttu-id="a380b-108">A gramática seria a seguinte:</span><span class="sxs-lookup"><span data-stu-id="a380b-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
