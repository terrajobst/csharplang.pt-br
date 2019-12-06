---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74867998"
---
# <a name="conversions"></a>Conversões

Uma ***conversão*** permite que uma expressão seja tratada como sendo de um tipo específico. Uma conversão pode fazer com que uma expressão de um determinado tipo seja tratada como tendo um tipo diferente, ou pode causar uma expressão sem um tipo para obter um tipo. As conversões podem ser ***implícitas*** ou ***explícitas***, e isso determina se uma conversão explícita é necessária. Por exemplo, a conversão do tipo `int` para o tipo `long` é implícita, portanto, as expressões do tipo `int` podem ser tratadas implicitamente como tipo `long`. A conversão oposta, do tipo `long` para o tipo `int`, é explícita e, portanto, é necessária uma conversão explícita.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Algumas conversões são definidas pelo idioma. Os programas também podem definir suas próprias conversões ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Conversões implícitas

As conversões a seguir são classificadas como conversões implícitas:

*  Conversões de identidade
*  Conversões numéricas implícitas
*  Conversões implícitas de enumeração
*  Conversões de cadeia de caracteres interpoladas implícitas
*  Conversões anuláveis implícitas
*  Conversões literais nulas
*  Conversões de referência implícitas
*  Conversões de Boxing
*  Conversões dinâmicas implícitas
*  Conversões de expressão de constante implícita
*  Conversões implícitas definidas pelo usuário
*  Conversões de função anônimas
*  Conversões de grupo de métodos

