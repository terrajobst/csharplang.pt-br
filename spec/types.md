---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616133"
---
# <a name="types"></a>Tipos

Os tipos de C# idioma são divididos em duas categorias principais: ***tipos de valor*** e tipos de ***referência***. Os tipos de valor e de referência podem ser ***tipos genéricos***, que usam um ou mais ***parâmetros de tipo***. Os parâmetros de tipo podem designar tipos de valor e de referência.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

A categoria final dos tipos, ponteiros, está disponível somente em código não seguro. Isso é abordado mais detalhadamente em [tipos de ponteiro](unsafe-code.md#pointer-types).

Tipos de valor diferem dos tipos de referência, pois as variáveis dos tipos de valor contêm diretamente seus dados, enquanto as variáveis dos tipos de referência armazenam ***referências*** a seus dados, o último é conhecido como ***objetos***. Com os tipos de referência, é possível que duas variáveis referenciem o mesmo objeto e, assim, possíveis para operações em uma variável afetem o objeto referenciado pela outra variável. Com tipos de valor, as variáveis têm sua própria cópia dos dados e não é possível que as operações em um afetem a outra.

C#o sistema de tipos do é unificado, de modo que um valor de qualquer tipo possa ser tratado como um objeto. Cada tipo no C#, direta ou indiretamente, deriva do tipo de classe `object`, e `object` é a classe base definitiva de todos os tipos. Os valores de tipos de referência são tratados como objetos simplesmente exibindo os valores como tipo `object`. Os valores dos tipos de valor são tratados como objetos executando as operações boxing e unboxing ([boxing e unboxing](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Tipos de valor

Um tipo de valor é um tipo de struct ou um tipo de enumeração. C#fornece um conjunto de tipos de struct predefinidos chamados de ***tipos simples***. Os tipos simples são identificados por meio de palavras reservadas.

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

Ao contrário de uma variável de um tipo de referência, uma variável de um tipo de valor pode conter o valor `null` somente se o tipo de valor for um tipo anulável.  Para cada tipo de valor não anulável, há um tipo de valor anulável correspondente, indicando o mesmo conjunto de valores, mais o valor `null`.

A atribuição a uma variável de um tipo de valor cria uma cópia do valor que está sendo atribuído. Isso difere da atribuição a uma variável de um tipo de referência, que copia a referência, mas não o objeto identificado pela referência.

### <a name="the-systemvaluetype-type"></a>O tipo System. ValueType

Todos os tipos de valor herdam implicitamente da classe `System.ValueType`, que, por sua vez, herda da classe `object`. Não é possível que qualquer tipo seja derivado de um tipo de valor, e tipos de valor são, portanto, lacrados implicitamente ([classes lacradas](classes.md#sealed-classes)).

Observe que `System.ValueType` não é, em si, um *value_type*. Em vez disso, é um *class_type* do qual todos os *value_type*são derivados automaticamente.

### <a name="default-constructors"></a>Construtores padrão

Todos os tipos de valor declaram implicitamente um construtor de instância sem parâmetros público chamado ***construtor padrão***. O construtor padrão retorna uma instância inicializada em zero conhecida como o ***valor padrão*** para o tipo de valor:

*  Para todos os *Simple_Type*s, o valor padrão é o valor produzido por um padrão de bit de todos os zeros:
    * Para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`e `ulong`, o valor padrão é `0`.
    * Por `char`, o valor padrão é `'\x0000'`.
    * Por `float`, o valor padrão é `0.0f`.
    * Por `double`, o valor padrão é `0.0d`.
    * Por `decimal`, o valor padrão é `0.0m`.
    * Por `bool`, o valor padrão é `false`.
*  Para um `E`*enum_type* , o valor padrão é `0`, convertido para o tipo `E`.
*  Para um *struct_type*, o valor padrão é o valor produzido pela configuração de todos os campos de tipo de valor para seu valor padrão e todos os campos de tipo de referência como `null`.
*  Para um *nullable_type* , o valor padrão é uma instância para a qual a propriedade `HasValue` é false e a propriedade `Value` é indefinida. O valor padrão também é conhecido como o ***valor nulo*** do tipo anulável.

Como qualquer outro construtor de instância, o construtor padrão de um tipo de valor é invocado usando o operador de `new`. Por motivos de eficiência, esse requisito não tem como objetivo realmente fazer com que a implementação gere uma chamada de construtor. No exemplo a seguir, as variáveis `i` e `j` são inicializadas como zero.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Como cada tipo de valor implicitamente tem um construtor de instância sem parâmetros público, não é possível que um tipo de struct contenha uma declaração explícita de um construtor sem parâmetros. No entanto, um tipo struct é permitido para declarar construtores de instância com parâmetros ([construtores](structs.md#constructors)).

### <a name="struct-types"></a>Tipos struct

Um tipo de struct é um tipo de valor que pode declarar constantes, campos, métodos, propriedades, indexadores, operadores, construtores de instância, construtores estáticos e tipos aninhados. A declaração de tipos de struct é descrita em [declarações de struct](structs.md#struct-declarations).

### <a name="simple-types"></a>Tipos simples

C#fornece um conjunto de tipos de struct predefinidos chamados de ***tipos simples***. Os tipos simples são identificados por meio de palavras reservadas, mas essas palavras reservadas são simplesmente aliases para tipos de struct predefinidos no namespace `System`, conforme descrito na tabela a seguir.


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

Como um tipo simples é alias de um tipo struct, cada tipo simples tem membros. Por exemplo, `int` tem os membros declarados em `System.Int32` e os membros herdados de `System.Object`, e as instruções a seguir são permitidas:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Os tipos simples diferem de outros tipos de struct, pois permitem determinadas operações adicionais:

*  Os tipos mais simples permitem que os valores sejam criados escrevendo *literais* ([literais](lexical-structure.md#literals)). Por exemplo, `123` é um literal do tipo `int` e `'a'` é um literal do tipo `char`. C#Não faz nenhuma provisão para literais de tipos struct em geral, e valores não padrão de outros tipos struct são, por fim, sempre criados por meio de construtores de instância desses tipos struct.
*  Quando os operandos de uma expressão são todas constantes de tipo simples, é possível que o compilador avalie a expressão em tempo de compilação. Essa expressão é conhecida como *constant_expression* ([expressões constantes](expressions.md#constant-expressions)). As expressões que envolvem operadores definidos por outros tipos de struct não são consideradas expressões constantes.
*  Por meio de declarações `const` é possível declarar constantes dos tipos simples ([constantes](classes.md#constants)). Não é possível ter constantes de outros tipos de struct, mas um efeito semelhante é fornecido por `static readonly` campos.
*  As conversões que envolvem tipos simples podem participar da avaliação de operadores de conversão definidos por outros tipos de struct, mas um operador de conversão definido pelo usuário nunca pode participar da avaliação de outro operador definido pelo usuário ([avaliação de conversões definidas pelo usuário](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Tipos integrais

C#dá suporte a nove tipos integrais: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`e `char`. Os tipos integrais têm os seguintes tamanhos e intervalos de valores:

*  O tipo de `sbyte` representa inteiros de 8 bits assinados com valores entre-128 e 127.
*  O tipo de `byte` representa inteiros de 8 bits não assinados com valores entre 0 e 255.
*  O tipo de `short` representa inteiros de 16 bits assinados com valores entre-32768 e 32767.
*  O tipo de `ushort` representa inteiros de 16 bits não assinados com valores entre 0 e 65535.
*  O tipo de `int` representa números inteiros de 32 bits assinados com valores entre-2147483648 e 2147483647.
*  O tipo de `uint` representa inteiros de 32 bits sem sinal com valores entre 0 e 4294967295.
*  O tipo de `long` representa números inteiros de 64 bits assinados com valores entre-9.223.372.036.854.775.808 e 9223372036854775807.
*  O tipo de `ulong` representa inteiros de 64 bits sem sinal com valores entre 0 e 18446744073709551615.
*  O tipo de `char` representa inteiros de 16 bits não assinados com valores entre 0 e 65535. O conjunto de valores possíveis para o tipo `char` corresponde ao conjunto de caracteres Unicode. Embora `char` tenha a mesma representação que `ushort`, nem todas as operações permitidas em um tipo são permitidas no outro.

Os operadores unários e binários de tipo integral sempre operam com precisão de 32 bits assinada, precisão de 32 bits sem sinal, precisão de 64 bits assinada ou precisão de 64 bits não assinado:

*  Para os operadores unários `+` e `~`, o operando é convertido no tipo `T`, onde `T` é o primeiro de `int`, `uint`, `long`e `ulong` que pode representar totalmente todos os valores possíveis do operando. Em seguida, a operação é executada usando a precisão do tipo `T`e o tipo do resultado é `T`.
*  Para o operador de `-` unário, o operando é convertido no tipo `T`, em que `T` é o primeiro de `int` e `long` que pode representar totalmente todos os valores possíveis do operando. Em seguida, a operação é executada usando a precisão do tipo `T`e o tipo do resultado é `T`. O operador unário `-` não pode ser aplicado a operandos do tipo `ulong`.
*  Para os operadores binários `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`e `<=` , os operandos são convertidos no tipo `T`, em que `T` é o primeiro de `int`, `uint`, `long`e `ulong` que pode representar totalmente todos os valores possíveis de ambos os operandos. Em seguida, a operação é executada usando a precisão do tipo `T`e o tipo do resultado é `T` (ou `bool` para os operadores relacionais). Não é permitido que um operando seja do tipo `long` e o outro seja do tipo `ulong` com os operadores binários.
*  Para os operadores binários `<<` e `>>`, o operando esquerdo é convertido para o tipo `T`, em que `T` é o primeiro de `int`, `uint`, `long`e `ulong` que pode representar totalmente todos os valores possíveis do operando. Em seguida, a operação é executada usando a precisão do tipo `T`e o tipo do resultado é `T`.

O tipo de `char` é classificado como um tipo integral, mas difere dos outros tipos integrais de duas maneiras:

*  Não há conversões implícitas de outros tipos para o tipo de `char`. Em particular, embora os tipos `sbyte`, `byte`e `ushort` tenham intervalos de valores que são totalmente representáveis usando o tipo de `char`, conversões implícitas de `sbyte`, `byte`ou `ushort` para `char` não existem.
*  As constantes do tipo `char` devem ser gravadas como *character_literal*s ou como *integer_literal*s em combinação com uma conversão para o tipo `char`. Por exemplo, `(char)10` é o mesmo que `'\x000A'`.

Os operadores e as instruções `checked` e `unchecked` são usados para controlar a verificação de estouro para operações aritméticas de tipo integral e conversões ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)). Em um contexto de `checked`, um estouro produz um erro de tempo de compilação ou faz com que uma `System.OverflowException` seja gerada. Em um contexto de `unchecked`, os estouros são ignorados e quaisquer bits de ordem superior que não caibam no tipo de destino são descartados.

### <a name="floating-point-types"></a>Tipos de ponto flutuante

C#dá suporte a dois tipos de ponto flutuante: `float` e `double`. Os tipos `float` e `double` são representados usando os formatos IEEE 754 de precisão simples de 32 bits e de 64 bits, que fornecem os seguintes conjuntos de valores:

*  Zero positivo e zero negativo. Na maioria das situações, zero positivo e zero negativo se comportam de forma idêntica ao valor zero simples, mas determinadas operações distinguem entre os dois ([operador de divisão](expressions.md#division-operator)).
*  Infinity positivo e infinito negativo. Os infinitos são produzidos por operações como a divisão de um número diferente de zero por zero. Por exemplo, `1.0 / 0.0` produz infinito positivo e `-1.0 / 0.0` produz infinito negativo.
*  O valor ***não-a-numérico*** , geralmente abreviado como NaN. NaNs são produzidos por operações de ponto flutuante inválidas, como a divisão zero por zero.
*  O conjunto finito de valores diferentes de zero do formulário `s * m * 2^e`, em que `s` é 1 ou-1, e `m` e `e` são determinados pelo tipo de ponto flutuante específico: para `float`, `0 < m < 2^24` e `-149 <= e <= 104`e, para `double`, `0 < m < 2^53` e `-1075 <= e <= 970`. Números de ponto flutuante desnormalizados são considerados valores não zero válidos.

O tipo de `float` pode representar valores que variam de aproximadamente `1.5 * 10^-45` para `3.4 * 10^38` com uma precisão de 7 dígitos.

O tipo de `double` pode representar valores que variam de aproximadamente `5.0 * 10^-324` para `1.7 × 10^308` com uma precisão de 15-16 dígitos.

Se um dos operandos de um operador binário for de um tipo de ponto flutuante, o outro operando deverá ser de um tipo integral ou de um tipo de ponto flutuante, e a operação será avaliada da seguinte maneira:

*  Se um dos operandos for de um tipo integral, esse operando será convertido no tipo de ponto flutuante do outro operando.
*  Em seguida, se qualquer um dos operandos for do tipo `double`, o outro operando será convertido em `double`, a operação será executada usando pelo menos `double` intervalo e precisão, e o tipo do resultado será `double` (ou `bool` para os operadores relacionais).
*  Caso contrário, a operação é executada usando pelo menos `float` intervalo e precisão, e o tipo do resultado é `float` (ou `bool` para os operadores relacionais).

Os operadores de ponto flutuante, incluindo os operadores de atribuição, nunca produzem exceções. Em vez disso, em situações excepcionais, as operações de ponto flutuante produzem zero, infinito ou NaN, conforme descrito abaixo:

*  Se o resultado de uma operação de ponto flutuante for muito pequeno para o formato de destino, o resultado da operação se tornará positivo zero ou negativo zero.
*  Se o resultado de uma operação de ponto flutuante for muito grande para o formato de destino, o resultado da operação se tornará infinito positivo ou infinito negativo.
*  Se uma operação de ponto flutuante for inválida, o resultado da operação se tornará NaN.
*  Se um ou ambos os operandos de uma operação de ponto flutuante forem NaN, o resultado da operação se tornará NaN.

As operações de ponto flutuante podem ser executadas com precisão mais alta do que o tipo de resultado da operação. Por exemplo, algumas arquiteturas de hardware dão suporte a um tipo de ponto flutuante "estendido" ou "longo Duplo" com maior intervalo e precisão do que o tipo de `double` e executam implicitamente todas as operações de ponto flutuante usando esse tipo de precisão mais alto. Somente o custo excessivo no desempenho pode ser feito por tais arquiteturas de hardware para executar operações de ponto flutuante com menos precisão e, em vez de exigir uma implementação para perder o desempenho C# e a precisão, permite um tipo de precisão mais alto a ser usado para todas as operações de ponto flutuante. Além de fornecer resultados mais precisos, isso raramente tem efeitos mensuráveis. No entanto, em expressões do formulário `x * y / z`, em que a multiplicação produz um resultado que está fora do intervalo de `double`, mas a divisão subsequente traz o resultado temporário de volta para o intervalo de `double`, o fato de que a expressão é avaliada em um o formato de intervalo superior pode fazer com que um resultado finito seja produzido em vez de um infinito.

### <a name="the-decimal-type"></a>O tipo decimal

O tipo `decimal` é um tipo de dados de 128 bits adequado para cálculos financeiros e monetários. O tipo de `decimal` pode representar valores que variam de `1.0 * 10^-28` para aproximadamente `7.9 * 10^28` com 28-29 dígitos significativos.

O conjunto finito de valores do tipo `decimal` estão no formato `(-1)^s * c * 10^-e`, em que o sinal `s` é 0 ou 1, o coeficiente `c` é fornecido por `0 <= *c* < 2^96`e a `e` de escala é tal `0 <= e <= 28`. O tipo de `decimal` não dá suporte a zeros assinados, infinitos ou NaN. Uma `decimal` é representada como um inteiro de 96 bits dimensionado por uma potência de dez. Para `decimal`s com um valor absoluto menor que `1.0m`, o valor é exato para a casa de 28 casas decimais, mas não há mais. Para `decimal`s com um valor absoluto maior ou igual a `1.0m`, o valor é exato para 28 ou 29 dígitos. Ao contrário dos tipos de dados `float` e `double`, números fracionários decimais, como 0,1, podem ser representados exatamente na representação de `decimal`. Nas representações de `float` e `double`, esses números geralmente são frações infinitas, tornando essas representações mais propensas a erros de arredondamento.

Se um dos operandos de um operador binário for do tipo `decimal`, o outro operando deverá ser de um tipo integral ou do tipo `decimal`. Se um operando de tipo integral estiver presente, ele será convertido em `decimal` antes de a operação ser executada.

O resultado de uma operação em valores do tipo `decimal` é que isso resultaria no cálculo de um resultado exato (preservando a escala, conforme definido para cada operador) e, em seguida, arredondando para se ajustar à representação. Os resultados são arredondados para o valor representável mais próximo e, quando um resultado é igualmente próximo a dois valores representáveis, para o valor que tem um número par na posição de dígito menos significativa (isso é conhecido como "arredondamento do banco"). Um resultado zero sempre tem um sinal de 0 e uma escala de 0.

Se uma operação aritmética decimal produzir um valor menor ou igual a `5 * 10^-29` em valor absoluto, o resultado da operação se tornará zero. Se uma `decimal` operação aritmética produzir um resultado muito grande para o formato de `decimal`, uma `System.OverflowException` será lançada.

O tipo de `decimal` tem maior precisão, mas um intervalo menor do que os tipos de ponto flutuante. Assim, as conversões dos tipos de ponto flutuante para `decimal` podem produzir exceções de estouro, e as conversões de `decimal` para os tipos de ponto flutuante podem causar perda de precisão. Por esses motivos, não existem conversões implícitas entre os tipos de ponto flutuante e `decimal`e sem conversões explícitas, não é possível misturar os operandos de ponto flutuante e `decimal` na mesma expressão.

### <a name="the-bool-type"></a>O tipo bool

O tipo de `bool` representa quantidades lógicas boolianas. Os valores possíveis do tipo `bool` são `true` e `false`.

Não existem conversões padrão entre `bool` e outros tipos. Em particular, o tipo de `bool` é distinto e separado dos tipos integrais, e um valor de `bool` não pode ser usado no lugar de um valor integral e vice-versa.

Nas linguagens C C++ e, um valor integral zero ou de ponto flutuante, ou um ponteiro nulo pode ser convertido para o valor booliano `false`e um valor de ponto flutuante ou integral diferente de zero, ou um ponteiro não nulo pode ser convertido para o valor booliano `true`. No C#, essas conversões são realizadas comparando explicitamente um valor integral ou de ponto flutuante para zero ou comparando explicitamente uma referência de objeto para `null`.

### <a name="enumeration-types"></a>Tipos de enumeração

Um tipo de enumeração é um tipo distinto com constantes nomeadas. Cada tipo de enumeração tem um tipo subjacente, que deve ser `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`. O conjunto de valores do tipo de enumeração é o mesmo que o conjunto de valores do tipo subjacente. Os valores do tipo de enumeração não são restritos aos valores das constantes nomeadas. Os tipos de enumeração são definidos por meio de declarações de enumeração ([declarações enum](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Tipos que permitem valor nulo

Um tipo anulável pode representar todos os valores de seu ***tipo subjacente*** , além de um valor nulo adicional. Um tipo anulável é gravado `T?`, em que `T` é o tipo subjacente. Essa sintaxe é abreviada para `System.Nullable<T>`e os dois formulários podem ser usados de forma intercambiável.

Um ***tipo de valor não anulável por*** outro lado é qualquer tipo de valor diferente de `System.Nullable<T>` e sua `T?` abreviada (para qualquer `T`), além de qualquer parâmetro de tipo restrito a ser um tipo de valor não anulável (ou seja, qualquer parâmetro de tipo com um `struct` restrição). O tipo de `System.Nullable<T>` especifica a restrição de tipo de valor para `T` ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), o que significa que o tipo subjacente de um tipo anulável pode ser qualquer tipo de valor não anulável. O tipo subjacente de um tipo anulável não pode ser um tipo anulável ou um tipo de referência. Por exemplo, `int??` e `string?` são tipos inválidos.

Uma instância de um tipo anulável `T?` tem duas propriedades públicas somente leitura:

*  Uma propriedade `HasValue` do tipo `bool`
*  Uma propriedade `Value` do tipo `T`

Uma instância para a qual `HasValue` é true é considerada não nula. Uma instância não nula contém um valor conhecido e `Value` retorna esse valor.

Uma instância para a qual `HasValue` é falsa é indicada como NULL. Uma instância nula tem um valor indefinido. A tentativa de ler a `Value` de uma instância nula faz com que uma `System.InvalidOperationException` seja gerada. O processo de acessar a propriedade `Value` de uma instância anulável é chamado de ***desencapsulamento***.

Além do construtor padrão, todo tipo anulável `T?` tem um construtor público que usa um único argumento do tipo `T`. Dado um valor `x` do tipo `T`, uma invocação de construtor do formulário

```csharp
new T?(x)
```
Cria uma instância não nula do `T?` para o qual a propriedade `Value` é `x`. O processo de criação de uma instância não nula de um tipo anulável para um determinado valor é chamado de ***encapsulamento***.

Conversões implícitas estão disponíveis no `null` literal para `T?` ([conversões literais nulas](conversions.md#null-literal-conversions)) e de `T` a `T?` ([conversões anuláveis implícitas](conversions.md#implicit-nullable-conversions)).

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

Um valor de tipo de referência é uma referência a uma ***instância*** do tipo, o último conhecido como um ***objeto***. O valor especial `null` é compatível com todos os tipos de referência e indica a ausência de uma instância.

### <a name="class-types"></a>Tipos de classe

Um tipo de classe define uma estrutura de dados que contém membros de dados (constantes e campos), membros de função (métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e tipos aninhados. Os tipos de classe dão suporte à herança, um mecanismo pelo qual classes derivadas podem estender e especializar classes base. Instâncias de tipos de classe são criadas usando *object_creation_expression*s ([expressões de criação de objeto](expressions.md#object-creation-expressions)).

Os tipos de classe são descritos em [classes](classes.md).

Determinados tipos de classe predefinidos têm significado C# especial na linguagem, conforme descrito na tabela a seguir.


| __Tipo de classe__     | __Descrição__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | A classe base definitiva de todos os outros tipos. Consulte [o tipo de objeto](types.md#the-object-type). | 
| `System.String`    | O tipo de cadeia de C# caracteres do idioma. Consulte [o tipo de cadeia de caracteres](types.md#the-string-type).         |
| `System.ValueType` | A classe base de todos os tipos de valor. Consulte [o tipo System. ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | A classe base de todos os tipos enum. Consulte [enums](enums.md).              |
| `System.Array`     | A classe base de todos os tipos de matriz. Consulte [Matrizes](arrays.md).             |
| `System.Delegate`  | A classe base de todos os tipos delegados. Consulte [delegados](delegates.md).          |
| `System.Exception` | A classe base de todos os tipos de exceção. Consulte [exceções](exceptions.md).         |

### <a name="the-object-type"></a>O tipo de objeto

O tipo de classe `object` é a classe base definitiva de todos os outros tipos. Cada tipo em C# deriva direta ou indiretamente do tipo de classe `object`.

A palavra-chave `object` é simplesmente um alias para a classe predefinida `System.Object`.

### <a name="the-dynamic-type"></a>O tipo dinâmico

O tipo de `dynamic`, como `object`, pode fazer referência a qualquer objeto. Quando os operadores são aplicados a expressões do tipo `dynamic`, sua resolução é adiada até que o programa seja executado. Portanto, se o operador não puder ser aplicado legalmente ao objeto referenciado, nenhum erro será fornecido durante a compilação. Em vez disso, uma exceção será lançada quando a resolução do operador falhar em tempo de execução.

Sua finalidade é permitir a vinculação dinâmica, que é descrita em detalhes na [vinculação dinâmica](expressions.md#dynamic-binding).

`dynamic` é considerado idêntico ao `object`, exceto nos seguintes aspectos:

*  As operações em expressões do tipo `dynamic` podem ser vinculadas dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).
*  A inferência de tipos ([inferência de tipos](expressions.md#type-inference)) prefere `dynamic` sobre `object` se ambos forem candidatos.

Devido a essa equivalência, o seguinte contém:

*  Há uma conversão de identidade implícita entre `object` e `dynamic`e entre os tipos construídos que são os mesmos ao substituir `dynamic` por `object`
*  Conversões implícitas e explícitas de e para `object` também se aplicam de e para `dynamic`.
*  As assinaturas de método que são iguais ao substituir `dynamic` com `object` são consideradas a mesma assinatura
*  O tipo `dynamic` é indistinguíveis de `object` em tempo de execução.
*  Uma expressão do tipo `dynamic` é referenciada como uma ***expressão dinâmica***.

### <a name="the-string-type"></a>O tipo de cadeia de caracteres

O tipo de `string` é um tipo de classe lacrado que herda diretamente de `object`. As instâncias da classe `string` representam cadeias de caracteres Unicode.

Os valores do tipo de `string` podem ser gravados como literais de cadeia de caracteres ([literais de cadeia de caracteres](lexical-structure.md#string-literals)).

A palavra-chave `string` é simplesmente um alias para a classe predefinida `System.String`.

### <a name="interface-types"></a>Tipos de interface

Uma interface define um contrato. Uma classe ou struct que implementa uma interface deve aderir a seu contrato. Uma interface pode herdar de várias interfaces base e uma classe ou estrutura pode implementar várias interfaces.

Os tipos de interface são descritos em [interfaces](interfaces.md).

### <a name="array-types"></a>Tipos de matriz

Uma matriz é uma estrutura de dados que contém zero ou mais variáveis que são acessadas por meio de índices computados. As variáveis contidas em uma matriz, também chamadas de elementos da matriz, são do mesmo tipo, e esse tipo é chamado de tipo de elemento da matriz.

Os tipos de matriz são descritos em [matrizes](arrays.md).

### <a name="delegate-types"></a>Tipos delegados

Um delegado é uma estrutura de dados que se refere a um ou mais métodos. Para métodos de instância, ele também se refere às suas instâncias de objeto correspondentes.

O equivalente mais próximo de um delegado em C ou C++ é um ponteiro de função, mas enquanto um ponteiro de função só pode referenciar funções estáticas, um delegado pode fazer referência a métodos estáticos e de instância. No último caso, o delegado armazena não apenas uma referência ao ponto de entrada do método, mas também uma referência à instância do objeto na qual invocar o método.

Os tipos delegados são descritos em [delegados](delegates.md).

## <a name="boxing-and-unboxing"></a>Conversões boxing e unboxing

O conceito de boxing e unboxing é fundamental para C#o sistema de tipos. Ele fornece uma ponte entre *value_type*s e *reference_type*s, permitindo que qualquer valor de um *value_type* seja convertido de e para o tipo `object`. Boxing e unboxing permitem uma exibição unificada do sistema de tipos, em que um valor de qualquer tipo pode ser tratado, por fim, como um objeto.

### <a name="boxing-conversions"></a>Conversões de Boxing

Uma conversão boxing permite que um *value_type* seja convertido implicitamente em um *reference_type*. Existem as seguintes conversões de boxing:

*  De qualquer *value_type* para o tipo `object`.
*  De qualquer *value_type* para o tipo `System.ValueType`.
*  De qualquer *non_nullable_value_type* para qualquer *interface_type* implementado pelo *value_type*.
*  De qualquer *nullable_type* para qualquer *interface_type* implementado pelo tipo subjacente do *nullable_type*.
*  De qualquer *enum_type* para o tipo `System.Enum`.
*  De qualquer *nullable_type* com um *enum_type* subjacente para o tipo `System.Enum`.
*  Observe que uma conversão implícita de um parâmetro de tipo será executada como uma conversão boxing se em tempo de execução acabar convertendo de um tipo de valor para um tipo de referência ([conversões implícitas envolvendo parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)).

Boxing um valor de um *non_nullable_value_type* consiste em alocar uma instância de objeto e copiar o valor de *non_nullable_value_type* nessa instância.

Boxing um valor de um *nullable_type* produzirá uma referência nula se for o valor `null` (`HasValue` é `false`) ou o resultado de desencapsulamento e Boxing do valor subjacente, caso contrário.

O processo real de boxing, um valor de um *non_nullable_value_type* , é mais bem explicado ao imaginar a existência de uma ***classe Boxing***genérica, que se comporta como se fosse declarada da seguinte maneira:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

A Boxing de um valor `v` do tipo `T` agora consiste em executar a expressão `new Box<T>(v)`e retornar a instância resultante como um valor do tipo `object`. Portanto, as instruções
```csharp
int i = 123;
object box = i;
```
corresponde conceitualmente a
```csharp
int i = 123;
object box = new Box<int>(i);
```

Uma classe Boxing como `Box<T>` acima não existe realmente e o tipo dinâmico de um valor boxed não é, na verdade, um tipo de classe. Em vez disso, um valor em caixa do tipo `T` tem o tipo dinâmico `T`e uma verificação de tipo dinâmico usando o operador `is` pode simplesmente referenciar o tipo `T`. Por exemplo,
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
produzirá a cadeia de caracteres "`Box contains an int`" no console.

Uma conversão de Boxing implica fazer uma cópia do valor em caixa. Isso é diferente de uma conversão de um *reference_type* para o tipo `object`, no qual o valor continua referenciando a mesma instância e simplesmente é considerado como o tipo menos derivado `object`. Por exemplo, dada a declaração
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
produzirá o valor 10 no console, pois a operação Boxing implícita que ocorre na atribuição de `p` para `box` faz com que o valor de `p` seja copiado. `Point` foi declarada uma `class` em vez disso, o valor 20 seria resultado porque `p` e `box` referenciariam a mesma instância.

### <a name="unboxing-conversions"></a>Conversões de unboxing

Uma conversão unboxing permite que um *reference_type* seja explicitamente convertido em um *value_type*. Existem as seguintes conversões de não conboxing:

*  Do tipo `object` para qualquer *value_type*.
*  Do tipo `System.ValueType` para qualquer *value_type*.
*  De qualquer *interface_type* a qualquer *non_nullable_value_type* que implemente o *interface_type*.
*  De qualquer *interface_type* a qualquer *nullable_type* cujo tipo subjacente implemente o *interface_type*.
*  Do tipo `System.Enum` para qualquer *enum_type*.
*  Do tipo `System.Enum` para qualquer *nullable_type* com um *enum_type*subjacente.
*  Observe que uma conversão explícita em um parâmetro de tipo será executada como uma conversão sem Boxing se, em tempo de execução, ele terminar a conversão de um tipo de referência para um tipo de valor ([conversões dinâmicas explícitas](conversions.md#explicit-dynamic-conversions)).

Uma operação de unboxing para um *non_nullable_value_type* consiste na primeira verificação de que a instância do objeto é um valor em caixa do *non_nullable_value_type*especificado e, em seguida, copiar o valor para fora da instância.

Unboxing para um *nullable_type* produz o valor nulo de *nullable_type* se o operando de origem for `null`ou o resultado encapsulado de uma instância de objeto para o tipo subjacente de *nullable_type* não for o caso.

Fazendo referência à classe de conversão de erros imaginário descrita na seção anterior, uma conversão unboxing de um objeto `box` a uma `T` *value_type* consiste em executar a expressão `((Box<T>)box).value`. Portanto, as instruções
```csharp
object box = 123;
int i = (int)box;
```
corresponde conceitualmente a
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Para que uma conversão de unboxing para um determinado *non_nullable_value_type* seja realizada em tempo de execução, o valor do operando de origem deve ser uma referência a um valor de Boxed desse *non_nullable_value_type*. Se o operando de origem for `null`, um `System.NullReferenceException` será gerado. Se o operando de origem for uma referência a um objeto incompatível, um `System.InvalidCastException` será gerado.

Para que uma conversão sem Boxing para um determinado *nullable_type* tenha sucesso em tempo de execução, o valor do operando de origem deve ser `null` ou uma referência a um valor em caixa do *non_nullable_value_type* subjacente do *nullable_type*. Se o operando de origem for uma referência a um objeto incompatível, um `System.InvalidCastException` será gerado.

## <a name="constructed-types"></a>Tipos construídos

Uma declaração de tipo genérico, por si só, denota um ***tipo genérico não associado*** que é usado como um "plano gráfico" para formar muitos tipos diferentes, por meio da aplicação de ***argumentos de tipo***. Os argumentos de tipo são gravados entre colchetes angulares (`<` e `>`) imediatamente após o nome do tipo genérico. Um tipo que inclui pelo menos um argumento de tipo é chamado de ***tipo construído***. Um tipo construído pode ser usado na maioria dos lugares no idioma em que um nome de tipo pode aparecer. Um tipo genérico não associado só pode ser usado em um *typeof_expression* ([o operador typeof](expressions.md#the-typeof-operator)).

Os tipos construídos também podem ser usados em expressões como nomes simples ([nomes simples](expressions.md#simple-names)) ou ao acessar um membro ([acesso de membro](expressions.md#member-access)).

Quando um *namespace_or_type_name* é avaliado, somente tipos genéricos com o número correto de parâmetros de tipo são considerados. Portanto, é possível usar o mesmo identificador para identificar diferentes tipos, desde que os tipos tenham números diferentes de parâmetros de tipo. Isso é útil ao misturar classes genéricas e não genéricas no mesmo programa:

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

Um *type_name* pode identificar um tipo construído, mesmo que ele não especifique parâmetros de tipo diretamente. Isso pode ocorrer quando um tipo é aninhado dentro de uma declaração de classe genérica, e o tipo de instância da declaração recipiente é usado implicitamente para pesquisa de nome ([tipos aninhados em classes genéricas](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

Em código não seguro, um tipo construído não pode ser usado como um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumentos de tipo

Cada argumento em uma lista de argumentos de tipo é simplesmente um *tipo*.

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

Em código não seguro ([código não seguro](unsafe-code.md)), um *type_argument* não pode ser um tipo de ponteiro. Cada argumento de tipo deve atender a qualquer restrição no parâmetro de tipo correspondente ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Tipos abertos e fechados

Todos os tipos podem ser classificados como ***tipos abertos*** ou ***fechados***. Um tipo aberto é um tipo que envolve parâmetros de tipo. Mais especificamente:

*  Um parâmetro de tipo define um tipo aberto.
*  Um tipo de matriz é um tipo aberto se e somente se seu tipo de elemento for um tipo aberto.
*  Um tipo construído é um tipo aberto se e somente se um ou mais dos seus argumentos de tipo for um tipo aberto. Um tipo aninhado construído é um tipo aberto se e somente se um ou mais dos seus argumentos de tipo ou os argumentos de tipo de seus tipos contiverem um tipo aberto.

Um tipo fechado é um tipo que não é um tipo aberto.

Em tempo de execução, todo o código dentro de uma declaração de tipo genérico é executado no contexto de um tipo construído fechado que foi criado pela aplicação de argumentos de tipo à declaração genérica. Cada parâmetro de tipo dentro do tipo genérico está associado a um tipo de tempo de execução específico. O processamento em tempo de execução de todas as instruções e expressões sempre ocorre com tipos fechados e os tipos abertos ocorrem somente durante o processamento de tempo de compilação.

Cada tipo construído fechado tem seu próprio conjunto de variáveis estáticas, que não são compartilhadas com nenhum outro tipo construído fechado. Como um tipo aberto não existe em tempo de execução, não há variáveis estáticas associadas a um tipo aberto. Dois tipos construídos fechados são o mesmo tipo se forem construídos do mesmo tipo genérico não associado e seus argumentos de tipo correspondentes forem do mesmo tipo.

### <a name="bound-and-unbound-types"></a>Tipos vinculados e desvinculados

O termo ***tipo não associado*** se refere a um tipo não genérico ou a um tipo genérico não associado. O termo ***tipo associado*** refere-se a um tipo não genérico ou construído.

Um tipo não associado refere-se à entidade declarada por uma declaração de tipo. Um tipo genérico não associado não é, em si, um tipo e não pode ser usado como o tipo de uma variável, um argumento ou um valor de retorno, ou como um tipo base. A única construção na qual um tipo genérico não associado pode ser referenciado é a expressão `typeof` ([o operador typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Atendendo às restrições

Sempre que um tipo construído ou um método genérico é referenciado, os argumentos de tipo fornecidos são verificados em relação às restrições de parâmetro de tipo declaradas no tipo ou método genérico ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Para cada cláusula de `where`, o argumento de tipo `A` que corresponde ao parâmetro de tipo nomeado é verificado em relação a cada restrição da seguinte maneira:

*  Se a restrição for um tipo de classe, um tipo de interface ou um parâmetro de tipo, deixe `C` representar essa restrição com os argumentos de tipo fornecidos substituídos por qualquer parâmetro de tipo que apareça na restrição. Para atender à restrição, ele deve ser o caso em que o tipo `A` é conversível para digitar `C` por um dos seguintes:
    * Uma conversão de identidade ([conversão de identidade](conversions.md#identity-conversion))
    * Uma conversão de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions))
    * Uma conversão boxing ([conversões Boxing](conversions.md#boxing-conversions)), desde que o tipo a seja um tipo de valor não anulável.
    * Uma referência implícita, Boxing ou conversão de parâmetro de tipo de um parâmetro de tipo `A` para `C`.
*  Se a restrição for a restrição de tipo de referência (`class`), o tipo `A` deverá satisfazer um dos seguintes:
    * `A` é um tipo de interface, tipo de classe, tipo delegado ou tipo de matriz. Observe que `System.ValueType` e `System.Enum` são tipos de referência que atendem a essa restrição.
    * `A` é um parâmetro de tipo que é conhecido como um tipo de referência ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
*  Se a restrição for a restrição de tipo de valor (`struct`), o tipo `A` deverá satisfazer um dos seguintes:
    * `A` é um tipo de struct ou tipo de enumeração, mas não um tipo anulável. Observe que `System.ValueType` e `System.Enum` são tipos de referência que não atendem a essa restrição.
    * `A` é um parâmetro de tipo que tem a restrição de tipo de valor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
*  Se a restrição for a restrição de Construtor `new()`, o tipo `A` não deverá ser `abstract` e deverá ter um construtor público sem parâmetros. Isso será atendido se uma das seguintes opções for verdadeira:
    * `A` é um tipo de valor, já que todos os tipos de valor têm um construtor público padrão ([construtores padrão](types.md#default-constructors)).
    * `A` é um parâmetro de tipo que tem a restrição de Construtor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
    * `A` é um parâmetro de tipo que tem a restrição de tipo de valor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).
    * `A` é uma classe que não é `abstract` e contém um construtor de `public` declarado explicitamente sem parâmetros.
    * `A` não é `abstract` e tem um construtor padrão ([construtores padrão](classes.md#default-constructors)).

Ocorrerá um erro de tempo de compilação se uma ou mais das restrições de um parâmetro de tipo não forem atendidas pelos argumentos de tipo fornecidos.

Como os parâmetros de tipo não são herdados, as restrições nunca são herdadas. No exemplo a seguir, `D` precisa especificar a restrição em seu parâmetro de tipo `T` para que `T` atenda à restrição imposta pela classe base `B<T>`. Por outro lado, a classe `E` não precisa especificar uma restrição, porque `List<T>` implementa `IEnumerable` para qualquer `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parâmetros de tipo

Um parâmetro de tipo é um identificador que designa um tipo de valor ou tipo de referência ao qual o parâmetro está associado em tempo de execução.

```antlr
type_parameter
    : identifier
    ;
```

Como um parâmetro de tipo pode ser instanciado com muitos argumentos de tipo real diferentes, os parâmetros de tipo têm operações e restrições ligeiramente diferentes dos outros tipos. Elas incluem:

*  Um parâmetro de tipo não pode ser usado diretamente para declarar uma classe base ([classe base](classes.md#base-class)) ou uma interface ([listas de parâmetros de tipo Variant](interfaces.md#variant-type-parameter-lists)).
*  As regras para pesquisa de membro em parâmetros de tipo dependem das restrições, se houver, aplicadas ao parâmetro de tipo. Eles são detalhados na [pesquisa de membros](expressions.md#member-lookup).
*  As conversões disponíveis para um parâmetro de tipo dependem das restrições, se houver, aplicadas ao parâmetro de tipo. Eles são detalhados em [conversões implícitas envolvendo parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) e [conversões dinâmicas explícitas](conversions.md#explicit-dynamic-conversions).
*  O `null` literal não pode ser convertido em um tipo fornecido por um parâmetro de tipo, exceto se o parâmetro de tipo for conhecido como um tipo de referência ([conversões implícitas envolvendo parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)). No entanto, uma expressão de `default` ([expressões de valor padrão](expressions.md#default-value-expressions)) pode ser usada em seu lugar. Além disso, um valor com um tipo fornecido por um parâmetro de tipo pode ser comparado com `null` usando `==` e `!=` ([operadores de igualdade de tipo de referência](expressions.md#reference-type-equality-operators)), a menos que o parâmetro de tipo tenha a restrição de tipo de valor.
*  Uma expressão de `new` ([expressões de criação de objeto](expressions.md#object-creation-expressions)) só poderá ser usada com um parâmetro de tipo se o parâmetro de tipo for restrito por uma *constructor_constraint* ou a restrição de tipo de valor (restrições de[parâmetro de tipo](classes.md#type-parameter-constraints)).
*  Um parâmetro de tipo não pode ser usado em nenhum lugar dentro de um atributo.
*  Um parâmetro de tipo não pode ser usado em um acesso de membro ([acesso de membro](expressions.md#member-access)) ou nome de tipo ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)) para identificar um membro estático ou um tipo aninhado.
*  Em código não seguro, um parâmetro de tipo não pode ser usado como um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).

Como um tipo, os parâmetros de tipo são puramente uma construção de tempo de compilação. Em tempo de execução, cada parâmetro de tipo é associado a um tipo de tempo de execução que foi especificado fornecendo um argumento de tipo para a declaração de tipo genérico. Assim, o tipo de uma variável declarada com um parâmetro de tipo será, em tempo de execução, ser um tipo construído fechado ([tipos abertos e fechados](types.md#open-and-closed-types)). A execução em tempo de execução de todas as instruções e expressões que envolvem parâmetros de tipo usa o tipo real que foi fornecido como o argumento de tipo para esse parâmetro.

## <a name="expression-tree-types"></a>Tipos de árvore de expressão

As ***árvores de expressão*** permitem que as expressões lambda sejam representadas como estruturas de dados em vez de código executável. Árvores de expressão são valores de ***tipos de árvore de expressão*** do formulário `System.Linq.Expressions.Expression<D>`, em que `D` é qualquer tipo delegado. Para o restante desta especificação, iremos nos referir a esses tipos usando o `Expression<D>`abreviado.

Se existir uma conversão de uma expressão lambda para um tipo delegado `D`, uma conversão também existirá para o tipo de árvore de expressão `Expression<D>`. Enquanto a conversão de uma expressão lambda em um tipo delegado gera um delegado que referencia o código executável para a expressão lambda, a conversão para um tipo de árvore de expressão cria uma representação de árvore de expressão da expressão lambda.

As árvores de expressão são representações de dados na memória eficientes de expressões lambda e tornam a estrutura da expressão lambda transparente e explícita.

Assim como um tipo delegado `D`, o `Expression<D>` é dito para ter tipos de parâmetro e de retorno, que são os mesmos do `D`.

O exemplo a seguir representa uma expressão lambda tanto como código executável quanto como uma árvore de expressão. Como existe uma conversão para `Func<int,int>`, uma conversão também existe para `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Após essas atribuições, o delegado `del` faz referência a um método que retorna `x + 1`e a árvore de expressão `exp` faz referência a uma estrutura de dados que descreve a `x => x + 1`de expressão.

A definição exata do tipo genérico `Expression<D>`, bem como as regras precisas para construir uma árvore de expressão quando uma expressão lambda é convertida em um tipo de árvore de expressão, estão fora do escopo desta especificação.

Duas coisas são importantes para tornar explícitas:

*  Nem todas as expressões lambda podem ser convertidas em árvores de expressão. Por exemplo, expressões lambda com corpos de instrução e expressões lambda contendo expressões de atribuição não podem ser representadas. Nesses casos, uma conversão ainda existe, mas falhará em tempo de compilação. Essas exceções são detalhadas em [conversões de função anônimas](conversions.md#anonymous-function-conversions).
*   `Expression<D>` oferece um método de instância `Compile` que produz um delegado do tipo `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Invocar esse delegado faz com que o código representado pela árvore de expressão seja executado. Assim, dadas as definições acima, del e del2 são equivalentes, e as duas instruções a seguir terão o mesmo efeito:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Depois de executar esse código, `i1` e `i2` terão o valor `2`.

