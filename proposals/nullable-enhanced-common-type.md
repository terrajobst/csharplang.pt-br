---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484527"
---
# <a name="nullable-enhanced-common-type"></a>Nullable-tipo comum aprimorado

* [x] proposta
* [] Protótipo: nenhum
* [] Implementação: nenhuma
* [] Especificação: Veja abaixo

## <a name="summary"></a>Resumo
[summary]: #summary

Há uma situação na qual os resultados atuais de algoritmos de tipo comum são de contador e resultam no programador que adiciona o que se parece com uma conversão redundante no código. Com essa alteração, uma expressão como `condition ? 1 : null` resultaria em um valor do tipo `int?`, e uma expressão como `condition ? x : 1.0` em que `x` é do tipo `int?` resultaria em um valor do tipo `double?`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Essa é uma causa comum do que se sente ao programador, como código clichê indesejado.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Modificamos a especificação para [encontrar o melhor tipo comum de um conjunto de expressões](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) para afetar as seguintes situações:

- Se uma expressão for de um tipo de valor não anulável `T` e a outra for um literal nulo, o resultado será do tipo `T?`.
- Se uma expressão for de um tipo de valor anulável `T?` e a outra for de um tipo de valor `U`e houver uma conversão implícita de `T` para `U`, o resultado será do tipo `U?`.

Espera-se que isso afete os seguintes aspectos do idioma:

- a [expressão Ternário](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- [expressão de criação de matriz](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions) digitada implicitamente
- inferindo o [tipo de retorno de um lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) para inferência de tipos
- casos que envolvem genéricos, como invocar `M<T>(T a, T b)` como `M(1, null)`.

Mais precisamente, alteramos as seguintes seções da especificação (inserções em negrito, exclusões em tachado):

> #### <a name="output-type-inferences"></a>Inferências de tipo de saída
> 
> Uma *inferência de tipo de saída* é feita *de* uma expressão `E` *para* um tipo `T` da seguinte maneira:
> 
> *  Se `E` for uma função anônima com tipo de retorno inferido `U`[(tipo de retorno inferido](expressions.md#inferred-return-type)) e `T` for um tipo delegado ou tipo de árvore de expressão com tipo de retorno `Tb`, uma *inferência de limite inferior* ([inferências de limite inferior](expressions.md#lower-bound-inferences)) será feita *de* `U` *para* `Tb`.
> *  Caso contrário, se `E` for um grupo de métodos e `T` for um tipo delegado ou tipo de árvore de expressão com tipos de parâmetro `T1...Tk` e tipo de retorno `Tb`, e a resolução de sobrecarga de `E` com os tipos `T1...Tk` gerar um único método com o tipo de retorno `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `Tb`.
> *  \* * Caso contrário, se `E` for uma expressão com o tipo de valor anulável `U?`, uma *inferência de limite inferior* será feita *de* `U` *para* `T` e um *limite nulo* será adicionado ao `T`. **
> *  Caso contrário, se `E` for uma expressão com o tipo `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `T`.
> *  **Caso contrário, se `E` for uma expressão constante com valor `null`, um *limite nulo* será adicionado a `T`** 
> *  Caso contrário, nenhuma inferência será feita.

> #### <a name="fixing"></a>Resolver
> 
> Uma variável de tipo não *fixa* `Xi` com um conjunto de limites é *fixa* da seguinte maneira:
> 
> *  O conjunto de *tipos candidatos* `Uj` começa como o conjunto de todos os tipos no conjunto de limites para `Xi`.
> *  Em seguida, examinamos cada associado por `Xi` por sua vez: para cada `U` limite exato de `Xi` todos os tipos `Uj` que não são idênticos aos `U` são removidos do conjunto de candidatos. Para cada `U` limite inferior de `Xi` todos os tipos `Uj` para o qual *não* há uma conversão implícita de `U` são removidos do conjunto de candidatos. Para cada `U` limite superior de `Xi` todos os tipos `Uj` de onde *não* há uma conversão implícita para `U` são removidos do conjunto de candidatos.
> *  Se entre os tipos candidatos restantes `Uj` houver um tipo exclusivo `V` do qual há uma conversão implícita para todos os outros tipos candidatos, ~~`Xi` será corrigido para `V`.~~
>     -  **Se `V` for um tipo de valor e houver um *limite nulo* para `Xi`, `Xi` será corrigido para `V?`**
>     -  **Caso contrário `Xi` é corrigido para `V`**
> *  Caso contrário, a inferência de tipos falhará.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Pode haver algumas incompatibilidades introduzidas por essa proposta.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

None.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [] Qual é a gravidade da incompatibilidade introduzida por esta proposta, se houver, e como ela pode ser moderada?

## <a name="design-meetings"></a>Criar reuniões

None.
