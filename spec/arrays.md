---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229490"
---
# <a name="arrays"></a>Matrizes

Uma matriz é uma estrutura de dados que contém um número de variáveis que são acessados por meio de índices calculados. As variáveis contidas em uma matriz, também chamada de elementos da matriz, são todos do mesmo tipo, e esse tipo é chamado o tipo de elemento da matriz.

Uma matriz tem uma classificação que determina o número de índices associados com cada elemento da matriz. A classificação de uma matriz também é chamada como as dimensões da matriz. Uma matriz com uma classificação de um é chamada um ***matriz unidimensional***. Uma matriz com uma classificação maior do que uma é chamada de um ***matriz multidimensional***. Matrizes multidimensionais de tamanhos específicos são geralmente denominados matrizes bidimensionais, matrizes tridimensionais e assim por diante.

Cada dimensão de uma matriz tem um comprimento associado que é um número inteiro maior ou igual a zero. Os tamanhos da dimensão não fazem parte do tipo de matriz, mas em vez disso, são definidos quando uma instância do tipo de matriz é criada em tempo de execução. O comprimento de uma dimensão determina o intervalo válido de índices para a dimensão: Para uma dimensão de comprimento `N`, índices podem variar desde `0` para `N - 1` inclusivo. O número total de elementos em uma matriz é o produto dos comprimentos de cada dimensão da matriz. Se um ou mais das dimensões de uma matriz tem um comprimento igual a zero, a matriz deve estar vazio.

O tipo do elemento de uma matriz pode ser qualquer tipo, incluindo um tipo de matriz.

## <a name="array-types"></a>Tipos de matriz

Um tipo de matriz é escrito como um *non_array_type* seguido por um ou mais *rank_specifier*s:

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

Um *non_array_type* é qualquer *tipo* que é não por uma *array_type*.

A classificação de um tipo de matriz é determinada pela mais à esquerda *rank_specifier* na *array_type*: Um *rank_specifier* indica que a matriz é uma matriz com uma classificação de um mais o número de "`,`" tokens no *rank_specifier*.

O tipo de elemento de um tipo de matriz é o tipo que é o resultado da exclusão de mais à esquerda *rank_specifier*:

*  Um tipo de matriz no formato `T[R]` é uma matriz com rank `R` e um tipo de elemento de não matriz `T`.
*  Um tipo de matriz no formato `T[R][R1]...[Rn]` é uma matriz com rank `R` e um tipo de elemento `T[R1]...[Rn]`.

Na verdade, o *rank_specifier*s são lidos da esquerda para a direita antes do tipo de elemento de não matriz final. O tipo `int[][,,][,]` é uma matriz unidimensional de matrizes tridimensionais de matrizes bidimensionais de `int`.

Em tempo de execução, um valor de um tipo de matriz pode ser `null` ou uma referência a uma instância desse tipo de matriz.

### <a name="the-systemarray-type"></a>O tipo System. Array

O tipo `System.Array` é o tipo base abstrato de todos os tipos de matriz. Uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe a partir de qualquer tipo de matriz `System.Array`e uma conversão de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)) existe de `System.Array` para qualquer tipo de matriz. Observe que `System.Array` não é em si um *array_type*. Em vez disso, ele é um *class_type* da qual todas as *array_type*s são derivados.

Em tempo de execução, um valor do tipo `System.Array` pode ser `null` ou uma referência a uma instância de qualquer tipo de matriz.

### <a name="arrays-and-the-generic-ilist-interface"></a>Matrizes e a interface IList genérica

