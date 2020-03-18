---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484513"
---
# <a name="c-language-proposals"></a>C#Propostas de linguagem

As propostas de linguagem são documentos dinâmicos que descrevem a reflexão atual sobre um determinado recurso de idioma.

As propostas podem estar *ativas*, *inativas*, *rejeitadas* ou *concluídas*. As propostas *ativas* são armazenadas diretamente na pasta propostas, as propostas *inativas* e *rejeitadas* são armazenadas nas subpastas [inativas](proposals/inactive) e [rejeitadas](proposals/rejected) , e as propostas *feitas* são arquivadas em uma pasta correspondente à versão de idioma da qual fazem parte.

## <a name="lifetime-of-a-proposal"></a>Tempo de vida de uma proposta

Uma proposta inicia sua vida quando a equipe de design de linguagem decide que ela pode fazer uma boa adição à linguagem por dia. Normalmente, ele começará a ficar *ativo*, mas se quisermos capturar uma ideia sem querer trabalhar nele agora, uma proposta também poderá ser iniciada na subpasta *inativa* . As propostas podem até mesmo começar diretamente no estado *rejeitado* , se quisermos fazer um registro de algo que não pretendemos fazer. Por exemplo, se uma solicitação recorrente e popular não for possível implementar, podemos capturá-la como uma proposta rejeitada.

A proposta pode começar como uma ideia em um [problema de discussão](https://github.com/dotnet/csharplang/labels/Discussion), ou pode vir de discussões na reunião de design de linguagem ou chegar de muitas outras frentes. A principal coisa é que a equipe de design deve ser feita e que há alguém que está disposto a escrevê-la. Normalmente, um membro da equipe de design de linguagem se atribuirá como um especialista para o recurso, acompanhado por um [problema de especialista](https://github.com/dotnet/csharplang/labels/Proposal%20champion). O especialista é responsável por mover a proposta por meio do processo de design.

Uma proposta estará *ativa* se estiver avançando no design e na implementação em direção a uma versão futura. Quando ele estiver completamente *concluído*, ou seja, uma implementação foi mesclada em uma versão e o recurso foi especificado, ele será movido para um subdiretório correspondente ao seu lançamento.

Se um recurso não for mais provável de deixá-lo na linguagem, por exemplo, porque isso não é possível, não parece adicionar um valor suficiente ou simplesmente não é adequado para a linguagem, ele será [rejeitado](proposals/rejected). Se um recurso tiver uma promessa razoável, mas não estiver sendo priorizada no momento para trabalhar, poderá ser declarado [inativo](proposals/inactive) para evitar a confusão da pasta principal. É perfeitamente bom que o trabalho aconteça em propostas inativas ou rejeitadas e para que eles sejam ressuscitada mais tarde. As categorias estão lá para refletir a tentativa de design atual.

## <a name="nature-of-a-proposal"></a>Natureza de uma proposta

Uma proposta deve seguir o [modelo de proposta](proposal-template.md). Uma boa proposta:

- Ajustar-se ao espírito geral e estética da linguagem.
- Não apresente sintaxe de alternativa sutilmente para recursos existentes.
- Adicione um grande valor para um conjunto claro de usuários.
- Não adicione significativamente à complexidade da linguagem, especialmente para novos usuários.  

## <a name="discussion-of-proposals"></a>Discussão de propostas

Os comentários e a discussão acontecem em [problemas de discussão](https://github.com/dotnet/csharplang/labels/Discussion). Quando uma nova proposta é adicionada à pasta propostas, ela deve ser anunciada em um problema de discussão pelo especialista ou autor da proposta. 

 