---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485059"
---
# <a name="recursive-pattern-matching"></a>Correspondência de padrão recursivo

## <a name="summary"></a>Resumo
[summary]: #summary

As extensões de correspondência C# de padrões para habilitar muitos dos benefícios de tipos de dados algébricas e correspondência de padrões de linguagens funcionais, mas de forma que se integre perfeitamente com a sensação da linguagem subjacente. Os elementos dessa abordagem são inspirados por recursos relacionados nas linguagens [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Correspondência de padrão extensível por meio de uma linguagem leve") de programação e [escalabilidade](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Objetos correspondentes com padrões, página 273").

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

### <a name="is-expression"></a>Expressão is

O operador `is` é estendido para testar uma expressão em relação a um *padrão*.

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

Essa forma de *relational_expression* é além dos formulários existentes na C# especificação. É um erro de tempo de compilação se a *relational_expression* à esquerda do token de `is` não designa um valor ou não tem um tipo.

Cada *identificador* do padrão apresenta uma nova variável local que é *definitivamente atribuída* depois que o operador de `is` é `true` (ou seja, *definitivamente atribuído quando verdadeiro*).

> Observação: há tecnicamente uma ambiguidade entre o *tipo* em um `is-expression` e *constant_pattern*, que pode ser uma análise válida de um identificador qualificado. Tentamos associá-lo como um tipo de compatibilidade com as versões anteriores do idioma; somente se isso falhar, resolveremos como fazemos uma expressão em outros contextos, para a primeira coisa encontrada (que deve ser uma constante ou um tipo). Essa ambiguidade só está presente no lado direito de uma expressão de `is`.

### <a name="patterns"></a>Padrões

Os padrões são usados no operador de *is_pattern* , em uma *switch_statement*e em uma *switch_expression* para expressar a forma de dados em que os dados de entrada (que chamamos de valor de entrada) devem ser comparados. Os padrões podem ser recursivos para que as partes dos dados possam ser correspondidas em subpadrões.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>Padrão de declaração

```antlr
declaration_pattern
    : type simple_designation
    ;
```

O *declaration_pattern* testa se uma expressão é de um determinado tipo e a converte para esse tipo se o teste for bem sucedido. Isso pode introduzir uma variável local do tipo fornecido nomeado pelo identificador fornecido, se a designação for uma *single_variable_designation*. Essa variável local é *definitivamente atribuída* quando o resultado da operação de correspondência de padrões é `true`.

A semântica de tempo de execução dessa expressão é que ela testa o tipo de tempo de execução do operando de *relational_expression* à esquerda em relação ao *tipo* no padrão.  Se for desse tipo de tempo de execução (ou algum subtipo) e não `null`, o resultado da `is operator` será `true`.

Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação. Um valor de tipo estático `E` é considerado *compatível* com um tipo `T` se existir uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão de unboxing de `E` para `T`ou se um desses tipos for um tipo aberto. É um erro de tempo de compilação se uma entrada do tipo `E` não for *compatível* com o padrão com o *tipo* em um padrão de tipo com o qual ele é correspondido.

O padrão de tipo é útil para executar testes de tipo de tempo de execução de tipos de referência e substitui o idioma

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

Com um pouco mais conciso

```csharp
if (expr is Type v) { // code using v
```

Erro se o *tipo* for um tipo de valor anulável.

O padrão de tipo pode ser usado para testar valores de tipos anuláveis: um valor do tipo `Nullable<T>` (ou um `T`em caixa) corresponde a um padrão de tipo `T2 id` se o valor for não nulo e o tipo de `T2` for `T`ou algum tipo ou interface base de `T`. Por exemplo, no fragmento de código

```csharp
int? x = 3;
if (x is int v) { // code using v
```

A condição da instrução de `if` é `true` em tempo de execução e a variável `v` contém o valor `3` do tipo `int` dentro do bloco. Depois que o bloco, a variável `v` está no escopo, mas não é atribuída definitivamente.

#### <a name="constant-pattern"></a>Padrão de constante

```antlr
constant_pattern
    : constant_expression
    ;
```

Um padrão constante testa o valor de uma expressão em relação a um valor constante. A constante pode ser qualquer expressão constante, como um literal, o nome de uma variável de `const` declarada ou uma constante de enumeração. Quando o valor de entrada não é um tipo aberto, a expressão constante é convertida implicitamente no tipo da expressão correspondente; Se o tipo do valor de entrada não for *compatível* com o padrão com o tipo da expressão constante, a operação de correspondência de padrões será um erro.

O padrão *c* é considerado compatível com o valor de entrada convertido *e* , se `object.Equals(c, e)` retornar `true`.

Esperamos ver `e is null` como a maneira mais comum de testar `null` no código recém gravado, pois não é possível invocar uma `operator==`definida pelo usuário.

#### <a name="var-pattern"></a>Padrão de var

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

Se a *designação* for uma *simple_designation*, uma expressão *e* corresponder ao padrão. Em outras palavras, uma correspondência a um *padrão var* sempre é realizada com um *simple_designation*. Se o *simple_designation* for um *single_variable_designation*, o valor de *e* será associado a uma variável local introduzida recentemente. O tipo da variável local é o tipo estático de *e*.

