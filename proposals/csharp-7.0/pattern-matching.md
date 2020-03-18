---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485164"
---
# <a name="pattern-matching-for-c-7"></a>Correspondência de padrões C# para 7

As extensões de correspondência C# de padrões para habilitar muitos dos benefícios de tipos de dados algébricas e correspondência de padrões de linguagens funcionais, mas de forma que se integre perfeitamente com a sensação da linguagem subjacente. Os recursos básicos são: [tipos de registro](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), que são tipos cujo significado semântico é descrito pela forma dos dados; e correspondência de padrões, que é uma nova forma de expressão que permite a decomposição de vários níveis extremamente conciso desses tipos de dados. Os elementos dessa abordagem são inspirados por recursos relacionados nas linguagens [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Correspondência de padrão extensível por meio de uma linguagem leve") de programação e [escalabilidade](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Objetos correspondentes com padrões").

## <a name="is-expression"></a>Expressão is

O operador `is` é estendido para testar uma expressão em relação a um *padrão*.

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

Essa forma de *relational_expression* é além dos formulários existentes na C# especificação. É um erro de tempo de compilação se a *relational_expression* à esquerda do token de `is` não designa um valor ou não tem um tipo.

Cada *identificador* do padrão apresenta uma nova variável local que é *definitivamente atribuída* depois que o operador de `is` é `true` (ou seja, *definitivamente atribuído quando verdadeiro*).

> Observação: há tecnicamente uma ambiguidade entre o *tipo* em um `is-expression` e *constant_pattern*, que pode ser uma análise válida de um identificador qualificado. Tentamos associá-lo como um tipo de compatibilidade com as versões anteriores do idioma; somente se isso falhar, resolveremos como fazemos em outros contextos, para a primeira coisa encontrada (que deve ser uma constante ou um tipo). Essa ambiguidade só está presente no lado direito de uma expressão de `is`.

## <a name="patterns"></a>Padrões

Padrões são usados no operador de `is` e em uma *switch_statement* para expressar a forma de dados em que os dados de entrada devem ser comparados. Os padrões podem ser recursivos para que as partes dos dados possam ser correspondidas em subpadrões.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> Observação: há tecnicamente uma ambiguidade entre o *tipo* em um `is-expression` e *constant_pattern*, que pode ser uma análise válida de um identificador qualificado. Tentamos associá-lo como um tipo de compatibilidade com as versões anteriores do idioma; somente se isso falhar, resolveremos como fazemos em outros contextos, para a primeira coisa encontrada (que deve ser uma constante ou um tipo). Essa ambiguidade só está presente no lado direito de uma expressão de `is`.

### <a name="declaration-pattern"></a>Padrão de declaração

O *declaration_pattern* testa se uma expressão é de um determinado tipo e a converte para esse tipo se o teste for bem sucedido. Se o *simple_designation* for um identificador, ele apresentará uma variável local do tipo fornecido nomeado pelo identificador fornecido. Essa variável local é *definitivamente atribuída* quando o resultado da operação de correspondência de padrões é true.

```antlr
declaration_pattern
    : type simple_designation
    ;
```

A semântica de tempo de execução dessa expressão é que ela testa o tipo de tempo de execução do operando de *relational_expression* à esquerda em relação ao *tipo* no padrão. Se for desse tipo de tempo de execução (ou algum subtipo), o resultado da `is operator` será `true`. Ele declara uma nova variável local denominada pelo *identificador* que recebe o valor do operando à esquerda quando o resultado é `true`.

Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação. Um valor de tipo estático `E` é chamado de *padrão compatível* com o tipo `T` se houver uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão unboxing de `E` para `T`. É um erro de tempo de compilação se uma expressão do tipo `E` não for compatível com o tipo em um padrão de tipo com o qual ele é correspondido.

