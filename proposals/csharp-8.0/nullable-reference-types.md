---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484611"
---
# <a name="nullable-reference-types-in-c"></a>Tipos de referência anuláveis emC# #

O objetivo desse recurso é:

* Permitir que os desenvolvedores expressem se uma variável, um parâmetro ou um resultado de um tipo de referência deve ser nulo ou não.
* Forneça avisos quando tais variáveis, parâmetros e resultados não forem usados de acordo com essa intenção.

## <a name="expression-of-intent"></a>Expressão de intenção

O idioma já contém a sintaxe `T?` para tipos de valor. É simples estender essa sintaxe para tipos de referência.

Supõe-se que a intenção de um tipo de referência não adornado `T` seja não nulo.

## <a name="checking-of-nullable-references"></a>Verificação de referências anuláveis

Uma análise de fluxo acompanha variáveis de referência anuláveis. Quando a análise considera que ela não seria nula (por exemplo, após uma verificação ou uma atribuição), seu valor será considerado uma referência não nula.

Uma referência anulável também pode ser tratada explicitamente como não nula com o operador de `x!` de sufixo (o operador "dammit"), para quando a análise de fluxo não puder estabelecer uma situação não nula que o desenvolvedor saiba.

Caso contrário, um aviso será fornecido se uma referência anulável for desreferenciada ou for convertida em um tipo não nulo.

Um aviso é fornecido ao converter de `S[]` para `T?[]` e de `S?[]` para `T[]`.

Um aviso é fornecido ao converter de `C<S>` para `C<T?>` exceto quando o parâmetro de tipo é covariant (`out`) e ao converter de `C<S?>` em `C<T>`, exceto quando o parâmetro de tipo é contravariant (`in`).

Um aviso será fornecido em `C<T?>` se o parâmetro de tipo tiver restrições não nulas. 

## <a name="checking-of-non-null-references"></a>Verificando referências não nulas

Um aviso será fornecido se um literal nulo for atribuído a uma variável não nula ou passado como um parâmetro não nulo.

Um aviso também será fornecido se um construtor não inicializar explicitamente os campos de referência não nulos.

Não podemos rastrear adequadamente que todos os elementos de uma matriz de referências não nulas são inicializados. No entanto, poderíamos emitir um aviso se nenhum elemento de uma matriz recém-criada for atribuído antes de a matriz ser lida ou passada. Isso pode lidar com o caso comum sem muito ruído.

Precisamos decidir se `default(T)` gera um aviso ou é simplesmente tratado como sendo do tipo `T?`.

## <a name="metadata-representation"></a>Representação de metadados

Adornos de nulidade devem ser representados em metadados como atributos. Isso significa que os compiladores de nível inferior irão ignorá-los.

Precisamos decidir se apenas anotações anuláveis estão incluídas, ou também há alguma indicação de se não nulo foi "ligado" no assembly.

## <a name="generics"></a>Genéricos

Se um parâmetro de tipo `T` tiver restrições não anuláveis, ele será tratado como não anulável dentro de seu escopo.

Se um parâmetro de tipo for irrestrito ou tiver apenas restrições anuláveis, a situação será um pouco mais complexa: isso significa que o argumento de tipo correspondente pode *ser anulável ou não anulável.* A coisa segura a fazer nessa situação é tratar o *parâmetro de tipo como anulável* e não anulável, fornecendo avisos quando um deles for violado. 

Vale a pena considerar se as restrições de referência anuláveis explícitas devem ser permitidas. No entanto, observe que não podemos evitar ter tipos de referência anuláveis *implicitamente* restrições em determinados casos (restrições herdadas).

A restrição `class` é não nula. Podemos considerar se `class?` deve ser uma restrição anulável válida, indicando "tipo de referência anulável".

## <a name="type-inference"></a>Inferência de tipos

