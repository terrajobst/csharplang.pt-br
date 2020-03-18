---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484513"
---
# <a name="c-language-proposals"></a><span data-ttu-id="417a3-101">C#Propostas de linguagem</span><span class="sxs-lookup"><span data-stu-id="417a3-101">C# Language Proposals</span></span>

<span data-ttu-id="417a3-102">As propostas de linguagem são documentos dinâmicos que descrevem a reflexão atual sobre um determinado recurso de idioma.</span><span class="sxs-lookup"><span data-stu-id="417a3-102">Language proposals are living documents describing the current thinking about a given language feature.</span></span>

<span data-ttu-id="417a3-103">As propostas podem estar *ativas*, *inativas*, *rejeitadas* ou *concluídas*.</span><span class="sxs-lookup"><span data-stu-id="417a3-103">Proposals can be either *active*, *inactive*, *rejected* or *done*.</span></span> <span data-ttu-id="417a3-104">As propostas *ativas* são armazenadas diretamente na pasta propostas, as propostas *inativas* e *rejeitadas* são armazenadas nas subpastas [inativas](proposals/inactive) e [rejeitadas](proposals/rejected) , e as propostas *feitas* são arquivadas em uma pasta correspondente à versão de idioma da qual fazem parte.</span><span class="sxs-lookup"><span data-stu-id="417a3-104">*Active* proposals are stored directly in the proposals folder, *inactive* and *rejected* proposals are stored in the [inactive](proposals/inactive) and [rejected](proposals/rejected) subfolders, and *done* proposals are archived in a folder corresponding to the language version they are part of.</span></span>

## <a name="lifetime-of-a-proposal"></a><span data-ttu-id="417a3-105">Tempo de vida de uma proposta</span><span class="sxs-lookup"><span data-stu-id="417a3-105">Lifetime of a proposal</span></span>

<span data-ttu-id="417a3-106">Uma proposta inicia sua vida quando a equipe de design de linguagem decide que ela pode fazer uma boa adição à linguagem por dia.</span><span class="sxs-lookup"><span data-stu-id="417a3-106">A proposal starts its life when the language design team decides that it might make a good addition to the language some day.</span></span> <span data-ttu-id="417a3-107">Normalmente, ele começará a ficar *ativo*, mas se quisermos capturar uma ideia sem querer trabalhar nele agora, uma proposta também poderá ser iniciada na subpasta *inativa* .</span><span class="sxs-lookup"><span data-stu-id="417a3-107">Typically it will start out being *active*, but if we want to capture an idea without wanting to work on it right now, a proposal can also start out in the *inactive* subfolder.</span></span> <span data-ttu-id="417a3-108">As propostas podem até mesmo começar diretamente no estado *rejeitado* , se quisermos fazer um registro de algo que não pretendemos fazer.</span><span class="sxs-lookup"><span data-stu-id="417a3-108">Proposals may even start out directly in the *rejected* state, if we want to make a record of something we don't intend to do.</span></span> <span data-ttu-id="417a3-109">Por exemplo, se uma solicitação recorrente e popular não for possível implementar, podemos capturá-la como uma proposta rejeitada.</span><span class="sxs-lookup"><span data-stu-id="417a3-109">For instance, if a popular and recurring request is not possible to implement, we can capture that as a rejected proposal.</span></span>

