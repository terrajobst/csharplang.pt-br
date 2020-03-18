---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484583"
---
# <a name="fixed-sized-buffers"></a>Buffers de tamanho fixo

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

Forneça um mecanismo de uso geral e seguro para declarar buffers de tamanho fixo para C# o idioma.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Hoje, os usuários têm a capacidade de criar buffers de tamanho fixo em um contexto não seguro. No entanto, isso exige que o usuário lide com ponteiros, execute verificações de limites manualmente e só dê suporte a um conjunto limitado de tipos (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`e `double`).

A reclamação mais comum é que os buffers de tamanho fixo não podem ser indexados em código seguro. A incapacidade de usar mais tipos é a segunda.

Com alguns ajustes secundários, poderíamos fornecer buffers de tamanho fixo de uso geral que oferecem suporte a qualquer tipo, pode ser usado em um contexto seguro e ter a verificação de limites automáticos realizada.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Um declararia um buffer de tamanho fixo seguro por meio do seguinte:

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

A declaração seria convertida em uma representação interna pelo compilador que é semelhante ao seguinte

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

Como esses buffers de tamanho fixo não exigem mais o uso de `fixed`, faz sentido permitir qualquer tipo de elemento.  

> Observação: `fixed` ainda terá suporte, mas somente se o tipo de elemento for `blittable`

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

* Pode haver alguns desafios com compatibilidade com versões anteriores, mas, Considerando que os buffers de tamanho fixo existentes funcionem apenas com uma seleção de tipos primitivos, deve ser possível que o compilador Continue "apenas trabalhando" se o usuário tratar o buffer fixo como um refere.
* Construções incompatíveis podem precisar usar codificação de `v2` ligeiramente diferente para ocultar os campos do compilador antigo.
* A embalagem não está bem definida na especificação de IL para tipos genéricos. Embora a abordagem deva funcionar, estamos nos estornos em comportamento não documentado. Devemos fazer isso documentado e verificar se outros JITss, como o mono, têm o mesmo comportamento.
* A especificação de um tipo separado para cada comprimento (possivelmente outro para `readonly` campos, se houver suporte) terá impacto sobre os metadados. Ele será associado pelo número de matrizes de tamanhos diferentes no aplicativo especificado.
* `ref` matemática não é formalmente verificável (pois não é segura). Precisaremos encontrar uma maneira de atualizar as regras de verificação para saber que nosso uso está ok.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Declare manualmente suas estruturas e use código não seguro para construir indexadores.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- Devemos permitir `readonly`?  (com o indexador ReadOnly)
- Devemos permitir inicializadores de matriz?
- `fixed` palavra-chave é necessária?
- `foreach`?
- apenas campos de instância em structs?

## <a name="design-meetings"></a>Criar reuniões

Vincule a anotações de design que afetam essa proposta e descreva em uma frase para cada alteração que elas levaram.