Uma matriz unidimensional `T[]` implementa a interface `System.Collections.Generic.IList<T>` (`IList<T>` de forma abreviada) e suas interfaces base. Da mesma forma, há uma conversão implícita da `T[]` para `IList<T>` e suas interfaces base. Além disso, se houver uma conversão de referência implícita da `S` para `T` , em seguida, `S[]` implementa `IList<T>` e não há uma conversão de referência implícita da `S[]` para `IList<T>` e sua base de interfaces ( [Conversões de referência implícita](conversions.md#implicit-reference-conversions)). Se não houver uma conversão de referência explícita de `S` para `T` e em seguida, há uma conversão de referência explícita de `S[]` à `IList<T>` e suas interfaces base ([conversões de referência explícita](conversions.md#explicit-reference-conversions)). Por exemplo:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

A atribuição `lst2 = oa1` gera um erro de tempo de compilação desde a conversão de `object[]` para `IList<string>` é uma conversão explícita, não é implícita. A conversão `(IList<string>)oa1` fará com que uma exceção seja lançada em tempo de execução desde `oa1` referências um `object[]` e não um `string[]`. No entanto a conversão `(IList<string>)oa2` não causará uma exceção seja lançada desde `oa2` referências um `string[]`.

Sempre que houver uma conversão de referência implícita ou explícita de `S[]` ao `IList<T>`, também há uma conversão de referência explícita do `IList<T>` e sua base de interfaces para `S[]` ([referência explícita conversões](conversions.md#explicit-reference-conversions)).

Quando um tipo de matriz `S[]` implementa `IList<T>`, alguns dos membros da interface implementada podem lançar exceções. O comportamento preciso da implementação da interface está além do escopo desta especificação.

## <a name="array-creation"></a>Criação de matriz

Instâncias de matriz são criadas pelo *array_creation_expression*s ([expressões de criação de matriz](expressions.md#array-creation-expressions)) ou por campo ou declarações de variável local que incluem um *array_initializer*([Inicializadores de matriz](arrays.md#array-initializers)).

Quando uma instância de matriz é criada, a classificação e o tamanho de cada dimensão são estabelecidas e permanecem constantes para o tempo de vida inteiro da instância. Em outras palavras, não é possível alterar a classificação de uma instância de matriz existente, nem é possível redimensionar suas dimensões.

Uma instância de matriz sempre é de um tipo de matriz. O `System.Array` tipo é um tipo abstrato que não pode ser instanciado.

Elementos de matrizes criados pelo *array_creation_expression*s sempre são inicializados com seus valores padrão ([valores padrão](variables.md#default-values)).

## <a name="array-element-access"></a>Acesso de elemento de matriz

Elementos da matriz são acessados usando *element_access* expressões ([acesso de matriz](expressions.md#array-access)) do formulário `A[I1, I2, ..., In]`, onde `A` é uma expressão de um tipo de matriz e cada `Ix` é um expressão do tipo `int`, `uint`, `long`, `ulong`, ou pode ser convertido implicitamente em um ou mais desses tipos. O resultado de um acesso de elemento de matriz é uma variável, ou seja, o elemento da matriz selecionada pelos índices.

Os elementos de uma matriz podem ser enumerados usando um `foreach` instrução ([a instrução foreach](statements.md#the-foreach-statement)).

## <a name="array-members"></a>Membros da matriz

Cada tipo de matriz herda os membros declarados pelo `System.Array` tipo.

## <a name="array-covariance"></a>Covariância de matriz

Para quaisquer dois *reference_type*s `A` e `B`, se uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) ou conversão de referência explícita ([ Conversões de referência explícita](conversions.md#explicit-reference-conversions)) existe na `A` à `B`, em seguida, também existe a mesma conversão de referência do tipo de matriz `A[R]` para o tipo de matriz `B[R]`, onde `R` é qualquer determinada *rank_specifier* (mas o mesmo para ambos os tipos de matriz). Essa relação é conhecida como ***covariância de matriz***. Covariância de matriz em particular significa que um valor de um tipo de matriz `A[R]` , na verdade, pode ser uma referência a uma instância de um tipo de matriz `B[R]`, desde que existe uma conversão de referência implícita da `B` para `A`.

Devido a covariância de matriz, atribuições aos elementos de matrizes de tipo de referência incluem uma verificação de tempo de execução que garante que o valor que está sendo atribuído ao elemento de matriz, na verdade, seja de um tipo permitido ([atribuição simples](expressions.md#simple-assignment)). Por exemplo:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

A atribuição ao `array[i]` no `Fill` método implicitamente inclui uma verificação de tempo de execução que garante que o objeto referenciado pela `value` seja `null` ou uma instância que é compatível com o tipo de elemento real do `array`. Na `Main`, as duas primeiras invocações de `Fill` bem-sucedida, mas as causas de invocação de terceiro uma `System.ArrayTypeMismatchException` seja lançada após executar a primeira atribuição para `array[i]`. A exceção ocorre porque um demarcado `int` não podem ser armazenados em um `string` matriz.

Covariância de matriz especificamente não se estende a matrizes de *value_type*s. Por exemplo, não existe conversão que permite que um `int[]` deve ser tratado como um `object[]`.

## <a name="array-initializers"></a>Inicializadores de matriz

Inicializadores de matriz podem ser especificados em declarações de campo ([campos](classes.md#fields)), declarações de variável local ([declarações de variável Local](statements.md#local-variable-declarations)) e expressões de criação de matriz ([criação de matriz expressões](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Um inicializador de matriz consiste em uma sequência de inicializadores de variável, delimitados por "`{`"e"`}`"tokens e separados por"`,`" tokens. Cada inicializador de variável é uma expressão ou, no caso de uma matriz multidimensional, um inicializador de matriz aninhados.

O contexto no qual um inicializador de matriz é usado determina o tipo da matriz que está sendo inicializado. Em uma expressão de criação de matriz, o tipo de matriz imediatamente precede o inicializador, ou é inferido das expressões no inicializador de matriz. Em um campo ou uma declaração de variável, o tipo de matriz é o tipo do campo ou variável sendo declarada. Quando um inicializador de matriz é usado em um campo ou uma declaração de variável, como:
```csharp
int[] a = {0, 2, 4, 6, 8};
```
ele é simplesmente uma abreviação para uma expressão de criação de matriz equivalente:
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

Para uma matriz unidimensional, o inicializador de matriz deve conter uma sequência de expressões de atribuição compatível com o tipo de elemento da matriz. As expressões de inicializar os elementos da matriz em ordem crescente, começando com o elemento no índice zero. O número de expressões no inicializador de matriz determina o tamanho da instância de matriz que está sendo criado. Por exemplo, o inicializador de matriz acima cria um `int[]` instância de comprimento 5 e, em seguida, inicializa a instância com os seguintes valores:
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

Para uma matriz multidimensional, o inicializador de matriz deve ter tantos níveis de aninhamento quanto há dimensões na matriz. O nível de aninhamento mais externo corresponde à dimensão mais à esquerda e o nível de aninhamento mais interno corresponde à dimensão mais à direita. O comprimento de cada dimensão da matriz é determinado pelo número de elementos no nível de aninhamento correspondente no inicializador de matriz. Para cada inicializador de matriz aninhados, o número de elementos deve ser o mesmo que outros inicializadores de matriz no mesmo nível. O Exemplo:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
cria uma matriz bidimensional com um comprimento de cinco para a dimensão mais à esquerda e um comprimento de dois para a dimensão mais à direita:
```csharp
int[,] b = new int[5, 2];
```
e, em seguida, inicializa a matriz da instância com os seguintes valores:
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

Se uma dimensão que não seja mais à direita é fornecida com comprimento zero, as dimensões subsequentes serão consideradas como também tem comprimento zero. O Exemplo:
```csharp
int[,] c = {};
```
cria uma matriz bidimensional com um comprimento de zero para o mais à esquerda e a dimensão mais à direita:
```csharp
int[,] c = new int[0, 0];
```

Quando uma expressão de criação de matriz inclui tamanhos da dimensão explícita e um inicializador de matriz, os comprimentos devem ser expressões constantes e o número de elementos em cada nível de aninhamento deve corresponder o tamanho da dimensão correspondente. Estes são alguns exemplos:
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

Aqui, o inicializador para `y` resulta em um erro de tempo de compilação, porque a expressão de comprimento da dimensão não é uma constante e o inicializador para `z` resulta em um erro de tempo de compilação porque o comprimento e o número de elementos no inicializador não concordar.