Conversões implícitas podem ocorrer em várias situações, incluindo invocações de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), expressões de conversão ([expressões de conversão](expressions.md#cast-expressions)) e atribuições ([operadores de atribuição](expressions.md#assignment-operators)).

As conversões implícitas predefinidas sempre são bem sucedidos e nunca causam a geração de exceções. Conversões implícitas definidas pelo usuário corretamente criadas também devem apresentar essas características.

Para fins de conversão, os tipos `object` e `dynamic` são considerados equivalentes.

No entanto, conversões dinâmicas (conversões[dinâmicas implícitas](conversions.md#implicit-dynamic-conversions) e [conversões dinâmicas explícitas](conversions.md#explicit-dynamic-conversions)) se aplicam somente a expressões do tipo `dynamic` ([o tipo dinâmico](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Conversão de identidade

Uma conversão de identidade converte de qualquer tipo para o mesmo tipo. Essa conversão existe de modo que uma entidade que já tenha um tipo necessário seja considerada para ser conversível para esse tipo.

*  Como `object` e `dynamic` são considerados equivalentes, há uma conversão de identidade entre `object` e `dynamic`e entre os tipos construídos que são os mesmos ao substituir todas as ocorrências de `dynamic` por `object`.

### <a name="implicit-numeric-conversions"></a>Conversões numéricas implícitas

As conversões numéricas implícitas são:

*  De `sbyte` para `short`, `int`, `long`, `float`, `double`ou `decimal`.
*  De `byte` para `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`ou `decimal`.
*  De `short` para `int`, `long`, `float`, `double`ou `decimal`.
*  De `ushort` para `int`, `uint`, `long`, `ulong`, `float`, `double`ou `decimal`.
*  De `int` para `long`, `float`, `double`ou `decimal`.
*  De `uint` para `long`, `ulong`, `float`, `double`ou `decimal`.
*  De `long` para `float`, `double`ou `decimal`.
*  De `ulong` para `float`, `double`ou `decimal`.
*  De `char` para `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`ou `decimal`.
*  De `float` a `double`.

As conversões de `int`, `uint`, `long`ou `ulong` para `float` e de `long` ou `ulong` para `double` podem causar perda de precisão, mas nunca causarão uma perda de magnitude. As outras conversões numéricas implícitas nunca perdem nenhuma informação.

Não há conversões implícitas para o tipo de `char`, de modo que os valores dos outros tipos integrais não são convertidos automaticamente para o tipo de `char`.

### <a name="implicit-enumeration-conversions"></a>Conversões implícitas de enumeração

Uma conversão de enumeração implícita permite que o `0` de *decimal_integer_literal* seja convertido em qualquer *enum_type* e em qualquer *nullable_type* cujo tipo subjacente seja um *enum_type*. No último caso, a conversão é avaliada convertendo para o *enum_type* subjacente e encapsulando o resultado ([tipos anuláveis](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Conversões de cadeia de caracteres interpoladas implícitas

Uma conversão de cadeia de caracteres interpolada implícita permite que um *interpolated_string_expression* ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)) seja convertido em `System.IFormattable` ou `System.FormattableString` (que implementa `System.IFormattable`).

Quando essa conversão é aplicada, um valor de cadeia de caracteres não é composto da cadeia de caracteres interpolada. Em vez disso, uma instância de `System.FormattableString` é criada, conforme descrito em [cadeias de caracteres interpoladas](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Conversões anuláveis implícitas

Conversões implícitas predefinidas que operam em tipos de valores não anuláveis também podem ser usadas com formulários anuláveis desses tipos. Para cada uma das conversões implícitas e numéricas predefinidas que são convertidas de um tipo de valor não anulável `S` a um tipo de valor não anulável `T`, existem as seguintes conversões anuláveis implícitas:

*  Uma conversão implícita de `S?` para `T?`.
*  Uma conversão implícita de `S` para `T?`.

A avaliação de uma conversão anulável implícita com base em uma conversão subjacente de `S` para `T` prossegue da seguinte maneira:

*  Se a conversão anulável for de `S?` para `T?`:
    * Se o valor de origem for nulo (`HasValue` propriedade for false), o resultado será o valor nulo do tipo `T?`.
    * Caso contrário, a conversão é avaliada como um desencapsulamento de `S?` para `S`, seguido pela conversão subjacente de `S` para `T`, seguida por um encapsulamento ([tipos anuláveis](types.md#nullable-types)) de `T` para `T?`.

*  Se a conversão anulável for de `S` para `T?`, a conversão será avaliada como a conversão subjacente de `S` para `T` seguido por um encapsulamento de `T` para `T?`.

### <a name="null-literal-conversions"></a>Conversões literais nulas

Existe uma conversão implícita do literal `null` para qualquer tipo anulável. Essa conversão produz o valor nulo ([tipos anuláveis](types.md#nullable-types)) do tipo anulável especificado.

### <a name="implicit-reference-conversions"></a>Conversões de referência implícitas

As conversões de referência implícitas são:

*  De qualquer *reference_type* para `object` e `dynamic`.
*  De qualquer *class_type* `S` a qualquer `T`de *class_type* , desde que `S` seja derivado de `T`.
*  De qualquer *class_type* `S` a qualquer `T`de *interface_type* , fornecida `S` implementa `T`.
*  De qualquer *interface_type* `S` a qualquer `T`de *interface_type* , desde que `S` seja derivado de `T`.
*  De um *array_type* `S` com um tipo de elemento `SE` a um *array_type* `T` com um tipo de elemento `TE`, desde que todas as seguintes opções sejam verdadeiras:
    * `S` e `T` diferem apenas no tipo de elemento. Em outras palavras, `S` e `T` têm o mesmo número de dimensões.
    * Os `SE` e `TE` são *reference_type*s.
    * Existe uma conversão de referência implícita de `SE` para `TE`.
*  De qualquer *array_type* para `System.Array` e as interfaces que ele implementa.
*  De um tipo de matriz unidimensional `S[]` para `System.Collections.Generic.IList<T>` e suas interfaces base, desde que haja uma conversão implícita de identidade ou referência de `S` para `T`.
*  De qualquer *delegate_type* para `System.Delegate` e as interfaces que ele implementa.
*  Do literal nulo para qualquer *reference_type*.
*  De qualquer *reference_type* a um *reference_type* `T` se ele tiver uma conversão implícita de identidade ou referência em um `T0` de *reference_type* e `T0` tiver uma conversão de identidade para `T`.
*  De qualquer *reference_type* a uma interface ou tipo delegado `T` se ele tiver uma conversão implícita de identidade ou referência para uma interface ou tipo delegado `T0` e `T0` for de variação conversível ([conversão de variância](interfaces.md#variance-conversion)) em `T`.
*  Conversões implícitas envolvendo parâmetros de tipo que são conhecidos como tipos de referência. Confira [conversões implícitas envolvendo parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) para obter mais detalhes sobre conversões implícitas envolvendo parâmetros de tipo.

As conversões de referência implícitas são aquelas que são conversões entre *reference_type*s que podem ser comprovadas sempre com sucesso e, portanto, não exigem verificações em tempo de execução.

As conversões de referência, implícita ou explícita, nunca alteram a identidade referencial do objeto que está sendo convertido. Em outras palavras, embora uma conversão de referência possa alterar o tipo da referência, ela nunca altera o tipo ou o valor do objeto que está sendo referenciado.

### <a name="boxing-conversions"></a>Conversões de Boxing

Uma conversão boxing permite que um *value_type* seja convertido implicitamente em um tipo de referência. Existe uma conversão boxing de qualquer *non_nullable_value_type* para `object` e `dynamic`, para `System.ValueType` e para qualquer *interface_type* implementada pelo *non_nullable_value_type*. Além disso, um *enum_type* pode ser convertido para o tipo `System.Enum`.

Uma conversão boxing existe de um *nullable_type* para um tipo de referência, se e somente se uma conversão boxing existir do *non_nullable_value_type* subjacente para o tipo de referência.

Um tipo de valor tem uma conversão boxing em um tipo de interface `I` se ele tiver uma conversão boxing em um tipo de interface `I0` e `I0` tiver uma conversão de identidade para `I`.

Um tipo de valor tem uma conversão boxing em um tipo de interface `I` se ele tiver uma conversão boxing para uma interface ou tipo delegado `I0` e `I0` for Variance-conversíveis ([conversão de variância](interfaces.md#variance-conversion)) para `I`.

Boxing um valor de uma *non_nullable_value_type* consiste em alocar uma instância de objeto e copiar o valor de *value_type* nessa instância. Um struct pode ser encaixado no tipo `System.ValueType`, já que essa é uma classe base para todas as structs ([herança](structs.md#inheritance)).

Boxing um valor de uma *nullable_type* procede da seguinte maneira:

*  Se o valor de origem for nulo (`HasValue` propriedade for false), o resultado será uma referência nula do tipo de destino.
*  Caso contrário, o resultado é uma referência a um `T` in a box produzido por desencapsulamento e conversão de código-fonte no valor de origem.

As conversões Boxing são descritas em mais detalhes nas [conversões Boxing](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Conversões dinâmicas implícitas

Existe uma conversão dinâmica implícita de uma expressão do tipo `dynamic` para qualquer tipo `T`. A conversão é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)), o que significa que uma conversão implícita será procurada em tempo de execução do tipo de tempo de execução da expressão para `T`. Se nenhuma conversão for encontrada, uma exceção de tempo de execução será lançada.

Observe que essa conversão implícita aparentemente viola o Conselho no início de [conversões implícitas](conversions.md#implicit-conversions) que uma conversão implícita nunca deve causar uma exceção. No entanto, ela não é a conversão em si, mas a *localização* da conversão que causa a exceção. O risco de exceções em tempo de execução é inerente ao uso de associação dinâmica. Se a vinculação dinâmica da conversão não for desejada, a expressão poderá ser convertida primeiro em `object`e, em seguida, para o tipo desejado.

O exemplo a seguir ilustra as conversões dinâmicas implícitas:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

As atribuições para `s2` e `i` empregam conversões dinâmicas implícitas, em que a Associação das operações é suspensa até o tempo de execução. Em tempo de execução, as conversões implícitas são buscadas do tipo de tempo de execução de `d` -- `string`--para o tipo de destino. Uma conversão é encontrada para `string` mas não para `int`.

### <a name="implicit-constant-expression-conversions"></a>Conversões de expressão de constante implícita

Uma conversão de expressão constante implícita permite as seguintes conversões:

*  Uma *constant_expression* ([expressões constantes](expressions.md#constant-expressions)) do tipo `int` pode ser convertida para o tipo `sbyte`, `byte`, `short`, `ushort`, `uint`ou `ulong`, desde que o valor da *constant_expression* esteja dentro do intervalo do tipo de destino.
*  Uma *constant_expression* do tipo `long` pode ser convertida para o tipo `ulong`, desde que o valor da *constant_expression* não seja negativo.

### <a name="implicit-conversions-involving-type-parameters"></a>Conversões implícitas envolvendo parâmetros de tipo

Existem as seguintes conversões implícitas para um determinado parâmetro de tipo `T`:

*  De `T` para sua classe base efetiva `C`, de `T` para qualquer classe base de `C`e de `T` para qualquer interface implementada pelo `C`. Em tempo de execução, se `T` for um tipo de valor, a conversão será executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.
*  De `T` para um tipo de interface `I` no conjunto de interfaces efetivas do `T`e de `T` para qualquer interface base do `I`. Em tempo de execução, se `T` for um tipo de valor, a conversão será executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.
*  De `T` para um parâmetro de tipo `U`, desde que `T` dependa do `U` ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Em tempo de execução, se `U` for um tipo de valor, `T` e `U` serão necessariamente o mesmo tipo e nenhuma conversão será executada. Caso contrário, se `T` for um tipo de valor, a conversão será executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.
*  Do literal nulo para `T`, desde que `T` seja conhecido como um tipo de referência.
*  De `T` para um tipo de referência `I` se ele tiver uma conversão implícita para um tipo de referência `S0` e `S0` tiver uma conversão de identidade para `S`. Em tempo de execução, a conversão é executada da mesma maneira que a conversão para `S0`.
*  De `T` para um tipo de interface `I` se ela tiver uma conversão implícita em uma interface ou tipo delegado `I0` e `I0` for de variação conversível em `I` ([conversão de variância](interfaces.md#variance-conversion)). Em tempo de execução, se `T` for um tipo de valor, a conversão será executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.

Se `T` for conhecido como um tipo de referência ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), as conversões acima serão todas classificadas como conversões de referência implícita ([conversões de referência implícitas](conversions.md#implicit-reference-conversions)). Se `T` não for conhecido como um tipo de referência, as conversões acima serão classificadas como conversões Boxing ([conversões Boxing](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Conversões implícitas definidas pelo usuário

Uma conversão implícita definida pelo usuário consiste em uma conversão implícita padrão opcional, seguida pela execução de um operador de conversão implícita definido pelo usuário, seguido por outra conversão implícita padrão opcional. As regras exatas para avaliar conversões implícitas definidas pelo usuário são descritas em [processamento de conversões implícitas definidas pelo usuário](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Conversões de função anônima e conversões de grupo de métodos

Funções anônimas e grupos de métodos não têm tipos próprios, mas podem ser convertidos implicitamente em tipos delegados ou tipos de árvore de expressão. As conversões de função anônimas são descritas mais detalhadamente em conversões de [função anônima](conversions.md#anonymous-function-conversions) e conversões de grupo de métodos em [conversões de grupos de métodos](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Conversões explícitas

As conversões a seguir são classificadas como conversões explícitas:

*  Todas as conversões implícitas.
*  Conversões numéricas explícitas.
*  Conversões de enumeração explícitas.
*  Conversões anuláveis explícitas.
*  Conversões de referência explícita.
*  Conversões de interface explícitas.
*  Conversões de unboxing.
*  Conversões dinâmicas explícitas
*  Conversões explícitas definidas pelo usuário.

Conversões explícitas podem ocorrer em expressões de conversão ([expressões de conversão](expressions.md#cast-expressions)).

O conjunto de conversões explícitas inclui todas as conversões implícitas. Isso significa que as expressões de conversão redundantes são permitidas.

As conversões explícitas que não são conversões implícitas são conversões que não podem ser comprovadas sempre com sucesso, conversões que são conhecidas por possivelmente perderem informações e conversões em domínios de tipos suficientemente diferentes para mérito explícito anotações.

### <a name="explicit-numeric-conversions"></a>Conversões numéricas explícitas

As conversões numéricas explícitas são as conversões de um *numeric_type* para outro *numeric_type* para o qual uma conversão numérica implícita ([conversões numéricas implícitas](conversions.md#implicit-numeric-conversions)) ainda não existe:

*  De `sbyte` para `byte`, `ushort`, `uint`, `ulong`ou `char`.
*  De `byte` para `sbyte` e `char`.
*  De `short` para `sbyte`, `byte`, `ushort`, `uint`, `ulong`ou `char`.
*  De `ushort` para `sbyte`, `byte`, `short`ou `char`.
*  De `int` para `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`ou `char`.
*  De `uint` para `sbyte`, `byte`, `short`, `ushort`, `int`ou `char`.
*  De `long` para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`ou `char`.
*  De `ulong` para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`ou `char`.
*  De `char` para `sbyte`, `byte`ou `short`.
*  De `float` para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`ou `decimal`.
*  De `double` para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`ou `decimal`.
*  De `decimal` para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`ou `double`.

Como as conversões explícitas incluem todas as conversões numéricas implícitas e explícitas, sempre é possível converter de qualquer *numeric_type* em qualquer outro *numeric_type* usando uma expressão de conversão ([expressões de conversão](expressions.md#cast-expressions)).

As conversões numéricas explícitas possivelmente perdem informações ou possivelmente causam a geração de exceções. Uma conversão numérica explícita é processada da seguinte maneira:

*  Para uma conversão de um tipo integral para outro tipo integral, o processamento depende do contexto de verificação de estouro ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)) nos quais a conversão ocorre:
    * Em um contexto de `checked`, a conversão terá sucesso se o valor do operando de origem estiver dentro do intervalo do tipo de destino, mas lançará uma `System.OverflowException` se o valor do operando de origem estiver fora do intervalo do tipo de destino.
    * Em um contexto de `unchecked`, a conversão sempre é bem sucedido e prossegue da seguinte maneira.
        * Se o tipo de origem for maior do que o tipo de destino, então o valor de origem será truncado descartando seus bits "extra" mais significativos. O resultado é então tratado como um valor do tipo de destino.
        * Se o tipo de origem for menor do que o tipo de destino, então o valor de origem será estendido por sinal ou por zero para que tenha o mesmo tamanho que o tipo de destino. A extensão por sinal será usada se o tipo de origem tiver sinal; a extensão por zero será usada se o tipo de origem não tiver sinal. O resultado é então tratado como um valor do tipo de destino.
        * Se o tipo de origem tiver o mesmo tamanho que o tipo de destino, então o valor de origem será tratado como um valor do tipo de destino.
*  Para uma conversão de `decimal` para um tipo integral, o valor de origem é arredondado para zero para o valor integral mais próximo, e esse valor integral se torna o resultado da conversão. Se o valor integral resultante estiver fora do intervalo do tipo de destino, um `System.OverflowException` será gerado.
*  Para uma conversão de `float` ou `double` a um tipo integral, o processamento depende do contexto de verificação de estouro ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)) nos quais a conversão ocorre:
    * Em um contexto de `checked`, a conversão continua da seguinte maneira:
        * Se o valor do operando for NaN ou infinito, um `System.OverflowException` será lançado.
        * Caso contrário, o operando de origem será arredondado em direção a zero para o valor integral mais próximo. Se esse valor integral estiver dentro do intervalo do tipo de destino, esse valor será o resultado da conversão.
        * Caso contrário, uma `System.OverflowException` será gerada.
    * Em um contexto de `unchecked`, a conversão sempre é bem sucedido e prossegue da seguinte maneira.
        * Se o valor do operando for NaN ou infinito, o resultado da conversão será um valor não especificado do tipo de destino.
        * Caso contrário, o operando de origem será arredondado em direção a zero para o valor integral mais próximo. Se esse valor integral estiver dentro do intervalo do tipo de destino, esse valor será o resultado da conversão.
        * Caso contrário, o resultado da conversão é um valor não especificado do tipo de destino.
*  Para uma conversão de `double` para `float`, o valor `double` é arredondado para o valor de `float` mais próximo. Se o valor de `double` for muito pequeno para representar como um `float`, o resultado se tornará positivo zero ou negativo zero. Se o valor de `double` for muito grande para representar como um `float`, o resultado se tornará infinito positivo ou infinito negativo. Se o valor de `double` for NaN, o resultado também será NaN.
*  Para uma conversão de `float` ou `double` para `decimal`, o valor de origem é convertido em `decimal` representação e arredondado para o número mais próximo após a casa decimal 28, se necessário ([o tipo decimal](types.md#the-decimal-type)). Se o valor de origem for muito pequeno para representar como um `decimal`, o resultado se tornará zero. Se o valor de origem for NaN, Infinity ou muito grande para representar como um `decimal`, um `System.OverflowException` será gerado.
*  Para uma conversão de `decimal` para `float` ou `double`, o valor `decimal` é arredondado para o valor `double` ou `float` mais próximo. Embora essa conversão possa perder a precisão, ela nunca faz com que uma exceção seja gerada.

### <a name="explicit-enumeration-conversions"></a>Conversões de enumeração explícitas

As conversões de enumeração explícitas são:

*  De `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`ou `decimal` a qualquer *enum_type*.
*  De qualquer *enum_type* para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`ou `decimal`.
*  De qualquer *enum_type* a qualquer outro *enum_type*.

Uma conversão de enumeração explícita entre dois tipos é processada tratando qualquer *enum_type* participante como o tipo subjacente do *enum_type*e, em seguida, executando uma conversão numérica implícita ou explícita entre os tipos resultantes. Por exemplo, dado um *enum_type* `E` com e o tipo subjacente de `int`, uma conversão de `E` para `byte` é processada como uma conversão numérica explícita ([conversões numéricas explícitas](conversions.md#explicit-numeric-conversions)) de `int` para `byte`e uma conversão de `byte` para `E` é processada como uma conversão numérica implícita ([conversões numéricas implícitas](conversions.md#implicit-numeric-conversions)) de `byte` para `int`.

### <a name="explicit-nullable-conversions"></a>Conversões anuláveis explícitas

As ***conversões anuláveis explícitas*** permitem conversões explícitas predefinidas que operam em tipos de valores não anuláveis para também serem usadas com formulários anuláveis desses tipos. Para cada uma das conversões explícitas predefinidas que são convertidas de um tipo de valor não anulável `S` a um tipo de valor não anulável `T` ([conversão de identidade](conversions.md#identity-conversion), [conversões numéricas implícitas](conversions.md#implicit-numeric-conversions), [conversões implícitas de enumeração](conversions.md#implicit-enumeration-conversions), [conversões numéricas explícitas](conversions.md#explicit-numeric-conversions)e [conversões de enumeração explícitas](conversions.md#explicit-enumeration-conversions)), existem as seguintes conversões anuláveis:

*  Uma conversão explícita de `S?` para `T?`.
*  Uma conversão explícita de `S` para `T?`.
*  Uma conversão explícita de `S?` para `T`.

A avaliação de uma conversão anulável com base em uma conversão subjacente de `S` para `T` prossegue da seguinte maneira:

*  Se a conversão anulável for de `S?` para `T?`:
    * Se o valor de origem for nulo (`HasValue` propriedade for false), o resultado será o valor nulo do tipo `T?`.
    * Caso contrário, a conversão é avaliada como um desencapsulamento de `S?` para `S`, seguido pela conversão subjacente de `S` para `T`, seguida por um encapsulamento de `T` para `T?`.
*  Se a conversão anulável for de `S` para `T?`, a conversão será avaliada como a conversão subjacente de `S` para `T` seguido por um encapsulamento de `T` para `T?`.
*  Se a conversão anulável for de `S?` para `T`, a conversão será avaliada como um desencapsulamento de `S?` para `S` seguido pela conversão subjacente de `S` para `T`.

Observe que uma tentativa de desencapsular um valor anulável gerará uma exceção se o valor for `null`.

### <a name="explicit-reference-conversions"></a>Conversões de referência explícitas

As conversões de referência explícitas são:

*  De `object` e `dynamic` a qualquer outro *reference_type*.
*  De qualquer *class_type* `S` a qualquer `T`de *class_type* , desde `S` é uma classe base de `T`.
*  De qualquer *class_type* `S` a qualquer `T`de *interface_type* , desde que `S` não seja lacrado e fornecido `S` não implementa `T`.
*  De qualquer *interface_type* `S` a qualquer `T`de *class_type* , desde que `T` não seja lacrado ou fornecido `T` implemente `S`.
*  De qualquer *interface_type* `S` a qualquer `T`de *interface_type* , desde que `S` não seja derivado de `T`.
*  De um *array_type* `S` com um tipo de elemento `SE` a um *array_type* `T` com um tipo de elemento `TE`, desde que todas as seguintes opções sejam verdadeiras:
    * `S` e `T` diferem apenas no tipo de elemento. Em outras palavras, `S` e `T` têm o mesmo número de dimensões.
    * Os `SE` e `TE` são *reference_type*s.
    * Existe uma conversão de referência explícita de `SE` para `TE`.
*  De `System.Array` e as interfaces que ele implementa para qualquer *array_type*.
*  De um tipo de matriz unidimensional `S[]` para `System.Collections.Generic.IList<T>` e suas interfaces base, desde que haja uma conversão de referência explícita de `S` para `T`.
*  De `System.Collections.Generic.IList<S>` e suas interfaces base para um tipo de matriz unidimensional `T[]`, desde que haja uma conversão explícita ou de referência de `S` para `T`.
*  De `System.Delegate` e as interfaces que ele implementa para qualquer *delegate_type*.
*  De um tipo de referência para um tipo de referência `T` se ele tiver uma conversão de referência explícita para um tipo de referência `T0` e `T0` tiver uma conversão de identidade `T`.
*  De um tipo de referência para uma interface ou tipo delegado `T` se ele tiver uma conversão de referência explícita para um tipo de interface ou delegado `T0` e `T0` for de conversível de variação em `T` ou `T` é convertido em variância para `T0` ([conversão de variância](interfaces.md#variance-conversion)).
*  De `D<S1...Sn>` para `D<T1...Tn>` em que `D<X1...Xn>` é um tipo de delegado genérico, `D<S1...Sn>` não é compatível com ou é idêntico a `D<T1...Tn>`e para cada parâmetro de tipo `Xi` de `D` as seguintes isenções:
    * Se `Xi` for invariável, `Si` será idêntica a `Ti`.
    * Se `Xi` for covariant, haverá uma identidade implícita ou explícita ou conversão de referência de `Si` para `Ti`.
    * Se `Xi` for contravariant, `Si` e `Ti` serão idênticos ou ambos os tipos de referência.
*  Conversões explícitas envolvendo parâmetros de tipo que são conhecidos como tipos de referência. Para obter mais detalhes sobre conversões explícitas envolvendo parâmetros de tipo, consulte [conversões explícitas envolvendo parâmetros de tipo](conversions.md#explicit-conversions-involving-type-parameters).

As conversões de referência explícitas são as conversões entre os tipos de referência que exigem verificações de tempo de execução para garantir que estejam corretos.

Para que uma conversão de referência explícita seja realizada com sucesso em tempo de execução, o valor do operando de origem deve ser `null`ou o tipo real do objeto referenciado pelo operando de origem deve ser um tipo que possa ser convertido para o tipo de destino por uma conversão de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) ou conversão de Boxing ([conversões Boxing](conversions.md#boxing-conversions)). Se uma conversão de referência explícita falhar, um `System.InvalidCastException` será gerado.

As conversões de referência, implícita ou explícita, nunca alteram a identidade referencial do objeto que está sendo convertido. Em outras palavras, embora uma conversão de referência possa alterar o tipo da referência, ela nunca altera o tipo ou o valor do objeto que está sendo referenciado.

### <a name="unboxing-conversions"></a>Conversões de unboxing

Uma conversão sem Boxing permite que um tipo de referência seja explicitamente convertido em um *value_type*. Existe uma conversão não Boxing dos tipos `object`, `dynamic` e `System.ValueType` a qualquer *non_nullable_value_type*e de qualquer *interface_type* para qualquer *non_nullable_value_type* que implemente o *interface_type*. Além disso, o tipo `System.Enum` pode ser desemoldurado para qualquer *enum_type*.

Uma conversão unboxing existe de um tipo de referência para um *nullable_type* se existir uma conversão unboxing do tipo de referência para o *non_nullable_value_type* subjacente do *nullable_type*.

Um tipo de valor `S` tem uma conversão unboxing de um tipo de interface `I` se ele tiver uma conversão unboxing de um tipo de interface `I0` e `I0` tiver uma conversão de identidade para `I`.

Um tipo de valor `S` tem uma conversão unboxing de um tipo de interface `I` se ele tiver uma conversão de unboxing de um tipo de interface ou delegado `I0` e `I0` for de conversível para `I` ou `I` for de variação conversível para `I0` ([conversão de variância](interfaces.md#variance-conversion)).

Uma operação de unboxing consiste na primeira verificação de que a instância do objeto é um valor em caixa do determinado *value_type*e, em seguida, copiar o valor para fora da instância. Unboxing uma referência nula a um *nullable_type* produz o valor nulo do *nullable_type*. Um struct pode ser desemoldurado do tipo `System.ValueType`, pois essa é uma classe base para todas as structs ([herança](structs.md#inheritance)).

As conversões de unboxing são descritas em detalhes em [conversões unboxing](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Conversões dinâmicas explícitas

Existe uma conversão dinâmica explícita de uma expressão do tipo `dynamic` para qualquer tipo `T`. A conversão é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)), o que significa que uma conversão explícita será procurada em tempo de execução do tipo de tempo de execução da expressão para `T`. Se nenhuma conversão for encontrada, uma exceção de tempo de execução será lançada.

Se a vinculação dinâmica da conversão não for desejada, a expressão poderá ser convertida primeiro em `object`e, em seguida, para o tipo desejado.

Suponha que a seguinte classe seja definida:
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

O exemplo a seguir ilustra as conversões dinâmicas explícitas:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

A melhor conversão de `o` para `C` é encontrada em tempo de compilação para ser uma conversão de referência explícita. Isso falha em tempo de execução, porque `"1"` não é, na verdade, uma `C`. A conversão de `d` para `C` no entanto, como uma conversão dinâmica explícita, é suspensa para tempo de execução, em que uma conversão definida pelo usuário do tipo de tempo de execução de `d` -- `string`--para `C` é encontrada e é bem sucedido.

### <a name="explicit-conversions-involving-type-parameters"></a>Conversões explícitas envolvendo parâmetros de tipo

Existem as conversões explícitas a seguir para um determinado parâmetro de tipo `T`:

*  Da classe base em vigor `C` de `T` para `T` e de qualquer classe base de `C` para `T`. Em tempo de execução, se `T` for um tipo de valor, a conversão será executada como uma conversão sem boxing. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.
*  De qualquer tipo de interface para `T`. Em tempo de execução, se `T` for um tipo de valor, a conversão será executada como uma conversão sem boxing. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.
*  De `T` para qualquer *interface_type* `I` fornecida, ainda não há uma conversão implícita de `T` para `I`. Em tempo de execução, se `T` for um tipo de valor, a conversão será executada como uma conversão boxing seguida de uma conversão de referência explícita. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.
*  De um parâmetro de tipo `U` para `T`, fornecido `T` depende de `U` ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Em tempo de execução, se `U` for um tipo de valor, `T` e `U` serão necessariamente o mesmo tipo e nenhuma conversão será executada. Caso contrário, se `T` for um tipo de valor, a conversão será executada como uma conversão não boxing. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.

Se `T` for conhecido como um tipo de referência, as conversões acima serão todas classificadas como conversões de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)). Se `T` não for conhecido como um tipo de referência, as conversões acima serão classificadas como conversões não Boxing ([conversões unboxing](conversions.md#unboxing-conversions)).

As regras acima não permitem uma conversão explícita direta de um parâmetro de tipo irrestrito para um tipo que não seja de interface, o que pode ser surpreendente. A razão para essa regra é evitar confusão e tornar a semântica dessas conversões clara. Por exemplo, considere a seguinte declaração:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Se a conversão explícita direta de `t` para `int` fosse permitida, alguém poderia esperar que `X<int>.F(7)` retornasse `7L`. No entanto, isso não ocorre porque as conversões numéricas padrão só são consideradas quando os tipos são conhecidos como numéricos no tempo de associação. Para tornar a semântica clara, o exemplo acima deve ser escrito em vez disso:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Esse código agora será compilado, mas a execução de `X<int>.F(7)` geraria uma exceção em tempo de execução, já que uma `int` em caixa não pode ser convertida diretamente em uma `long`.

### <a name="user-defined-explicit-conversions"></a>Conversões explícitas definidas pelo usuário

Uma conversão explícita definida pelo usuário consiste em uma conversão explícita padrão opcional, seguida pela execução de um operador de conversão implícita ou explícita definido pelo usuário, seguido por outra conversão opcional explícita padrão. As regras exatas para avaliar conversões explícitas definidas pelo usuário são descritas em [processamento de conversões explícitas definidas pelo usuário](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Conversões padrão

As conversões padrão são aquelas conversões predefinidas que podem ocorrer como parte de uma conversão definida pelo usuário.

### <a name="standard-implicit-conversions"></a>Conversões implícitas padrão

As conversões implícitas a seguir são classificadas como conversões implícitas padrão:

*  Conversões de identidade ([conversão de identidade](conversions.md#identity-conversion))
*  Conversões numéricas implícitas ([conversões numéricas implícitas](conversions.md#implicit-numeric-conversions))
*  Conversões anuláveis implícitas ([conversões anuláveis implícitas](conversions.md#implicit-nullable-conversions))
*  Conversões implícitas de referência ([conversões de referência implícitas](conversions.md#implicit-reference-conversions))
*  Conversões Boxing ([conversões Boxing](conversions.md#boxing-conversions))
*  Conversões de expressão de constante implícita ([conversões dinâmicas implícitas](conversions.md#implicit-dynamic-conversions))
*  Conversões implícitas envolvendo parâmetros de tipo ([conversões implícitas envolvendo parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters))

As conversões implícitas padrão excluem especificamente conversões implícitas definidas pelo usuário.

### <a name="standard-explicit-conversions"></a>Conversões explícitas padrão

As conversões explícitas padrão são todas as conversões implícitas padrão mais o subconjunto das conversões explícitas para as quais existe uma conversão implícita padrão oposta. Em outras palavras, se existir uma conversão implícita padrão de um tipo `A` a um tipo `B`, uma conversão explícita padrão existirá do tipo `A` para o tipo `B` e do tipo `B` para o tipo `A`.

## <a name="user-defined-conversions"></a>Conversões definidas pelo usuário

C#permite que as conversões implícitas e explícitas predefinidas sejam aumentadas por ***conversões definidas pelo usuário***. Conversões definidas pelo usuário são introduzidas pela declaração de operadores de conversão ([operadores de conversão](classes.md#conversion-operators)) em tipos de classe e struct.

### <a name="permitted-user-defined-conversions"></a>Conversões permitidas definidas pelo usuário

C#permite que apenas determinadas conversões definidas pelo usuário sejam declaradas. Em particular, não é possível redefinir uma conversão implícita ou explícita já existente.

Para um determinado tipo de fonte `S` e tipo de destino `T`, se `S` ou `T` forem tipos anuláveis, permita que `S0` e `T0` refiram-se aos seus tipos subjacentes, caso contrário, `S0` e `T0` são iguais a `S` e `T` respectivamente. Uma classe ou estrutura tem permissão para declarar uma conversão de um tipo de origem `S` para um tipo de destino `T` somente se todas as seguintes opções forem verdadeiras:

*  `S0` e `T0` são tipos diferentes.
*  `S0` ou `T0` é o tipo de classe ou struct no qual a declaração do operador ocorre.
*  Nem `S0` nem `T0` é um *interface_type*.
*  Excluindo conversões definidas pelo usuário, uma conversão não existe de `S` para `T` ou de `T` para `S`.

As restrições que se aplicam a conversões definidas pelo usuário são discutidas mais detalhadamente nos [operadores de conversão](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Operadores de conversão levantados

Dado um operador de conversão definido pelo usuário que converte de um tipo de valor não anulável `S` para um tipo de valor não anulável `T`, existe um ***operador de conversão levantado*** que converte de `S?` para `T?`. Esse operador de conversão levantado executa um desencapsulamento de `S?` para `S` seguido pela conversão definida pelo usuário de `S` para `T` seguido por um encapsulamento de `T` para `T?`, exceto que um valor nulo `S?` é convertido diretamente em um `T?`com valor nulo.

Um operador de conversão elevado tem a mesma classificação implícita ou explícita que seu operador de conversão subjacente definido pelo usuário. O termo "conversão definida pelo usuário" aplica-se ao uso de operadores de conversão que foram definidos pelo usuário e com elevação.

### <a name="evaluation-of-user-defined-conversions"></a>Avaliação de conversões definidas pelo usuário

Uma conversão definida pelo usuário converte um valor de seu tipo, chamado de ***tipo de origem***, para outro tipo, chamado de ***tipo de destino***. Avaliação de um centro de conversão definido pelo usuário para localizar o operador de conversão definido pelo usuário ***mais específico*** para os tipos de origem e destino específicos. Essa determinação é dividida em várias etapas:

*  Localizando o conjunto de classes e structs dos quais operadores de conversão definidos pelo usuário serão considerados. Esse conjunto consiste no tipo de origem e suas classes base, e o tipo de destino e suas classes base (com as suposições implícitas que somente classes e structs podem declarar operadores definidos pelo usuário e que não têm classes base). Para os fins desta etapa, se o tipo de origem ou de destino for um *nullable_type*, seu tipo subjacente será usado em seu lugar.
*  Desse conjunto de tipos, determinando quais operadores de conversão levantados e definidos pelo usuário são aplicáveis. Para que um operador de conversão seja aplicável, deve ser possível executar uma conversão padrão ([conversões padrão](conversions.md#standard-conversions)) do tipo de origem para o tipo de operando do operador e deve ser possível executar uma conversão padrão do tipo de resultado do operador para o tipo de destino.
*  Do conjunto de operadores aplicáveis definidos pelo usuário, determinando qual operador não é ambíguomente o mais específico. Em termos gerais, o operador mais específico é o operador cujo tipo de operando é "mais próximo" do tipo de origem e cujo tipo de resultado é "mais próximo" ao tipo de destino. Os operadores de conversão definidos pelo usuário são preferenciais em relação aos operadores de conversão levantados. As regras exatas para estabelecer o operador de conversão mais específico definido pelo usuário são definidas nas seções a seguir.

Depois que um operador de conversão definido pelo usuário mais específico tiver sido identificado, a execução real da conversão definida pelo usuário envolverá até três etapas:

*  Primeiro, se necessário, executando uma conversão padrão do tipo de origem para o tipo de operando do operador de conversão definido pelo usuário ou levantado.
*  Em seguida, invocando o operador de conversão definido pelo usuário ou elevação para executar a conversão.
*  Por fim, se necessário, executar uma conversão padrão do tipo de resultado do operador de conversão definido pelo usuário ou de forma elevada para o tipo de destino.

A avaliação de uma conversão definida pelo usuário nunca envolve mais de um operador de conversão mais definido pelo usuário ou levantado. Em outras palavras, uma conversão do tipo `S` para o tipo `T` nunca executará primeiro uma conversão definida pelo usuário de `S` em `X` e, em seguida, executará uma conversão definida pelo usuário de `X` para `T`.

As definições exatas de avaliação de conversões implícitas ou explícitas definidas pelo usuário são dadas nas seções a seguir. As definições fazem uso dos seguintes termos:

*  Se uma conversão implícita padrão ([conversões implícitas padrão](conversions.md#standard-implicit-conversions)) existir de um tipo `A` a um tipo `B`, e se nem `A` nem `B` forem *interface_type*s, a `A` será considerada como sendo ***englobeda pelo*** `B`, e `B` será considerado para ***abranger*** `A`.
*  O ***tipo mais abrangente*** em um conjunto de tipos é aquele que abrange todos os outros tipos no conjunto. Se nenhum tipo único abranger todos os outros tipos, o conjunto não terá mais de abranger o tipo. Em termos mais intuitivos, o tipo mais abrangente é o tipo "maior" no conjunto — o tipo para o qual cada um dos outros tipos pode ser convertido implicitamente.
*  O ***tipo mais abrangente*** em um conjunto de tipos é aquele que é abrangedo por todos os outros tipos no conjunto. Se nenhum tipo único for abrangedo por todos os outros tipos, o conjunto não terá o tipo mais abrangedo. Em termos mais intuitivos, o tipo mais abrangente é o tipo "menor" no conjunto — aquele tipo que pode ser convertido implicitamente em cada um dos outros tipos.

### <a name="processing-of-user-defined-implicit-conversions"></a>Processamento de conversões implícitas definidas pelo usuário

Uma conversão implícita definida pelo usuário do tipo `S` para o tipo `T` é processada da seguinte maneira:

*  Determine os tipos `S0` e `T0`. Se `S` ou `T` forem tipos anuláveis, `S0` e `T0` serão os tipos subjacentes, caso contrário `S0` e `T0` serão iguais a `S` e `T`, respectivamente.
*  Localize o conjunto de tipos, `D`, do qual os operadores de conversão definidos pelo usuário serão considerados. Esse conjunto consiste em `S0` (se `S0` for uma classe ou struct), as classes base de `S0` (se `S0` for uma classe) e `T0` (se `T0` for uma classe ou struct).
*  Localize o conjunto de operadores de conversão aplicáveis definidos pelo usuário e com elevação, `U`. Esse conjunto consiste nos operadores de conversão implícitas definidos pelo usuário e levantados declarados pelas classes ou estruturas em `D` que convertem de um tipo que abrange `S` para um tipo que abrange por `T`. Se `U` estiver vazio, a conversão será indefinida e ocorrerá um erro de tempo de compilação.
*  Localize o tipo de fonte mais específico, `SX`, dos operadores em `U`:
    * Se qualquer um dos operadores em `U` converter de `S`, `SX` será `S`.
    * Caso contrário, `SX` é o tipo mais abrangente no conjunto combinado de tipos de origem dos operadores em `U`. Se não for possível encontrar exatamente um tipo mais abrangente, a conversão será ambígua e ocorrerá um erro de tempo de compilação.
*  Localize o tipo de destino mais específico, `TX`, dos operadores em `U`:
    * Se qualquer um dos operadores em `U` converter em `T`, `TX` será `T`.
    * Caso contrário, `TX` é o tipo mais abrangente no conjunto combinado de tipos de destino dos operadores em `U`. Se não for possível encontrar exatamente um tipo mais abrangente, a conversão será ambígua e ocorrerá um erro de tempo de compilação.
*  Encontre o operador de conversão mais específico:
    * Se `U` contiver exatamente um operador de conversão definido pelo usuário que converta de `SX` para `TX`, esse será o operador de conversão mais específico.
    * Caso contrário, se `U` contiver exatamente um operador de conversão levantado que converta de `SX` para `TX`, esse será o operador de conversão mais específico.
    * Caso contrário, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Por fim, aplique a conversão:
    * Se `S` não for `SX`, uma conversão implícita padrão de `S` para `SX` será executada.
    * O operador de conversão mais específico é invocado para converter de `SX` para `TX`.
    * Se `TX` não for `T`, uma conversão implícita padrão de `TX` para `T` será executada.

### <a name="processing-of-user-defined-explicit-conversions"></a>Processamento de conversões explícitas definidas pelo usuário

Uma conversão explícita definida pelo usuário do tipo `S` para o tipo `T` é processada da seguinte maneira:

*  Determine os tipos `S0` e `T0`. Se `S` ou `T` forem tipos anuláveis, `S0` e `T0` serão os tipos subjacentes, caso contrário `S0` e `T0` serão iguais a `S` e `T`, respectivamente.
*  Localize o conjunto de tipos, `D`, do qual os operadores de conversão definidos pelo usuário serão considerados. Esse conjunto consiste em `S0` (se `S0` for uma classe ou struct), as classes base de `S0` (se `S0` for uma classe), `T0` (se `T0` for uma classe ou struct) e as classes base de `T0` (se `T0` for uma classe).
*  Localize o conjunto de operadores de conversão aplicáveis definidos pelo usuário e com elevação, `U`. Esse conjunto consiste nos operadores de conversão implícitos ou explícitos definidos pelo usuário, declarados pelas classes ou estruturas em `D` que são convertidos de um tipo que abrange ou abrangedo por `S` a um tipo que abrange ou abrangedo por `T`. Se `U` estiver vazio, a conversão será indefinida e ocorrerá um erro de tempo de compilação.
*  Localize o tipo de fonte mais específico, `SX`, dos operadores em `U`:
    * Se qualquer um dos operadores em `U` converter de `S`, `SX` será `S`.
    * Caso contrário, se qualquer um dos operadores em `U` converter de tipos que abrangem `S`, `SX` será o tipo mais abrangedo no conjunto combinado de tipos de origem desses operadores. Se nenhum tipo mais abrangedo puder ser encontrado, a conversão será ambígua e ocorrerá um erro de tempo de compilação.
    * Caso contrário, `SX` é o tipo mais abrangente no conjunto combinado de tipos de origem dos operadores em `U`. Se não for possível encontrar exatamente um tipo mais abrangente, a conversão será ambígua e ocorrerá um erro de tempo de compilação.
*  Localize o tipo de destino mais específico, `TX`, dos operadores em `U`:
    * Se qualquer um dos operadores em `U` converter em `T`, `TX` será `T`.
    * Caso contrário, se qualquer um dos operadores em `U` for convertido em tipos que são incluídos por `T`, `TX` será o tipo mais abrangente no conjunto combinado de tipos de destino desses operadores. Se não for possível encontrar exatamente um tipo mais abrangente, a conversão será ambígua e ocorrerá um erro de tempo de compilação.
    * Caso contrário, `TX` é o tipo mais abrangente no conjunto combinado de tipos de destino dos operadores em `U`. Se nenhum tipo mais abrangedo puder ser encontrado, a conversão será ambígua e ocorrerá um erro de tempo de compilação.
*  Encontre o operador de conversão mais específico:
    * Se `U` contiver exatamente um operador de conversão definido pelo usuário que converta de `SX` para `TX`, esse será o operador de conversão mais específico.
    * Caso contrário, se `U` contiver exatamente um operador de conversão levantado que converta de `SX` para `TX`, esse será o operador de conversão mais específico.
    * Caso contrário, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Por fim, aplique a conversão:
    * Se `S` não for `SX`, uma conversão explícita padrão de `S` para `SX` será executada.
    * O operador de conversão definido pelo usuário mais específico é invocado para converter de `SX` para `TX`.
    * Se `TX` não for `T`, uma conversão explícita padrão de `TX` para `T` será executada.

## <a name="anonymous-function-conversions"></a>Conversões de função anônimas

Uma *anonymous_method_expression* ou *lambda_expression* é classificada como uma função anônima ([expressões de função anônimas](expressions.md#anonymous-function-expressions)). A expressão não tem um tipo, mas pode ser convertida implicitamente em um tipo de delegado ou tipo de árvore de expressão compatível. Especificamente, uma função anônima `F` é compatível com um tipo delegado `D` fornecido:

*  Se `F` contiver um *anonymous_function_signature*, `D` e `F` terão o mesmo número de parâmetros.
*  Se `F` não contiver um *anonymous_function_signature*, `D` poderá ter zero ou mais parâmetros de qualquer tipo, desde que nenhum parâmetro de `D` tenha o modificador de parâmetro `out`.
*  Se `F` tiver uma lista de parâmetros tipados explicitamente, cada parâmetro em `D` terá o mesmo tipo e modificadores que o parâmetro correspondente no `F`.
*  Se `F` tiver uma lista de parâmetros Tipados implicitamente, `D` não terá parâmetros `ref` ou `out`.
*  Se o corpo de `F` for uma expressão e `D` tiver um tipo de retorno `void` ou `F` for Async e `D` tiver o tipo de retorno `Task`, quando cada parâmetro de `F` receber o tipo do parâmetro correspondente em `D`, o corpo de `F` será uma expressão válida (WRT [Expressions](expressions.md)) que seria permitida como uma *statement_expression* ([instruções Expression](statements.md#expression-statements)).
*  Se o corpo de `F` for um bloco de instrução e `D` tiver um tipo de retorno `void` ou `F` for Async e `D` tiver o tipo de retorno `Task`, quando cada parâmetro de `F` receber o tipo do parâmetro correspondente em `D`, o corpo de `F` será um bloco de instrução válido ( [blocos](statements.md#blocks)WRT) no qual nenhuma instrução `return` especifica uma expressão.
*  Se o corpo de `F` for uma expressão *e `F` não* for async e `D` tiver um tipo de retorno não void `T`*ou* `F` for Async e `D` tiver um tipo de retorno `Task<T>`, quando cada parâmetro de `F` receber o tipo do parâmetro correspondente em `D`, o corpo de `F` será uma expressão válida (WRT [Expressions](expressions.md)) que é implicitamente conversível em `T`.
*  Se o corpo de `F` for um bloco de instrução e *`F` não for Async e `D`* tiver um tipo de retorno não nulo `T`, *ou* `F` é Async e `D` tem um tipo de retorno `Task<T>`, em seguida, quando cada parâmetro de `F` recebe o tipo do parâmetro correspondente em `D`, o corpo de `F` é um bloco de instrução válido ( [blocos](statements.md#blocks)WRT) com um ponto de extremidade não acessível no qual cada instrução `return` especifica uma expressão que é implicitamente conversível para `T`.

Para fins de brevidade, esta seção usa a forma abreviada para os tipos de tarefa `Task` e `Task<T>` ([funções assíncronas](classes.md#async-functions)).

Uma expressão lambda `F` é compatível com um tipo de árvore de expressão `Expression<D>` se `F` for compatível com o tipo de delegado `D`. Observe que isso não se aplica a métodos anônimos, somente expressões lambda.

Determinadas expressões lambda não podem ser convertidas em tipos de árvore de expressões: mesmo que a conversão *exista*, ela falhará em tempo de compilação. Esse é o caso se a expressão lambda:

*  Tem um corpo de *bloco*
*  Contém operadores de atribuição simples ou compostos
*  Contém uma expressão vinculada dinamicamente
*  É Async

Os exemplos a seguir usam um tipo de delegado genérico `Func<A,R>` que representa uma função que usa um argumento do tipo `A` e retorna um valor do tipo `R`:
```csharp
delegate R Func<A,R>(A arg);
```

Nas atribuições
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
os tipos de parâmetro e de retorno de cada função anônima são determinados do tipo da variável à qual a função anônima é atribuída.

A primeira atribuição converte com êxito a função anônima para o tipo delegado `Func<int,int>` porque, quando `x` recebe o tipo `int`, `x+1` é uma expressão válida que é implicitamente conversível no tipo `int`.

Da mesma forma, a segunda atribuição converte com êxito a função anônima no tipo delegado `Func<int,double>` porque o resultado da `x+1` (do tipo `int`) é implicitamente conversível no tipo `double`.

No entanto, a terceira atribuição é um erro de tempo de compilação porque, quando `x` é fornecido `double`de tipo, o resultado de `x+1` (do tipo `double`) não é implicitamente conversível para o tipo `int`.

A quarta atribuição converte com êxito a função Async anônima no tipo delegado `Func<int, Task<int>>` porque o resultado da `x+1` (do tipo `int`) é implicitamente conversível para o tipo de resultado `int` do tipo de tarefa `Task<int>`.

As funções anônimas podem influenciar a resolução de sobrecarga e participar da inferência de tipos. Consulte [membros da função](expressions.md#function-members) para obter mais detalhes.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Avaliação de conversões de função anônima para tipos delegados

A conversão de uma função anônima em um tipo delegate produz uma instância delegada que faz referência à função anônima e ao conjunto (possivelmente vazio) de variáveis externas capturadas que estão ativas no momento da avaliação. Quando o delegado é invocado, o corpo da função anônima é executado. O código no corpo é executado usando o conjunto de variáveis externas capturadas referenciadas pelo delegado.

A lista de invocação de um delegado produzido por uma função anônima contém uma única entrada. O objeto de destino exato e o método de destino do delegado não são especificados. Em particular, ele não é especificado se o objeto de destino do delegado é `null`, o valor `this` do membro da função delimitadora ou algum outro objeto.

Conversões de funções anônimas, semanticamente idênticas com o mesmo conjunto (possivelmente vazio) de instâncias de variáveis externas capturadas para os mesmos tipos delegados, são permitidas (mas não obrigatórias) para retornar a mesma instância de delegado. O termo semanticamente idêntico é usado aqui para significar que a execução das funções anônimas irá, em todos os casos, produzir os mesmos efeitos, considerando os mesmos argumentos. Essa regra permite que um código como o seguinte seja otimizado.

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

Como os dois delegados de função anônimas têm o mesmo conjunto (vazio) de variáveis externas capturadas e, como as funções anônimas são semanticamente idênticas, o compilador tem permissão para fazer com que os delegados se refiram ao mesmo método de destino. Na verdade, o compilador tem permissão para retornar a mesma instância de delegado de ambas as expressões de função anônimas.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Avaliação de conversões de função anônima para tipos de árvore de expressão

A conversão de uma função anônima em um tipo de árvore de expressão produz uma árvore de expressão ([tipos de árvore de expressão](types.md#expression-tree-types)). Mais precisamente, a avaliação da conversão da função anônima leva à construção de uma estrutura de objeto que representa a estrutura da própria função anônima. A estrutura precisa da árvore de expressão, bem como o processo exato para criá-la, são definidas para implementação.

### <a name="implementation-example"></a>Exemplo de implementação

Esta seção descreve uma possível implementação de conversões de função anônima em termos de outras C# construções. A implementação descrita aqui se baseia nos mesmos princípios usados pelo compilador da Microsoft C# , mas não é por isso uma implementação obrigatória, nem é a única possível. Ele apenas menciona conversões em árvores de expressão, pois a semântica exata está fora do escopo desta especificação.

O restante desta seção fornece vários exemplos de código que contém funções anônimas com características diferentes. Para cada exemplo, é fornecida uma tradução correspondente ao código que usa C# apenas outras construções. Nos exemplos, o identificador `D` é assumido por representar o seguinte tipo de delegado:
```csharp
public delegate void D();
```

A forma mais simples de uma função anônima é aquela que captura nenhuma variável externa:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Isso pode ser convertido em uma instanciação de delegado que faz referência a um método estático gerado pelo compilador no qual o código da função anônima é colocado:
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

No exemplo a seguir, a função anônima faz referência aos membros da instância de `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Isso pode ser convertido em um método de instância gerado pelo compilador que contém o código da função anônima:
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

Neste exemplo, a função anônima captura uma variável local:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

O tempo de vida da variável local agora deve ser estendido para pelo menos o tempo de vida do delegado da função anônima. Isso pode ser obtido por meio da "guindaste" da variável local em um campo de uma classe gerada pelo compilador. A instanciação da variável local ([instanciação de variáveis locais](expressions.md#instantiation-of-local-variables)) corresponde à criação de uma instância da classe gerada pelo compilador e o acesso à variável local corresponde ao acesso a um campo na instância da classe gerada pelo compilador. Além disso, a função anônima torna-se um método de instância da classe gerada pelo compilador:
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

Por fim, a função anônima a seguir captura `this`, bem como duas variáveis locais com tempos de vida diferentes:
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

Aqui, uma classe gerada pelo compilador é criada para cada bloco de instrução no qual os locais são capturados, de modo que os locais nos diferentes blocos possam ter tempos de vida independentes. Uma instância de `__Locals2`, a classe gerada pelo compilador para o bloco de instrução interna, contém a variável local `z` e um campo que faz referência a uma instância de `__Locals1`.  Uma instância de `__Locals1`, a classe gerada pelo compilador para o bloco de instrução externa, contém a variável local `y` e um campo que faz referência `this` do membro da função delimitadora. Com essas estruturas de dados, é possível alcançar todas as variáveis externas capturadas por meio de uma instância do `__Local2`, e o código da função anônima pode, portanto, ser implementado como um método de instância dessa classe.

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

A mesma técnica aplicada aqui para capturar variáveis locais também pode ser usada ao converter funções anônimas em árvores de expressão: referências aos objetos gerados pelo compilador podem ser armazenadas na árvore de expressão e o acesso às variáveis locais pode ser representado como acessos de campo nesses objetos. A vantagem dessa abordagem é que ela permite que as variáveis locais "levantadas" sejam compartilhadas entre delegados e árvores de expressão.

## <a name="method-group-conversions"></a>Conversões de grupo de métodos

Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe de um grupo de métodos ([classificações de expressão](expressions.md#expression-classifications)) para um tipo de delegado compatível. Dado um tipo delegado `D` e uma expressão `E` classificada como um grupo de métodos, existe uma conversão implícita de `E` para `D` se `E` contiver pelo menos um método que seja aplicável em seu formato normal ([membro de função aplicável](expressions.md#applicable-function-member)) a uma lista de argumentos construída pelo uso dos tipos de parâmetro e modificadores de `D`, conforme descrito a seguir.

O aplicativo de tempo de compilação de uma conversão de um grupo de métodos `E` a um tipo de delegado `D` é descrito a seguir. Observe que a existência de uma conversão implícita de `E` para `D` não garante que o aplicativo de tempo de compilação da conversão terá sucesso sem erros.

*  Um único método `M` é selecionado correspondente a uma invocação de método ([invocações de método](expressions.md#method-invocations)) do formulário `E(A)`, com as seguintes modificações:
    * A lista de argumentos `A` é uma lista de expressões, cada uma classificada como uma variável e com o tipo e o modificador (`ref` ou `out`) do parâmetro correspondente no *formal_parameter_list* de `D`.
    * Os métodos candidatos considerados são apenas os métodos que são aplicáveis em seu formato normal ([membro de função aplicável](expressions.md#applicable-function-member)), não aqueles aplicáveis apenas em sua forma expandida.
*  Se o algoritmo das [invocações de método](expressions.md#method-invocations) produzir um erro, ocorrerá um erro de tempo de compilação. Caso contrário, o algoritmo produz um único método melhor `M` ter o mesmo número de parâmetros que `D` e a conversão é considerada como existe.
*  O método selecionado `M` deve ser compatível ([delegar compatibilidade](delegates.md#delegate-compatibility)) com o tipo delegado `D`ou, caso contrário, ocorrerá um erro de tempo de compilação.
*  Se o método selecionado `M` for um método de instância, a expressão de instância associada a `E` determinará o objeto de destino do delegado.
*  Se o método selecionado M for um método de extensão indicado por meio de um acesso de membro em uma expressão de instância, essa expressão de instância determinará o objeto de destino do delegado.
*  O resultado da conversão é um valor do tipo `D`, ou seja, um delegado recém-criado que se refere ao método selecionado e ao objeto de destino.
*  Observe que esse processo pode levar à criação de um delegado para um método de extensão, se o algoritmo das [invocações de método](expressions.md#method-invocations) não conseguir encontrar um método de instância, mas tiver êxito no processamento da invocação de `E(A)` como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)). Um delegado criado então captura o método de extensão, bem como seu primeiro argumento.

O exemplo a seguir demonstra as conversões de grupo de métodos:
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

A atribuição para `d1` converte implicitamente o grupo de métodos `F` em um valor do tipo `D1`.

A atribuição para `d2` mostra como é possível criar um delegado para um método que tenha tipos de parâmetro menos derivados (contravariant) e um tipo de retorno mais derivado (Covariance).

A atribuição para `d3` mostra como nenhuma conversão existe se o método não for aplicável.

A atribuição para `d4` mostra como o método deve ser aplicável em seu formato normal.

A atribuição para `d5` mostra como os tipos de parâmetro e de retorno do delegado e do método têm permissão para diferir somente para tipos de referência.

Assim como acontece com todas as outras conversões implícitas e explícitas, o operador cast pode ser usado para executar explicitamente uma conversão de grupo de métodos. Portanto, o exemplo
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
em vez disso, poderia ser escrito
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Os grupos de métodos podem influenciar a resolução de sobrecarga e participar da inferência de tipos. Consulte [membros da função](expressions.md#function-members) para obter mais detalhes.

A avaliação de tempo de execução de uma conversão de grupo de métodos procede da seguinte maneira:

*  Se o método selecionado em tempo de compilação for um método de instância, ou for um método de extensão que é acessado como um método de instância, o objeto de destino do delegado é determinado da expressão de instância associada ao `E`:
    * A expressão de instância é avaliada. Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.
    * Se a expressão de instância for de um *reference_type*, o valor calculado pela expressão de instância se tornará o objeto de destino. Se o método selecionado for um método de instância e o objeto de destino for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.
    * Se a expressão de instância for de um *value_type*, uma operação Boxing ([conversões Boxing](types.md#boxing-conversions)) será executada para converter o valor em um objeto e esse objeto se tornará o objeto de destino.
*  Caso contrário, o método selecionado faz parte de uma chamada de método estático e o objeto de destino do delegado é `null`.
*  Uma nova instância do tipo delegado `D` é alocada. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.
*  A nova instância de delegado é inicializada com uma referência ao método que foi determinado em tempo de compilação e uma referência ao objeto de destino calculado acima.
