---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484982"
---
# <a name="target-typed-default-literal"></a>Literal "padrão" de tipo de destino

* [x] proposta
* [x] protótipo
* [x] implementação
* [] Especificação

## <a name="summary"></a>Resumo
[summary]: #summary

O recurso de `default` de tipo de destino é uma variação de forma mais curta do operador `default(T)`, que permite que o tipo seja omitido. Seu tipo é inferido por digitação de destino em vez disso. Além disso, ele se comporta como `default(T)`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

A principal motivação é evitar digitar informações redundantes.

Por exemplo, ao invocar `void Method(ImmutableArray<SomeType> array)`, o literal *padrão* permite `M(default)` no lugar de `M(default(ImmutableArray<SomeType>))`.

Isso é aplicável em vários cenários, como:

- declarando locais (`ImmutableArray<SomeType> x = default;`)
- operações ternários (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- retornando em métodos e lambdas (`return default;`)
- declarando valores padrão para parâmetros opcionais (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- incluindo valores padrão em expressões de criação de matriz (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Uma nova expressão é introduzida, a literal *padrão* . Uma expressão com essa classificação pode ser convertida implicitamente em qualquer tipo, por uma *conversão literal padrão*. 

A inferência do tipo para o literal *padrão* funciona da mesma forma que para o literal *nulo* , exceto que qualquer tipo é permitido (não apenas tipos de referência).

Essa conversão produz o valor padrão do tipo inferido.

O literal *padrão* pode ter um valor constante, dependendo do tipo inferido. Portanto, `const int x = default;` é legal, mas `const int? y = default;` não é.

O literal *padrão* pode ser o operando de operadores de igualdade, desde que o outro operando tenha um tipo. Portanto, `default == x` e `x == default` são expressões válidas, mas `default == default` é ilegal.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Uma desvantagem secundária é que o literal *padrão* pode ser usado no lugar do literal *nulo* na maioria dos contextos. Duas das exceções são `throw null;` e `null == null`, que são permitidas para o literal *nulo* , mas não para o literal *padrão* .

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Há algumas alternativas a serem consideradas:

- O status quo: o recurso não é justificado em seus próprios méritos e os desenvolvedores continuam a usar o operador padrão com um tipo explícito.
- Estendendo o literal nulo: essa é a abordagem do VB com `Nothing`. Poderíamos permitir `int x = null;`.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [x] o *padrão* deve ser permitido como o operando dos *is* operadores is *ou as?* Resposta: não permitir `default is T`, permitir `x is default`, permitir `default as RefType` (com aviso sempre nulo)

## <a name="design-meetings"></a>Criar reuniões

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
