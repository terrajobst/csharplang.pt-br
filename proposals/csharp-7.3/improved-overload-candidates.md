---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484590"
---
# <a name="improved-overload-candidates"></a>Candidatos de sobrecarga aprimorados

## <a name="summary"></a>Resumo
[summary]: #summary

As regras de resolução de sobrecarga foram atualizadas em C# quase todas as atualizações de idioma para melhorar a experiência de programadores, fazendo invocações ambíguas selecionando a opção "óbvia". Isso deve ser feito cuidadosamente para preservar a compatibilidade com versões anteriores, mas como geralmente estamos resolvendo o que seria casos de erro, esses aprimoramentos geralmente funcionam bem.

1. Quando um grupo de métodos contém membros de instância e estáticos, descartamos os membros da instância se forem invocados sem um receptor de instância ou contexto e descartaremos os membros estáticos se forem invocados com um receptor de instância. Quando não há receptor, incluímos somente membros estáticos em um contexto estático, caso contrário, os membros estáticos e de instância. Quando o receptor é ambíguomente uma instância ou tipo devido a uma situação de cor de cor, incluímos ambos. Um contexto estático, no qual um receptor de instância implícito não pode ser usado, inclui o corpo de membros onde não é definido, como membros estáticos, bem como locais onde isso não pode ser usado, como inicializadores de campo e inicializadores de construtor.
2. Quando um grupo de métodos contém alguns métodos genéricos cujos argumentos de tipo não satisfazem suas restrições, esses membros são removidos do conjunto de candidatos.
3. Para uma conversão de grupo de métodos, os métodos candidatos cujo tipo de retorno não corresponda ao tipo de retorno do delegado são removidos do conjunto.
