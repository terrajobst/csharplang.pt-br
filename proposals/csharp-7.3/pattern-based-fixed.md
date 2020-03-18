---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484653"
---
# <a name="pattern-based-fixed-statement"></a>Declaração `fixed` baseada em padrão

## <a name="summary"></a>Resumo
[summary]: #summary

Introduza um padrão que permitiria que os tipos participem de instruções `fixed`. 

## <a name="motivation"></a>Motivação
[motivation]: #motivation

O idioma fornece um mecanismo para fixar dados gerenciados e obter um ponteiro nativo para o buffer subjacente.

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

O conjunto de tipos que podem participar de `fixed` é codificado e limitado a matrizes e `System.String`. O código de tipos "especiais" não é dimensionado quando novos primitivos, como `ImmutableArray<T>`, `Span<T>``Utf8String` são introduzidos. 

Além disso, a solução atual para `System.String` se baseia em uma API razoavelmente rígida. A forma da API implica que `System.String` é um objeto contíguo que incorpora dados codificados UTF16 em um deslocamento fixo do cabeçalho do objeto. Essa abordagem foi encontrada com problemas em várias propostas que poderiam exigir alterações no layout subjacente. Seria desejável poder mudar para algo mais flexível que desassocie `System.String` objeto da sua representação interna para fins de interoperabilidade não gerenciada. 

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

## <a name="pattern"></a>*Padrão* ##
Uma "fixa" com base em padrões viável precisa:
-   Forneça as referências gerenciadas para fixar a instância e inicializar o ponteiro (preferivelmente esta é a mesma referência)
-   Transmita sem ambigüidade o tipo do elemento não gerenciado (ou seja, "char" para "String")
-   Prescrever o comportamento no caso "Empty" quando não houver nada para fazer referência a ele. 
-   Não deve enviar por push autores de API para decisões de design que prejudicam o uso do tipo fora do `fixed`.

Acho que a acima pode ser satisfeita reconhecendo um membro de retorno de referência de ref-renomeado especialmente: `ref [readonly] T GetPinnableReference()`.

Para ser usado pela instrução `fixed`, as seguintes condições devem ser atendidas:

1. Há apenas um desses membros fornecido para um tipo.
1. Retorna por `ref` ou `ref readonly`. (`readonly` é permitido para que os autores de tipos imutáveis/ReadOnly pudessem implementar o padrão sem adicionar uma API gravável que pudesse ser usada em código seguro)
1. T é um tipo não gerenciado.
(como `T*` se torna o tipo de ponteiro. Naturalmente, a restrição expandirá se/quando a noção de "não gerenciado" for expandida)
1. Retorna `nullptr` gerenciado quando não há dados para fixar – provavelmente a maneira mais barata de transmitir a esvaziação.
(Observe que "" a cadeia de caracteres retorna uma referência a ' \ 0 ', uma vez que as cadeias são terminadas em nulo)

Como alternativa para o `#3`, podemos permitir que o resultado em casos vazios seja indefinido ou específico da implementação. No entanto, isso pode tornar a API mais perigosa e propenso a abusos e incômodos de compatibilidade indesejados. 

## <a name="translation"></a>*Tradução* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

torna-se o pseudocódigo a seguir (nem C#todos expressos no)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

- O GetPinnableReference destina-se a ser usado somente em `fixed`, mas nada impede seu uso em código seguro, portanto, o implementador deve ter isso em mente.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Os usuários podem introduzir GetPinnableReference ou membro semelhante e usá-lo como
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

Não há solução para `System.String` se for desejada uma solução alternativa.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [] Comportamento no estado "Empty". - `nullptr` ou `undefined`? 
- [] Os métodos de extensão devem ser considerados? 
- [] Se um padrão for detectado em `System.String`, ele deverá vencer? 

## <a name="design-meetings"></a>Criar reuniões

Nenhum ainda. 
