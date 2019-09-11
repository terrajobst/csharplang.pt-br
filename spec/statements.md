---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876844"
---
# <a name="statements"></a>Instruções

C#fornece uma variedade de instruções. A maioria dessas instruções será familiar para os desenvolvedores que programaram em C e C++.

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

O *embedded_statement* não terminal é usado para instruções que aparecem dentro de outras instruções. O uso de *embedded_statement* em vez de *instrução* exclui o uso de instruções de declaração e instruções rotuladas nesses contextos. O exemplo
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
resulta em um erro de tempo de compilação porque `if` uma instrução requer um *embedded_statement* em vez de uma *instrução* para sua ramificação If. Se esse código fosse permitido, a variável `i` seria declarada, mas nunca poderia ser usada. No entanto, observe que, `i`colocando a declaração de em um bloco, o exemplo é válido.

## <a name="end-points-and-reachability"></a>Pontos de extremidade e acessibilidade

Cada instrução tem um ***ponto de extremidade***. Em termos intuitivos, o ponto de extremidade de uma instrução é o local que segue imediatamente a instrução. As regras de execução para instruções compostas (instruções que contêm instruções inseridas) especificam a ação que é executada quando o controle atinge o ponto de extremidade de uma instrução inserida. Por exemplo, quando o controle atinge o ponto de extremidade de uma instrução em um bloco, o controle é transferido para a próxima instrução no bloco.

Se uma instrução puder ser alcançada pela execução, a instrução será considerada ***acessível***. Por outro lado, se não houver nenhuma possibilidade de que uma instrução seja executada, a instrução será considerada ***inacessível***.

No exemplo
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
a segunda invocação de `Console.WriteLine` está inacessível porque não há possibilidade de que a instrução seja executada.

Um aviso será relatado se o compilador determinar que uma instrução está inacessível. Especificamente, não é um erro para que uma instrução fique inacessível.

