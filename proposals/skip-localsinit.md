---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484485"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>Suprimir emissão de `localsinit` sinalizador.

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

Permitir supressão de emissão de `localsinit` sinalizador por meio do atributo `SkipLocalsInitAttribute`. 

## <a name="motivation"></a>Motivação
[motivation]: #motivation


### <a name="background"></a>Tela de fundo
Por variáveis locais de especificação CLR que não contêm referências não são inicializadas para um valor específico pela VM/JIT. A leitura dessas variáveis sem inicialização é de tipo seguro, mas, caso contrário, o comportamento é indefinido e específico da implementação. Normalmente, os locais não inicializados contêm quaisquer valores que foram deixados na memória que agora estão ocupados pelo quadro de pilha. Isso poderia levar a um comportamento não determinístico e difícil reproduzir bugs. 

Há duas maneiras de "atribuir" uma variável local: 
- ao armazenar um valor ou 
- ao especificar `localsinit` sinalizador que força tudo que está alocado para o pool de memória local ser inicializado com zero Observação: isso inclui variáveis locais e `stackalloc` dados.    

O uso de dados não inicializados é desencorajado e não é permitido no código verificável. Embora seja possível provar que, por meio da análise de fluxo, é permitido que o algoritmo de verificação seja conservador e simplesmente exija que `localsinit` esteja definido.

Historicamente C# , o compilador emite `localsinit` sinalizador em todos os métodos que declaram locais.

Embora C# o empregue uma análise de atribuição definitiva, que é mais estrita do que a especificaçãoC# CLR exigida (também precisa considerar o escopo de locais), não é estritamente garantido que o código resultante seria formalmente verificável:
- O CLR C# e as regras podem não concordar se a passagem de um argumento local as `out` é uma `use`.
- O CLR C# e as regras podem não concordar com o tratamento de ramificações condicionais quando as condições são conhecidas (propagação constante).
- O CLR também poderia simplesmente exigir `localinits`, já que isso é permitido.  

### <a name="problem"></a>Problema
No aplicativo de alto desempenho, o custo da inicialização zero forçada pode ser perceptível. É particularmente perceptível quando `stackalloc` é usado.

Em alguns casos, o JIT pode Elide inicialização zero inicial de locais individuais quando tal inicialização é "eliminada" por atribuições subsequentes. Nem todos os JITs fazem isso e essa otimização tem limites. Ele não ajuda com `stackalloc`.

Para ilustrar que o problema é real, há um bug conhecido em que um método que não contém nenhum local de `IL` não teria `localsinit` sinalizador. O bug já está sendo explorado pelos usuários ao colocar `stackalloc` em tais métodos, intencionalmente, para evitar os custos de inicialização. Isso é apesar do fato de que a ausência de `IL` locais é uma métrica instável e pode variar dependendo das alterações na estratégia de codegen. O bug deve ser corrigido e os usuários devem ter uma maneira mais documentada e confiável de suprimir o sinalizador. 

## <a name="detailed-design"></a>Design detalhado

Permitir a especificação de `System.Runtime.CompilerServices.SkipLocalsInitAttribute` como uma maneira de informar ao compilador para não emitir `localsinit` sinalizador.
 
O resultado final disso será que os locais podem não ser inicializados com zero pelo JIT, que está na maioria dos casos inobservados no C#.  
Além disso `stackalloc` dados não serão inicializados com zero. Isso é definitivamente observável, mas também é o cenário mais motivador.

Os destinos de atributo permitidos e reconhecidos são: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`. No entanto, o compilador não exigirá que esse atributo seja definido com os destinos listados, nem irá se preocupar em qual assembly o atributo está definido. 

Quando o atributo é especificado em um contêiner (`class`, `module`, que contém o método para um método aninhado,...), o sinalizador afeta todos os métodos contidos no contêiner.

Os métodos sintetizados "herdam" o sinalizador do contêiner/proprietário lógico. 

O sinalizador afeta apenas a estratégia CodeGen para corpos de métodos reais. I.E. o sinalizador não tem efeito sobre métodos abstratos e não é propagado para substituir/implementar métodos.

Isso é explicitamente um **_recurso de compilador_** e **_não um recurso de linguagem_** .  
Da mesma forma que as opções de linha de comando do compilador, o recurso controla os detalhes de implementação de uma estratégia CodeGen específica C# e não precisa ser exigido pela especificação.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

- Os compiladores antigos/outros podem não honrar o atributo.
Ignorar o atributo é um comportamento compatível. Pode resultar apenas em uma ligeira queda de desempenho.

- O código sem `localinits` sinalizador pode disparar falhas de verificação.
Os usuários que solicitam esse recurso geralmente não são preocupados com a capacidade de verificação. 
 
- A aplicação do atributo em níveis mais altos do que um método individual tem efeito não local, que é observável quando `stackalloc` é usado. Ainda assim, esse é o cenário mais solicitado.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

- Omita `localinits` sinalizar quando o método for declarado no contexto de `unsafe`. Isso poderia causar uma alteração de comportamento silencioso e perigoso de determinístico para não determinístico em um caso de `stackalloc`.

- Omita `localinits` sinalizar sempre.
Ainda pior do que acima.

- Omita `localinits` sinalizador, a menos que `stackalloc` seja usado no corpo do método.
Não aborda o cenário mais solicitado e pode desativar o código não verificável sem a opção de revertê-lo de volta.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- O atributo deve ser realmente emitido para os metadados? 

## <a name="design-meetings"></a>Criar reuniões

Nenhum ainda. 