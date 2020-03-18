---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484625"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>A indexação de campos de `fixed` não deve exigir fixação independentemente do contexto móvel/não móvel. #

A alteração tem o tamanho de uma correção de bug. Ele pode estar em 7,3 e não entrar em conflito com a direção que demoramos.
Essa alteração é apenas para permitir que o cenário a seguir funcione embora `s` seja móvel. Ele já é válido quando `s` não é móvel. 

Observação: em ambos os casos, ele ainda requer `unsafe` contexto. É possível ler dados não inicializados ou até mesmo fora do intervalo. Que não está mudando.

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

O principal "desafio" que vejo aqui é como explicar o relaxamento na especificação. Em particular, como o seguinte ainda precisaria de fixação. (como `s` é móvel e usamos explicitamente o campo como um ponteiro)

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

Um motivo pelo qual exigimos a fixação do destino quando ele é móvel é o artefato de nossa estratégia de geração de código,-sempre convertemos em um ponteiro não gerenciado e, assim, forçamos o usuário a fixar por meio da instrução `fixed`. No entanto, a conversão para não gerenciado é desnecessária ao fazer a indexação. O mesmo matemática de ponteiro não seguro é igualmente aplicável quando temos o receptor na forma de um ponteiro gerenciado. Se fizermos isso, a referência intermediária será gerenciada (GC-acompanhado) e a fixação será desnecessária.

A alteração https://github.com/dotnet/roslyn/pull/24966 é um protótipo de RP que relaxará esse requisito.