Para determinar se uma determinada instrução ou ponto de extremidade pode ser acessado, o compilador executa a análise de fluxo de acordo com as regras de acessibilidade definidas para cada instrução. A análise de fluxo leva em conta os valores de expressões constantes ([expressões constantes](expressions.md#constant-expressions)) que controlam o comportamento de instruções, mas os valores possíveis de expressões não constantes não são considerados. Em outras palavras, para fins de análise de fluxo de controle, uma expressão não constante de um determinado tipo é considerada com qualquer valor possível desse tipo.

No exemplo
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
a expressão booliana da `if` instrução é uma expressão constante porque ambos os operandos `==` do operador são constantes. Como a expressão constante é avaliada em tempo de compilação, produzindo `false`o valor `Console.WriteLine` , a invocação é considerada inacessível. No entanto `i` , se for alterado para ser uma variável local
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
a `Console.WriteLine` invocação é considerada acessível, apesar de, na realidade, nunca será executada.

O *bloco* de um membro de função é sempre considerado acessível. Ao avaliar sucessivamente as regras de acessibilidade de cada instrução em um bloco, a acessibilidade de qualquer instrução específica pode ser determinada.

No exemplo
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
a acessibilidade do segundo `Console.WriteLine` é determinada da seguinte maneira:

*  A primeira `Console.WriteLine` instrução `F` de expressão pode ser acessada porque o bloco do método está acessível.
*  O ponto de extremidade da primeira `Console.WriteLine` instrução de expressão pode ser acessado porque essa instrução pode ser acessada.
*  A `if` instrução pode ser acessada porque o ponto final `Console.WriteLine` da primeira instrução de expressão está acessível.
*  A segunda `Console.WriteLine` instrução `if` de expressão está acessível porque a expressão booliana da instrução não tem o valor `false`constante.

Há duas situações em que é um erro de tempo de compilação para o ponto de extremidade de uma instrução ser acessível:

*  Como a `switch` instrução não permite que uma seção de comutador "passe" para a próxima seção switch, é um erro de tempo de compilação para o ponto final da lista de instruções de uma seção de switch ser acessível. Se esse erro ocorrer, normalmente é uma indicação de que uma `break` instrução está ausente.
*  É um erro de tempo de compilação para o ponto de extremidade do bloco de um membro de função que computa um valor a ser acessado. Se esse erro ocorrer, normalmente é uma indicação de que uma `return` instrução está ausente.

## <a name="blocks"></a>Blocos

Um *bloco* permite a produção de várias instruções em contextos nos quais uma única instrução é permitida.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Um *bloco* consiste em um *statement_list* opcional ([listas de instruções](statements.md#statement-lists)), entre chaves. Se a lista de instruções for omitida, o bloco será considerado vazio.

Um bloco pode conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)). O escopo de uma variável local ou constante declarada em um bloco é o bloco.

Um bloco é executado da seguinte maneira:

*  Se o bloco estiver vazio, o controle será transferido para o ponto de extremidade do bloco.
*  Se o bloco não estiver vazio, o controle será transferido para a lista de instruções. Quando e se o controle atingir o ponto de extremidade da lista de instruções, o controle será transferido para o ponto de extremidade do bloco.

A lista de instruções de um bloco pode ser acessada se o próprio bloco estiver acessível.

O ponto de extremidade de um bloco pode ser acessado se o bloco estiver vazio ou se o ponto final da lista de instruções estiver acessível.

Um *bloco* que contém uma ou mais `yield` instruções ([a instrução yield](statements.md#the-yield-statement)) é chamado de bloco de iterador. Os blocos de iteradores são usados para implementar membros de função como iteradores ([iteradores](classes.md#iterators)). Algumas restrições adicionais se aplicam a blocos de iteradores:

*  É um erro de tempo de compilação para que `return` uma instrução apareça em um bloco de iterador `yield return` (mas as instruções são permitidas).
*  É um erro de tempo de compilação para um bloco de iterador conter um contexto não seguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)). Um bloco do iterador sempre define um contexto seguro, mesmo quando sua declaração está aninhada em um contexto sem segurança.

### <a name="statement-lists"></a>Listas de instruções

Uma ***lista*** de instruções consiste em uma ou mais instruções escritas em sequência. As listas de instruções *ocorrem em blocos*s ([blocos](statements.md#blocks)) e em *switch_block*s ([a instrução switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Uma lista de instruções é executada transferindo o controle para a primeira instrução. Quando e se o controle atingir o ponto de extremidade de uma instrução, o controle será transferido para a próxima instrução. Quando e se o controle atingir o ponto de extremidade da última instrução, o controle será transferido para o ponto de extremidade da lista de instruções.

Uma instrução em uma lista de instruções pode ser acessada se pelo menos uma das seguintes opções for verdadeira:

*  A instrução é a primeira instrução e a própria lista de instruções pode ser acessada.
*  O ponto de extremidade da instrução anterior pode ser acessado.
*  A instrução é uma instrução rotulada e o rótulo é referenciado por uma `goto` instrução alcançável.

O ponto de extremidade de uma lista de instruções pode ser acessado se o ponto final da última instrução na lista estiver acessível.

## <a name="the-empty-statement"></a>A instrução vazia

Um *empty_statement* não faz nada.

```antlr
empty_statement
    : ';'
    ;
```

Uma instrução vazia é usada quando não há nenhuma operação a ser executada em um contexto em que uma instrução é necessária.

A execução de uma instrução vazia simplesmente transfere o controle para o ponto de extremidade da instrução. Assim, o ponto de extremidade de uma instrução vazia estará acessível se a instrução vazia puder ser acessada.

Uma instrução vazia pode ser usada ao gravar uma `while` instrução com um corpo nulo:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Além disso, uma instrução vazia pode ser usada para declarar um rótulo logo antes do fechamento`}`"" de um bloco:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Instruções rotuladas

Um *labeled_statement* permite que uma instrução seja prefixada por um rótulo. Instruções rotuladas são permitidas em blocos, mas não são permitidas como instruções inseridas.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Uma instrução rotulada declara um rótulo com o nome fornecido pelo *identificador*. O escopo de um rótulo é o bloco inteiro no qual o rótulo é declarado, incluindo qualquer bloco aninhado. É um erro de tempo de compilação para dois rótulos com o mesmo nome para ter escopos sobrepostos.

Um rótulo pode ser referenciado `goto` de instruções ([a instrução goto](statements.md#the-goto-statement)) dentro do escopo do rótulo. Isso significa que `goto` as instruções podem transferir o controle dentro de blocos e fora de blocos, mas nunca em blocos.

Os rótulos têm seu próprio espaço de declaração e não interferem em outros identificadores. O exemplo
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
é válido e usa o nome `x` como um parâmetro e um rótulo.

A execução de uma instrução rotulada corresponde exatamente à execução da instrução após o rótulo.

Além da acessibilidade fornecida pelo fluxo de controle normal, uma instrução rotulada pode ser acessada se o rótulo for referenciado por uma `goto` instrução alcançável. Exception Se uma `goto` instrução estiver dentro de `try` um que inclui `finally` um bloco, e a instrução rotulada estiver fora `try` `finally` do, e o ponto final do bloco estiver inacessível, a instrução rotulada não será acessível de Essa `goto` instrução.)

## <a name="declaration-statements"></a>Instruções de declaração

Um *declaration_statement* declara uma variável ou constante local. Instruções de declaração são permitidas em blocos, mas não são permitidas como instruções inseridas.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Declarações de variáveis locais

Um *local_variable_declaration* declara uma ou mais variáveis locais.

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

O *local_variable_type* de um *local_variable_declaration* especifica diretamente o tipo das variáveis introduzidas pela declaração ou indica com o identificador `var` que o tipo deve ser inferido com base em um inicializador. O tipo é seguido por uma lista de *local_variable_declarator*s, cada um dos quais introduz uma nova variável. Um *local_variable_declarator* consiste em um *identificador* que nomeia a variável, opcionalmente seguido por um token`=`"" e um *local_variable_initializer* que fornece o valor inicial da variável.

No contexto de uma declaração de variável local, o identificador var age como uma palavra-chave contextual ([palavras-chave](lexical-structure.md#keywords)). Quando o *local_variable_type* é especificado como `var` e nenhum tipo chamado `var` está no escopo, a declaração é uma ***declaração de variável local digitada implicitamente***, cujo tipo é inferido do tipo do inicializador associado expressão. As declarações de variáveis locais digitadas implicitamente estão sujeitas às seguintes restrições:

*  O *local_variable_declaration* não pode incluir vários *local_variable_declarator*s.
*  O *local_variable_declarator* deve incluir um *local_variable_initializer*.
*  O *local_variable_initializer* deve ser uma *expressão*.
*  A *expressão* do inicializador deve ter um tipo de tempo de compilação.
*  A *expressão* do inicializador não pode se referir à própria variável declarada

Veja a seguir exemplos de declarações de variáveis locais incorretas de tipo implícito:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

O valor de uma variável local é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)) e o valor de uma variável local é modificado usando uma *atribuição* ([operadores de atribuição](expressions.md#assignment-operators)). Uma variável local deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) em cada local em que seu valor é obtido.

O escopo de uma variável local declarada em um *local_variable_declaration* é o bloco no qual a declaração ocorre. É um erro fazer referência a uma variável local em uma posição textual que precede o *local_variable_declarator* da variável local. Dentro do escopo de uma variável local, é um erro de tempo de compilação para declarar outra variável local ou constante com o mesmo nome.

Uma declaração de variável local que declara várias variáveis é equivalente a várias declarações de variáveis únicas com o mesmo tipo. Além disso, um inicializador de variável em uma declaração de variável local corresponde exatamente a uma instrução de atribuição que é inserida imediatamente após a declaração.

O exemplo
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
corresponde exatamente a
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

Em uma declaração de variável local digitada implicitamente, o tipo da variável local que está sendo declarada é usado como sendo o mesmo que o tipo da expressão usada para inicializar a variável. Por exemplo:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

As declarações de variável local digitadas implicitamente acima são exatamente equivalentes às seguintes declarações tipadas explicitamente:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Declarações de constantes locais

Um *local_constant_declaration* declara uma ou mais constantes locais.

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

O *tipo* de um *local_constant_declaration* especifica o tipo das constantes introduzidas pela declaração. O tipo é seguido por uma lista de *constant_declarator*s, cada um dos quais introduz uma nova constante. Um *constant_declarator* consiste em um *identificador* que nomeia a constante, seguido por um token`=`"", seguido por um *constant_expression* ([expressões constantes](expressions.md#constant-expressions)) que fornece o valor da constante.

O *tipo* e *constant_expression* de uma declaração de constante local devem seguir as mesmas regras que as de uma declaração de membro constante ([constantes](classes.md#constants)).

O valor de uma constante local é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)).

O escopo de uma constante local é o bloco no qual a declaração ocorre. É um erro fazer referência a uma constante local em uma posição textual que precede seu *constant_declarator*. Dentro do escopo de uma constante local, é um erro de tempo de compilação para declarar outra variável local ou constante com o mesmo nome.

Uma declaração de constante local que declara várias constantes é equivalente a várias declarações de constantes únicas com o mesmo tipo.

## <a name="expression-statements"></a>Instruções de expressão

Um *expression_statement* avalia uma determinada expressão. O valor calculado pela expressão, se houver, é Descartado.

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

Nem todas as expressões são permitidas como instruções. Em particular, as `x + y` expressões como e `x == 1` que meramente computam um valor (que será Descartado) não são permitidas como instruções.

A execução de um *expression_statement* avalia a expressão contida e, em seguida, transfere o controle para o ponto de extremidade do *expression_statement*. O ponto de extremidade de um *expression_statement* será acessível se o *expression_statement* estiver acessível.

## <a name="selection-statements"></a>Instruções de seleção

Instruções de seleção selecione um número de possíveis instruções para execução com base no valor de alguma expressão.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>A instrução If

A `if` instrução seleciona uma instrução para execução com base no valor de uma expressão booliana.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Uma `else` parte é associada ao precedentes `if` lexicalmente mais próximo que é permitido pela sintaxe. Portanto, uma `if` instrução do formulário
```csharp
if (x) if (y) F(); else G();
```
equivale a
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

Uma `if` instrução é executada da seguinte maneira:

*  O *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)) é avaliado.
*  Se a expressão booliana produz `true`, o controle é transferido para a primeira instrução inserida. Quando e se o controle atingir o ponto final dessa instrução, o controle será transferido para o ponto de extremidade `if` da instrução.
*  Se a expressão booliana produz `false` e se uma `else` parte estiver presente, o controle será transferido para a segunda instrução inserida. Quando e se o controle atingir o ponto final dessa instrução, o controle será transferido para o ponto de extremidade `if` da instrução.
*  Se a expressão booliana produz `false` e se uma `else` parte não estiver presente, o controle será transferido para `if` o ponto final da instrução.

A primeira instrução inserida de `if` uma instrução pode ser acessada se a `if` instrução estiver acessível e a expressão booliana não `false`tiver o valor constante.

A segunda instrução inserida de `if` uma instrução, se presente, estará acessível se `if` a instrução estiver acessível e a expressão booliana não tiver o valor `true`constante.

O ponto de extremidade de `if` uma instrução pode ser acessado se o ponto de extremidade de pelo menos uma de suas instruções inseridas estiver acessível. Além disso, o ponto de extremidade de `if` uma instrução sem `else` nenhuma parte será acessível se `if` a instrução estiver acessível e a expressão booliana não tiver o valor `true`constante.

### <a name="the-switch-statement"></a>A instrução switch

A instrução switch seleciona a execução de uma lista de instruções com um rótulo de comutador associado que corresponde ao valor da expressão switch.

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

Um *switch_statement* consiste na palavra- `switch`chave, seguida por uma expressão entre parênteses (chamada de expressão switch), seguida por um *switch_block*. O *switch_block* consiste em zero ou mais *switch_section*s, entre chaves. Cada *switch_section* consiste em um ou mais *switch_label*, seguido por um *statement_list* ([listas de instruções](statements.md#statement-lists)).

O ***tipo de controle*** de uma `switch` instrução é estabelecido pela expressão switch.

*  Se o `sbyte`tipo da expressão switch for, `uint` `ushort` `int` ,,`ulong`,,,, ,`bool` ,`string`, ou`char`um `short` `byte` `long`  *enum_type*, ou se for o tipo anulável correspondente a um desses tipos, esse é o tipo regulador da `switch` instrução.
*  Caso contrário, exatamente uma conversão implícita definida pelo usuário[(conversões definidas pelo usuário](conversions.md#user-defined-conversions)) deve existir a partir do tipo da expressão switch para um dos seguintes tipos de controle possíveis: `sbyte`, `byte`,, `ushort` `short` `int` ,,`uint`,,,, ou, um tipo anulável correspondente a um desses tipos. `long` `ulong` `char` `string`
*  Caso contrário, se nenhuma conversão implícita existir, ou se houver mais de uma conversão implícita, ocorrerá um erro em tempo de compilação.

A expressão constante de cada `case` rótulo deve indicar um valor que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo regulador da `switch` instrução. Um erro de tempo de compilação ocorre se dois ou `case` mais rótulos na mesma `switch` instrução especificarem o mesmo valor de constante.

Pode haver no máximo um `default` rótulo em uma instrução switch.

Uma `switch` instrução é executada da seguinte maneira:

*  A expressão switch é avaliada e convertida no tipo regulador.
*  Se uma das constantes especificadas em um `case` rótulo na mesma `switch` instrução for igual ao valor da expressão switch, o controle será transferido para a lista de instruções após o rótulo correspondente `case` .
*  Se nenhuma `case` das constantes especificadas em rótulos na mesma `switch` instrução for igual ao valor da expressão switch e, se um `default` rótulo estiver presente, o controle será transferido para a lista de instruções após o `default` chamada.
*  Se nenhuma `case` das constantes especificadas em rótulos na mesma `switch` instrução for igual ao valor da expressão switch e, se nenhum `default` rótulo estiver presente, o controle `switch` será transferido para o ponto final da instrução.

Se o ponto de extremidade da lista de instruções de uma seção de comutador estiver acessível, ocorrerá um erro em tempo de compilação. Isso é conhecido como a regra "não cair". O exemplo
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
é válido porque nenhuma seção de opção tem um ponto de extremidade acessível. Ao contrário de C++C e, a execução de uma seção switch não é permitida para "passar" para a próxima seção switch e o exemplo
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
resulta em um erro de tempo de compilação. Quando a execução de uma seção switch é seguida pela execução de outra seção switch, uma instrução or `goto case` `goto default` explícita deve ser usada:
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

Vários rótulos são permitidos em um *switch_section*. O exemplo
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
é válido. O exemplo não viola a regra "no enquadramento" porque os rótulos `case 2:` e `default:` fazem parte do mesmo *switch_section*.

A regra "no enquadramento" impede uma classe comum de bugs que ocorrem em C C++ e `break` quando as instruções são omitidas acidentalmente. Além disso, devido a essa regra, as seções switch de uma `switch` instrução podem ser reorganizadas arbitrariamente sem afetar o comportamento da instrução. Por exemplo, as seções da `switch` instrução acima podem ser revertidas sem afetar o comportamento da instrução:
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

A lista de instruções de uma seção de alternância normalmente termina `break`em `goto case`uma instrução `goto default` , ou, mas qualquer construção que renderiza o ponto final da lista de instrução inacessível é permitida. Por exemplo, uma `while` instrução controlada pela expressão `true` booliana é conhecida como nunca atingir seu ponto de extremidade. Da mesma forma `throw` , `return` uma instrução or sempre transfere o controle em outro lugar e nunca atinge seu ponto de extremidade. Portanto, o exemplo a seguir é válido:
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

O tipo regulador de uma `switch` instrução pode ser o tipo `string`. Por exemplo:
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

Como os operadores de igualdade de cadeia de caracteres (operadores de `switch` igualdade de cadeia de[caracteres](expressions.md#string-equality-operators)), a instrução diferencia maiúsculas de minúsculas e executará uma determinada seção de comutador somente se a cadeia de caracteres de expressão switch corresponder exatamente a uma constante `case`

Quando o tipo regulador de uma `switch` instrução é `string`, o valor `null` é permitido como uma constante de rótulo de caso.

Os *statement_list*s de um *switch_block* podem conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)). O escopo de uma variável local ou constante declarada em um bloco switch é o bloco switch.

A lista de instruções de uma determinada seção de comutador pode `switch` ser acessada se a instrução estiver acessível e pelo menos uma das seguintes opções for verdadeira:

*  A expressão switch é um valor não constante.
*  A expressão switch é um valor constante que corresponde a `case` um rótulo na seção switch.
*  A expressão switch é um valor constante que não corresponde a `case` nenhum rótulo e a seção switch contém o `default` rótulo.
*  Um rótulo de switch da seção switch é referenciado por uma `goto case` instrução `goto default` ou alcançável.

O ponto de extremidade de `switch` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:

*  A `switch` instrução contém uma instrução `break` alcançável que sai da `switch` instrução.
*  A `switch` instrução está acessível, a expressão switch é um valor não constante e nenhum `default` rótulo está presente.
*  A `switch` instrução está acessível, a expressão switch é um valor constante que não corresponde a `case` nenhum rótulo e nenhum `default` rótulo está presente.

## <a name="iteration-statements"></a>Instruções de iteração

Instruções de iteração executa repetidamente uma instrução inserida.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>A instrução while

A `while` instrução Executa condicionalmente uma instrução inserida zero ou mais vezes.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Uma `while` instrução é executada da seguinte maneira:

*  O *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)) é avaliado.
*  Se a expressão booliana produz `true`, o controle é transferido para a instrução inserida. Quando e se o controle atingir o ponto de extremidade da instrução inserida (possivelmente da execução `continue` de uma instrução), o controle será transferido para o `while` início da instrução.
*  Se a expressão booliana produz `false`, o controle é transferido para o ponto final `while` da instrução.

Dentro da instrução inserida de `while` uma instrução, `break` uma instrução ([a instrução break](statements.md#the-break-statement)) pode ser usada para transferir o `while` controle para o ponto final da instrução (encerrando assim a iteração da instrução inserida) e um `continue` a instrução ([a instrução Continue](statements.md#the-continue-statement)) pode ser usada para transferir o controle para o ponto final da instrução inserida (assim, executando outra `while` iteração da instrução).

A instrução inserida de `while` uma instrução pode ser acessada se a `while` instrução estiver acessível e a expressão booliana não `false`tiver o valor constante.

O ponto de extremidade de `while` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:

*  A `while` instrução contém uma instrução `break` alcançável que sai da `while` instrução.
*  A `while` instrução está acessível e a expressão booliana não tem o valor `true`constante.

### <a name="the-do-statement"></a>A instrução do

A `do` instrução Executa condicionalmente uma instrução inserida uma ou mais vezes.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Uma `do` instrução é executada da seguinte maneira:

*  O controle é transferido para a instrução inserida.
*  Quando e se o controle atingir o ponto de extremidade da instrução inserida (possivelmente da execução `continue` de uma instrução), o *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)) será avaliado. Se a expressão booliana produz `true`, o controle é transferido para o início `do` da instrução. Caso contrário, o controle será transferido para o ponto de `do` extremidade da instrução.

Dentro da instrução inserida de `do` uma instrução, `break` uma instrução ([a instrução break](statements.md#the-break-statement)) pode ser usada para transferir o `do` controle para o ponto final da instrução (encerrando assim a iteração da instrução inserida) e um `continue` a instrução ([a instrução Continue](statements.md#the-continue-statement)) pode ser usada para transferir o controle para o ponto final da instrução inserida.

A instrução inserida de `do` uma instrução pode ser acessada se a `do` instrução estiver acessível.

O ponto de extremidade de `do` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:

*  A `do` instrução contém uma instrução `break` alcançável que sai da `do` instrução.
*  O ponto de extremidade da instrução inserida pode ser acessado e a expressão booliana não `true`tem o valor constante.

### <a name="the-for-statement"></a>A instrução for

A `for` instrução avalia uma sequência de expressões de inicialização e, em seguida, enquanto uma condição é verdadeira, executa repetidamente uma instrução inserida e avalia uma sequência de expressões de iteração.

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

O *for_initializer*, se presente, consiste em um *local_variable_declaration* ([declarações de variável local](statements.md#local-variable-declarations)) ou uma lista de *statement_expression*s ([instruções de expressão](statements.md#expression-statements)) separadas por vírgulas. O escopo de uma variável local declarada por um *for_initializer* começa no *local_variable_declarator* para a variável e se estende ao final da instrução inserida. O escopo inclui o *for_condition* e o *for_iterator*.

O *for_condition*, se presente, deve ser um *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)).

O *for_iterator*, se presente, consiste em uma lista de *statement_expression*s ([instruções de expressão](statements.md#expression-statements)) separadas por vírgulas.

Uma instrução for é executada da seguinte maneira:

*  Se um *for_initializer* estiver presente, os inicializadores de variável ou as expressões de instrução serão executadas na ordem em que são gravadas. Esta etapa é executada apenas uma vez.
*  Se um *for_condition* estiver presente, ele será avaliado.
*  Se o *for_condition* não estiver presente ou se a avaliação resultar `true`, o controle será transferido para a instrução inserida. Quando e se o controle atingir o ponto de extremidade da instrução inserida (possivelmente da execução `continue` de uma instrução), as expressões de *for_iterator*, se houver, serão avaliadas em sequência e outra iteração será executada, começando com avaliação do *for_condition* na etapa anterior.
*  Se o *for_condition* estiver presente e a avaliação resultar `false`, o controle será transferido para o ponto de `for` extremidade da instrução.

Dentro da instrução inserida de `for` uma instrução, `break` uma instrução ([a instrução break](statements.md#the-break-statement)) pode ser usada para transferir o `for` controle para o ponto final da instrução (encerrando assim a iteração da instrução inserida) e um `continue` a instrução ([a instrução Continue](statements.md#the-continue-statement)) pode ser usada para transferir o controle para o ponto final da instrução inserida (executando, assim, a *for_iterator* e `for` executando outra iteração da instrução, começando com o *for_condition*).

A instrução inserida de `for` uma instrução pode ser acessada se uma das seguintes opções for verdadeira:

*  A `for` instrução está acessível e nenhum *for_condition* está presente.
*  A `for` instrução está acessível e um *for_condition* está presente e não tem o valor `false`constante.

O ponto de extremidade de `for` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:

*  A `for` instrução contém uma instrução `break` alcançável que sai da `for` instrução.
*  A `for` instrução está acessível e um *for_condition* está presente e não tem o valor `true`constante.

### <a name="the-foreach-statement"></a>A instrução foreach

A `foreach` instrução enumera os elementos de uma coleção, executando uma instrução incorporada para cada elemento da coleção.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

O *tipo* e o *identificador* de `foreach` uma instrução declaram a ***variável de iteração*** da instrução. Se o `var` identificador for fornecido como *local_variable_type*e nenhum tipo chamado `var` estiver no escopo, a variável de iteração será considerada uma variável de ***iteração digitada implicitamente***e seu tipo será usado para ser o tipo de elemento do `foreach` , conforme especificado abaixo. A variável de iteração corresponde a uma variável local somente leitura com um escopo que se estende pela instrução incorporada. Durante a execução de `foreach` uma instrução, a variável de iteração representa o elemento de coleção para o qual uma iteração está sendo executada no momento. Ocorrerá um erro de tempo de compilação se a instrução inserida tentar modificar a variável de iteração ( `++` por `--` meio de atribuição ou os operadores e) ou `ref` passar `out` a variável de iteração como um parâmetro ou.

A seguir, para fins de brevidade `IEnumerable`, `IEnumerator` `IEnumerable<T>` , e `IEnumerator<T>` consulte os tipos correspondentes nos namespaces `System.Collections` e `System.Collections.Generic`.

O processamento em tempo de compilação de uma instrução foreach primeiro determina o ***tipo de coleção***, o tipo de ***enumerador*** e o ***tipo de elemento*** da expressão. Essa determinação continua da seguinte maneira:

*  Se o tipo `X` de *expressão* for um tipo de matriz, haverá uma conversão de referência implícita `X` de para `IEnumerable` a interface ( `System.Array` desde que implementa essa interface). O ***tipo de coleção*** é `IEnumerable` a interface, ***o tipo de enumerador*** é `IEnumerator` a interface e o tipo de ***elemento*** é o tipo de `X`elemento do tipo de matriz.
*  Se o tipo `X` de *expressão* for `dynamic` , haverá uma conversão implícita de *expression* para a `IEnumerable` interface ([conversões dinâmicas implícitas](conversions.md#implicit-dynamic-conversions)). O ***tipo de coleção*** é `IEnumerable` a interface e o ***tipo de enumerador*** é a `IEnumerator` interface. Se o `var` identificador for fornecido como o *local_variable_type* , o ***tipo*** de elemento `dynamic`será, caso contrário `object`, será.
*  Caso contrário, determine se o `X` tipo tem um `GetEnumerator` método apropriado:
   * Executar pesquisa de membro no tipo `X` com identificador `GetEnumerator` e nenhum argumento de tipo. Se a pesquisa de membro não produzir uma correspondência ou se gerar uma ambiguidade ou produzir uma correspondência que não seja um grupo de métodos, verifique se há uma interface enumerável, conforme descrito abaixo. É recomendável que um aviso seja emitido se a pesquisa de membros produzir qualquer coisa, exceto um grupo de métodos ou nenhuma correspondência.
   * Execute a resolução de sobrecarga usando o grupo de métodos resultante e uma lista de argumentos vazia. Se a resolução de sobrecarga não resultar em métodos aplicáveis, resultar em uma ambiguidade ou resultar em um único método melhor, mas o método for estático ou não público, verifique se há uma interface enumerável, conforme descrito abaixo. É recomendável que um aviso seja emitido se a resolução de sobrecarga produzir algo, exceto um método de instância pública não ambígua, ou nenhum dos métodos aplicáveis.
   * Se o tipo `E` `GetEnumerator` de retorno do método não for uma classe, struct ou tipo de interface, um erro será produzido e nenhuma etapa adicional será executada.
   * A pesquisa de membros é `E` executada em com `Current` o identificador e nenhum argumento de tipo. Se a pesquisa de membro não produzir nenhuma correspondência, o resultado é um erro ou o resultado é qualquer coisa, exceto uma propriedade de instância pública que permite a leitura, um erro é produzido e nenhuma etapa adicional é executada.
   * A pesquisa de membros é `E` executada em com `MoveNext` o identificador e nenhum argumento de tipo. Se a pesquisa de membro não produzir nenhuma correspondência, o resultado é um erro ou o resultado é qualquer coisa, exceto um grupo de métodos, um erro é produzido e nenhuma etapa adicional é executada.
   * A resolução de sobrecarga é executada no grupo de métodos com uma lista de argumentos vazia. Se a resolução de sobrecarga não resultar em métodos aplicáveis, resultar em uma ambiguidade ou resultar em um único método melhor, mas esse método for estático ou não público, ou seu tipo de retorno não `bool`for, um erro será produzido e nenhuma etapa adicional será executada.
   * O ***tipo*** de coleção `X`é, o tipo de `E` ***enumerador*** é, `Current` e o ***tipo de elemento*** é o tipo da propriedade.

*  Caso contrário, verifique se há uma interface enumerável:
   * Se entre todos os tipos `Ti` para os quais há uma conversão implícita de `X` para `IEnumerable<Ti>`, há um tipo `T` exclusivo, que `T` não `dynamic` é e para todos os outros `Ti` , há um conversão implícita de `IEnumerable<T>` para `IEnumerable<Ti>`, o ***tipo de coleção*** é a interface `IEnumerable<T>`, o ***tipo de enumerador*** é `IEnumerator<T>`a interface e o ***tipo de elemento*** é `T`.
   * Caso contrário, se houver mais de um desses tipos `T`, um erro será produzido e nenhuma etapa adicional será executada.
   * Caso contrário, se houver uma conversão implícita de `X` `System.Collections.IEnumerable` na interface, o tipo de ***coleção*** será essa interface, o ***tipo de enumerador*** será a `System.Collections.IEnumerator`interface e o ***tipo de elemento*** será `object`.
   * Caso contrário, um erro é produzido e nenhuma etapa adicional é executada.

As etapas acima, se forem bem-sucedidas, produzirão um tipo `C`de coleção, tipo `E` de enumerador e `T`tipo de elemento de forma inequívoca. Uma instrução foreach do formulário
```csharp
foreach (V v in x) embedded_statement
```
é então expandido para:
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

A variável `e` não é visível ou acessível à expressão `x` ou à instrução incorporada ou a qualquer outro código-fonte do programa. A variável `v` é somente leitura na instrução incorporada. Se não houver uma conversão explícita ([conversões explícitas](conversions.md#explicit-conversions)) de `T` (o tipo de elemento) para `V` (o *local_variable_type* na instrução foreach), um erro será produzido e nenhuma outra etapa será executada. Se `x` tiver o valor `null`, um `System.NullReferenceException` será lançado em tempo de execução.

Uma implementação tem permissão para implementar uma determinada instrução foreach de forma diferente, por exemplo, por motivos de desempenho, desde que o comportamento seja consistente com a expansão acima.

O posicionamento de `v` dentro do loop while é importante para a forma como ele é capturado por qualquer função anônima que ocorra no *embedded_statement*.

Por exemplo:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Se `v` foi declarado fora do loop while, ele seria compartilhado entre todas as iterações e seu valor após o loop for seria o valor final, `13`, que é o `f` que a invocação imprimiria. Em vez disso, como cada iteração tem `v`sua própria variável, aquela `f` capturada pela primeira iteração continuará a manter `7`o valor, que é o que será impresso. (Observação: versões anteriores do C# declaradas `v` fora do loop While.)

O corpo do bloco finally é construído de acordo com as seguintes etapas:

*  Se houver uma conversão implícita do `E` `System.IDisposable` na interface, então
   *  Se `E` for um tipo de valor não anulável, a cláusula finally será expandida para o equivalente semântico de:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  Caso contrário, a cláusula finally será expandida para o equivalente semântico de:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   Exceto que se `E` for um tipo de valor ou um parâmetro de tipo instanciado para um tipo de valor, a conversão `e` de `System.IDisposable` para não fará com que a Boxing ocorra.

*  Caso contrário, `E` se for um tipo lacrado, a cláusula finally será expandida para um bloco vazio:

   ```csharp
   finally {
   }
   ```

*  Caso contrário, a cláusula finally será expandida para:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   A variável `d` local não é visível ou acessível a nenhum código de usuário. Em particular, ele não entra em conflito com nenhuma outra variável cujo escopo inclui o bloco finally.

A ordem na qual `foreach` percorre os elementos de uma matriz é a seguinte: Para elementos de matrizes unidimensionais são atravessados em ordem de índice crescente, `0` começando com index e `Length - 1`terminando com index. Para matrizes multidimensionais, os elementos são percorridos de forma que os índices da dimensão mais à direita sejam aumentados primeiro, depois a dimensão à esquerda e assim por diante à esquerda.

O exemplo a seguir imprime cada valor em uma matriz bidimensional, na ordem do elemento:
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
A saída produzida é a seguinte:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

No exemplo
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
o tipo de `n` é inferido `int`como, o tipo de elemento de `numbers`.

## <a name="jump-statements"></a>Instruções de atalho

As instruções de salto transferem o controle incondicionalmente.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

O local para o qual uma instrução de salto transfere o controle é chamado de ***destino*** da instrução de salto.

Quando uma instrução de salto ocorre dentro de um bloco e o destino dessa instrução de salto está fora desse bloco, a instrução de salto é chamada para ***sair*** do bloco. Embora uma instrução de salto possa transferir o controle para fora de um bloco, ela nunca pode transferir o controle para um bloco.

A execução de instruções de salto é complicada pela presença de `try` instruções intermediárias. Na ausência de tais `try` instruções, uma instrução de salto transfere incondicionalmente o controle da instrução de salto para seu destino. Na presença dessas `try` instruções intermediárias, a execução é mais complexa. Se a instrução de salto sair de um ou `try` mais blocos com `finally` blocos associados, o controle será transferido inicialmente `finally` para o bloco da `try` instrução mais interna. Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição. Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.

No exemplo
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
os `finally` blocos associados a duas `try` instruções são executados antes de o controle ser transferido para o destino da instrução de salto.

A saída produzida é a seguinte:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>A instrução break

A `break` instrução sai da instrução `while` `switch`, `do`,, ou`foreach` delimitadora mais próxima. `for`

```antlr
break_statement
    : 'break' ';'
    ;
```

O destino de uma `break` instrução é o ponto final da instrução, `do` `while`,, `for`ou `switch` `foreach` delimitadora mais próxima. Se uma `break` instrução não estiver entre uma `switch`instrução, `while` `do`,, `for`ou `foreach` , ocorrerá um erro de tempo de compilação.

Quando várias `switch`instruções `while`, `do`, ,`for` ou`foreach` são aninhadas umas nas outras, uma `break` instrução aplica-se somente à instrução mais interna. Para transferir o controle entre vários níveis de aninhamento `goto` , uma instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usada.

Uma `break` instrução não pode sair `finally` de um bloco ([a instrução try](statements.md#the-try-statement)). Quando uma `break` instrução ocorre dentro de `finally` um bloco `break` , o destino da instrução deve estar dentro do mesmo `finally` bloco; caso contrário, ocorrerá um erro em tempo de compilação.

Uma `break` instrução é executada da seguinte maneira:

*  Se a `break` instrução sair de um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna. Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição. Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.
*  O controle é transferido para o destino da `break` instrução.

Como uma `break` instrução transfere incondicionalmente o controle em outro lugar, o `break` ponto de extremidade de uma instrução nunca é acessível.

### <a name="the-continue-statement"></a>A instrução Continue

A `continue` instrução inicia uma nova iteração da instrução,, ou `while` `do` `foreach` delimitadora `for`mais próxima.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

O destino de uma `continue` instrução é o ponto final da instrução inserida da instrução, `do`, `for`ou `foreach` delimitadora `while`mais próxima. Se uma `continue` instrução não estiver delimitada por `while`uma `do`instrução `for`,, `foreach` ou, ocorrerá um erro em tempo de compilação.

Quando várias `while`instruções `do`, `for`, `continue` ou `foreach` são aninhadas umas nas outras, uma instrução aplica-se somente à instrução mais interna. Para transferir o controle entre vários níveis de aninhamento `goto` , uma instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usada.

Uma `continue` instrução não pode sair `finally` de um bloco ([a instrução try](statements.md#the-try-statement)). Quando uma `continue` instrução ocorre dentro de `finally` um bloco `continue` , o destino da instrução deve estar dentro do mesmo `finally` bloco; caso contrário, ocorrerá um erro em tempo de compilação.

Uma `continue` instrução é executada da seguinte maneira:

*  Se a `continue` instrução sair de um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna. Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição. Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.
*  O controle é transferido para o destino da `continue` instrução.

Como uma `continue` instrução transfere incondicionalmente o controle em outro lugar, o `continue` ponto de extremidade de uma instrução nunca é acessível.

### <a name="the-goto-statement"></a>A instrução goto

A `goto` instrução transfere o controle para uma instrução que é marcada por um rótulo.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

O destino de uma `goto` instrução de *identificador* é a instrução rotulada com o rótulo fornecido. Se um rótulo com o nome fornecido não existir no membro da função atual ou se a `goto` instrução não estiver dentro do escopo do rótulo, ocorrerá um erro em tempo de compilação. Essa regra permite o uso de uma `goto` instrução para transferir o controle de um escopo aninhado, mas não para um escopo aninhado. No exemplo
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
uma `goto` instrução é usada para transferir o controle de um escopo aninhado.

O destino de uma `goto case` instrução é a lista de instruções na instrução de circunscrição `switch` imediatamente ([a instrução switch](statements.md#the-switch-statement)), que contém `case` um rótulo com o valor constante fornecido. Se a `goto case` instrução não estiver delimitada por `switch` uma instrução, se *constant_expression* não for implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo regulador da instrução de circunscrição `switch` mais próxima, ou se a instrução de circunscrição `switch` mais próxima não contém um `case` rótulo com o valor constante fornecido, ocorre um erro de tempo de compilação.

O destino de uma `goto default` instrução é a lista de instruções na instrução de circunscrição `switch` imediatamente ([a instrução switch](statements.md#the-switch-statement)), que contém `default` um rótulo. Se a `goto default` instrução não estiver delimitada por `switch` uma instrução ou se a instrução delimitadora `switch` mais próxima não contiver `default` um rótulo, ocorrerá um erro em tempo de compilação.

Uma `goto` instrução não pode sair `finally` de um bloco ([a instrução try](statements.md#the-try-statement)). Quando uma `goto` instrução ocorre dentro de `finally` um bloco `goto` , o destino da instrução deve estar dentro do mesmo `finally` bloco ou ocorre um erro de tempo de compilação.

Uma `goto` instrução é executada da seguinte maneira:

*  Se a `goto` instrução sair de um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna. Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição. Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.
*  O controle é transferido para o destino da `goto` instrução.

Como uma `goto` instrução transfere incondicionalmente o controle em outro lugar, o `goto` ponto de extremidade de uma instrução nunca é acessível.

### <a name="the-return-statement"></a>A instrução return

A `return` instrução retorna o controle para o chamador atual da função na qual a `return` instrução é exibida.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Uma `return` instrução sem expressão só pode ser usada em um membro de função que não Compute um valor, ou seja, um método com o tipo de resultado ([corpo](classes.md#method-body)do método `void`), `set` o acessador de uma propriedade ou um `add` indexador, o e `remove` acessadores de um evento, um construtor de instância, um construtor estático ou um destruidor.

Uma `return` instrução com uma expressão só pode ser usada em um membro de função que computa um valor, ou seja, um método com um tipo de resultado não void, o `get` acessador de uma propriedade ou um indexador ou um operador definido pelo usuário. Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão para o tipo de retorno do membro da função que a contém.

Instruções de retorno também podem ser usadas no corpo de expressões de função anônimas ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) e participam da determinação de quais conversões existem para essas funções.

É um erro de tempo de compilação para que `return` uma instrução apareça em um `finally` bloco ([a instrução try](statements.md#the-try-statement)).

Uma `return` instrução é executada da seguinte maneira:

*  Se a `return` instrução especificar uma expressão, a expressão será avaliada e o valor resultante será convertido no tipo de retorno da função que a contém por uma conversão implícita. O resultado da conversão se torna o valor de resultado produzido pela função.
*  Se a `return` instrução estiver entre um ou mais `try` blocos ou `catch` com blocos associados `finally` , o controle será transferido inicialmente para o `finally` bloco da instrução mais `try` interna. Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição. Esse processo é repetido até `finally` que os blocos de todas `try` as instruções de circunscrição tenham sido executados.
*  Se a função que a contém não for uma função assíncrona, o controle será retornado para o chamador da função recipiente junto com o valor de resultado, se houver.
*  Se a função que a contém for uma função assíncrona, o controle será retornado para o chamador atual e o valor de resultado, se houver, será registrado na tarefa de retorno conforme descrito em ([interfaces do enumerador](classes.md#enumerator-interfaces)).

Como uma `return` instrução transfere incondicionalmente o controle em outro lugar, o `return` ponto de extremidade de uma instrução nunca é acessível.

### <a name="the-throw-statement"></a>A instrução Throw

A `throw` instrução gera uma exceção.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Uma `throw` instrução com uma expressão lança o valor produzido avaliando a expressão. A expressão deve indicar um valor do tipo `System.Exception`de classe, de um tipo de classe que deriva de `System.Exception` ou de um tipo de parâmetro de tipo que tenha `System.Exception` (ou uma subclasse dele) como sua classe base efetiva. Se a avaliação da expressão produzir `null`, um `System.NullReferenceException` será lançado em vez disso.

Uma `throw` instrução sem expressão só pode ser usada em um `catch` bloco, caso em que a instrução relança a exceção que está sendo manipulada por esse `catch` bloco no momento.

Como uma `throw` instrução transfere incondicionalmente o controle em outro lugar, o `throw` ponto de extremidade de uma instrução nunca é acessível.

Quando uma exceção é lançada, o controle é transferido para a `catch` primeira cláusula em uma instrução `try` delimitadora que pode manipular a exceção. O processo que ocorre desde o ponto da exceção sendo gerada para o ponto de transferência do controle para um manipulador de exceção adequado é conhecido como ***propagação de exceção***. A propagação de uma exceção consiste em avaliar repetidamente as etapas a seguir `catch` até que uma cláusula correspondente à exceção seja encontrada. Nesta descrição, o ***ponto de lançamento*** é inicialmente o local no qual a exceção é lançada.

*  No membro da função atual, cada `try` instrução que se aproxima do ponto de lançamento é examinada. Para cada instrução `S`, começando com a instrução `try` mais interna e terminando com `try` a instrução mais externa, as etapas a seguir são avaliadas:

   * Se o `try` bloco de `S` fechar o ponto de lançamento e se S tiver uma ou mais `catch` cláusulas, as `catch` cláusulas serão examinadas em ordem de aparência para localizar um manipulador adequado para a exceção, de acordo com as regras especificadas em Seção [a instrução try](statements.md#the-try-statement). Se uma cláusula `catch` correspondente estiver localizada, a propagação de exceção será concluída transferindo o controle para o bloco `catch` dessa cláusula.

   * Caso contrário, se `try` o bloco ou `catch` um bloco `S` de fechar o ponto de lançamento e se `S` tiver um `finally` bloco, o controle será transferido para `finally` o bloco. Se o `finally` bloco lançar outra exceção, o processamento da exceção atual será encerrado. Caso contrário, quando o controle atingir o ponto de `finally` extremidade do bloco, o processamento da exceção atual será continuado.

*  Se um manipulador de exceção não estiver localizado na invocação de função atual, a invocação de função será encerrada e uma das seguintes situações ocorrerá:

   * Se a função atual for não Async, as etapas acima serão repetidas para o chamador da função com um ponto de geração correspondente à instrução da qual o membro da função foi invocado.

   * Se a função atual for Async e retornando Task, a exceção será registrada na tarefa de retorno, que será colocada em um estado falho ou cancelado, conforme descrito em [interfaces do enumerador](classes.md#enumerator-interfaces).

   * Se a função atual for Async e void-retornando, o contexto de sincronização do thread atual será notificado conforme descrito em [interfaces enumeráveis](classes.md#enumerable-interfaces).

*  Se o processamento de exceções terminar todas as invocações de membro de função no thread atual, indicando que o thread não tem nenhum manipulador para a exceção, o thread será encerrado. O impacto dessa terminação é definido pela implementação.

## <a name="the-try-statement"></a>A instrução try

A `try` instrução fornece um mecanismo para capturar exceções que ocorrem durante a execução de um bloco. Além disso, `try` a instrução fornece a capacidade de especificar um bloco de código que é sempre executado quando o controle `try` sai da instrução.

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

Há três formas de `try` instruções possíveis:

*  Um `try` bloco seguido por um ou mais `catch` blocos.
*  Um `try` bloco seguido por um `finally` bloco.
*  Um `try` bloco seguido por um ou mais `catch` blocos seguidos por `finally` um bloco.

Quando uma `catch` cláusula Especifica um *exception_specifier*, o tipo deve ser `System.Exception`, um tipo que deriva de `System.Exception` ou um tipo de parâmetro de tipo que `System.Exception` tenha (ou uma subclasse dele) como sua classe base em vigor.

Quando uma `catch` cláusula Especifica um *exception_specifier* com um *identificador*, uma ***variável de exceção*** do nome e tipo fornecido é declarada. A variável de exceção corresponde a uma variável local com um escopo que se estende `catch` pela cláusula. Durante a execução do *exception_filter* e do *bloco*, a variável de exceção representa a exceção que está sendo manipulada no momento. Para fins de verificação de atribuição definitiva, a variável de exceção é considerada definitivamente atribuída em seu escopo inteiro.

A menos `catch` que uma cláusula inclua um nome de variável de exceção, é impossível acessar o objeto de exceção no `catch` filtro e no bloco.

Uma `catch` cláusula que não especifica um *exception_specifier* é chamada de cláusula geral `catch` .

Algumas linguagens de programação podem dar suporte a exceções que não podem ser representes `System.Exception`como um objeto derivado de, embora essas exceções C# nunca pudessem ser geradas pelo código. Uma cláusula `catch` geral pode ser usada para capturar essas exceções. Portanto, uma cláusula `catch` geral é semanticamente diferente de uma que especifica o tipo `System.Exception`, pois a primeira também pode detectar exceções de outras linguagens.

Para localizar um manipulador de uma exceção, `catch` as cláusulas são examinadas em ordem lexical. Se uma `catch` cláusula Especifica um tipo, mas nenhum filtro de exceção, é um erro de tempo de compilação para `catch` uma cláusula posterior na `try` mesma instrução para especificar um tipo que seja igual ou derivado desse tipo. Se uma `catch` cláusula não especificar nenhum tipo e nenhum filtro, ela deverá ser a `catch` última cláusula para `try` essa instrução.

Dentro de `catch` um bloco, `throw` uma instrução ([a instrução Throw](statements.md#the-throw-statement)) sem expressão pode ser usada para relançar `catch` a exceção que foi detectada pelo bloco. As atribuições a uma variável de exceção não alteram a exceção gerada novamente.

No exemplo
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
o método `F` captura uma exceção, grava algumas informações de diagnóstico no console, altera a variável de exceção e lança novamente a exceção. A exceção gerada novamente é a exceção original, portanto, a saída produzida é:
```
Exception in F: G
Exception in Main: G
```

Se o primeiro bloco catch tiver sido `e` lançado em vez de relançar a exceção atual, a saída produzida seria a seguinte:
```
Exception in F: G
Exception in Main: F
```

É um erro de tempo de compilação para uma `break`instrução `continue`, ou `goto` para transferir o controle de um `finally` bloco. Quando uma `break`instrução `continue`,, `goto` ou ocorre em um `finally` bloco, o destino da instrução deve estar dentro do mesmo `finally` bloco ou, caso contrário, ocorrerá um erro de tempo de compilação.

É um erro de tempo de compilação para que `return` uma instrução ocorra em um `finally` bloco.

Uma `try` instrução é executada da seguinte maneira:

*  O controle é transferido para `try` o bloco.
*  Quando e se o controle atingir o ponto de extremidade `try` do bloco:
   *  Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.
   *  O controle é transferido para o ponto de extremidade `try` da instrução.

*  Se uma exceção for propagada para `try` a instrução durante a `try` execução do bloco:
   *  As `catch` cláusulas, se houver, são examinadas em ordem de aparência para localizar um manipulador adequado para a exceção. Se uma `catch` cláusula não especificar um tipo, ou especificar o tipo de exceção ou um tipo base do tipo de exceção:
      *  Se a `catch` cláusula declarar uma variável de exceção, o objeto de exceção será atribuído à variável de exceção.
      *  Se a `catch` cláusula declarar um filtro de exceção, o filtro será avaliado. Se ele for avaliado como `false`, a cláusula catch não será uma correspondência e a pesquisa continuará em todas as `catch` cláusulas subsequentes para um manipulador adequado.
      *  Caso contrário, `catch` a cláusula será considerada uma correspondência e o controle será transferido para o `catch` bloco correspondente.
      *  Quando e se o controle atingir o ponto de extremidade `catch` do bloco:
         * Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.
         * O controle é transferido para o ponto de extremidade `try` da instrução.
      *  Se uma exceção for propagada para `try` a instrução durante a `catch` execução do bloco:
         *  Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.
         *  A exceção é propagada para a próxima instrução `try` de circunscrição.
   *  Se a `try` instrução não `catch` tiver cláusulas ou se nenhuma `catch` cláusula corresponder à exceção:
      *  Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.
      *  A exceção é propagada para a próxima instrução `try` de circunscrição.

As instruções de um `finally` bloco são sempre executadas quando o controle sai `try` de uma instrução. Isso é verdadeiro se a transferência de controle ocorre como resultado da execução normal, como resultado da execução de uma `break`instrução `continue`, `goto`, ou `return` , ou `try` como resultado da propagação de uma exceção do privacidade.

Se uma exceção for lançada durante a execução de `finally` um bloco e não for detectada no mesmo bloco finally, a exceção será propagada para a próxima instrução `try` de circunscrição. Se outra exceção estava no processo de ser propagada, essa exceção é perdida. O processo de propagação de uma exceção é abordado mais detalhadamente na descrição `throw` da instrução ([a instrução Throw](statements.md#the-throw-statement)).

O `try` bloco de uma `try` instrução estará acessível se a `try` instrução puder ser acessada.

Um `catch` bloco de uma `try` instrução pode ser acessado se a `try` instrução estiver acessível.

O `finally` bloco de uma `try` instrução estará acessível se a `try` instrução puder ser acessada.

O ponto de extremidade de `try` uma instrução estará acessível se as seguintes opções forem verdadeiras:

*  O ponto de extremidade do `try` bloco é acessível ou o ponto de extremidade de pelo menos `catch` um bloco está acessível.
*  Se um `finally` bloco estiver presente, o ponto final `finally` do bloco estará acessível.

## <a name="the-checked-and-unchecked-statements"></a>As instruções marcadas e não verificadas

As `checked` instruções `unchecked` e são usadas para controlar o ***contexto de verificação de estouro*** para operações aritméticas de tipo integral e conversões.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

A `checked` instrução faz com que todas as expressões no *bloco* sejam avaliadas em um contexto selecionado, `unchecked` e a instrução faz com que todas as expressões no *bloco* sejam avaliadas em um contexto desmarcado.

As `checked` instruções `unchecked` e`checked` são precisamente equivalentes aos operadores `unchecked` e ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)), exceto que operam em blocos em vez de expressões.

## <a name="the-lock-statement"></a>A instrução Lock

A `lock` instrução Obtém o bloqueio de mutual-exclusion para um determinado objeto, executa uma instrução e, em seguida, libera o bloqueio.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

A expressão de uma `lock` instrução deve indicar um valor de um tipo conhecido como *reference_type*. Nenhuma conversão de Boxing implícita ([conversões Boxing](conversions.md#boxing-conversions)) é executada para a expressão de uma `lock` instrução e, portanto, é um erro de tempo de compilação para a expressão denotar um valor de um *value_type*.

Uma `lock` instrução do formulário
```csharp
lock (x) ...
```
em `x` que é uma expressão de um *reference_type*, é precisamente equivalente a
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
exceto que `x` é avaliado apenas uma vez.

Enquanto um bloqueio de exclusão mútua é mantido, o código em execução no mesmo thread de execução também pode obter e liberar o bloqueio. No entanto, o código em execução em outros threads é impedido de obter o bloqueio até que o bloqueio seja liberado.

O `System.Type` bloqueio de objetos para sincronizar o acesso a dados estáticos não é recomendado. Outro código pode bloquear no mesmo tipo, o que pode resultar em deadlock. Uma abordagem melhor é sincronizar o acesso a dados estáticos bloqueando um objeto estático privado. Por exemplo:
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>A instrução using

A `using` instrução Obtém um ou mais recursos, executa uma instrução e, em seguida, descarta o recurso.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Um ***recurso*** é uma classe ou struct que implementa `System.IDisposable`, que inclui um único método sem parâmetros chamado `Dispose`. O código que está usando um recurso pode `Dispose` chamar para indicar que o recurso não é mais necessário. Se `Dispose` não for chamado, a eliminação automática eventualmente ocorrerá como consequência da coleta de lixo.

Se a forma de *resource_acquisition* for *local_variable_declaration* , o tipo de `dynamic` local_variable_declaration deverá ser ou um tipo que possa ser convertido implicitamente em. `System.IDisposable` Se a forma de *resource_acquisition* for *expression* , essa expressão deverá ser conversível `System.IDisposable`implicitamente.

Variáveis locais declaradas em um *resource_acquisition* são somente leitura e devem incluir um inicializador. Ocorrerá um erro de tempo de compilação se a instrução inserida tentar modificar essas variáveis locais (por meio `++` de `--` atribuição ou os operadores e), pegar o endereço delas ou passá `ref` - `out` las como parâmetros ou.

Uma `using` instrução é convertida em três partes: aquisição, uso e alienação. O uso do recurso é implicitamente incluído em uma `try` instrução que inclui uma `finally` cláusula. Essa `finally` cláusula descarta o recurso. Se um `null` recurso for adquirido, nenhuma chamada para `Dispose` será feita e nenhuma exceção será gerada. Se o recurso for do tipo `dynamic` , ele será convertido dinamicamente por meio de uma conversão dinâmica implícita ([conversões dinâmicas implícitas](conversions.md#implicit-dynamic-conversions)) `IDisposable` durante a aquisição para garantir que a conversão seja bem-sucedida antes do uso e liberação.

Uma `using` instrução do formulário
```csharp
using (ResourceType resource = expression) statement
```
corresponde a uma das três expansões possíveis. Quando `ResourceType` é um tipo de valor não anulável, a expansão é
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

Caso contrário, `ResourceType` quando for um tipo de valor anulável ou um tipo de `dynamic`referência diferente de, a expansão será
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

Caso contrário, `ResourceType` quando `dynamic`for, a expansão será
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

Em qualquer expansão, a `resource` variável é somente leitura na instrução incorporada, e a `d` variável é inacessível em, e invisível para, a instrução inserida.

Uma implementação tem permissão para implementar uma determinada instrução using de forma diferente, por exemplo, por motivos de desempenho, desde que o comportamento seja consistente com a expansão acima.

Uma `using` instrução do formulário
```csharp
using (expression) statement
```
tem as mesmas três expansões possíveis. Nesse caso `ResourceType` `expression`, é implicitamente o tipo de tempo de compilação do, se tiver um. Caso contrário, `IDisposable` a própria interface será usada `ResourceType`como o. A `resource` variável é inacessível em, e invisível para, a instrução inserida.

Quando um *resource_acquisition* assume a forma de um *local_variable_declaration*, é possível adquirir vários recursos de um determinado tipo. Uma `using` instrução do formulário
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
é precisamente equivalente a uma sequência de instruções `using` aninhadas:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

O exemplo a seguir cria um arquivo `log.txt` chamado e grava duas linhas de texto no arquivo. Em seguida, o exemplo abre o mesmo arquivo para ler e copiar as linhas de texto contidas para o console.
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

Como as `TextWriter` classes `TextReader` e implementam `IDisposable` a interface, o exemplo pode `using` usar instruções para garantir que o arquivo subjacente seja fechado corretamente seguindo as operações de gravação ou leitura.

## <a name="the-yield-statement"></a>A instrução yield

A `yield` instrução é usada em um bloco do iterador ([blocos](statements.md#blocks)) para gerar um valor para o objeto do enumerador ([objetos do enumerador](classes.md#enumerator-objects)) ou objeto enumerável ([objetos enumeráveis](classes.md#enumerable-objects)) de um iterador ou para sinalizar o final da iteração.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`Não é uma palavra reservada; Ele tem um significado especial somente quando usado imediatamente antes `return` de `break` uma palavra-chave ou. Em outros contextos, `yield` pode ser usado como um identificador.

Há várias restrições sobre onde uma `yield` instrução pode aparecer, conforme descrito a seguir.

*  É um erro de tempo de compilação para uma `yield` instrução (de qualquer forma) aparecer fora de um *method_body*, *operator_body* ou *accessor_body*
*  É um erro de tempo de compilação para uma `yield` instrução (de qualquer forma) aparecer dentro de uma função anônima.
*  É um erro de tempo de compilação para uma `yield` instrução (de qualquer forma) aparecer `finally` na cláusula de uma `try` instrução.
*  É um erro de tempo de compilação para uma `yield return` instrução aparecer em qualquer lugar em `try` uma instrução que contenha qualquer `catch` cláusula.

O exemplo a seguir mostra alguns usos válidos e inválidos de `yield` instruções.

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão na `yield return` instrução para o tipo yield ([tipo yield](classes.md#yield-type)) do iterador.

Uma `yield return` instrução é executada da seguinte maneira:

*  A expressão fornecida na instrução é avaliada, implicitamente convertida para o tipo yield e atribuída à `Current` Propriedade do objeto Enumerator.
*  A execução do bloco do iterador está suspensa. Se a `yield return` instrução estiver dentro de um ou `try` mais blocos, os `finally` blocos associados não serão executados neste momento.
*  O `MoveNext` método do objeto enumerador retorna `true` para seu chamador, indicando que o objeto enumerador foi avançado com êxito para o próximo item.

A próxima chamada para o método do `MoveNext` objeto enumerador retoma a execução do bloco iterador de onde ele foi suspenso pela última vez.

Uma `yield break` instrução é executada da seguinte maneira:

*  Se a `yield break` instrução estiver contida em um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna. Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição. Esse processo é repetido até `finally` que os blocos de todas `try` as instruções de circunscrição tenham sido executados.
*  O controle é retornado para o chamador do bloco do iterador. Esse é o `MoveNext` método ou `Dispose` método do objeto enumerador.

Como uma `yield break` instrução transfere incondicionalmente o controle em outro lugar, o `yield break` ponto de extremidade de uma instrução nunca é acessível.