> Observação: [em C# 7,1, estendemos isso](../csharp-7.1/generics-pattern-match.md) para permitir uma operação de correspondência de padrões se o tipo de entrada ou o tipo `T` for um tipo aberto. Este parágrafo é substituído pelo seguinte:
> 
> Determinadas combinações de tipo estático do lado esquerdo e do tipo fornecido são consideradas incompatíveis e resultam em erro de tempo de compilação. Um valor de tipo estático `E` é chamado de *padrão compatível* com o tipo `T` se houver uma conversão de identidade, uma conversão de referência implícita, uma conversão boxing, uma conversão de referência explícita ou uma conversão unboxing de `E` para `T`**ou se `E` ou `T` for um tipo aberto**. É um erro de tempo de compilação se uma expressão do tipo `E` não for compatível com o tipo em um padrão de tipo com o qual ele é correspondido.

O padrão de declaração é útil para executar testes de tipo de tempo de execução de tipos de referência e substitui o idioma

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

Com um pouco mais conciso

```csharp
if (expr is Type v) { // code using v }
```

Erro se o *tipo* for um tipo de valor anulável.

O padrão de declaração pode ser usado para testar valores de tipos anuláveis: um valor do tipo `Nullable<T>` (ou um `T`em caixa) corresponde a um padrão de tipo `T2 id` se o valor for não nulo e o tipo de `T2` for `T`ou algum tipo ou interface base de `T`. Por exemplo, no fragmento de código

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

A condição da instrução de `if` é `true` em tempo de execução e a variável `v` contém o valor `3` do tipo `int` dentro do bloco.

### <a name="constant-pattern"></a>Padrão de constante

```antlr
constant_pattern
    : shift_expression
    ;
```

Um padrão constante testa o valor de uma expressão em relação a um valor constante. A constante pode ser qualquer expressão constante, como um literal, o nome de uma variável de `const` declarada ou uma constante de enumeração ou uma expressão de `typeof`.

Se e *e* *c* forem de tipos integrais, o padrão será considerado correspondido se o resultado da `e == c` de expressão for `true`.

Caso contrário, o padrão será considerado correspondente se `object.Equals(e, c)` retornar `true`. Nesse caso, é um erro de tempo de compilação se o tipo estático de *e* não for *compatível* com o tipo da constante.

### <a name="var-pattern"></a>Padrão de var

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

Uma expressão *e* corresponde a uma *var_pattern* sempre. Em outras palavras, uma correspondência a um *padrão de var* sempre é realizada com sucesso. Se o *simple_designation* for um identificador, em tempo de execução o valor de *e* será associado a uma variável local introduzida recentemente. O tipo da variável local é o tipo estático de *e*.

Erro se o nome `var` for associado a um tipo.

## <a name="switch-statement"></a>Instrução switch

A instrução `switch` é estendida para selecionar para execução o primeiro bloco com um padrão associado que corresponda à *expressão de comutador*.

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

A ordem na qual os padrões são correspondidos não está definida. Um compilador tem permissão para corresponder padrões fora de ordem e reutilizar os resultados de padrões já correspondidos para calcular o resultado da correspondência de outros padrões.

Se uma *proteção de caso* estiver presente, sua expressão será do tipo `bool`. Ele é avaliado como uma condição adicional que deve ser satisfeita para que o caso seja considerado satisfeito.

Erro se um *switch_label* não puder ter nenhum efeito no tempo de execução porque seu padrão é incorporadas por casos anteriores. [TODO: devemos ser mais precisos sobre as técnicas que o compilador precisa usar para alcançar esse julgamento.]

Uma variável de padrão declarada em um *switch_label* é definitivamente atribuída em seu bloco de caso se e somente se esse bloco de caso contiver precisamente um *switch_label*.

[TODO: devemos especificar quando um *bloco de alternância* pode ser acessado.]

### <a name="scope-of-pattern-variables"></a>Escopo de variáveis de padrão

O escopo de uma variável declarada em um padrão é o seguinte:

- Se o padrão for um rótulo de caso, o escopo da variável será o *bloco de caso*.

Caso contrário, a variável é declarada em uma expressão *is_pattern* , e seu escopo é baseado na construção que está delimitando imediatamente a expressão que contém a expressão *is_pattern* da seguinte maneira:

- Se a expressão estiver em uma lambda Expression-apto para, seu escopo será o corpo do lambda.
- Se a expressão estiver em um método ou Propriedade apto para de expressão, seu escopo será o corpo do método ou da propriedade.
- Se a expressão estiver em uma cláusula `when` de uma cláusula `catch`, seu escopo será a cláusula `catch`.
- Se a expressão estiver em um *iteration_statement*, seu escopo será apenas essa instrução.
- Caso contrário, se a expressão estiver em algum outro formulário de instrução, seu escopo será o escopo que contém a instrução.

Com a finalidade de determinar o escopo, um *embedded_statement* é considerado em seu próprio escopo. Por exemplo, a gramática para uma *if_statement* é

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Portanto, se a instrução controlada de um *if_statement* declarar uma variável de padrão, seu escopo será restrito a esse *embedded_statement*:

```csharp
if (x) M(y is var z);
```

Nesse caso, o escopo de `z` é a instrução inserida `M(y is var z);`.

Outros casos são erros por outros motivos (por exemplo, no valor padrão de um parâmetro ou em um atributo, ambos são um erro porque esses contextos exigem uma expressão constante).

> [Em C# 7,3, adicionamos os seguintes contextos](../csharp-7.3/expression-variables-in-initializers.md) nos quais uma variável de padrão pode ser declarada:
> - Se a expressão estiver em um *inicializador de Construtor*, seu escopo será o *inicializador de Construtor* e o corpo do construtor.
> - Se a expressão estiver em um inicializador de campo, seu escopo será o *equals_value_clause* em que ele aparece.
> - Se a expressão estiver em uma cláusula de consulta que é especificada para ser convertida no corpo de um lambda, seu escopo será apenas essa expressão.

## <a name="changes-to-syntactic-disambiguation"></a>Alterações na desambiguidade sintática

Há situações que envolvem genéricos em que C# a gramática é ambígua e a especificação da linguagem indica como resolver essas ambiguidades:

> #### <a name="7652-grammar-ambiguities"></a>ambiguidades de gramática 7.6.5.2
> As produções para *nome simples* (§ 7.6.3) e *acesso de membro* (§ 7.6.5) podem dar aumento às ambiguidades na gramática para expressões. Por exemplo, a instrução:
> ```csharp
> F(G<A,B>(7));
> ```
> pode ser interpretado como uma chamada para `F` com dois argumentos, `G < A` e `B > (7)`. Como alternativa, ele pode ser interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.

> Se uma sequência de tokens puder ser analisada (no contexto) como um *nome simples* (§ 7.6.3), *acesso de membro* (§ 7.6.5) ou *acesso de membro de ponteiro* (§ 18.5.2) terminando com uma *lista de argumentos de tipo* (§ 4.4.1), o token imediatamente após o `>` token de fechamento é examinado. Se for um de
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> em seguida, a *lista de argumentos de tipo* é mantida como parte do *nome simples*, *acesso de membro* ou ponteiro-acesso de *membro* e qualquer outra análise possível da sequência de tokens é descartada. Caso contrário, *a lista de argumentos de tipo* não é considerada como parte do *nome simples*, *acesso de membro* ou > ponteiro- *acesso de membro*, mesmo se não houver outra análise possível da sequência de tokens. Observe que essas regras não são aplicadas ao analisar uma *lista de argumentos de tipo* em um *namespace-ou-Type-name* (§ 3,8). A instrução
> ```csharp
> F(G<A,B>(7));
> ```
> de acordo com essa regra, será interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular. As instruções
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> cada um será interpretado como uma chamada para `F` com dois argumentos. A instrução
> ```csharp
> x = F < A > +y;
> ```
> será interpretado como um operador menor que, maior que operador, e operador de adição unário, como se a instrução tivesse sido gravada `x = (F < A) > (+y)`, em vez de como um *nome simples* com uma *lista de argumentos de tipo* seguido por um operador Binary Plus. Na instrução
> ```csharp
> x = y is C<T> + z;
> ```
> os tokens `C<T>` são interpretados como um *namespace-ou-Type-Name* com uma *lista de argumentos de tipo*.

Há várias alterações introduzidas no C# 7 que tornam essas regras de Desambigüidade não mais suficientes para lidar com a complexidade da linguagem.

### <a name="out-variable-declarations"></a>Declarações de variável out

Agora é possível declarar uma variável em um argumento out:

```csharp
M(out Type name);
```

No entanto, o tipo pode ser genérico: 

```csharp
M(out A<B> name);
```

Como a gramática de idioma do argumento usa *expression*, esse contexto está sujeito à regra de desambiguidade. Nesse caso, o `>` de fechamento é seguido por um *identificador*, que não é um dos tokens que permite que ele seja tratado como uma *lista de argumentos de tipo*. Portanto, propondo para **Adicionar o *identificador* ao conjunto de tokens que dispara a Desambigüidade para uma lista de *argumentos de tipo*.**

### <a name="tuples-and-deconstruction-declarations"></a>Declarações de tuplas e de desconstrução

Um literal de tupla é executado exatamente com o mesmo problema. Considere a expressão de tupla

```csharp
(A < B, C > D, E < F, G > H)
```

Nas 6 regras C# antigas para analisar uma lista de argumentos, isso seria analisado como uma tupla com quatro elementos, começando com `A < B` como o primeiro. No entanto, quando isso aparece à esquerda de uma desconstrução, queremos a desambiguidade disparada pelo token do *identificador* , conforme descrito acima:

```csharp
(A<B,C> D, E<F,G> H) = e;
```

Essa é uma declaração de desconstrução que declara duas variáveis, a primeira que é do tipo `A<B,C>` e nomeada `D`. Em outras palavras, o literal de tupla contém duas expressões, cada uma delas como uma expressão de declaração.

Para simplificar a especificação e o compilador, eu propondo que esse literal de tupla seja analisado como uma tupla de dois elementos onde quer que ele apareça (seja ou não exibido no lado esquerdo de uma atribuição). Isso seria um resultado natural da Desambigüidade descrita na seção anterior.

### <a name="pattern-matching"></a>Correspondência de padrões

A correspondência de padrões introduz um novo contexto em que surge a ambiguidade do tipo de expressão. Anteriormente, o lado direito de um operador de `is` era um tipo. Agora ele pode ser um tipo ou uma expressão e, se for um tipo, pode ser seguido por um identificador. Isso pode, tecnicamente, alterar o significado do código existente:

```csharp
var x = e is T < A > B;
```

Isso pode ser analisado sob as regras do C# 6 como

```csharp
var x = ((e is T) < A) > B;
```

Mas, sob as regras do C# 7 (com a Inambiguidade proposta acima) seria analisado como

```csharp
var x = e is T<A> B;
```

que declara uma variável `B` do tipo `T<A>`. Felizmente, os compiladores nativo e Roslyn têm um bug no qual eles fornecem um erro de sintaxe no código C# 6. Portanto, essa alteração significativa em particular não é uma preocupação.

Correspondência de padrões introduz tokens adicionais que devem orientar a resolução de ambiguidade para selecionar um tipo. Os seguintes exemplos de código C# 6 válido serão desfeitos sem regras de Desambigüidade adicionais:

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>Alteração proposta na regra de desambiguidade

Eu propondo para revisar a especificação para alterar a lista de tokens de ambiguidade de

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

até

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

E, em determinados contextos, tratamos o *identificador* como um token de ambiguidade. Esses contextos são onde a sequência de tokens que estão sendo desambiguados é imediatamente precedida por uma das palavras-chave `is`, `case`ou `out`ou surge ao analisar o primeiro elemento de um literal de tupla (nesse caso, os tokens são precedidos por `(` ou `:` e o identificador é seguido por um `,`) ou um elemento subsequente de um literal de tupla.

### <a name="modified-disambiguation-rule"></a>Regra de desambiguidade modificada

A regra de desambiguidade revisada seria algo assim

> Se uma sequência de tokens puder ser analisada (no contexto) como um *nome simples* (§ 7.6.3), *acesso de membro* (§ 7.6.5) ou *acesso de membro de ponteiro* (§ 18.5.2) terminando com uma *lista de argumentos de tipo* (§ 4.4.1), o token imediatamente após o `>` token de fechamento é examinado, para ver se ele está
> - Um dos `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or
> - Um dos operadores relacionais `<  >  <=  >=  is as`; or
> - Uma palavra-chave de consulta contextual que aparece dentro de uma expressão de consulta; or
> - Em determinados contextos, tratamos o *identificador* como um token de ambiguidade. Esses contextos são onde a sequência de tokens que estão sendo desambiguados é imediatamente precedida por uma das palavras-chave `is`, `case` ou `out`ou surge ao analisar o primeiro elemento de um literal de tupla (nesse caso, os tokens são precedidos por `(` ou `:` e o identificador é seguido por um `,`) ou um elemento subsequente de um literal de tupla.
> 
> Se o seguinte token estiver entre essa lista ou um identificador nesse contexto, a lista de argumentos de *tipo* será mantida como parte do *nome simples*, *acesso de membro* ou *ponteiro-acesso de membro* e qualquer outra análise possível da sequência de tokens será descartada.  Caso contrário, *a lista de argumentos de tipo* não será considerada como parte do *nome simples*, *acesso de membro* ou acesso de *membro de ponteiro*, mesmo que não haja nenhuma outra análise possível da sequência de tokens. Observe que essas regras não são aplicadas ao analisar uma *lista de argumentos de tipo* em um *namespace-ou-Type-name* (§ 3,8).

### <a name="breaking-changes-due-to-this-proposal"></a>Alterações recentes devido a esta proposta

Nenhuma alteração significativa é conhecida devido a essa regra de Desambigüidade proposta.

### <a name="interesting-examples"></a>Exemplos interessantes

Aqui estão alguns resultados interessantes dessas regras de Desambigüidade:

A expressão `(A < B, C > D)` é uma tupla com dois elementos, cada uma delas.

A expressão `(A<B,C> D, E)` é uma tupla com dois elementos, a primeira é uma expressão de declaração.

O `M(A < B, C > D, E)` de invocação tem três argumentos.

O `M(out A<B,C> D, E)` de invocação tem dois argumentos, o primeiro deles é uma declaração de `out`.

A expressão `e is A<B> C` usa uma expressão de declaração.

O rótulo case `case A<B> C:` usa uma expressão de declaração.

## <a name="some-examples-of-pattern-matching"></a>Alguns exemplos de correspondência de padrões

### <a name="is-as"></a>É-como

Podemos substituir o idioma

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

Com um pouco mais conciso e direto

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>Testando anulável

Podemos substituir o idioma

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

Com um pouco mais conciso e direto

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>Simplificação aritmética

Suponha que definimos um conjunto de tipos recursivos para representar expressões (por uma proposta separada):

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

Agora, podemos definir uma função para computar a derivada (não reduzida) de uma expressão:

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

Um simplificador de expressão demonstra padrões posicionais:

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