Na inferência de tipos, se um tipo de contribuição for um tipo de referência anulável, o tipo resultante deverá ser anulável. Em outras palavras, a nulidade é propagada.

Devemos considerar se o literal `null` como uma expressão participante deve contribuir com a nulidade. Não hoje: para tipos de valor, ele leva a um erro, ao passo que, para tipos de referência, o NULL converte com êxito o tipo Plain. 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a>Alterações da falha

Avisos não nulos são uma alteração significativa de quebra no código existente e devem ser acompanhados por um mecanismo de aceitação.

Menos óbvio, os avisos de tipos anuláveis (conforme descrito acima) são uma alteração significativa no código existente em determinados cenários em que a nulidade é implícita:

* Os parâmetros de tipo irrestrito serão tratados como anuláveis implicitamente, portanto, atribuí-los a `object` ou acessar, por exemplo, `ToString` produzirão avisos.
* se a inferência de tipos inferir a nulidade de `null` expressões, o código existente, às vezes, resultará em tipos anuláveis em vez de não anuláveis, o que pode levar a novos avisos.

Portanto, os avisos anuláveis também precisam ser opcionais

Por fim, adicionar anotações a uma API existente será uma alteração significativa para os usuários que optaram por avisos, quando eles atualizarem a biblioteca. Isso também merece a capacidade de aceitar ou sair. "Desejo as correções de bugs, mas não estou pronto para lidar com suas novas anotações"

Em resumo, você precisa ser capaz de aceitar/sair de:
* Avisos anuláveis
* Avisos não nulos
* Avisos de anotações em outros arquivos

A granularidade da aceitação sugere um modelo como o analisador, em que faixas de código pode aceitar e cancelar com pragmas e níveis de severidade podem ser escolhidos pelo usuário. Além disso, as opções por biblioteca ("ignorar as anotações de JSON.NET até que eu esteja pronto para lidar com a saída") podem ser expressas em código como atributos.

O design da experiência de aceitação/transição é crucial para o sucesso e a utilidade desse recurso. Precisamos garantir que:

* Os usuários podem adotar a verificação de nulidade gradualmente, como desejam
* Os autores de biblioteca podem adicionar anotações de nulidade sem medo de quebrar clientes
* Apesar desses, não há uma noção de "pesadelo de configuração"

## <a name="tweaks"></a>Ajustes

Poderíamos considerar não usar as anotações de `?` em locais, mas apenas observando se elas são usadas de acordo com o que é atribuído a elas. Eu não prefiro isso; Acho que devemos permitir que as pessoas expressem suas intenções.

Poderíamos considerar um `T! x` abreviado em parâmetros, que gera automaticamente uma verificação nula em tempo de execução.

Determinados padrões em tipos genéricos, como `FirstOrDefault` ou `TryGet`, têm um comportamento um pouco estranho com argumentos de tipo não anuláveis, pois eles implicitamente geram valores padrão em determinadas situações. Poderíamos tentar nuancer o sistema de tipos para acomodá-lo melhor. Por exemplo, poderíamos permitir `?` em parâmetros de tipo irrestrito, embora o argumento de tipo já possa ser anulável. Acho que vale a pena, e isso leva à estranhaidade relacionada à interação com tipos de *valor* anulável. 

## <a name="nullable-value-types"></a>Tipos de valor anuláveis

Poderíamos considerar a adoção de algumas das semânticas acima para tipos de valor anuláveis também.

Já mencionamos inferência de tipos, onde poderíamos inferir `int?` de `(7, null)`, em vez de apenas dar um erro.

Outra oportunidade é aplicar a análise de fluxo a tipos de valor anuláveis. Quando eles são considerados não nulos, podemos realmente permitir o uso como o tipo não anulável de determinadas maneiras (por exemplo, acesso de membro). Precisamos apenas ter cuidado para que as coisas que você *já* possa fazer em um tipo de valor anulável sejam preferenciais, para fins de compatibilidade de volta.
