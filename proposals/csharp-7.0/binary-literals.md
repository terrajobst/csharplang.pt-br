---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484422"
---
# <a name="binary-literals"></a>Literais binários

Há uma solicitação relativamente comum para adicionar literais binários a C# e VB. Para bitmasks (por exemplo, enumerações de sinalizador), isso parece autênticomente útil, mas também seria ótimo apenas para fins educacionais.

Os literais binários teriam a seguinte aparência:

```csharp
int nineteen = 0b10011;
```

Sintaticamente e semanticamente eles são idênticos a literais hexadecimais, exceto pelo uso de `b`/`B` em vez de `x`/`X`, tendo apenas dígitos `0` e `1` e sendo interpretados na base 2 em vez de 16.

Há pouco custo para implementá-los e pouca sobrecarga conceitual para os usuários do idioma.

## <a name="syntax"></a>Sintaxe

A gramática seria a seguinte:

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