<span data-ttu-id="417a3-110">A proposta pode começar como uma ideia em um [problema de discussão](https://github.com/dotnet/csharplang/labels/Discussion), ou pode vir de discussões na reunião de design de linguagem ou chegar de muitas outras frentes.</span><span class="sxs-lookup"><span data-stu-id="417a3-110">The proposal may start out as an idea in a [discussion issue](https://github.com/dotnet/csharplang/labels/Discussion), or it may come from discussions in the Language Design Meeting, or arrive from many other fronts.</span></span> <span data-ttu-id="417a3-111">A principal coisa é que a equipe de design deve ser feita e que há alguém que está disposto a escrevê-la.</span><span class="sxs-lookup"><span data-stu-id="417a3-111">The main thing is that the design team feels that it should be done, and that there's someone who is willing to write it up.</span></span> <span data-ttu-id="417a3-112">Normalmente, um membro da equipe de design de linguagem se atribuirá como um especialista para o recurso, acompanhado por um [problema de especialista](https://github.com/dotnet/csharplang/labels/Proposal%20champion).</span><span class="sxs-lookup"><span data-stu-id="417a3-112">Typically a member of the Language Design Team will assign themselves as a champion for the feature, tracked by a [Champion issue](https://github.com/dotnet/csharplang/labels/Proposal%20champion).</span></span> <span data-ttu-id="417a3-113">O especialista é responsável por mover a proposta por meio do processo de design.</span><span class="sxs-lookup"><span data-stu-id="417a3-113">The champion is responsible for moving the proposal through the design process.</span></span>

<span data-ttu-id="417a3-114">Uma proposta estará *ativa* se estiver avançando no design e na implementação em direção a uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="417a3-114">A proposal is *active* if it is moving forward through design and implementation toward an upcoming release.</span></span> <span data-ttu-id="417a3-115">Quando ele estiver completamente *concluído*, ou seja, uma implementação foi mesclada em uma versão e o recurso foi especificado, ele será movido para um subdiretório correspondente ao seu lançamento.</span><span class="sxs-lookup"><span data-stu-id="417a3-115">Once it is completely *done*, i.e. an implementation has been merged into a release and the feature has been specified, it is moved into a subdirectory corresponding to its release.</span></span>

<span data-ttu-id="417a3-116">Se um recurso não for mais provável de deixá-lo na linguagem, por exemplo, porque isso não é possível, não parece adicionar um valor suficiente ou simplesmente não é adequado para a linguagem, ele será [rejeitado](proposals/rejected).</span><span class="sxs-lookup"><span data-stu-id="417a3-116">If a feature turns out not to be likely to make it into the language at all, e.g. because it proves unfeasible, does not seem to add enough value or just isn't right for the language, it will be [rejected](proposals/rejected).</span></span> <span data-ttu-id="417a3-117">Se um recurso tiver uma promessa razoável, mas não estiver sendo priorizada no momento para trabalhar, poderá ser declarado [inativo](proposals/inactive) para evitar a confusão da pasta principal.</span><span class="sxs-lookup"><span data-stu-id="417a3-117">If a feature has reasonable promise but is not currently being prioritized to work on, it may be declared [inactive](proposals/inactive) to avoid cluttering the main folder.</span></span> <span data-ttu-id="417a3-118">É perfeitamente bom que o trabalho aconteça em propostas inativas ou rejeitadas e para que eles sejam ressuscitada mais tarde.</span><span class="sxs-lookup"><span data-stu-id="417a3-118">It is perfectly fine for work to happen on inactive or rejected proposals, and for them to be resurrected later.</span></span> <span data-ttu-id="417a3-119">As categorias estão lá para refletir a tentativa de design atual.</span><span class="sxs-lookup"><span data-stu-id="417a3-119">The categories are there to reflect current design intent.</span></span>

## <a name="nature-of-a-proposal"></a><span data-ttu-id="417a3-120">Natureza de uma proposta</span><span class="sxs-lookup"><span data-stu-id="417a3-120">Nature of a proposal</span></span>

<span data-ttu-id="417a3-121">Uma proposta deve seguir o [modelo de proposta](proposal-template.md).</span><span class="sxs-lookup"><span data-stu-id="417a3-121">A proposal should follow the [proposal template](proposal-template.md).</span></span> <span data-ttu-id="417a3-122">Uma boa proposta:</span><span class="sxs-lookup"><span data-stu-id="417a3-122">A good proposal should:</span></span>

- <span data-ttu-id="417a3-123">Ajustar-se ao espírito geral e estética da linguagem.</span><span class="sxs-lookup"><span data-stu-id="417a3-123">Fit with the general spirit and aesthetic of the language.</span></span>
- <span data-ttu-id="417a3-124">Não apresente sintaxe de alternativa sutilmente para recursos existentes.</span><span class="sxs-lookup"><span data-stu-id="417a3-124">Not introduce subtly alternate syntax for existing features.</span></span>
- <span data-ttu-id="417a3-125">Adicione um grande valor para um conjunto claro de usuários.</span><span class="sxs-lookup"><span data-stu-id="417a3-125">Add a lot of value for a clear set of users.</span></span>
- <span data-ttu-id="417a3-126">Não adicione significativamente à complexidade da linguagem, especialmente para novos usuários.</span><span class="sxs-lookup"><span data-stu-id="417a3-126">Not add significantly to the complexity of the language, especially for new users.</span></span>  

## <a name="discussion-of-proposals"></a><span data-ttu-id="417a3-127">Discussão de propostas</span><span class="sxs-lookup"><span data-stu-id="417a3-127">Discussion of proposals</span></span>

<span data-ttu-id="417a3-128">Os comentários e a discussão acontecem em [problemas de discussão](https://github.com/dotnet/csharplang/labels/Discussion).</span><span class="sxs-lookup"><span data-stu-id="417a3-128">Feedback and discussion happens in [discussion issues](https://github.com/dotnet/csharplang/labels/Discussion).</span></span> <span data-ttu-id="417a3-129">Quando uma nova proposta é adicionada à pasta propostas, ela deve ser anunciada em um problema de discussão pelo especialista ou autor da proposta.</span><span class="sxs-lookup"><span data-stu-id="417a3-129">When a new proposal is added to the proposals folder, it should be announced in a discussion issue by the champion or proposal author.</span></span> 

 