Se a *designação* for uma *tuple_designation*, o padrão será equivalente a uma *positional_pattern* do formulário `(var` *designação*,... `)` onde os s de *design*são aqueles encontrados na *tuple_designation*.  Por exemplo, o padrão `var (x, (y, z))` é equivalente a `(var x, (var y, var z))`.

Erro se o nome `var` for associado a um tipo.

#### <a name="discard-pattern"></a>Descartar padrão

```antlr
discard_pattern
    : '_'
    ;
```

Uma expressão *e* corresponde ao padrão `_` sempre. Em outras palavras, cada expressão corresponde ao padrão de descarte.

Um padrão de descarte não pode ser usado como o padrão de um *is_pattern_expression*.

#### <a name="positional-pattern"></a>Padrão posicional

Um padrão posicional verifica se o valor de entrada não é `null`, invoca um método `Deconstruct` apropriado e executa uma correspondência de padrão adicional nos valores resultantes.  Ele também dá suporte a uma sintaxe de padrão semelhante a tupla (sem o tipo que está sendo fornecido) quando o tipo do valor de entrada é o mesmo que o tipo que contém `Deconstruct`, ou se o tipo do valor de entrada é um tipo de tupla ou se o tipo do valor de entrada é `object` ou `ITuple` e o tipo de tempo de execução da expressão implementa `ITuple`.

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

Se o *tipo* for omitido, levaremos para ser o tipo estático do valor de entrada.

Dada uma correspondência de um valor de entrada ao *tipo* de padrão `(` *subpattern_list* `)`, um método é selecionado pesquisando em *tipo* para declarações acessíveis de `Deconstruct` e selecionando um entre eles usando as mesmas regras que para a declaração de desconstrução.

Erro se um *positional_pattern* omitir o tipo, tiver um único *subpadrão* sem um *identificador*, não tiver nenhum *property_subpattern* e não tiver *simple_designation*. Essa ambiguidade é desambiguada entre um *constant_pattern* que está entre parênteses e um *positional_pattern*.

Para extrair os valores para corresponder aos padrões na lista,
- Se o *tipo* foi omitido e o tipo do valor de entrada é um tipo de tupla, o número de subpadrões é necessário para ser o mesmo que a cardinalidade da tupla. Cada elemento de tupla é correspondido em relação ao *subpadrão*correspondente e a correspondência é realizada com sucesso se todas elas forem bem sucedidos. Se qualquer *subpadrão* tiver um *identificador*, isso deverá nomear um elemento de tupla na posição correspondente no tipo de tupla.
- Caso contrário, se um `Deconstruct` adequado existir como um membro do *tipo*, será um erro de tempo de compilação se o tipo do valor de entrada não for compatível com o *padrão* com o *tipo*. Em tempo de execução, o valor de entrada é testado em relação ao *tipo*. Se isso falhar, a correspondência de padrão posicional falhará. Se tiver sucesso, o valor de entrada será convertido nesse tipo e `Deconstruct` será invocado com novas variáveis geradas pelo compilador para receber os parâmetros de `out`. Cada valor recebido é correspondido em relação ao *subpadrão*correspondente e a correspondência é realizada com êxito se todas elas forem bem sucedidos. Se qualquer *subpadrão* tiver um *identificador*, isso deverá nomear um parâmetro na posição correspondente de `Deconstruct`.
- Caso contrário, se o *tipo* foi omitido, e o valor de entrada for do tipo `object` ou `ITuple` ou algum tipo que possa ser convertido em `ITuple` por uma conversão de referência implícita e nenhum *identificador* aparecer entre os subpadrãos, então corresponderei usando `ITuple`.
- Caso contrário, o padrão é um erro de tempo de compilação.

A ordem na qual os subpadrãos são correspondidos em tempo de execução não é especificado e uma correspondência com falha pode não tentar corresponder a todos os subpadrãos.

##### <a name="example"></a>{1&gt;Exemplo&lt;1}

Este exemplo usa muitos dos recursos descritos nesta especificação

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>Padrão de propriedade

Um padrão de propriedade verifica se o valor de entrada não é `null` e corresponde recursivamente valores extraídos pelo uso de propriedades ou campos acessíveis.

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

Erro se qualquer _subpadrão_ de um _property_pattern_ não contiver um _identificador_ (ele deve ser do segundo formulário, que tem um _identificador_).  Uma vírgula à direita após o último subpadrão é opcional.

Observe que um padrão de verificação nula sai de um padrão de propriedade trivial. Para verificar se a cadeia de caracteres `s` é não nula, você pode escrever qualquer um dos seguintes formulários

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

