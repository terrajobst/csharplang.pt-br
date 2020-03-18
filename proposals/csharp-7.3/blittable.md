---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484618"
---
# <a name="unmanaged-type-constraint"></a>Restrição de tipo não gerenciado

## <a name="summary"></a>Resumo
[summary]: #summary

O recurso de restrição não gerenciado fornecerá a imposição de linguagem para a classe de tipos conhecida como "tipos não gerenciados C# " na especificação da linguagem.  Isso é definido na seção 18,2 como um tipo que não é um tipo de referência e não contém campos de tipo de referência em nenhum nível de aninhamento.  

## <a name="motivation"></a>Motivação
[motivation]: #motivation

A principal motivação é facilitar o autor do código de interoperabilidade de nível C#baixo no. Os tipos não gerenciados são um dos principais blocos de construção do código de interoperabilidade, mas a falta de suporte em genéricos torna impossível criar rotinas reutilizáveis em todos os tipos não gerenciados. Em vez disso, os desenvolvedores são forçados a criar o mesmo código de placa para cada tipo não gerenciado em sua biblioteca:

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

Para habilitar esse tipo de cenário, o idioma apresentará uma nova restrição: não gerenciado:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

Essa restrição só pode ser atendida por tipos que se encaixam na definição de tipo não gerenciado C# na especificação da linguagem. Outra maneira de observar isso é que um tipo satisfaz a restrição não gerenciada IFF ela também pode ser usada como um ponteiro. 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

Os parâmetros de tipo com a restrição não gerenciada podem usar todos os recursos disponíveis para tipos não gerenciados: ponteiros, fixos, etc... 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

Essa restrição também possibilitará ter conversões eficientes entre dados estruturados e fluxos de bytes. Essa é uma operação que é comum em pilhas de rede e camadas de serialização:

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

Essas rotinas são vantajosas porque são provavelmente seguras no tempo de compilação e na alocação gratuita.  Os autores de interoperabilidade atualmente não podem fazer isso (embora seja em uma camada em que o desempenho é crítico).  Em vez disso, eles precisam contar com a alocação de rotinas que têm verificações de tempo de execução caras para verificar se os valores estão corretamente não gerenciados.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

O idioma apresentará uma nova restrição chamada `unmanaged`. Para atender a essa restrição, um tipo deve ser uma struct e todos os campos do tipo devem se enquadrar em uma das seguintes categorias:

- O tipo `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` ou `UIntPtr`.
- Ser qualquer tipo de `enum`.
- Ser um tipo de ponteiro.
- Ser um struct definido pelo usuário que satsifies a restrição `unmanaged`.

Os campos de instância gerados pelo compilador, como os que fazem backup das propriedades implementadas automaticamente, também devem atender a essas restrições. 

Por exemplo:

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

A restrição `unmanaged` não pode ser combinada com `struct`, `class` ou `new()`. Essa restrição deriva do fato de que `unmanaged` implica `struct`, portanto, as outras restrições não fazem sentido.

A restrição `unmanaged` não é imposta pelo CLR, somente pelo idioma. Para evitar o uso incorretamente por outras linguagens, os métodos que têm essa restrição serão protegidos por um mod-req. Isso impedirá que outras linguagens usem argumentos de tipo que não sejam tipos não gerenciados.

O token `unmanaged` na restrição não é uma palavra-chave nem uma palavra-chave contextual. Em vez disso, é como `var` em que ele é avaliado nesse local e irá:

- Associar ao tipo definido pelo usuário ou referenciado `unmanaged`: isso será tratado da mesma forma que qualquer outra restrição de tipo nomeado é tratada. 
- Associar a nenhum tipo: isso será interpretado como a restrição de `unmanaged`.

No caso de existir um tipo chamado `unmanaged` e ele estiver disponível sem qualificação no contexto atual, não haverá como usar a restrição `unmanaged`. Isso paraleliza as regras que envolvem o `var` de recursos e os tipos definidos pelo usuário com o mesmo nome. 

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

A principal desvantagem desse recurso é que ele atende a um pequeno número de desenvolvedores: normalmente, autores ou estruturas de biblioteca de nível baixo.  Portanto, está gastando um tempo de linguagem precioso para um pequeno número de desenvolvedores. 

Ainda assim, essas estruturas são a base para a maioria dos aplicativos .NET.  Portanto, o desempenho/exatidão WINS nesse nível pode ter um efeito de ondulação no ecossistema do .NET.  Isso faz com que o recurso valha a pena considerar mesmo com o público limitado.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Há algumas alternativas a serem consideradas:

- O status quo: o recurso não é justificado em seus próprios méritos e os desenvolvedores continuam a usar o comportamento de consentimento implícito.

## <a name="questions"></a>Perguntas
[quesions]: #questions

### <a name="metadata-representation"></a>Representação de metadados

O F# idioma codifica a restrição no arquivo de assinatura, o que significa C# que não é possível reutilizar sua representação. Será necessário escolher um novo atributo para essa restrição. Além disso, um método que tem essa restrição deve ser protegido por um mod-req.

### <a name="blittable-vs-unmanaged"></a>Blittable versus não gerenciado
O F# idioma tem um [recurso muito semelhante](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) que usa a palavra-chave unmanaged. O nome blittable é proveniente do uso em Midori.  Talvez você queira verificar a precedência aqui e usar não gerenciado. 

**Resolução** A linguagem decide usar não gerenciado 

### <a name="verifier"></a>Verificação

O verificador/tempo de execução precisa ser atualizado para entender o uso de ponteiros para parâmetros de tipo genérico?  Ou pode simplesmente funcionar como está sem alterações?

**Resolução** Nenhuma alteração é necessária. Todos os tipos de ponteiro simplesmente não são verificáveis. 

## <a name="design-meetings"></a>Criar reuniões

N/D
