---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484625"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="38598-101">A indexação de campos de `fixed` não deve exigir fixação independentemente do contexto móvel/não móvel.</span><span class="sxs-lookup"><span data-stu-id="38598-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="38598-102">A alteração tem o tamanho de uma correção de bug.</span><span class="sxs-lookup"><span data-stu-id="38598-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="38598-103">Ele pode estar em 7,3 e não entrar em conflito com a direção que demoramos.</span><span class="sxs-lookup"><span data-stu-id="38598-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="38598-104">Essa alteração é apenas para permitir que o cenário a seguir funcione embora `s` seja móvel.</span><span class="sxs-lookup"><span data-stu-id="38598-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="38598-105">Ele já é válido quando `s` não é móvel.</span><span class="sxs-lookup"><span data-stu-id="38598-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="38598-106">Observação: em ambos os casos, ele ainda requer `unsafe` contexto.</span><span class="sxs-lookup"><span data-stu-id="38598-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="38598-107">É possível ler dados não inicializados ou até mesmo fora do intervalo.</span><span class="sxs-lookup"><span data-stu-id="38598-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="38598-108">Que não está mudando.</span><span class="sxs-lookup"><span data-stu-id="38598-108">That is not changing.</span></span>

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

<span data-ttu-id="38598-109">O principal "desafio" que vejo aqui é como explicar o relaxamento na especificação. Em particular, como o seguinte ainda precisaria de fixação.</span><span class="sxs-lookup"><span data-stu-id="38598-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="38598-110">(como `s` é móvel e usamos explicitamente o campo como um ponteiro)</span><span class="sxs-lookup"><span data-stu-id="38598-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

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

<span data-ttu-id="38598-111">Um motivo pelo qual exigimos a fixação do destino quando ele é móvel é o artefato de nossa estratégia de geração de código,-sempre convertemos em um ponteiro não gerenciado e, assim, forçamos o usuário a fixar por meio da instrução `fixed`.</span><span class="sxs-lookup"><span data-stu-id="38598-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="38598-112">No entanto, a conversão para não gerenciado é desnecessária ao fazer a indexação.</span><span class="sxs-lookup"><span data-stu-id="38598-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="38598-113">O mesmo matemática de ponteiro não seguro é igualmente aplicável quando temos o receptor na forma de um ponteiro gerenciado.</span><span class="sxs-lookup"><span data-stu-id="38598-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="38598-114">Se fizermos isso, a referência intermediária será gerenciada (GC-acompanhado) e a fixação será desnecessária.</span><span class="sxs-lookup"><span data-stu-id="38598-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="38598-115">A alteração https://github.com/dotnet/roslyn/pull/24966 é um protótipo de RP que relaxará esse requisito.</span><span class="sxs-lookup"><span data-stu-id="38598-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
