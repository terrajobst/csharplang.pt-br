# <a name="conversions"></a>Conversões

Um ***conversão*** permite que uma expressão a ser tratada como sendo de um tipo específico. Uma conversão pode causar uma expressão de um tipo específico para ser tratada como tendo um tipo diferente, ou pode fazer com que uma expressão sem um tipo para obter um tipo. As conversões podem ser ***implícita*** ou ***explícita***, e isso determina se uma conversão explícita é necessária. Por exemplo, a conversão do tipo `int` digitar `long` é implícito, portanto, expressões do tipo `int` implicitamente pode ser tratado como tipo `long`. A conversão oposta, do tipo `long` digitar `int`, é explícito e, portanto, uma conversão explícita é necessária.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Algumas conversões são definidos pela linguagem. Programas também podem definir suas próprias conversões ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Conversões implícitas

As conversões a seguir são classificadas como conversões implícitas:

*  Conversões de identidade
*  Conversões numéricas implícitas
*  Conversões implícitas de enumeração.
*  Conversões implícitas de permite valor nulas
*  Conversões de literais nulas
*  Conversões de referência implícita
*  Conversões boxing
*  Conversões implícitas de dinâmicas
*  Conversões implícitas de expressão de constante
*  Conversões implícitas definidas pelo usuário
*  Conversões de função anônima
*  Conversões de grupo de método

