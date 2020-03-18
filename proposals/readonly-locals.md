---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484478"
---
# <a name="readonly-locals-and-parameters"></a>parâmetros e locais ReadOnly

* [x] proposta
* [] Protótipo
* [] Implementação
* [] Especificação

## <a name="summary"></a>Resumo
[summary]: #summary

Permitir que locais e parâmetros sejam anotados como ReadOnly para evitar a mutação superficial desses locais e parâmetros.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Hoje, a palavra-chave `readonly` pode ser aplicada a campos; Isso tem o efeito de garantir que um campo só possa ser gravado durante a construção (construção estática no caso de um campo estático ou de uma construção de instância no caso de um campo de instância), o que ajuda os desenvolvedores a evitar erros, substituindo acidentalmente o estado que não deve ser modificado. Mas os campos não são os únicos locais que os desenvolvedores querem garantir que os valores não sejam modificados. Em particular, é comum criar uma variável local para armazenar o estado temporário, e a atualização acidental desse estado temporário pode resultar em cálculos errados e em outros bugs, especialmente quando tais "locais" são capturados em lambdas, em que ponto eles são levantados em campos, mas não há nenhuma maneira de marcar tais campos levantados como `readonly`.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Os locais também serão anotados como `readonly`, com o compilador, garantindo que eles sejam definidos apenas no momento da declaração (determinados locais no C# já estão implicitamente ReadOnly, como a variável de iteração em um loop ' foreach ' ou a variável usada em um bloco ' Using ', mas atualmente um desenvolvedor não tem a capacidade de marcar outros locais como `readonly`). Tais `readonly` locais devem ter um inicializador:

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

E, como abreviação de `readonly var`, a palavra-chave contextual existente `let` pode ser usada, por exemplo,

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Não há nenhuma restrição especial para o que o inicializador pode ser e pode ser qualquer coisa válida no momento como um inicializador para locais, por exemplo,

```csharp
readonly T data = arg1 ?? arg2;
```

`readonly` em locais é particularmente valiosa ao trabalhar com lambdas e fechamentos. Quando um método anônimo ou lambda acessa o estado local do escopo delimitador, esse estado é capturado em um fechamento pelo compilador, que é representado por uma "classe de exibição".  Cada "local" capturado é um campo nessa classe, ainda que o compilador esteja gerando esse campo em seu nome, você não tem oportunidade de anotá-lo como `readonly` para impedir que o lambda grave erroneamente no "local" (entre aspas, pois não é realmente local, pelo menos não no MSIL resultante). Com `readonly` locais, o compilador pode impedir que o lambda grave em local, o que é particularmente valioso em cenários que envolvem multithread, em que uma gravação errada poderia resultar em um bug de simultaneidade perigoso, mas raro e difícil de encontrar.

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

Como uma forma especial de locais, os parâmetros também serão anotados como `readonly`. Isso não teria nenhum efeito sobre o que o chamador do método pode passar para o parâmetro (assim como não há nenhuma restrição sobre quais valores podem ser armazenados em um campo de `readonly`), mas como acontece com qualquer `readonly` local, o compilador proibiria o código de gravar no parâmetro após a declaração, o que significa que o corpo do método é proibido de gravar no parâmetro.

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` parâmetros não afetam a assinatura/os metadados emitidos pelo compilador para esse método e simplesmente afetam como o compilador trata a compilação do corpo do método. Portanto, por exemplo, um método virtual base pode ter um parâmetro `readonly`, e esse parâmetro poderia ser gravável em uma substituição.

Assim como nos campos, `readonly` para locais e parâmetros é superficial, afetando o local de armazenamento, mas não afetando transitivamente o grafo do objeto. No entanto, também como acontece com campos, chamar um método em um `readonly` struct local/Parameter realmente fará uma cópia da struct e chamará o método na cópia, a fim de evitar a mutação interna de `this`.

`readonly` locais e parâmetros não podem ser passados como argumentos `ref` ou `out`, a menos que/até `ref readonly` também tenha suporte.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

- `val` pode ser usado como uma abreviação alternativa para `let`.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: deixei a pergunta de como lidar com `ref readonly` como separado dessa proposta.
- Esta proposta não lida com tipos de structs/imutáveis de ReadOnly. Que é deixado por uma proposta separada.

## <a name="design-meetings"></a>Criar reuniões

- Discutido brevemente em 21 de janeiro de 2015 (<https://github.com/dotnet/roslyn/issues/98>)