Dada uma correspondência de uma expressão *e* ao *tipo* de padrão `{` *property_pattern_list* `}`, será um erro de tempo de compilação se a expressão *e* não for *compatível* com o padrão com o tipo *t* designado por *tipo*. Se o tipo estiver ausente, levaremos para ser o tipo estático de *e*. Se o *identificador* estiver presente, ele declara uma variável de padrão *do tipo Type.* Cada um dos identificadores que aparecem no lado esquerdo de seu *property_pattern_list* deve designar uma propriedade legível ou um campo de *T*acessível. Se o *simple_designation* do *property_pattern* estiver presente, ele definirá uma variável de padrão do tipo *T*.

Em tempo de execução, a expressão é testada em relação a *T*. Se isso falhar, a correspondência de padrão de propriedade falhará e o resultado será `false`. Se tiver sucesso, cada campo ou propriedade de *property_subpattern* será lido e seu valor corresponderá ao seu padrão correspondente. O resultado da correspondência inteira será `false` apenas se o resultado de qualquer um deles for `false`. A ordem na qual os subpadrãos são correspondidos não é especificada e uma correspondência com falha pode não corresponder a todos os subpadrãos em tempo de execução. Se a correspondência for realizada com sucesso e a *simple_designation* da *property_pattern* for uma *single_variable_designation*, ela definirá uma variável do tipo *T* que recebe o valor correspondente.

> Observação: o padrão de propriedade pode ser usado para correspondência de padrões com tipos anônimos.

##### <a name="example"></a>{1&gt;Exemplo&lt;1}

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Expressão switch

Um *switch_expression* é adicionado para dar suporte à semântica semelhante a `switch`para um contexto de expressão.

A C# sintaxe da linguagem é aumentada com as seguintes produções sintáticas:

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

O *switch_expression* não é permitido como um *expression_statement*.

> Estamos pensando em relaxar isso em uma revisão futura.

O tipo de *switch_expression* é o [*melhor tipo comum*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) de expressões que aparecem à direita dos tokens de `=>` do *switch_expression_arm*s se esse tipo existir e a expressão em todos os ARM da expressão switch puder ser convertida implicitamente nesse tipo.  Além disso, adicionamos uma nova *conversão de expressão de comutador*, que é uma conversão implícita predefinida de uma expressão de switch para cada tipo `T` para a qual existe uma conversão implícita da expressão de cada arm para `T`.

Ocorrerá um erro se algum padrão de *switch_expression_arm*não puder afetar o resultado, pois algum padrão e proteção anteriores sempre corresponderá.

Uma expressão de switch é considerada *exaustiva* se algum ARM da expressão switch tratar cada valor de sua entrada.  O compilador deverá produzir um aviso se uma expressão de comutador não for *exaustiva*.

Em tempo de execução, o resultado da *switch_expression* é o valor da *expressão* da primeira *switch_expression_arm* para a qual a expressão no lado esquerdo da *switch_expression* corresponde ao padrão do *switch_expression_arm*e para o qual a *case_guard* do *switch_expression_arm*, se presente, é avaliada como `true`. Se não houver tal *switch_expression_arm*, o *switch_expression* lançará uma instância da exceção `System.Runtime.CompilerServices.SwitchExpressionException`.

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>Parênteses opcional ao alternar para um literal de tupla

Para alternar para um literal de tupla usando o *switch_statement*, você precisa escrever o que parece ser parênteses redundante

```csharp
switch ((a, b))
{
```

Para permitir

```csharp
switch (a, b)
{
```

os parênteses da instrução switch são opcionais quando a expressão que está sendo ativada é um literal de tupla.

### <a name="order-of-evaluation-in-pattern-matching"></a>Ordem de avaliação na correspondência de padrões

Dar a flexibilidade do compilador ao reordenar as operações executadas durante a correspondência de padrões pode permitir a flexibilidade que pode ser usada para melhorar a eficiência da correspondência de padrões. O requisito (não imposto) seria que as propriedades acessadas em um padrão, e os métodos desconstruir, precisam ser "puras" (com efeito colateral gratuito, idempotente, etc.). Isso não significa que adicionaremos pureza como um conceito de linguagem, apenas que permitiremos a flexibilidade do compilador em operações de reordenação.

**Resolução 2018-04-04 LDM**: confirmada: o compilador tem permissão para reordenar chamadas para `Deconstruct`, acessos de propriedade e invocações de métodos no `ITuple`e pode assumir que os valores retornados são os mesmos de várias chamadas. O compilador não deve invocar funções que não afetem o resultado, e teremos muito cuidado antes de fazer qualquer alteração na ordem de avaliação gerada pelo compilador no futuro.

### <a name="some-possible-optimizations"></a>Algumas otimizações possíveis

A compilação da correspondência de padrões pode tirar proveito de partes comuns de padrões. Por exemplo, se o teste de tipo de nível superior de dois padrões sucessivos em um *switch_statement* for o mesmo tipo, o código gerado poderá ignorar o teste de tipo para o segundo padrão.

Quando alguns dos padrões são inteiros ou cadeias de caracteres, o compilador pode gerar o mesmo tipo de código que ele gera para uma instrução switch em versões anteriores do idioma.

Para obter mais informações sobre esses tipos de otimizações, consulte [[Scott e Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Quando fazer a correspondência da heurística de compilação é importante?").
