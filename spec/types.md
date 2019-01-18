---
ms.openlocfilehash: a28397b1ce97dbead6d5014e2b20e108a1018502
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229488"
---
# <a name="types"></a>Tipos

Os tipos de linguagem c# são divididos em duas categorias principais: ***tipos de valor*** e ***tipos de referência***. Tipos de valor e tipos de referência podem ser ***tipos genéricos***, que leva um ou mais ***parâmetros de tipo***. Parâmetros de tipo podem designar os dois tipos de valor e tipos de referência.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

A categoria final dos tipos de ponteiros, está disponível apenas em código não seguro. Isso é discutido mais detalhadamente em [tipos de ponteiro](unsafe-code.md#pointer-types).

Tipos de valor são diferentes dos tipos de referência de variáveis dos tipos de valor contêm diretamente seus dados, enquanto o armazenamento de tipos de variáveis de referência ***referências*** aos seus dados, o último sendo conhecido como ***objetos***. Com tipos de referência, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível operações em uma variável afetem o objeto referenciado por outra variável. Com tipos de valor, cada variável tem sua própria cópia dos dados e não é possível que operações em um afetem o outro.

O sistema de tipos do # é unificado, de modo que um valor de qualquer tipo pode ser tratado como um objeto. Cada tipo no C#, direta ou indiretamente, deriva do tipo de classe `object`, e `object` é a classe base definitiva de todos os tipos. Os valores de tipos de referência são tratados como objetos simplesmente exibindo os valores como tipo `object`. Valores de tipos de valor são tratados como objetos, executando operações de conversão boxing e unboxing ([conversões Boxing e unboxing](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Tipos de valor

Um tipo de valor é um tipo de estrutura ou um tipo de enumeração. O c# fornece um conjunto de tipos de struct predefinidos chamados de ***tipos simples***. Os tipos simples são identificados por meio de palavras reservadas.

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

Ao contrário de uma variável do tipo de referência, uma variável de um tipo de valor pode conter o valor `null` somente se o tipo de valor é um tipo anulável.  Há um tipo de valor anulável correspondente denotando o mesmo conjunto de valores mais o valor para cada tipo de valor não anulável `null`.

Atribuição para uma variável de um tipo de valor cria uma cópia do valor que está sendo atribuído. Isso difere da atribuição a uma variável do tipo de referência, que copia a referência, mas não o objeto identificado por referência.

### <a name="the-systemvaluetype-type"></a>O tipo System. ValueType

Todos os tipos de valor herdam implicitamente da classe `System.ValueType`, que, por sua vez, herda da classe `object`. Não é possível para qualquer tipo de derivar de um tipo de valor e tipos de valor, portanto, são lacrados implicitamente ([lacrado classes](classes.md#sealed-classes)).

Observe que `System.ValueType` não é em si um *value_type*. Em vez disso, ele é um *class_type* da qual todas as *value_type*s automaticamente são derivados.

### <a name="default-constructors"></a>Construtores padrão

Todos os tipos de valor declaram implicitamente um construtor de instância pública sem-parâmetros chamado a ***construtor padrão***. O construtor padrão retorna uma instância inicializada do zero, conhecida como o ***valor padrão*** para o tipo de valor:

*  Para todos os *simple_type*s, o valor padrão é o valor produzido por um padrão de bits de todos os zeros:
    * Para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, e `ulong`, o valor padrão é `0`.
    * Para `char`, o valor padrão é `'\x0000'`.
    * Para `float`, o valor padrão é `0.0f`.
    * Para `double`, o valor padrão é `0.0d`.
    * Para `decimal`, o valor padrão é `0.0m`.
    * Para `bool`, o valor padrão é `false`.
*  Para um *enum_type* `E`, o valor padrão é `0`, convertido no tipo `E`.
*  Para um *struct_type*, o valor padrão é o valor produzido pela configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`.
*  Para um *nullable_type* o valor padrão é uma instância para o qual o `HasValue` propriedade é false e o `Value` propriedade é indefinida. O valor padrão é também conhecido como o ***valor nulo*** do tipo que permite valor nulo.

Como qualquer outro construtor de instância, o construtor padrão de um tipo de valor é invocado usando o `new` operador. Por motivos de eficiência, esse requisito não se destina para, na verdade, a implementação de gerar uma chamada de construtor. No exemplo a seguir, as variáveis `i` e `j` são inicializados em zero.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Como cada tipo de valor tem implicitamente um construtor de instância pública sem-parâmetros, não é possível que um tipo de estrutura conter uma declaração explícita de um construtor sem parâmetros. No entanto, um tipo de struct é permitido para declarar construtores de instância com parâmetros ([construtores](structs.md#constructors)).

### <a name="struct-types"></a>Tipos struct

Um tipo de struct é um tipo de valor que pode declarar constantes, campos, métodos, propriedades, indexadores, operadores, construtores de instância, construtores estáticos e tipos aninhados. A declaração de tipos de estrutura é descrita em [declarações de Struct](structs.md#struct-declarations).

### <a name="simple-types"></a>Tipos simples

O c# fornece um conjunto de tipos de struct predefinidos chamados de ***tipos simples***. Os tipos simples são identificados por meio de palavras reservadas, mas essas palavras reservadas são simplesmente aliases para tipos de struct predefinidos no `System` namespace, conforme descrito na tabela a seguir.


| __Palavra reservada__ | __Tipo com alias__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

Porque é um tipo simples aliases de um tipo de struct, todos os tipos simples tem membros. Por exemplo, `int` tem os membros declarados na `System.Int32` e os membros herdados de `System.Object`, e as instruções a seguir são permitidas:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Os tipos simples diferem de outros tipos de struct, pois permitem determinadas operações adicionais:

*  A maioria dos tipos simples permite valores a serem criados, escrevendo *literais* ([literais](lexical-structure.md#literals)). Por exemplo, `123` é um literal do tipo `int` e `'a'` é um literal do tipo `char`. C# não faz nenhuma provisão para literais de tipos de struct em geral, e não-padrão valores de outros tipos de struct, por fim, são sempre criadas por meio de construtores de instância desses tipos de struct.
*  Quando os operandos de uma expressão forem todas as constantes de tipo simples, é possível que o compilador avaliar a expressão em tempo de compilação. Essa expressão é conhecida como uma *constant_expression* ([expressões constantes](expressions.md#constant-expressions)). Expressões que envolvem operadores definidos por outros tipos de struct não são consideradas para ser expressões constantes.
*  Por meio `const` declarações é possível declarar constantes dos tipos simples ([constantes](classes.md#constants)). Não é possível ter constantes de outros tipos de struct, mas um efeito semelhante é fornecido pelo `static readonly` campos.
*  Conversões que envolvem tipos simples podem participar de avaliação de operadores de conversão definidos por outros tipos de struct, mas um operador de conversão definida pelo usuário nunca pode participar de avaliação de outro operador definido pelo usuário ([avaliação do conversões definidas pelo usuário](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Tipos integrais

C# oferece suporte a nove tipos integrais: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, e `char`. Os tipos integrais têm os tamanhos e os intervalos de valores a seguir:

*  O `sbyte` tipo representa inteiros com sinal de 8 bits com valores entre -128 e 127.
*  O `byte` tipo representa inteiros de 8 bits sem sinal com valores entre 0 e 255.
*  O `short` tipo representa inteiros de 16 bits com valores entre -32768 e 32767.
*  O `ushort` tipo representa inteiros de 16 bits sem sinal com valores entre 0 e 65535.
*  O `int` tipo representa inteiros de 32 bits com valores entre -2147483648 e 2147483647.
*  O `uint` tipo representa inteiros de 32 bits sem sinal com valores entre 0 e 4294967295.
*  O `long` tipo representa inteiros de 64 bits com valores entre -9223372036854775808 e 9223372036854775807.
*  O `ulong` tipo representa inteiros sem sinal de 64 bits com valores entre 0 e 18446744073709551615.
*  O `char` tipo representa inteiros de 16 bits sem sinal com valores entre 0 e 65535. O conjunto de valores possíveis para o tipo `char` corresponde ao conjunto de caracteres Unicode. Embora `char` tem a mesma representação `ushort`, nem todas as operações permitidas em um tipo são permitidas no outro.

O tipo integral operador unário ou binário sempre operam com precisão de 32 bits com sinal, precisão de 32 bits sem sinal, precisão de 64 bits com sinal ou precisão de 64 bits sem sinal:

*  Para o operador unário `+` e `~` operadores, o operando é convertido para o tipo `T`, onde `T` é o primeiro `int`, `uint`, `long`, e `ulong` que pode representar totalmente todos valores possíveis do operando. A operação, em seguida, é executada usando a precisão do tipo `T`, e o tipo do resultado é `T`.
*  Para o operador unário `-` operador, operando será convertido ao tipo `T`, onde `T` é o primeiro `int` e `long` que possam representar todos os valores possíveis do operando. A operação, em seguida, é executada usando a precisão do tipo `T`, e o tipo do resultado é `T`. O operador unário `-` operador não pode ser aplicado a operandos do tipo `ulong`.
*  Para o binário `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, e `<=` operadores, os operandos serão convertidos ao tipo `T`, onde `T` é a primeira das `int`, `uint`, `long`, e `ulong` que possam representar todos os possíveis valores de ambos os operandos. A operação, em seguida, é executada usando a precisão do tipo `T`, e o tipo do resultado é `T` (ou `bool` para os operadores relacionais). Não é permitido para um operando ser do tipo `long` e outra para ser do tipo `ulong` com os operadores binários.
*  Para o binário `<<` e `>>` operadores, o operando esquerdo é convertido para o tipo `T`, onde `T` é o primeiro `int`, `uint`, `long`, e `ulong` que pode representar totalmente todos valores possíveis do operando. A operação, em seguida, é executada usando a precisão do tipo `T`, e o tipo do resultado é `T`.

O `char` tipo é classificado como um tipo integral, mas ele difere de outros tipos integrais de duas maneiras:

*  Não há nenhuma conversão implícita de outros tipos de `char` tipo. Em particular, mesmo que o `sbyte`, `byte`, e `ushort` tipos têm intervalos de valores que são totalmente representável usando o `char` conversões implícitas de, digite `sbyte`, `byte`, ou `ushort` para `char` não existem.
*  Constantes do `char` tipo deve ser escrito como *character_literal*s ou como *integer_literal*s em combinação com uma conversão para o tipo `char`. Por exemplo, `(char)10` é o mesmo que `'\x000A'`.

O `checked` e `unchecked` instruções e operadores são usadas para controlar a verificação estouro para conversões e operações aritméticas de tipo integral ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)). Em um `checked` produz um erro em tempo de compilação de contexto, um estouro ou faz com que um `System.OverflowException` seja lançada. Em um `unchecked` contexto, estouros são ignorados e quaisquer bits de ordem superior que não cabem no tipo de destino serão descartados.

### <a name="floating-point-types"></a>Tipos de ponto flutuante

C# oferece suporte a dois tipos de ponto flutuante: `float` e `double`. O `float` e `double` tipos são representados usando os 32 bits de precisão simples e 64 bits precisão dupla IEEE 754 formatos, que fornecem os seguintes conjuntos de valores:

*  Positivo zero e o negativo de zero. Na maioria das situações, zero positivo e negativo zero se comportam de forma idêntica como simples de valor zero, mas determinadas operações de distinguir entre os dois ([operador de divisão](expressions.md#division-operator)).
*  Infinito positivo e negativo infinito. Infinitos são produzidos por operações como divisão por zero de um número diferente de zero. Por exemplo, `1.0 / 0.0` resulta em infinito positivo, e `-1.0 / 0.0` produz negativo infinito.
*  O ***Not-a-Number*** valor NaN muitas vezes abreviado. NaNs são produzidas por operações de ponto flutuantes inválidas, como divisão por zero de zero.
*  O conjunto finito de valores diferentes de zero do formulário `s * m * 2^e`, onde `s` é 1 ou -1, e `m` e `e` são determinados pelo tipo de ponto flutuante específico: Para `float`, `0 < m < 2^24` e `-149 <= e <= 104`e para `double`, `0 < m < 2^53` e `1075 <= e <= 970`. Números de ponto flutuante desnormalizados são considerados valores válidos de diferente de zero.

O `float` tipo pode representar valores que variam de aproximadamente `1.5 * 10^-45` para `3.4 * 10^38` com uma precisão de 7 dígitos.

O `double` tipo pode representar valores que variam de aproximadamente `5.0 * 10^-324` para `1.7 × 10^308` com uma precisão de 15-16 dígitos.

Se um dos operandos de um operador binário é de um tipo de ponto flutuante, em seguida, o outro operando deve ser de um tipo integral ou um tipo de ponto flutuante e a operação é calculada da seguinte maneira:

*  Se um dos operandos for do tipo integral, o operando é convertido para o tipo de ponto flutuante de outro operando.
*  Em seguida, se qualquer um dos operandos é do tipo `double`, o outro operando é convertido em `double`, a operação é executada usando pelo menos `double` intervalo e precisão e o tipo do resultado é `double` (ou `bool` para o operadores relacionais).
*  Caso contrário, a operação é executada usando pelo menos `float` intervalo e precisão e o tipo do resultado é `float` (ou `bool` para os operadores relacionais).

Os operadores de ponto flutuantes, incluindo os operadores de atribuição, nunca geram exceções. Em vez disso, em situações excepcionais, operações de ponto flutuante produzem zero, infinito ou NaN, conforme descrito abaixo:

*  Se o resultado de uma operação de ponto flutuante for muito pequeno para o formato de destino, o resultado da operação se torna zero positivo ou negativo de zero.
*  Se o resultado de uma operação de ponto flutuante é muito grande para o formato de destino, o resultado da operação se torna infinito positivo ou negativo infinito.
*  Se uma operação de ponto flutuante for inválida, o resultado da operação se torna NaN.
*  Se um ou ambos os operandos de uma operação de ponto flutuante é NaN, o resultado da operação se torna NaN.

Operações de ponto flutuantes podem ser executadas com precisão maior do que o tipo de resultado da operação. Por exemplo, algumas arquiteturas de hardware oferecem suporte a um tipo de ponto flutuante "extended" ou "long double" com o maior intervalo e a precisão do que o `double` digite e execute implicitamente todas as operações de ponto flutuantes usando esse tipo de precisão mais alta. Somente a custo excessivo no desempenho podem essas arquiteturas de hardware ser feitas para executar operações de ponto flutuantes com menos precisão, e em vez de exigir uma implementação perder o desempenho e precisão, c# permite que um tipo de precisão mais alta seja usado para todas as operações de ponto flutuantes. Além de fornecer resultados mais precisos, isso raramente tem efeitos mensuráveis. No entanto, em expressões do formulário `x * y / z`, em que a multiplicação produz um resultado que está fora os `double` intervalo, mas a divisão subsequente leva o resultados temporários de volta para o `double` de intervalo, o fato de que a expressão é avaliadas em um intervalo maior formato pode causar um resultado finito para ser produzidos em vez de um infinito.

### <a name="the-decimal-type"></a>O tipo decimal

O tipo `decimal` é um tipo de dados de 128 bits adequado para cálculos financeiros e monetários. O `decimal` tipo pode representar valores variando `1.0 * 10^-28` para aproximadamente `7.9 * 10^28` com 28 a 29 dígitos significativos.

O conjunto finito de valores do tipo `decimal` estão no formato `(-1)^s * c * 10^-e`, em que o sinal `s` for 0 ou 1, o coeficiente `c` será determinada pelos `0 <= *c* < 2^96`e a escala `e` é, de modo que `0 <= e <= 28`. O `decimal` tipo não oferece suporte de NaN, infinitos ou zeros com sinal. Um `decimal` é representado como um inteiro de 96 bits dimensionado por uma potência de dez. Para `decimal`s com um valor absoluto menor que `1.0m`, o valor é exato para a 28ª casa decimal, mas nenhuma outra. Para `decimal`s com um valor absoluto maior que ou igual a `1.0m`, o valor é exatamente a 28 ou 29 dígitos. Contrary para o `float` e `double` números fracionários decimais como 0,1 de tipos de dados, podem ser representados exatamente no `decimal` representação. No `float` e `double` representações, esses números são geralmente frações infinitas, tornando essas representações mais propensas a arredondar erros.

Se um dos operandos de um operador binário é do tipo `decimal`, em seguida, o outro operando deve ser do tipo integral ou de tipo `decimal`. Se um operando do tipo integral estiver presente, ele será convertido em `decimal` antes da operação é executada.

O resultado de uma operação em valores do tipo `decimal` é o que resultaria de calcular um resultado exato (preservando escala, conforme definido para cada operador) e, em seguida, o arredondamento de acordo com a representação. Os resultados são arredondados para o mais próximo valor representável, e, quando um resultado for igualmente próximo de dois valores representáveis, como o valor que tem um número par na posição de dígitos menos significativa (Isso é conhecido como "arredondamento bancário"). Um resultado zero sempre tem um sinal de igual a 0 e uma escala de 0.

Se uma operação aritmética decimal produz um valor menor ou igual a `5 * 10^-29` no valor absoluto, o resultado da operação será zero. Se um `decimal` operação aritmética produz um resultado que é muito grande para o `decimal` formato, um `System.OverflowException` é gerada.

O `decimal` maior precisão do tipo tem um intervalo menor, mas que os tipos de ponto flutuantes. Portanto, conversões de tipos de ponto flutuante para `decimal` pode gerar exceções de estouro e conversões de `decimal` para os tipos de ponto flutuantes pode causar perda de precisão. Por esses motivos, não há nenhuma conversão implícita entre os tipos de ponto flutuantes e `decimal`, e sem conversões explícitas, não é possível misturar ponto flutuante e `decimal` operandos na mesma expressão.

### <a name="the-bool-type"></a>O tipo bool

O `bool` tipo representa quantidades lógicas booleanas. Os valores possíveis do tipo `bool` estão `true` e `false`.

Não há nenhuma conversão padrão entre `bool` e outros tipos. Em particular, o `bool` o tipo é distinto e separado de tipos integrais e um `bool` valor não pode ser usado no lugar de um valor integral e vice-versa.

Em linguagens C e C++, um valor zero de ponto flutuante ou integral ou um ponteiro nulo pode ser convertido para o valor booliano `false`, e um valor de ponto flutuante ou integral diferente de zero ou um ponteiro nulo não pode ser convertido para o valor booliano `true`. No c#, essas conversões são realizadas por comparar explicitamente um valor integral ou de ponto flutuante como zero ou comparando explicitamente uma referência de objeto `null`.

### <a name="enumeration-types"></a>Tipos de enumeração

Um tipo de enumeração é um tipo distinto com constantes nomeadas. Cada tipo de enumeração tem um tipo subjacente, o que deve ser `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`. O conjunto de valores do tipo de enumeração é o mesmo que o conjunto de valores do tipo subjacente. Valores do tipo de enumeração não são restritos aos valores das constantes nomeadas. Tipos de enumeração são definidos por meio de declarações de enumeração ([declarações de Enum](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Tipos que permitem valor nulo

Um tipo anulável pode representar todos os valores do seu ***tipo subjacente*** além de um valor nulo adicional. Um tipo que permite valor nulo é escrito `T?`, onde `T` é o tipo subjacente. Essa sintaxe é uma abreviação para `System.Nullable<T>`, e os dois formatos podem ser usados alternadamente.

Um ***tipo de valor não anulável*** por outro lado, é qualquer tipo de valor diferente de `System.Nullable<T>` e sua abreviação `T?` (para qualquer `T`), além de qualquer parâmetro de tipo que é restrito para ser um tipo de valor não anulável (ou seja, qualquer parâmetro de tipo com um `struct` restrição). O `System.Nullable<T>` tipo Especifica a restrição de tipo de valor para `T` ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), que significa que o tipo subjacente de um tipo anulável pode ser qualquer tipo de valor não anulável. O tipo subjacente de um tipo anulável não pode ser um tipo anulável ou um tipo de referência. Por exemplo, `int??` e `string?` são tipos inválidos.

Uma instância de um tipo anulável `T?` tem duas propriedades públicas somente leitura:

*  Um `HasValue` propriedade do tipo `bool`
*  Um `Value` propriedade do tipo `T`

Uma instância para o qual `HasValue` é true será considerada não-nulo. Uma instância não-nula contém um valor conhecido e `Value` retorna esse valor.

Uma instância para o qual `HasValue` é false deve ser nulo. Uma instância nula tem um valor indefinido. Ao tentar ler o `Value` de uma instância nula faz com que um `System.InvalidOperationException` seja lançada. O processo de acessar o `Value` propriedade de uma instância que permite valor nula é conhecida como ***descodificar***.

Além do construtor padrão, todos os tipos anuláveis `T?` tem um construtor público que aceita um único argumento do tipo `T`. Dado um valor `x` do tipo `T`, uma invocação de construtor do formulário

```csharp
new T?(x)
```
cria uma instância não nulos da `T?` para o qual o `Value` é de propriedade `x`. O processo de criação de uma instância não nulos de um tipo anulável de um determinado valor é conhecido como ***encapsulamento***.

Conversões implícitas estão disponíveis na `null` literal `T?` ([Null literais conversões](conversions.md#null-literal-conversions)) e do `T` para `T?` ([conversões implícitas anuláveis](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Tipos de referência

Um tipo de referência é um tipo de classe, um tipo de interface, um tipo de matriz ou um tipo de delegado.

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

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

delegate_type
    : type_name
    ;
```

Um valor de tipo de referência é uma referência a um ***instância*** do tipo, o último conhecido como um ***objeto***. O valor especial `null` é compatível com todos os tipos de referência e indica a ausência de uma instância.

### <a name="class-types"></a>Tipos de classe

Um tipo de classe define uma estrutura de dados que contém dados membros (campos e constantes), membros da função (métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e tipos aninhados. Tipos de classe dá suporte à herança, um mecanismo no qual as classes derivadas podem estender e especializar as classes base. Instâncias de tipos de classe são criadas usando *object_creation_expression*s ([expressões de criação do objeto](expressions.md#object-creation-expressions)).

Tipos de classe são descritos em [Classes](classes.md).

Determinados tipos de classe predefinida têm significado especial na linguagem c#, conforme descrito na tabela a seguir.


| __Tipo de classe__     | __Descrição__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | A classe base definitiva de todos os outros tipos. Ver [o tipo de objeto](types.md#the-object-type). | 
| `System.String`    | O tipo de cadeia de caracteres da linguagem c#. Ver [o tipo de cadeia de caracteres](types.md#the-string-type).         |
| `System.ValueType` | A classe base de todos os tipos de valor. Ver [ValueType o tipo](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | A classe base de todos os tipos de enum. Ver [Enums](enums.md).              |
| `System.Array`     | A classe base de todos os tipos de matriz. Consulte [Matrizes](arrays.md).             |
| `System.Delegate`  | A classe base de todos os tipos de delegado. Ver [delegados](delegates.md).          |
| `System.Exception` | A classe base de todos os tipos de exceção. Ver [exceções](exceptions.md).         |

### <a name="the-object-type"></a>O tipo de objeto

O `object` tipo de classe é a classe base definitiva de todos os outros tipos. Todos os tipos no c#, direta ou indiretamente derivem da `object` tipo de classe.

A palavra-chave `object` é simplesmente um alias para a classe predefinido `System.Object`.

### <a name="the-dynamic-type"></a>O tipo dinâmico

O `dynamic` tipo, como `object`, pode fazer referência a qualquer objeto. Quando os operadores são aplicados a expressões do tipo `dynamic`, sua resolução é adiada até que o programa é executado. Portanto, se o operador legalmente não pode ser aplicado ao objeto referenciado, nenhum erro será mostrado durante a compilação. Em vez disso, uma exceção será gerada quando a resolução do operador falha em tempo de execução.

Sua finalidade é permitir que a associação dinâmica, que é descrita detalhadamente no [vinculação dinâmica](expressions.md#dynamic-binding).

`dynamic` é considerado idêntico ao `object` , exceto nos seguintes aspectos:

*  Operações em expressões do tipo `dynamic` pode ser vinculado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).
*  Inferência de tipo ([inferência de tipos](expressions.md#type-inference)) vão preferir `dynamic` sobre `object` se ambos forem candidatos.

Devido a essa equivalência, a seguir contém:

*  Há uma conversão implícita de identidade entre `object` e `dynamic`e entre os tipos construídos são iguais ao substituir `dynamic` com `object`
*  Conversões implícitas e explícitas para e de `object` também se aplicam para e de `dynamic`.
*  Assinaturas de método são iguais ao substituir `dynamic` com `object` são consideradas a mesma assinatura
*  O tipo `dynamic` é indistinguível de `object` em tempo de execução.
*  Uma expressão do tipo `dynamic` é conhecido como um ***expressão dinâmica***.

### <a name="the-string-type"></a>O tipo de cadeia de caracteres

O `string` tipo é um tipo de classe sealed que herda diretamente de `object`. Instâncias do `string` classe representar cadeias de caracteres Unicode.

Os valores da `string` tipo pode ser gravado como literais de cadeia de caracteres ([literais de cadeia de caracteres](lexical-structure.md#string-literals)).

A palavra-chave `string` é simplesmente um alias para a classe predefinido `System.String`.

### <a name="interface-types"></a>Tipos de interface

Uma interface define um contrato. Uma classe ou struct que implementa uma interface deve cumprir o contrato. Uma interface pode herdar de várias interfaces base e uma classe ou struct pode implementar várias interfaces.

Tipos de interface são descritos em [Interfaces](interfaces.md).

### <a name="array-types"></a>Tipos de matriz

Uma matriz é uma estrutura de dados que contém zero ou mais variáveis que são acessadas por meio de índices calculados. As variáveis contidas em uma matriz, também chamada de elementos da matriz, são todos do mesmo tipo, e esse tipo é chamado o tipo de elemento da matriz.

Tipos de matriz são descritos em [matrizes](arrays.md).

### <a name="delegate-types"></a>Tipos delegados

Um delegado é uma estrutura de dados que se refere a um ou mais métodos. Por exemplo métodos, ele também se refere às suas instâncias de objeto correspondente.

O equivalente mais próximo de um delegado em C ou C++ é um ponteiro de função, mas, enquanto um ponteiro de função só pode fazer referência a funções estáticas, um delegado pode referenciar estáticos e métodos de instância. No último caso, o delegado armazena não apenas uma referência ao ponto de entrada do método, mas também uma referência à instância do objeto no qual invocar o método.

Tipos de delegado são descritos em [delegados](delegates.md).

## <a name="boxing-and-unboxing"></a>Conversões boxing e unboxing

O conceito de conversões boxing e unboxing é central para o sistema de tipos do #. Ele fornece uma ponte entre *value_type*s e *reference_type*s, permitindo que qualquer valor de um *value_type* a ser convertido para e do tipo `object`. Conversões boxing e unboxing permite que uma exibição unificada do sistema de tipo no qual um valor de qualquer tipo, por fim, pode ser tratado como um objeto.

### <a name="boxing-conversions"></a>Conversões boxing

Permite que uma conversão boxing um *value_type* a ser convertido implicitamente em um *reference_type*. As seguintes conversões boxing existirem:

*  De qualquer *value_type* para o tipo `object`.
*  De qualquer *value_type* para o tipo `System.ValueType`.
*  De qualquer *non_nullable_value_type* a qualquer *interface_type* implementado pelos *value_type*.
*  De qualquer *nullable_type* a qualquer *interface_type* implementado pelo tipo subjacente dos *nullable_type*.
*  De qualquer *enum_type* para o tipo `System.Enum`.
*  De qualquer *nullable_type* com uma subjacente *enum_type* para o tipo `System.Enum`.
*  Observe que uma conversão implícita de um parâmetro de tipo será executada como uma conversão boxing se em tempo de execução que ele acaba convertendo de um tipo de valor para um tipo de referência ([conversões implícitas que envolvem parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)).

Conversão boxing de um valor de uma *non_nullable_value_type* consiste de alocar uma instância do objeto e copiando o *non_nullable_value_type* valor para essa instância.

Conversão boxing de um valor de uma *nullable_type* produz uma referência nula se for o `null` valor (`HasValue` é `false`), ou o resultado de desempacotamento e boxing caso contrário, o valor subjacente.

O processo de conversão boxing de um valor de uma *non_nullable_value_type* é melhor explicada por imaginando a existência de um genérico ***classe boxing***, que se comporta como se tivesse sido declarada da seguinte maneira:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Conversão boxing de um valor `v` do tipo `T` agora consiste em executar a expressão `new Box<T>(v)`e retornar a instância resultante como um valor do tipo `object`. Portanto, as instruções
```csharp
int i = 123;
object box = i;
```
Conceitualmente, correspondem aos
```csharp
int i = 123;
object box = new Box<int>(i);
```

Uma classe de conversão boxing como `Box<T>` acima, na verdade, não existe e, na verdade, um tipo de classe não é do tipo dinâmico de um valor Demarcado. Em vez disso, um valor demarcado do tipo `T` tem o tipo dinâmico `T`e uma verificação de tipo dinâmico usando o `is` operador pode simplesmente fazer referência a tipo `T`. Por exemplo,
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
retornará a cadeia de caracteres "`Box contains an int`" no console.

Uma conversão boxing implica fazendo uma cópia do valor sendo convertido. Isso é diferente de uma conversão de um *reference_type* digitar `object`, em que o valor continua a referência à mesma instância e simplesmente é considerado como o tipo menos derivado `object`. Por exemplo, dada a declaração
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
as instruções a seguir
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
será o valor 10 no console de saída porque a operação de conversão boxing implícita que ocorre na atribuição de `p` à `box` faz com que o valor de `p` a ser copiado. Tinha `Point` foi declarado um `class` em vez disso, o valor 20 seria saída porque `p` e `box` faria referência à mesma instância.

### <a name="unboxing-conversions"></a>Conversões de conversão unboxing

Permite que uma conversão unboxing uma *reference_type* a ser convertido explicitamente em um *value_type*. Existem as seguintes conversões unboxing:

*  Do tipo `object` a qualquer *value_type*.
*  Do tipo `System.ValueType` a qualquer *value_type*.
*  De qualquer *interface_type* a qualquer *non_nullable_value_type* que implementa o *interface_type*.
*  De qualquer *interface_type* a qualquer *nullable_type* cujo tipo subjacente implementa o *interface_type*.
*  Do tipo `System.Enum` a qualquer *enum_type*.
*  Do tipo `System.Enum` a qualquer *nullable_type* com uma subjacente *enum_type*.
*  Observe que uma conversão explícita para um parâmetro de tipo será executada como uma conversão unboxing se em tempo de execução que ele acaba convertendo de um tipo de referência para um tipo de valor ([conversões explícitas de dinâmicas](conversions.md#explicit-dynamic-conversions)).

Uma operação de conversão unboxing para um *non_nullable_value_type* consiste em verificar primeiro que a instância do objeto é um valor demarcado da determinado *non_nullable_value_type*e, em seguida, copiar o valor da instância.

Unboxing para um *nullable_type* produz o valor null da *nullable_type* se o operando de origem for `null`, ou o resultado encapsulado de conversão unboxing a instância do objeto para o tipo subjacente dos *nullable_type* caso contrário.

Consultando a classe de conversão boxing imaginário descrita na seção anterior, uma conversão unboxing de um objeto `box` para um *value_type* `T` consiste em executar a expressão `((Box<T>)box).value`. Portanto, as instruções
```csharp
object box = 123;
int i = (int)box;
```
Conceitualmente, correspondem aos
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Para uma conversão unboxing para um determinado *non_nullable_value_type* para ter êxito em tempo de execução, o valor do operando de origem deve ser uma referência a um valor demarcado isso *non_nullable_value_type*. Se o operando de origem `null`, um `System.NullReferenceException` é gerada. Se o operando de origem for uma referência a um objeto incompatível, um `System.InvalidCastException` é gerada.

Para uma conversão unboxing para um determinado *nullable_type* para ter êxito em tempo de execução, o valor do operando de origem deve ser `null` ou uma referência a um valor demarcado de subjacente *non_nullable_value_type* do *nullable_type*. Se o operando de origem for uma referência a um objeto incompatível, um `System.InvalidCastException` é gerada.

## <a name="constructed-types"></a>Tipos construídos

Uma declaração de tipo genérico, por si só, denota um ***desassociada do tipo genérico*** que é usado como uma "esquema" para formar a muitos tipos diferentes, por meio da aplicação ***argumentos do tipo***. Os argumentos de tipo são gravados dentro de colchetes angulares (`<` e `>`) imediatamente após o nome do tipo genérico. Um tipo que inclui pelo menos um argumento de tipo é chamado de um ***tipo construído***. Um tipo construído pode ser usado na maioria dos lugares no idioma em que um nome de tipo pode aparecer. Um tipo genérico não associado pode ser usado somente dentro de um *typeof_expression* ([o operador typeof](expressions.md#the-typeof-operator)).

Tipos construídos também podem ser usados em expressões como nomes simples ([nomes simples](expressions.md#simple-names)) ou ao acessar um membro ([acesso de membro](expressions.md#member-access)).

Quando um *namespace_or_type_name* é avaliados, somente tipos genéricos com o número correto de parâmetros são considerados de tipo. Portanto, é possível usar o mesmo identificador para identificar tipos diferentes, desde que os tipos têm diferentes números de parâmetros de tipo. Isso é útil ao misturar as classes genéricas e não genéricas no mesmo programa:

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

Um *type_name* pode identificar um tipo construído, mesmo que ele não especifica os parâmetros de tipo diretamente. Isso pode ocorrer em que um tipo é aninhado dentro de uma declaração de classe genérica e o tipo de instância da declaração do recipiente implicitamente é usado para pesquisa de nome ([aninhados de tipos em classes genéricas](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

Um tipo construído em código não seguro, não pode ser usado como um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumentos de tipo

Cada argumento em uma lista de argumentos de tipo é simplesmente uma *tipo*.

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

Em código não seguro ([código não seguro](unsafe-code.md)), um *type_argument* não pode ser um tipo de ponteiro. Cada argumento de tipo deve atender quaisquer restrições no correspondente de parâmetro de tipo ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Tipos abertos e fechados

Todos os tipos podem ser classificados como ***tipos abertos*** ou ***fechado tipos***. Um tipo aberto é um tipo que envolve a parâmetros de tipo. Mais especificamente:

*  Um parâmetro de tipo define um tipo aberto.
*  Um tipo de matriz é um tipo aberto se e somente se o tipo de elemento é um tipo aberto.
*  Um tipo construído é um tipo aberto se e somente se um ou mais dos argumentos de tipo é um tipo aberto. Um tipo aninhado construído é um tipo aberto se e somente se um ou mais dos seus argumentos de tipo ou os argumentos de tipo do seu tipo recipiente (s) são um tipo aberto.

Um tipo fechado é um tipo que não é um tipo aberto.

Em tempo de execução, todo o código dentro de uma declaração de tipo genérico é executado no contexto de um tipo construído fechado que foi criado por meio da aplicação de argumentos de tipo para a declaração genérica. Cada parâmetro de tipo dentro de tipo genérico é associado a um determinado tipo de tempo de execução. O processamento de tempo de execução de todas as instruções e expressões sempre ocorre com tipos fechados, e tipos abertos ocorrerem somente durante o tempo de compilação processamento.

Cada tipo construído fechado tem seu próprio conjunto de variáveis estáticas, que não são compartilhadas com nenhum outro tipo construído fechado. Uma vez que um tipo aberto não existir no tempo de execução, não há nenhuma variável estática associado com um tipo aberto. Dois tipos construídos fechados são do mesmo tipo se eles são construídos a partir do mesmo tipo genérico não associado, e seus argumentos de tipo correspondente são do mesmo tipo.

### <a name="bound-and-unbound-types"></a>Associadas e tipos

O termo ***desassociada tipo*** se refere a um tipo não genérico ou um tipo genérico não associado. O termo ***vinculado tipo*** se refere a um tipo não genérico ou um tipo construído.

Um tipo não associado refere-se à entidade declarada com uma declaração de tipo. Um tipo genérico não associado não é propriamente um tipo e não pode ser usado como o tipo de uma variável, o argumento ou o valor de retorno ou como um tipo base. A construção de apenas um tipo genérico não associado no qual pode ser referenciado é o `typeof` expressão ([o operador typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Restrições satisfatória

Sempre que um tipo construído ou método genérico é referenciado, os argumentos de tipo fornecido são comparados com as restrições de parâmetro de tipo declaradas no tipo genérico ou método ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Para cada `where` cláusula, o argumento de tipo `A` que corresponde ao nomeado parâmetro de tipo é verificado em relação a cada restrição da seguinte maneira:

*  Se a restrição é um tipo de classe, um tipo de interface ou um parâmetro de tipo, permita que o `C` representam que a restrição com os argumentos de tipo fornecido substituído para qualquer parâmetro de tipo que aparecem na restrição. Para satisfazer a restrição, ele deve ser o caso que digita `A` pode ser convertido no tipo `C` por um dos seguintes:
    * Uma conversão de identidade ([conversão de identidade](conversions.md#identity-conversion))
    * Uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions))
    * Uma conversão boxing ([conversões Boxing](conversions.md#boxing-conversions)), desde que um é um tipo de valor não anulável.
    * Uma conversão implícita de parâmetro de referência, conversão boxing ou tipo de um parâmetro de tipo `A` para `C`.
*  Se a restrição é a restrição de tipo de referência (`class`), o tipo `A` deve atender a um dos seguintes:
    * `A` é um tipo de interface, o tipo de classe, o tipo de delegado ou o tipo de matriz. Observe que `System.ValueType` e `System.Enum` são tipos de referência que atendem a essa restrição.
    * `A` é um parâmetro de tipo que é conhecido por ser um tipo de referência ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
*  Se a restrição é a restrição de tipo de valor (`struct`), o tipo `A` deve atender a um dos seguintes:
    * `A` é um tipo de estrutura ou tipo enum, mas não é um tipo anulável. Observe que `System.ValueType` e `System.Enum` são tipos de referência que não atendem a essa restrição.
    * `A` é um parâmetro de tipo com a restrição de tipo de valor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
*  Se a restrição é a restrição de construtor `new()`, o tipo `A` não deve ser `abstract` e deve ter um construtor público sem parâmetros. Isso é atendido se uma das seguintes opções for verdadeira:
    * `A` é um tipo de valor, uma vez que todos os tipos de valor têm um construtor público padrão ([1&gt;construtores padrão](types.md#default-constructors)).
    * `A` é um parâmetro de tipo com restrição de construtor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
    * `A` é um parâmetro de tipo com a restrição de tipo de valor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
    * `A` é uma classe que não seja `abstract` e contém um explicitamente declarado `public` construtor sem parâmetros.
    * `A` não é `abstract` e tem um construtor padrão ([1&gt;construtores padrão](classes.md#default-constructors)).

Ocorrerá um erro de tempo de compilação se uma ou mais das restrições do parâmetro de tipo não são atendidas pelos argumentos de tipo em questão.

Uma vez que os parâmetros de tipo não são herdados, restrições nunca sejam herdados de qualquer um. No exemplo a seguir, `D` precisa especificar a restrição de parâmetro de tipo `T` , de modo que `T` satisfaz a restrição imposta pela classe base `B<T>`. Por outro lado, a classe `E` não é necessário especificar uma restrição, pois `List<T>` implementa `IEnumerable` para qualquer `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parâmetros de tipo

Um parâmetro de tipo é um identificador que designa um tipo de valor ou tipo de referência que o parâmetro está associado em tempo de execução.

```antlr
type_parameter
    : identifier
    ;
```

Uma vez que um parâmetro de tipo pode ser instanciado com muitos argumentos de tipo real diferente, parâmetros de tipo têm operações ligeiramente diferentes e restrições do que outros tipos. Elas incluem:

*  Um parâmetro de tipo não pode ser usado diretamente para declarar uma classe base ([classe Base](classes.md#base-class)) ou interface ([listas de parâmetros de tipo de variante](interfaces.md#variant-type-parameter-lists)).
*  As regras para a pesquisa de membro no tipo de parâmetros dependem de restrições, se houver, é aplicado ao parâmetro de tipo. Elas são detalhadas nas [pesquisa de membro](expressions.md#member-lookup).
*  As conversões disponíveis para um parâmetro de tipo depende das restrições, se houver, aplicado ao parâmetro de tipo. Elas são detalhadas nas [conversões implícitas que envolvem parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) e [conversões explícitas de dinâmicas](conversions.md#explicit-dynamic-conversions).
*  O literal `null` não pode ser convertido em um tipo de dado por um parâmetro de tipo, exceto se o parâmetro de tipo é conhecido por ser um tipo de referência ([conversões implícitas que envolvem parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)). No entanto, uma `default` expressão ([expressões de valor padrão](expressions.md#default-value-expressions)) pode ser usado em vez disso. Além disso, um valor com um tipo de dado por um parâmetro de tipo pode ser comparado com `null` usando `==` e `!=` ([operadores de igualdade de tipo de referência](expressions.md#reference-type-equality-operators)), a menos que o parâmetro de tipo tem a restrição de tipo de valor.
*  Um `new` expressão ([expressões de criação do objeto](expressions.md#object-creation-expressions)) só pode ser usado com um parâmetro de tipo, se o parâmetro de tipo é restrita por uma *constructor_constraint* ou o valor de tipo de restrição ([ Restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
*  Um parâmetro de tipo não pode ser usado em qualquer lugar dentro de um atributo.
*  Um parâmetro de tipo não pode ser usado em um acesso de membro ([acesso de membro](expressions.md#member-access)) ou nome de tipo ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) para identificar um membro estático ou um tipo aninhado.
*  O código não seguro, um parâmetro de tipo não pode ser usado como um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).

Como um tipo, parâmetros de tipo são puramente um constructo de tempo de compilação. Em tempo de execução, cada parâmetro de tipo é associado a um tipo de tempo de execução que foi especificado, fornecendo um argumento de tipo para a declaração de tipo genérico. Assim, o tipo de uma variável declarada com um testamento de parâmetro de tipo, em tempo de execução, ser um tipo construído fechado ([aberto e fechado tipos](types.md#open-and-closed-types)). O tempo de execução de todas as instruções e expressões que envolvem parâmetros de tipo usa o tipo real que foi fornecido como o argumento de tipo para esse parâmetro.

## <a name="expression-tree-types"></a>Tipos de árvore de expressão

***Árvores de expressão*** permitir que as expressões lambda sejam representadas como estruturas de dados em vez de código executável. Árvores de expressão são valores de ***tipos de árvore de expressão*** do formulário `System.Linq.Expressions.Expression<D>`, onde `D` é qualquer tipo de delegado. Para o restante dessa especificação nos referiremos a esses tipos usando a forma abreviada `Expression<D>`.

Se existe uma conversão de uma expressão lambda para um tipo de delegado `D`, também existe uma conversão para o tipo de árvore de expressão `Expression<D>`. Enquanto a conversão de uma expressão lambda para um tipo delegado gera um delegado que referencia o código executável para a expressão lambda, a conversão para um tipo de árvore de expressão cria uma representação de árvore de expressão da expressão lambda.

Árvores de expressão são representações eficiente de dados na memória de expressões lambda e tornam a estrutura da expressão lambda explícita e transparente.

Assim como um tipo de delegado `D`, `Expression<D>` tem o parâmetro e tipos de retorno, que são as mesmas que aquelas de `D`.

O exemplo a seguir representa uma expressão lambda como código executável e como uma árvore de expressão. Porque existe uma conversão para `Func<int,int>`, também existe uma conversão para `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Seguindo essas atribuições, o delegado `del` faz referência a um método que retorna `x + 1`e a árvore de expressão `exp` faz referência a uma estrutura de dados que descreve a expressão `x => x + 1`.

A definição exata do tipo genérico `Expression<D>` , bem como as regras precisas para construir uma árvore de expressão quando uma expressão lambda é convertida em um tipo de árvore de expressão, estão fora do escopo desta especificação.

Duas coisas são importantes para tornar explícito:

*  Nem todas as expressões de lambda podem ser convertidas em árvores de expressão. Por exemplo, as expressões lambda com corpos de instrução e expressões lambda que contêm expressões de atribuição não podem ser representado. Nesses casos, uma conversão ainda existe, mas falhará no tempo de compilação. Essas exceções são detalhadas em [conversões de função anônima](conversions.md#anonymous-function-conversions).
*   `Expression<D>` oferece um método de instância `Compile` que produz um delegado do tipo `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Invocar esse delegado faz com que o código representado pela árvore de expressão a ser executado. Assim, considerando as definições acima, del e del2 são equivalentes e as duas instruções a seguir terá o mesmo efeito:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Depois de executar esse código `i1` e `i2` terão o valor `2`.