Conversões implícitas podem ocorrer em uma variedade de situações, incluindo as invocações de função de membro ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), expressões de conversão ([expressões de conversão](expressions.md#cast-expressions)), atribuições e ([operadores de atribuição](expressions.md#assignment-operators)).

As conversões implícitas predefinidas sempre têm êxito e nunca fazer com que exceções sejam geradas. Projetado corretamente conversões implícitas definidas pelo usuário devem apresentar as seguintes características.

Para fins de conversão, os tipos `object` e `dynamic` são considerados equivalentes.

No entanto, conversões dinâmicas ([conversões implícitas de dinâmicas](conversions.md#implicit-dynamic-conversions) e [conversões explícitas de dinâmicas](conversions.md#explicit-dynamic-conversions)) se aplicam somente a expressões do tipo `dynamic` ([tipo dinâmico](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Conversão de identidade

Converte uma conversão de identidade de qualquer tipo para o mesmo tipo. Essa conversão existe, de modo que uma entidade que já tem um tipo necessário pode ser dito para poder ser convertido para esse tipo.

*  Como o objeto e dinâmicas são considerados equivalentes não há uma conversão de identidade entre `object` e `dynamic`e entre os tipos construídos são iguais ao substituir todas as ocorrências de `dynamic` com `object`.

### <a name="implicit-numeric-conversions"></a>Conversões numéricas implícitas

As conversões numéricas implícitas são:

*  Partir `sbyte` à `short`, `int`, `long`, `float`, `double`, ou `decimal`.
*  Partir `byte` à `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, ou `decimal`.
*  Partir `short` à `int`, `long`, `float`, `double`, ou `decimal`.
*  Partir `ushort` à `int`, `uint`, `long`, `ulong`, `float`, `double`, ou `decimal`.
*  Partir `int` à `long`, `float`, `double`, ou `decimal`.
*  Partir `uint` à `long`, `ulong`, `float`, `double`, ou `decimal`.
*  Partir `long` à `float`, `double`, ou `decimal`.
*  Partir `ulong` à `float`, `double`, ou `decimal`.
*  Partir `char` à `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, ou `decimal`.
*  Partir `float` para `double`.

Conversões de `int`, `uint`, `long`, ou `ulong` para `float` e do `long` ou `ulong` para `double` pode causar perda de precisão, mas nunca será causa uma perda de magnitude. As outras conversões numéricas implícitas nunca perdem todas as informações.

Não há nenhuma conversão implícita para o `char` de tipo, portanto, os valores dos outros tipos integrais não são automaticamente convertidos a `char` tipo.

### <a name="implicit-enumeration-conversions"></a>Conversões implícitas de enumeração

Permite que uma conversão implícita de enumeração de *decimal_integer_literal* `0` a ser convertido em qualquer *enum_type* e para qualquer *nullable_type* cujo o tipo subjacente é um *enum_type*. No último caso a conversão é avaliada, convertendo para subjacente *enum_type* e o resultado de encapsulamento ([tipos anuláveis](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Conversões implícitas de cadeia de caracteres interpolada

Implícito de autorizações de conversão de cadeia de caracteres interpolada uma *interpolated_string_expression* ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)) a ser convertido em `System.IFormattable` ou `System.FormattableString` (que implementa `System.IFormattable`).

Quando essa conversão é aplicada a um valor de cadeia de caracteres não é composto da cadeia de caracteres interpolada. Em vez disso, uma instância do `System.FormattableString` é criada, conforme descrito em [cadeias de caracteres interpoladas](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Conversões implícitas de permite valor nulas

Conversões implícitas predefinidas que operam em tipos de valor não anulável também podem ser usadas com os formulários que permitem valor nulos desses tipos. Para cada identidade implícitas predefinida e conversões numéricas convertem de um tipo de valor não anulável `S` para um tipo de valor não anulável `T`, existem as seguintes conversões implícitas que permitem valor nulas:

*  Uma conversão implícita da `S?` para `T?`.
*  Uma conversão implícita da `S` para `T?`.

Avaliação de uma conversão implícita de permite valor nula com base em uma conversão subjacente de `S` para `T` procede da seguinte maneira:

*  Se a conversão que permitem valor nula é de `S?` para `T?`:
    * Se o valor de origem é nulo (`HasValue` propriedade é false), o resultado é o valor nulo do tipo `T?`.
    * Caso contrário, a conversão será avaliada como um desempacotamento do `S?` para `S`, seguido pela conversão de base `S` ao `T`, seguido por um encapsulamento ([tipos anuláveis](types.md#nullable-types)) de `T` para `T?`.

*  Se a conversão que permitem valor nula é de `S` para `T?`, a conversão é avaliada como a conversão subjacente do `S` à `T` seguido por um encapsulamento de `T` para `T?`.

### <a name="null-literal-conversions"></a>Conversões de literais nulas

Existe uma conversão implícita do `null` literal a qualquer tipo que permite valor nulo. Essa conversão produz o valor nulo ([tipos anuláveis](types.md#nullable-types)) do tipo que permite valor nulo fornecido.

### <a name="implicit-reference-conversions"></a>Conversões de referência implícita

As conversões de referência implícita são:

*  De qualquer *reference_type* à `object` e `dynamic`.
*  De qualquer *class_type* `S` a qualquer *class_type* `T`, fornecido `S` é derivado de `T`.
*  De qualquer *class_type* `S` a qualquer *interface_type* `T`, fornecido `S` implementa `T`.
*  De qualquer *interface_type* `S` a qualquer *interface_type* `T`, fornecido `S` é derivado de `T`.
*  De um *array_type* `S` com um tipo de elemento `SE` para um *array_type* `T` com um tipo de elemento `TE`, desde que todos os itens a seguir forem verdadeiras:
    * `S` e `T` diferem apenas no tipo de elemento. Em outras palavras, `S` e `T` têm o mesmo número de dimensões.
    * Ambos `SE` e `TE` são *reference_type*s.
    * Existe uma conversão de referência implícita da `SE` para `TE`.
*  De qualquer *array_type* para `System.Array` e as interfaces que ele implementa.
*  De um tipo de matriz unidimensional `S[]` à `System.Collections.Generic.IList<T>` e suas interfaces base, fornecidos o que há uma conversão implícita de identidade ou referência de `S` para `T`.
*  De qualquer *delegate_type* para `System.Delegate` e as interfaces que ele implementa.
*  De que o literal nulo para qualquer *reference_type*.
*  De qualquer *reference_type* para um *reference_type* `T` se ele tem uma conversão implícita de identidade ou uma referência a um *reference_type* `T0` e `T0` tem uma conversão de identidade para `T`.
*  De qualquer *reference_type* a um tipo de interface ou delegado `T` se ele tem uma conversão implícita de identidade ou uma referência a um tipo de interface ou delegado `T0` e `T0` é convertido por variância ([ Conversão de variância](interfaces.md#variance-conversion)) para `T`.
*  Conversões implícitas que envolvem parâmetros de tipo que são conhecidos como tipos de referência. Ver [conversões implícitas que envolvem parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) para obter mais detalhes sobre as conversões implícitas que envolvem parâmetros de tipo.

As conversões implícitas de referência são conversões entre *reference_type*s que podem ser comprovados por sempre têm êxito e, portanto, não exigem nenhuma verificação em tempo de execução.

Conversões de referência, implícitas ou explícitas, nunca altere a identidade referencial do objeto que está sendo convertido. Em outras palavras, enquanto uma conversão de referência pode alterar o tipo de referência, ele nunca muda o tipo ou o valor do objeto que está sendo referenciado.

### <a name="boxing-conversions"></a>Conversões boxing

Permite que uma conversão boxing um *value_type* a ser convertido implicitamente em um tipo de referência. Uma conversão boxing existe em qualquer *non_nullable_value_type* para `object` e `dynamic`, para `System.ValueType` e para qualquer *interface_type* implementado pelo *non_ nullable_value_type*. Além de uma *enum_type* pode ser convertido no tipo `System.Enum`.

Há uma conversão boxing de uma *nullable_type* a um tipo de referência, se e somente se uma conversão boxing existe da subjacente *non_nullable_value_type* para o tipo de referência.

Um tipo de valor tem uma conversão de boxing para um tipo de interface `I` se ele tem uma conversão de boxing para um tipo de interface `I0` e `I0` tem uma conversão de identidade para `I`.

Um tipo de valor tem uma conversão de boxing para um tipo de interface `I` se ele tem uma conversão boxing para um tipo de interface ou delegado `I0` e `I0` é convertido por variância ([conversão de variância](interfaces.md#variance-conversion)) para `I`.

Conversão boxing de um valor de uma *non_nullable_value_type* consiste de alocar uma instância do objeto e copiando o *value_type* valor para essa instância. Um struct pode ser convertido no tipo `System.ValueType`, já que é uma classe base para todos os structs ([herança](structs.md#inheritance)).

Conversão boxing de um valor de uma *nullable_type* procede da seguinte maneira:

*  Se o valor de origem é nulo (`HasValue` propriedade é false), o resultado é uma referência nula do tipo de destino.
*  Caso contrário, o resultado é uma referência a um demarcado `T` produzidos por desempacotamento e conversão boxing o valor de origem.

Conversões boxing serão descritas detalhadamente em [conversões Boxing](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Conversões implícitas de dinâmicas

Uma conversão implícita de dinâmica existe a partir de uma expressão do tipo `dynamic` a qualquer tipo `T`. A conversão é associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)), o que significa que uma conversão implícita será procurada em tempo de execução do tipo da expressão em tempo de execução `T`. Se nenhuma conversão for encontrada, uma exceção de tempo de execução é gerada.

Observe que essa conversão implícita aparentemente viola o aviso no início da [conversões implícitas](conversions.md#implicit-conversions) que uma conversão implícita nunca deve causar uma exceção. No entanto não é a conversão em si, mas o *Localizando* da conversão que faz com que a exceção. O risco de exceções de tempo de execução é inerente no uso de associação dinâmica. Se a associação dinâmica da conversão não for desejada, a expressão pode ser convertida pela primeira vez para `object`e, em seguida, para o tipo desejado.

O exemplo a seguir ilustra as conversões implícitas dinâmicas:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

As atribuições para `s2` e `i` ambos empregam conversões implícitas de dinâmicas, onde a associação das operações é suspenso até que o tempo de execução. Em tempo de execução, as conversões implícitas são buscadas do tipo de tempo de execução da `d`  --  `string` – para o tipo de destino. Uma conversão for encontrada para `string` , mas não ao `int`.

### <a name="implicit-constant-expression-conversions"></a>Conversões implícitas de expressão de constante

Uma conversão implícita de expressão de constante permite que as conversões a seguir:

*  Um *constant_expression* ([expressões constantes](expressions.md#constant-expressions)) do tipo `int` pode ser convertido no tipo `sbyte`, `byte`, `short`, `ushort`, `uint`, ou `ulong`, o valor de fornecido a *constant_expression* está dentro do intervalo do tipo de destino.
*  Um *constant_expression* do tipo `long` pode ser convertido no tipo `ulong`, fornecido o valor da *constant_expression* não é negativo.

### <a name="implicit-conversions-involving-type-parameters"></a>Conversões implícitas que envolvem parâmetros de tipo

As seguintes conversões implícitas existem para um parâmetro de tipo em questão `T`:

*  Da `T` à sua classe base efetivo `C`, do `T` a qualquer classe base do `C`e de `T` a qualquer interface implementada por `C`. No tempo de execução, se `T` é um tipo de valor, a conversão é executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.
*  Da `T` para um tipo de interface `I` na `T`interface eficiente conjunto e de `T` a qualquer interface base de `I`. No tempo de execução, se `T` é um tipo de valor, a conversão é executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.
*  Partir `T` para um parâmetro de tipo `U`, fornecida `T` depende `U` ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). No tempo de execução, se `U` é um tipo de valor, em seguida, `T` e `U` são necessariamente do mesmo tipo e nenhuma conversão é executada. Caso contrário, se `T` é um tipo de valor, a conversão é executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.
*  De que o literal nulo para `T`, fornecida `T` é conhecido por ser um tipo de referência.
*  Partir `T` para um tipo de referência `I` se ele tem uma conversão implícita para um tipo de referência `S0` e `S0` tem uma conversão de identidade para `S`. Em tempo de execução, a conversão é executada da mesma maneira que a conversão em `S0`.
*  Partir `T` para um tipo de interface `I` se ele tem uma conversão implícita para um tipo de interface ou delegado `I0` e `I0` é convertido por variância para `I` ([conversão de variância](interfaces.md#variance-conversion) ). No tempo de execução, se `T` é um tipo de valor, a conversão é executada como uma conversão boxing. Caso contrário, a conversão é executada como uma conversão de referência implícita ou conversão de identidade.

Se `T` é conhecido por ser um tipo de referência ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), as conversões acima são classificadas como conversões de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)). Se `T` não for conhecido como um tipo de referência, as conversões acima são classificadas como conversões boxing ([conversões Boxing](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Conversões implícitas definidas pelo usuário

Uma conversão implícita definidas pelo usuário consiste em uma opcional padrão conversão implícita, seguida pela execução de um operador de conversão implícita definidas pelo usuário, seguido por outra opcional padrão conversão implícita. As regras exatas para avaliar as conversões implícitas definidas pelo usuário são descritas em [processamento de conversões implícitas definidas pelo usuário](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Conversões de função anônima e conversões de grupo de método

Funções anônimas e grupos de métodos não têm tipos de em e de si mesmo, mas podem ser convertidos implicitamente para delegar a tipos ou tipos de árvore de expressão. Conversões de função anônima são descritas mais detalhadamente [conversões de função anônima](conversions.md#anonymous-function-conversions) e conversões de grupo de método em [conversões de grupo de método](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Conversões explícitas

As conversões a seguir são classificadas como conversões explícitas:

*  Todas as conversões implícitas.
*  Conversões numéricas explícitas.
*  Conversões explícitas de enumeração.
*  Conversões explícitas de permite valor nulas.
*  Conversões de referência explícita.
*  Conversões explícitas de interface.
*  Conversões de conversão unboxing.
*  Conversões explícitas de dinâmicas
*  Conversões explícitas definidas pelo usuário.

Conversões explícitas podem ocorrer em expressões de conversão ([expressões de conversão](expressions.md#cast-expressions)).

O conjunto de conversões explícitas inclui todas as conversões implícitas. Isso significa que as expressões de conversão redundantes são permitidas.

As conversões explícitas que não são as conversões implícitas são conversões que não podem ser comprovadas sempre tenha êxito, conversões que são conhecidas como possivelmente perder informações e conversões entre domínios de tipos diferentes o suficiente para mérito explícito notação.

### <a name="explicit-numeric-conversions"></a>Conversões numéricas explícitas

Conversões numéricas explícitas são as conversões de um *numeric_type* para outra *numeric_type* para o qual uma conversão numérica implícita ([conversões numéricas implícitas](conversions.md#implicit-numeric-conversions)) ainda não existir:

*  Partir `sbyte` à `byte`, `ushort`, `uint`, `ulong`, ou `char`.
*  Partir `byte` à `sbyte` e `char`.
*  Partir `short` à `sbyte`, `byte`, `ushort`, `uint`, `ulong`, ou `char`.
*  Partir `ushort` à `sbyte`, `byte`, `short`, ou `char`.
*  Partir `int` à `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, ou `char`.
*  Partir `uint` à `sbyte`, `byte`, `short`, `ushort`, `int`, ou `char`.
*  Partir `long` à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, ou `char`.
*  Partir `ulong` à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, ou `char`.
*  Partir `char` à `sbyte`, `byte`, ou `short`.
*  Partir `float` à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, ou `decimal`.
*  Partir `double` à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, ou `decimal`.
*  Partir `decimal` à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, ou `double`.

Como as conversões explícitas incluem todas as conversões numéricas implícitas e explícitas, sempre é possível converter de qualquer *numeric_type* a qualquer outra *numeric_type* usando um (expressão de conversão [Expressões de conversão](expressions.md#cast-expressions)).

Conversões numéricas explícitas, possivelmente, perder informações ou possivelmente fazer com que exceções sejam geradas. Uma conversão numérica explícita é processada da seguinte maneira:

*  Para uma conversão de um tipo integral em outro tipo integral, o processamento depende do contexto de verificação de estouro ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)) em que a conversão usa coloque:
    * Em um `checked` contexto, a conversão for bem-sucedida se o valor do operando de origem está dentro do intervalo do tipo de destino, mas gera um `System.OverflowException` se o valor do operando de origem está fora do intervalo do tipo de destino.
    * Em um `unchecked` contexto, a conversão sempre terá êxito e procede da seguinte maneira.
        * Se o tipo de origem for maior do que o tipo de destino, então o valor de origem será truncado descartando seus bits "extra" mais significativos. O resultado é então tratado como um valor do tipo de destino.
        * Se o tipo de origem for menor do que o tipo de destino, então o valor de origem será estendido por sinal ou por zero para que tenha o mesmo tamanho que o tipo de destino. A extensão por sinal será usada se o tipo de origem tiver sinal; a extensão por zero será usada se o tipo de origem não tiver sinal. O resultado é então tratado como um valor do tipo de destino.
        * Se o tipo de origem tiver o mesmo tamanho que o tipo de destino, então o valor de origem será tratado como um valor do tipo de destino.
*  Para uma conversão de `decimal` para um tipo integral, o valor de origem será arredondado para zero para o valor inteiro mais próximo, e esse valor integral torna-se o resultado da conversão. Se o valor integral resultante estiver fora do intervalo do tipo de destino, um `System.OverflowException` é gerada.
*  Para uma conversão de `float` ou `double` para um tipo integral, o processamento depende do contexto de verificação de estouro ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)) em que a conversão usa coloque:
    * Em um `checked` contexto, a conversão ocorre da seguinte maneira:
        * Se o valor do operando for NaN ou infinito, um `System.OverflowException` é gerada.
        * Caso contrário, o operando de origem será arredondado para zero para o valor inteiro mais próximo. Se esse valor integral é dentro do intervalo de tipo de destino, em seguida, esse valor é o resultado da conversão.
        * Caso contrário, uma `System.OverflowException` será gerada.
    * Em um `unchecked` contexto, a conversão sempre terá êxito e procede da seguinte maneira.
        * Se o valor do operando é NaN ou infinito, o resultado da conversão é um valor não especificado do tipo de destino.
        * Caso contrário, o operando de origem será arredondado para zero para o valor inteiro mais próximo. Se esse valor integral é dentro do intervalo de tipo de destino, em seguida, esse valor é o resultado da conversão.
        * Caso contrário, o resultado da conversão é um valor não especificado do tipo de destino.
*  Para uma conversão de `double` à `float`, o `double` valor é arredondado para mais próximo `float` valor. Se o `double` valor for muito pequeno para ser representado como um `float`, o resultado se torna zero positivo ou negativo de zero. Se o `double` valor é muito grande para ser representado como um `float`, o resultado se torna o infinito positivo ou negativo infinito. Se o `double` valor é NaN, o resultado também é NaN.
*  Para uma conversão de `float` ou `double` à `decimal`, o valor de origem é convertido em `decimal` representação e arredondado para o próximo número após a 28ª casa decimal, se necessário ([o tipo decimal](types.md#the-decimal-type)). Se o valor de origem é muito pequeno para ser representado como um `decimal`, o resultado será zero. Se o valor de origem for NaN, infinito, ou muito grande para ser representado como uma `decimal`, um `System.OverflowException` é gerada.
*  Para uma conversão de `decimal` à `float` ou `double`, o `decimal` valor é arredondado para mais próximo `double` ou `float` valor. Embora essa conversão pode perder a precisão, ele nunca faz com que uma exceção seja lançada.

### <a name="explicit-enumeration-conversions"></a>Conversões explícitas de enumeração

As conversões de enumeração explícita são:

*  Partir `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, ou `decimal` para qualquer *enum_type*.
*  De qualquer *enum_type* à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, ou `decimal`.
*  De qualquer *enum_type* a qualquer outra *enum_type*.

Uma conversão explícita de enumeração entre dois tipos é processada, tratando qualquer participando *enum_type* como o tipo subjacente de que *enum_type*e, em seguida, executar uma implícita ou explícita conversão numérica entre os tipos resultantes. Por exemplo, dada uma *enum_type* `E` com e o tipo subjacente de `int`, uma conversão de `E` para `byte` é processado como uma conversão numérica explícita ([explícita conversões numéricas](conversions.md#explicit-numeric-conversions)) da `int` à `byte`e uma conversão de `byte` para `E` é processado como uma conversão numérica implícita ([conversões numéricas implícitas](conversions.md#implicit-numeric-conversions)) partir `byte` para `int`.

### <a name="explicit-nullable-conversions"></a>Conversões explícitas de permite valor nulas

***Conversões explícitas de anuláveis*** predefinidos de permitir conversões explícitas que operam em tipos de valor não anulável para também ser usados com formulários que permitem valor nulos desses tipos. Para cada uma das conversões explícitas predefinidas que convertem de um tipo de valor não anulável `S` para um tipo de valor não anulável `T` ([conversão de identidade](conversions.md#identity-conversion), [conversões numéricas implícitas](conversions.md#implicit-numeric-conversions), [Conversões implícitas de enumeração](conversions.md#implicit-enumeration-conversions), [conversões numéricas explícitas](conversions.md#explicit-numeric-conversions), e [conversões explícitas de enumeração](conversions.md#explicit-enumeration-conversions)), o seguinte Existem conversões que permitem valor nulas:

*  Uma conversão explícita de `S?` para `T?`.
*  Uma conversão explícita de `S` para `T?`.
*  Uma conversão explícita de `S?` para `T`.

Avaliação de uma conversão que permitem valor nula com base em uma conversão subjacente de `S` para `T` procede da seguinte maneira:

*  Se a conversão que permitem valor nula é de `S?` para `T?`:
    * Se o valor de origem é nulo (`HasValue` propriedade é false), o resultado é o valor nulo do tipo `T?`.
    * Caso contrário, a conversão será avaliada como um desempacotamento do `S?` para `S`, seguido pela conversão de subjacente `S` ao `T`, seguido por um encapsulamento de `T` para `T?`.
*  Se a conversão que permitem valor nula é de `S` para `T?`, a conversão é avaliada como a conversão subjacente do `S` à `T` seguido por um encapsulamento de `T` para `T?`.
*  Se a conversão que permitem valor nula é de `S?` para `T`, a conversão será avaliada como um desempacotamento do `S?` à `S` seguido pela conversão de subjacente `S` para `T`.

Observe que uma tentativa de decodificar um valor anulável lançará uma exceção se o valor for `null`.

### <a name="explicit-reference-conversions"></a>Conversões de referência explícita

As conversões de referência explícita são:

*  Partir `object` e `dynamic` a qualquer outra *reference_type*.
*  De qualquer *class_type* `S` a qualquer *class_type* `T`, fornecido `S` é uma classe base de `T`.
*  De qualquer *class_type* `S` a qualquer *interface_type* `T`, fornecido `S` não é selado e fornecidos `S` não implementa `T`.
*  De qualquer *interface_type* `S` a qualquer *class_type* `T`, fornecido `T` não está lacrado ou fornecida `T` implementa `S`.
*  De qualquer *interface_type* `S` a qualquer *interface_type* `T`, fornecido `S` não é derivado de `T`.
*  De um *array_type* `S` com um tipo de elemento `SE` para um *array_type* `T` com um tipo de elemento `TE`, desde que todos os itens a seguir forem verdadeiras:
    * `S` e `T` diferem apenas no tipo de elemento. Em outras palavras, `S` e `T` têm o mesmo número de dimensões.
    * Ambos `SE` e `TE` são *reference_type*s.
    * Existe uma conversão de referência explícita de `SE` para `TE`.
*  Partir `System.Array` e as interfaces que ele implementa a qualquer *array_type*.
*  De um tipo de matriz unidimensional `S[]` à `System.Collections.Generic.IList<T>` e suas interfaces base, fornecidos que há uma conversão de referência explícita do `S` para `T`.
*  Partir `System.Collections.Generic.IList<S>` e sua base de interfaces para um tipo de matriz unidimensional `T[]`, desde que haja uma conversão explícita de identidade ou referência de `S` para `T`.
*  Partir `System.Delegate` e as interfaces que ele implementa a qualquer *delegate_type*.
*  De um tipo de referência para um tipo de referência `T` se ele tem uma conversão de referência explícita para um tipo de referência `T0` e `T0` tem uma conversão de identidade `T`.
*  De um tipo de referência a um tipo de interface ou delegado `T` se ele tem uma conversão de referência explícita a um tipo de interface ou delegado `T0` e qualquer `T0` é convertido por variância para `T` ou `T` é convertido por variância para `T0` ([conversão de variância](interfaces.md#variance-conversion)).
*  Do `D<S1...Sn>` para `D<T1...Tn>` onde `D<X1...Xn>` é um tipo de delegado genérico `D<S1...Sn>` não é compatível com ou idêntico ao `D<T1...Tn>`e para cada parâmetro de tipo `Xi` de `D` mantém a seguir:
    * Se `Xi` é invariante, em seguida, `Si` é idêntico ao `Ti`.
    * Se `Xi` é covariante, em seguida, há uma conversão de identidade ou uma referência implícita ou explícita de `Si` para `Ti`.
    * Se `Xi` é contravariante, em seguida, `Si` e `Ti` são idênticos ou os dois tipos de referência.
*  Conversões explícitas que envolvem parâmetros de tipo que são conhecidos como tipos de referência. Para obter mais detalhes sobre as conversões explícitas que envolvem parâmetros de tipo, consulte [conversões explícitas que envolvem parâmetros de tipo](conversions.md#explicit-conversions-involving-type-parameters).

As conversões de referência explícita são conversões entre tipos de referência que exigem verificações de tempo de execução para garantir que eles estão corretos.

Para uma conversão de referência explícita seja bem-sucedida em tempo de execução, o valor do operando de origem deve ser `null`, ou o tipo real do objeto referenciado pelo operando de origem deve ser um tipo que pode ser convertido para o tipo de destino por uma referência implícita conversão ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) ou conversão boxing ([conversões Boxing](conversions.md#boxing-conversions)). Se uma conversão de referência explícita falhar, um `System.InvalidCastException` é gerada.

Conversões de referência, implícitas ou explícitas, nunca altere a identidade referencial do objeto que está sendo convertido. Em outras palavras, enquanto uma conversão de referência pode alterar o tipo de referência, ele nunca muda o tipo ou o valor do objeto que está sendo referenciado.

### <a name="unboxing-conversions"></a>Conversões de conversão unboxing

Uma conversão unboxing permite que um tipo de referência a ser convertido explicitamente em um *value_type*. Uma conversão unboxing existe entre os tipos `object`, `dynamic` e `System.ValueType` a qualquer *non_nullable_value_type*e de qualquer *interface_type* para qualquer *non_ nullable_value_type* que implementa o *interface_type*. Além disso digite `System.Enum` podem ser desconvertidos para qualquer *enum_type*.

Uma conversão unboxing existe a partir de um tipo de referência para um *nullable_type* se uma conversão unboxing existir do tipo de referência subjacente *non_nullable_value_type* do  *nullable_type*.

Um tipo de valor `S` tem uma conversão unboxing de um tipo de interface `I` se ele tem uma conversão unboxing de um tipo de interface `I0` e `I0` tem uma conversão de identidade para `I`.

Um tipo de valor `S` tem uma conversão unboxing de um tipo de interface `I` se ele tem uma conversão unboxing de um tipo de interface ou delegado `I0` e qualquer um dos `I0` é convertido por variância para `I` ou `I`é convertido por variância para `I0` ([conversão de variância](interfaces.md#variance-conversion)).

Uma operação de conversão unboxing consiste em verificar primeiro que a instância do objeto é um valor demarcado da determinada *value_type*e, em seguida, copiar o valor para a instância. Unboxing de uma referência nula para um *nullable_type* produz o valor null da *nullable_type*. Um struct pode ser não demarcado do tipo `System.ValueType`, já que é uma classe base para todos os structs ([herança](structs.md#inheritance)).

Conversões de conversão unboxing são descritas mais detalhadamente em [conversões de conversão Unboxing](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Conversões explícitas de dinâmicas

Existe uma conversão de dinâmica explícita de uma expressão do tipo `dynamic` a qualquer tipo `T`. A conversão é associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)), o que significa que uma conversão explícita será procurada em tempo de execução do tipo da expressão em tempo de execução `T`. Se nenhuma conversão for encontrada, uma exceção de tempo de execução é gerada.

Se a associação dinâmica da conversão não for desejada, a expressão pode ser convertida pela primeira vez para `object`e, em seguida, para o tipo desejado.

Suponha que a seguinte classe é definida:
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

O exemplo a seguir ilustra as conversões explícitas de dinâmicas:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Melhor conversão de `o` para `C` for encontrado em tempo de compilação para ser uma conversão de referência explícita. Isso falhará em tempo de execução, porque `"1"` não é na verdade um `C`. A conversão de `d` para `C` no entanto, como uma conversão explícita dinâmica, é suspenso para tempo de execução, onde um usuário definido a conversão do tipo de tempo de execução `d`  --  `string` – ao `C` for encontrado, e é bem-sucedida.

### <a name="explicit-conversions-involving-type-parameters"></a>Conversões explícitas que envolvem parâmetros de tipo

As conversões explícitas a seguir existem para um parâmetro de tipo determinado `T`:

*  Da classe base efetiva `C` dos `T` para `T` e de qualquer classe base da `C` para `T`. No tempo de execução, se `T` é um tipo de valor, a conversão é executada como uma conversão unboxing. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.
*  De qualquer tipo de interface para `T`. No tempo de execução, se `T` é um tipo de valor, a conversão é executada como uma conversão unboxing. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.
*  Partir `T` a qualquer *interface_type* `I` fornecido já não existe uma conversão implícita de `T` para `I`. No tempo de execução, se `T` é um tipo de valor, a conversão é executada como uma conversão boxing seguida por uma conversão de referência explícita. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.
*  De um parâmetro de tipo `U` à `T`, fornecida `T` depende `U` ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). No tempo de execução, se `U` é um tipo de valor, em seguida, `T` e `U` são necessariamente do mesmo tipo e nenhuma conversão é executada. Caso contrário, se `T` é um tipo de valor, a conversão é executada como uma conversão unboxing. Caso contrário, a conversão é executada como uma conversão de referência explícita ou conversão de identidade.

Se `T` é conhecido por ser um tipo de referência, as conversões acima são todos classificadas como conversões de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)). Se `T` não for conhecido como um tipo de referência, as conversões acima são classificadas como conversões unboxing ([conversões de conversão Unboxing](conversions.md#unboxing-conversions)).

As regras acima não é possível fazer uma conversão explícita direta de um parâmetro de tipo sem restrição para um tipo sem interface, que pode ser surpreendente. O motivo para essa regra é para evitar confusão e fazer a semântica de tais conversões claras. Por exemplo, considere a seguinte declaração:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Se a conversão explícita direta `t` à `int` eram permitidas, seria possível com facilidade que `X<int>.F(7)` retornaria `7L`. No entanto, ele seria não, porque as conversões numéricas padrão são consideradas apenas quando os tipos são conhecidos como numéricos em tempo de associação. Para fazer a semântica clear, o exemplo acima deve ser escrito:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Esse código será compilado agora em execução, mas `X<int>.F(7)` , em seguida, gerará uma exceção em tempo de execução, desde um demarcado `int` não podem ser convertidos diretamente em um `long`.

### <a name="user-defined-explicit-conversions"></a>Conversões explícitas definidas pelo usuário

Uma conversão explícita definida pelo usuário consiste em uma opcional padrão conversão explícita, seguida pela execução de um operador de conversão implícita ou explícita definida pelo usuário, seguido por outra opcional padrão conversão explícita. As regras exatas para avaliar as conversões explícitas definidas pelo usuário são descritas em [processamento de conversões explícitas definidas pelo usuário](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Conversões padrão

As conversões padrão são as conversões predefinidas que podem ocorrer como parte de uma conversão definida pelo usuário.

### <a name="standard-implicit-conversions"></a>Conversões implícitas padrão

As seguintes conversões implícitas são classificadas como conversões implícitas padrão:

*  Conversões de identidade ([conversão de identidade](conversions.md#identity-conversion))
*  Conversões numéricas implícitas ([conversões numéricas implícitas](conversions.md#implicit-numeric-conversions))
*  Conversões implícitas de permite valor nulas ([conversões implícitas de anuláveis](conversions.md#implicit-nullable-conversions))
*  Conversões de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions))
*  Conversões boxing ([conversões Boxing](conversions.md#boxing-conversions))
*  Conversões implícitas de expressão de constante ([conversões implícitas de dinâmicas](conversions.md#implicit-dynamic-conversions))
*  Conversões implícitas que envolvem parâmetros de tipo ([conversões implícitas que envolvem parâmetros de tipo](conversions.md#implicit-conversions-involving-type-parameters))

As conversões implícitas padrão excluem especificamente conversões implícitas definidas pelo usuário.

### <a name="standard-explicit-conversions"></a>Conversões explícitas padrão

As conversões explícitas padrão são todas as conversões implícitas standard mais o subconjunto de conversões explícitas para o qual existe uma conversão implícita de padrão oposta. Em outras palavras, se um padrão implícito existe conversão de um tipo `A` para um tipo de `B`, em seguida, existe uma conversão explícita padrão do tipo `A` digitar `B` e do tipo `B` para o tipo `A`.

## <a name="user-defined-conversions"></a>Conversões definidas pelo usuário

C# permite que as conversões implícitas e explícitas predefinidas ser aprimorados pelos ***conversões definidas pelo usuário***. Conversões definidas pelo usuário são introduzidas com a declaração de operadores de conversão ([operadores de conversão](classes.md#conversion-operators)) em tipos de classe e struct.

### <a name="permitted-user-defined-conversions"></a>Permitidas conversões definidas pelo usuário

O c# permite que somente certas conversões definidas pelo usuário deve ser declarado. Em particular, não é possível redefinir uma conversão implícita ou explícita já existente.

Para um tipo de origem especificado `S` e o tipo de destino `T`, se `S` ou `T` são tipos que permitem valor nulos, deixe `S0` e `T0` consulte seus tipos base, caso contrário `S0` e `T0` são igual a `S` e `T` , respectivamente. Uma classe ou struct é permitida para declarar uma conversão de um tipo de fonte `S` para um tipo de destino `T` somente se todos os itens a seguir forem verdadeiras:

*  `S0` e `T0` são de tipos diferentes.
*  Tanto `S0` ou `T0` é o tipo de classe ou struct em que a declaração do operador ocorre.
*  Nem `S0` nem `T0` é um *interface_type*.
*  Excluindo conversões definidas pelo usuário, uma conversão não existe na `S` à `T` ou do `T` para `S`.

As restrições que se aplicam a conversões definidas pelo usuário são discutidas mais detalhadamente em [operadores de conversão](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Operadores de conversão cancelada

Dado um operador de conversão definida pelo usuário que converte de um tipo de valor não anulável `S` para um tipo de valor não anulável `T`, um ***levantado o operador de conversão*** existe que converte do `S?` para `T?`. Este operador de conversão elevadas executa um desempacotamento do `S?` para `S` seguido pela conversão definida pelo usuário de `S` à `T` seguido por um encapsulamento de `T` para `T?`, exceto que um valor nulo Valued `S?` converte diretamente para um valor nulo com valor de `T?`.

Um operador de conversão elevadas tem a mesma classificação implícita ou explícita como seu operador de conversão definida pelo usuário subjacente. O termo "conversão definida pelo usuário" aplica-se ao uso de ambos definidos pelo usuário e retiradas operadores de conversão.

### <a name="evaluation-of-user-defined-conversions"></a>Avaliação das conversões definidas pelo usuário

Uma conversão definida pelo usuário converte um valor de seu tipo, chamado de ***tipo de fonte***, em outro tipo, chamado a ***tipo de destino***. Centros de avaliação de uma conversão definida pelo usuário sobre como localizar o ***mais específica*** operador de conversão definida pelo usuário para os tipos de origem e de destino específicos. Essa determinação é dividida em várias etapas:

*  Localizando o conjunto de classes e estruturas do qual os operadores de conversão definida pelo usuário serão considerados. Este conjunto consiste o tipo de origem e suas classes base e o tipo de destino e suas classes base (com as suposições implícitas somente classes e structs podem declarar operadores definidos pelo usuário e que tipos de classe não têm nenhuma classe base). Para os fins desta etapa, se o tipo de origem ou destino é um *nullable_type*, seu tipo subjacente é usado em vez disso.
*  Desse conjunto de tipos, determinando que definidas pelo usuário e retiradas operadores de conversão são aplicáveis. Para um operador de conversão seja aplicável, deve ser possível executar uma conversão padrão ([conversões padrão](conversions.md#standard-conversions)) do tipo de fonte ao operando tipo de operador e ele deve ser possível executar uma conversão padrão tipo do resultado do operador para o tipo de destino.
*  O conjunto de aplicáveis operadores definidos pelo usuário, determinar qual operador sem ambiguidade o mais específico. Em termos gerais, o operador mais específico é o operador cujo tipo de operando é "mais próximo" para o tipo de fonte e cujo tipo de resultado é "mais próximo" para o tipo de destino. Operadores de conversão definida pelo usuário têm preferência sobre os operadores de conversão cancelada. As regras exatas para estabelecer o operador de conversão definida pelo usuário mais específico são definidas nas seções a seguir.

Depois que um operador de conversão definida pelo usuário mais específico tiver sido identificado, a execução real da conversão definida pelo usuário envolve até três etapas:

*  Primeiro, se necessário, executar uma conversão padrão do tipo de fonte para o tipo de operando do operador de conversão definida pelo usuário ou cancelada.
*  Em seguida, chamando o operador de conversão definida pelo usuário ou elevadas para executar a conversão.
*  Por fim, se necessário, executar uma conversão padrão do tipo de resultado do operador de conversão definida pelo usuário ou cancelada para o tipo de destino.

Avaliação de uma conversão definida pelo usuário nunca envolve mais de um operador de conversão definida pelo usuário ou cancelada. Em outras palavras, uma conversão de tipo `S` digitar `T` nunca primeiro executará uma conversão definida pelo usuário de `S` à `X` e, em seguida, executar uma conversão definida pelo usuário de `X` para `T`.

Definições exatas de avaliação das conversões implícitas ou explícitas definidas pelo usuário são fornecidas nas seções a seguir. Verifique as definições de usar os seguintes termos:

*  Se uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) existe a partir de um tipo `A` em um tipo `B`e caso nem `A` nem `B` são *interface_type*s, em seguida, `A` será considerada ***abrangida pela*** `B`, e `B` será considerada ***abranger*** `A`.
*  O ***mais abrangente de tipo*** em um conjunto de tipos é o um tipo que abrange todos os outros tipos no conjunto. Se nenhum tipo abrange todos os outros tipos, o conjunto não tem nenhum tipo mais abrangente. Em termos mais intuitivos, o tipo mais abrangente é o tipo "maior" no conjunto — o um tipo ao qual cada um dos outros tipos pode ser convertida implicitamente.
*  O ***englobados mais tipo*** em um conjunto de tipos é o tipo de um determinado contido por todos os outros tipos no conjunto. Se nenhum tipo é abrangido pela todos os outros tipos, o conjunto tem não mais englobados tipo. Em termos mais intuitivos, o tipo mais contido é o tipo "menor" no conjunto — o um tipo que pode ser convertido implicitamente em cada um dos outros tipos.

### <a name="processing-of-user-defined-implicit-conversions"></a>Processamento de conversões implícitas definidas pelo usuário

Uma conversão implícita definidas pelo usuário do tipo `S` digitar `T` é processado da seguinte maneira:

*  Determinar os tipos `S0` e `T0`. Se `S` ou `T` são tipos anuláveis, `S0` e `T0` são seus tipos base, caso contrário `S0` e `T0` são iguais ao `S` e `T` , respectivamente.
*  Localize o conjunto de tipos, `D`, do qual a conversão definida pelo usuário serão considerados operadores. Este conjunto consiste `S0` (se `S0` é uma classe ou struct), as classes base `S0` (se `S0` é uma classe), e `T0` (se `T0` é uma classe ou struct).
*  Localizar o conjunto de operadores de conversão definida pelo usuário e elevadas aplicável, `U`. Este conjunto consiste nos operadores de conversão implícita elevadas e definidas pelo usuário declarados pelo classes ou structs em `D` que converter de um tipo que abranja `S` a um tipo abrangido pela `T`. Se `U` está vazio, a conversão é indefinida e ocorrer um erro de tempo de compilação.
*  Localizar o tipo de origem mais específico `SX`, os operadores em `U`:
    * Se qualquer um dos operadores `U` converter `S`, em seguida, `SX` é `S`.
    * Caso contrário, `SX` é o tipo mais contido no conjunto combinado de tipos de fonte dos operadores na `U`. Se exatamente um mais englobados tipo não for encontrado, em seguida, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Localizar o tipo de destino mais específico, `TX`, os operadores em `U`:
    * Se qualquer um dos operadores `U` converter `T`, em seguida, `TX` é `T`.
    * Caso contrário, `TX` é o tipo mais abrangente no conjunto combinado de tipos de destino dos operadores na `U`. Se não for encontrado exatamente um tipo mais abrangente, em seguida, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Localize o operador de conversão mais específico:
    * Se `U` contém exatamente um operador de conversão definida pelo usuário que converte do `SX` para `TX`, em seguida, este é o operador de conversão mais específico.
    * Caso contrário, se `U` contém exatamente um operador de conversão elevadas que converte do `SX` para `TX`, em seguida, este é o operador de conversão mais específico.
    * Caso contrário, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Por fim, aplique a conversão:
    * Se `S` não é `SX`, em seguida, uma conversão implícita padrão de `S` para `SX` é executada.
    * O operador de conversão mais específico é invocado para converter `SX` para `TX`.
    * Se `TX` não é `T`, em seguida, uma conversão implícita padrão de `TX` para `T` é executada.

### <a name="processing-of-user-defined-explicit-conversions"></a>Processamento de conversões explícitas definidas pelo usuário

Uma conversão explícita definida pelo usuário do tipo `S` digitar `T` é processado da seguinte maneira:

*  Determinar os tipos `S0` e `T0`. Se `S` ou `T` são tipos anuláveis, `S0` e `T0` são seus tipos base, caso contrário `S0` e `T0` são iguais ao `S` e `T` , respectivamente.
*  Localize o conjunto de tipos, `D`, do qual a conversão definida pelo usuário serão considerados operadores. Este conjunto consiste `S0` (se `S0` é uma classe ou struct), as classes base `S0` (se `S0` é uma classe), `T0` (se `T0` é uma classe ou struct) e as classes base `T0` (se `T0`é uma classe).
*  Localizar o conjunto de operadores de conversão definida pelo usuário e elevadas aplicável, `U`. Este conjunto consiste nas definidas pelo usuário e elevadas implícita ou operadores de conversão explícita declarado pelo classes ou estruturas nos `D` que converter de um tipo que abranja ou abrangida pela `S` para um tipo que abranja ou abrangida pela `T`. Se `U` está vazio, a conversão é indefinida e ocorrer um erro de tempo de compilação.
*  Localizar o tipo de origem mais específico `SX`, os operadores em `U`:
    * Se qualquer um dos operadores `U` converter `S`, em seguida, `SX` é `S`.
    * Caso contrário, se qualquer um dos operadores `U` converter de tipos que englobam `S`, em seguida, `SX` é o tipo mais contido no conjunto combinado de tipos de fonte desses operadores. Se nenhuma maioria englobados tipo pode ser encontrado, em seguida, a conversão é ambígua e ocorre um erro de tempo de compilação.
    * Caso contrário, `SX` é o tipo mais abrangente no conjunto combinado de tipos de fonte dos operadores na `U`. Se não for encontrado exatamente um tipo mais abrangente, em seguida, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Localizar o tipo de destino mais específico, `TX`, os operadores em `U`:
    * Se qualquer um dos operadores `U` converter `T`, em seguida, `TX` é `T`.
    * Caso contrário, se qualquer um dos operadores na `U` converter em tipos que são englobados por `T`, em seguida, `TX` é o tipo mais abrangente no conjunto combinado de tipos de destino desses operadores. Se não for encontrado exatamente um tipo mais abrangente, em seguida, a conversão é ambígua e ocorre um erro de tempo de compilação.
    * Caso contrário, `TX` é o tipo mais contido no conjunto combinado de tipos de destino dos operadores na `U`. Se nenhuma maioria englobados tipo pode ser encontrado, em seguida, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Localize o operador de conversão mais específico:
    * Se `U` contém exatamente um operador de conversão definida pelo usuário que converte do `SX` para `TX`, em seguida, este é o operador de conversão mais específico.
    * Caso contrário, se `U` contém exatamente um operador de conversão elevadas que converte do `SX` para `TX`, em seguida, este é o operador de conversão mais específico.
    * Caso contrário, a conversão é ambígua e ocorre um erro de tempo de compilação.
*  Por fim, aplique a conversão:
    * Se `S` não é `SX`, em seguida, uma conversão explícita padrão de `S` para `SX` é executada.
    * O operador de conversão definida pelo usuário mais específico é invocado para converter `SX` para `TX`.
    * Se `TX` não é `T`, em seguida, uma conversão explícita padrão de `TX` para `T` é executada.

## <a name="anonymous-function-conversions"></a>Conversões de função anônima

Uma *anonymous_method_expression* ou *lambda_expression* é classificado como uma função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)). A expressão não tem um tipo, mas pode ser convertida implicitamente em um tipo de delegado compatível ou tipo de árvore de expressão. Especificamente, uma função anônima `F` é compatível com um tipo de delegado `D` fornecido:

*  Se `F` contém uma *anonymous_function_signature*, em seguida, `D` e `F` têm o mesmo número de parâmetros.
*  Se `F` não contém um *anonymous_function_signature*, em seguida, `D` pode ter zero ou mais parâmetros de qualquer tipo, desde que nenhum parâmetro de `D` tem o `out` modificador de parâmetro.
*  Se `F` tem uma lista de parâmetros de tipo explícito, cada parâmetro na `D` tem o mesmo tipo e modificadores de parâmetro correspondente no `F`.
*  Se `F` tem uma lista de parâmetro de tipo implícito `D` não tem nenhum `ref` ou `out` parâmetros.
*  Se o corpo da `F` for uma expressão e `D` tem um `void` tipo de retorno ou `F` é assíncrono e `D` tem o tipo de retorno `Task`, em seguida, quando cada parâmetro de `F` está considerando o tipo dos o parâmetro correspondente em `D`, o corpo da `F` é uma expressão válida (wrt [expressões](expressions.md)) que seria permitido como um *statement_expression* ([Instruções de expressão](statements.md#expression-statements)).
*  Se o corpo da `F` é um bloco de instrução e `D` tem um `void` tipo de retorno ou `F` é assíncrono e `D` tem o tipo de retorno `Task`, em seguida, quando cada parâmetro de `F` está considerando o tipo de o parâmetro correspondente na `D`, o corpo da `F` é um bloco de instruções válido (wrt [blocos](statements.md#blocks)) no qual nenhum `return` declaração especifica uma expressão.
*  Se o corpo da `F` é uma expressão, e *qualquer* `F` é não assíncronas e `D` tem um tipo de retorno não nulo `T`, *ou* `F` é assíncrono e `D` tem um tipo de retorno `Task<T>`, em seguida, quando cada parâmetro do `F` recebe o tipo do parâmetro correspondente na `D`, o corpo da `F` é uma expressão válida (wrt [ Expressões](expressions.md)) que é implicitamente conversível para `T`.
*  Se o corpo da `F` é um bloco de instrução, e *qualquer* `F` é não assíncronas e `D` tem um tipo de retorno não nulo `T`, *ou* `F` é assíncrono e `D` tem um tipo de retorno `Task<T>`, em seguida, quando cada parâmetro do `F` recebe o tipo do parâmetro correspondente na `D`, o corpo da `F` é um bloco de instruções válido (wrt [blocos ](statements.md#blocks)) com um ponto de extremidade não pode ser acessado na qual cada `return` declaração especifica uma expressão que é implicitamente conversível para `T`.

Para fins de brevidade, esta seção usa a forma abreviada para os tipos de tarefa `Task` e `Task<T>` ([funções assíncronas](classes.md#async-functions)).

Uma expressão lambda `F` é compatível com um tipo de árvore de expressão `Expression<D>` se `F` é compatível com o tipo de delegado `D`. Observe que isso não se aplica aos métodos anônimos, somente as expressões lambda.

Determinadas expressões lambda não podem ser convertidos em tipos de árvore de expressão: mesmo que a conversão *existe*, ele falhará em tempo de compilação. Isso é o caso se a expressão lambda:

*  Tem um *bloco* corpo
*  Contém os operadores de atribuição simples ou composta
*  Contém uma expressão associada dinamicamente
*  É assíncrono

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
os parâmetro e tipos de retorno de cada função anônima são determinados pelo tipo da variável à qual a função anônima é atribuída.

A primeira atribuição converte com êxito a função anônima para o tipo de delegado `Func<int,int>` porque, quando `x` recebe o tipo `int`, `x+1` é uma expressão válida que é implicitamente conversível para o tipo `int`.

Da mesma forma, a segunda atribuição converte com êxito a função anônima para o tipo de delegado `Func<int,double>` porque o resultado da `x+1` (do tipo `int`) é implicitamente conversível para o tipo `double`.

No entanto, a atribuição de terceiro é um erro de tempo de compilação porque, quando `x` recebe o tipo `double`, o resultado da `x+1` (do tipo `double`) não é implicitamente conversível para o tipo `int`.

A atribuição de quarta converte com êxito a função assíncrona anônimo para o tipo de delegado `Func<int, Task<int>>` porque o resultado da `x+1` (do tipo `int`) é implicitamente conversível para o tipo de resultado `int` do tipo de tarefa `Task<int>`.

Funções anônimas podem influenciar a resolução de sobrecarga e participar de inferência de tipo. Ver [membros de função](expressions.md#function-members) para obter mais detalhes.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Avaliação de conversões de função anônima para tipos de delegado

Conversão de uma função anônima para um tipo de delegado produz uma instância de delegado que faz referência a função anônima e o conjunto (possivelmente vazio) de variáveis externas capturadas que estão ativas no momento da avaliação. Quando o delegado é invocado, o corpo da função anônima que é executado. O código no corpo é executado usando o conjunto de variáveis externas capturadas referenciado pelo delegado.

A lista de invocação de um delegado produzido a partir de uma função anônima contém uma única entrada. O objeto de destino exato e o método de destino do delegado são especificados. Em particular, ele é não especificado se o objeto de destino do delegado é `null`, o `this` valor o membro da função delimitadora ou algum outro objeto.

Conversões de funções anônimas semanticamente idênticas com o mesmo conjunto (possivelmente vazia) de instâncias capturadas de variável externas para os mesmos tipos de delegado são permitidas (mas não obrigatório) para retornar a mesma instância do delegado. O termo semanticamente idêntico é usado aqui para indicar que a execução das funções anônimas, em todos os casos, produzirá os mesmos efeitos considerando os mesmos argumentos. Essa regra permite o código a seguir para ser otimizada.

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

Como os dois delegados de função anônima têm as mesmas (vazio) das variáveis externas capturadas e, como as funções anônimas são semanticamente idênticas, o compilador tem permissão para ter os delegados se referem ao mesmo método de destino. Na verdade, o compilador tem permissão para retornar a instância do representante mesmo de ambas as expressões de função anônima.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Avaliação de conversões de função anônima para tipos de árvore de expressão

Conversão de uma função anônima para um tipo de árvore de expressão produz uma árvore de expressão ([tipos de árvore de expressão](types.md#expression-tree-types)). Mais precisamente, a avaliação da conversão de função anônima leva a construção de uma estrutura de objeto que representa a estrutura da função anônima em si. A estrutura exata da árvore de expressão, bem como o processo exato para criá-lo, são definido pela implementação.

### <a name="implementation-example"></a>Exemplo de implementação

Esta seção descreve uma possível implementação de conversões de função anônima em termos de outras construções de linguagem c#. A implementação descrita aqui baseia-se os mesmos princípios usados pelo compilador Microsoft c#, mas ele não é uma implementação obrigatória e não é o único possível. Resumidamente, ela menciona conversões em árvores de expressão, conforme sua semântica exata está fora do escopo desta especificação.

O restante desta seção fornece vários exemplos de código que contém funções anônimas com características diferentes. Para cada exemplo, uma tradução correspondente ao código que usa somente outros constructos de c# é fornecida. Nos exemplos, o identificador `D` será considerado por representam o seguinte tipo de delegado:
```csharp
public delegate void D();
```

A forma mais simples de uma função anônima é aquele que captura sem variáveis externas:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Isso pode ser convertido para uma instanciação de delegado que faz referência a um método estático de gerado pelo compilador em que o código da função anônima é colocado:
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

No exemplo a seguir, a função anônima faz referência a membros de instância de `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Isso pode ser convertido para um método de instância gerado pelo compilador que contém o código da função anônimo:
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

O tempo de vida da variável local deve ser estendido para pelo menos o tempo de vida do delegado de função anônima. Isso pode ser obtido por "elevação" da variável local em um campo de uma classe gerada pelo compilador. Instanciação de variável local ([instanciação de variáveis locais](expressions.md#instantiation-of-local-variables)), em seguida, corresponde à criação de uma instância da classe gerada pelo compilador e acessar a variável local corresponde ao acesso a um campo na instância do a classe gerada pelo compilador. Além disso, a função anônima se torna um método de instância da classe gerada pelo compilador:
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

Por fim, a anônimo função a seguir capturas `this` , bem como duas variáveis locais com tempos de vida diferentes:
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

Aqui, uma classe gerada pelo compilador é criada para cada instrução bloquear em quais locais são capturadas, de modo que os locais nos blocos diferentes podem ter tempos de vida independentes. Uma instância do `__Locals2`, a classe gerada pelo compilador para o bloco de instrução interna, contém a variável local `z` e um campo que faz referência a uma instância de `__Locals1`.  Uma instância do `__Locals1`, a classe gerada pelo compilador para o bloco de instrução externa, contém a variável local `y` e um campo que faz referência a `this` do membro da função delimitadora. Com essas estruturas de dados, é possível alcançar todas capturadas variáveis externas através de uma instância de `__Local2`, e o código da função anônimo, portanto, pode ser implementado como um método de instância dessa classe.

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

A mesma técnica aqui aplicada para capturar as variáveis locais também pode ser usada ao converter funções anônimas para árvores de expressão: referências aos objetos gerados pelo compilador que podem ser armazenadas na árvore de expressão e o acesso às variáveis de locais pode ser representado como campo acessa nesses objetos. A vantagem dessa abordagem é que ele permite que as variáveis locais "canceladas" ser compartilhado entre delegados e árvores de expressão.

## <a name="method-group-conversions"></a>Conversões de grupo de método

Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe a partir de um grupo de métodos ([classificações de expressão](expressions.md#expression-classifications)) para um tipo de delegado compatível. Dado um tipo de delegado `D` e uma expressão `E` que é classificado como um grupo de métodos, existe uma conversão implícita de `E` à `D` se `E` contém pelo menos um método que é aplicável em sua forma normal ( [Membro da função aplicáveis](expressions.md#applicable-function-member)) para uma lista de argumentos construído pelo uso dos tipos de parâmetro e modificadores de `D`, conforme descrito a seguir.

O aplicativo de tempo de compilação de uma conversão de um grupo de métodos `E` para um tipo delegado `D` é descrito a seguir. Observe que a existência de uma conversão implícita da `E` para `D` não garante que o aplicativo de tempo de compilação da conversão será bem-sucedida sem erro.

*  Um único método `M` está selecionado correspondente a uma invocação de método ([invocações de método](expressions.md#method-invocations)) do formulário `E(A)`, com as seguintes modificações:
    * A lista de argumentos `A` é uma lista de expressões, cada classificadas como uma variável e com o tipo e o modificador (`ref` ou `out`) do parâmetro correspondente no *formal_parameter_list* de `D`.
    * Os métodos de candidato considerados são somente os métodos que são aplicáveis em sua forma normal ([membro da função aplicável](expressions.md#applicable-function-member)), não os aplicável apenas em seus formulários expandidos.
*  Se o algoritmo de [invocações de método](expressions.md#method-invocations) produz um erro, em seguida, ocorre um erro de tempo de compilação. Caso contrário, o algoritmo produz um único método melhor `M` tendo o mesmo número de parâmetros como `D` e a conversão é considerada de existir.
*  O método selecionado `M` devem ser compatíveis ([compatibilidade de delegado](delegates.md#delegate-compatibility)) com o tipo de delegado `D`, ou caso contrário, ocorrerá um erro de tempo de compilação.
*  Se o método selecionado `M` é um método de instância, a expressão de instância associada `E` determina o objeto de destino do delegado.
*  Se o método selecionado M é um método de extensão que é indicado por meio de um acesso de membro em uma expressão de instância, essa expressão instância determina o objeto de destino do delegado.
*  O resultado da conversão é um valor do tipo `D`, ou seja, um delegado criado recentemente que se refere ao objeto de método e de destino selecionado.
*  Observe que esse processo pode levar à criação de um delegado para um método de extensão, se o algoritmo de [invocações de método](expressions.md#method-invocations) não consegue encontrar um método de instância, mas tem êxito no processamento de invocação de `E(A)` como uma extensão invocação de método ([invocações de método de extensão](expressions.md#extension-method-invocations)). Um delegado que foi criado, portanto, captura o método de extensão, bem como seu primeiro argumento.

O exemplo a seguir demonstra conversões de grupo de método:
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

A atribuição ao `d1` converte implicitamente o grupo de métodos `F` em um valor do tipo `D1`.

A atribuição ao `d2` mostra como é possível criar um delegado para um método que tem menos derivados que tipos de parâmetro (contra variant) e uma mais derivado (covariante) tipo de retorno.

A atribuição ao `d3` mostra como não existe conversão se o método não é aplicável.

A atribuição ao `d4` mostra como o método deve ser aplicável em sua forma normal.

A atribuição ao `d5` mostra como parâmetro e tipos de retorno do método e delegado têm permissão para diferem somente para tipos de referência.

Assim como acontece com todas as outras conversões implícitas e explícitas, o operador de conversão pode ser usado para explicitamente executar uma conversão de grupo de método. Portanto, o exemplo
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
em vez disso, poderia ser escrito
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Grupos de método podem influenciar a resolução de sobrecarga e participar de inferência de tipo. Ver [membros de função](expressions.md#function-members) para obter mais detalhes.

A avaliação do tempo de execução de uma conversão de grupo de método procede da seguinte maneira:

*  Se o método selecionado em tempo de compilação é um método de instância ou é um método de extensão que pode é acessado como um método de instância, o objeto de destino do delegado é determinado da expressão de instância associado `E`:
    * A expressão de instância é avaliada. Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.
    * Se a expressão de instância é de um *reference_type*, o valor calculado pela expressão instância torna-se o objeto de destino. Se o método selecionado é um método de instância e é o objeto de destino `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.
    * Se a expressão de instância é de um *value_type*, uma operação de conversão boxing ([conversões Boxing](types.md#boxing-conversions)) é executada para converter o valor a um objeto, e esse objeto se torna o objeto de destino.
*  Caso contrário, o método selecionado faz parte de uma chamada de método estático e o objeto de destino do delegado é `null`.
*  Uma nova instância do tipo de delegado `D` é alocado. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.
*  A nova instância de delegado é inicializada com uma referência para o método que foi determinada em tempo de compilação e uma referência ao objeto de destino é calculado acima.
