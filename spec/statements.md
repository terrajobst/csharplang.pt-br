# <a name="statements"></a>Instruções

C# fornece uma variedade de instruções. A maioria dessas instruções será familiar aos desenvolvedores que já programou em C e C++.

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

O *embedded_statement* não terminal é usado para as instruções que aparecem dentro de outras instruções. O uso de *embedded_statement* vez *instrução* exclui o uso de instruções de declaração e instruções rotuladas nesses contextos. O exemplo
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
resulta em um erro de tempo de compilação porque uma `if` instrução requer uma *embedded_statement* em vez de uma *instrução* , se seu branch. Se esse código eram permitidas, em seguida, a variável `i` será declarada, mas nunca pode ser usada. No entanto, observe que, colocando `i`da declaração em um bloco, o exemplo é válido.

## <a name="end-points-and-reachability"></a>Pontos de extremidade e acessibilidade

Cada instrução tem um ***ponto de extremidade***. Em termos de intuitivos, o ponto de extremidade de uma instrução é o local imediatamente após a instrução. As regras de execução de instruções compostas (instruções que contêm instruções inseridas) especificam a ação que é executada quando o controle atinge o ponto de extremidade de uma declaração inserido. Por exemplo, quando o controle atinge o ponto de extremidade de uma instrução em um bloco, o controle é transferido para a próxima instrução no bloco.

Se uma instrução, possivelmente, pode ser alcançada pela execução, a instrução será considerada ***acessível***. Por outro lado, se não houver nenhuma possibilidade de que uma instrução for executada, a instrução será considerada ***inacessível***.

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
a segunda chamada de `Console.WriteLine` está inacessível porque não há nenhuma possibilidade de que a instrução será executada.

Um aviso será relatado se o compilador determina que uma instrução está inacessível. É especificamente não um erro para uma instrução como inacessíveis.

Para determinar se uma determinada instrução ou um ponto de extremidade é acessível, o compilador executa a análise de fluxo de acordo com as regras de acessibilidade definidos para cada instrução. A análise de fluxo leva em consideração os valores das expressões de constante ([expressões constantes](expressions.md#constant-expressions)) que controlam o comportamento das instruções, mas os valores possíveis de expressões não constantes não são considerados. Em outras palavras, para fins de análise de fluxo de controle, uma expressão não constante de um determinado tipo é considerada para ter qualquer possível valor desse tipo.

No exemplo
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
a expressão booliana do `if` instrução é uma expressão constante, porque ambos os operandos do `==` operador são constantes. Como a expressão de constante é avaliada em tempo de compilação, produzindo o valor `false`, o `Console.WriteLine` invocação é considerada inacessível. No entanto, se `i` é alterada para ser uma variável local
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
o `Console.WriteLine` invocação é considerada acessível, mesmo que, na realidade, ele nunca será executado.

O *bloco* de uma função membro sempre é considerado acessível. Avaliando sucessivamente as regras de acessibilidade de cada instrução em um bloco, a acessibilidade de qualquer instrução fornecida pode ser determinada.

No exemplo
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
o problema de acessibilidade do segundo `Console.WriteLine` é determinado da seguinte maneira:

*  A primeira `Console.WriteLine` instrução de expressão está acessível porque o bloco do `F` método está acessível.
*  O ponto de extremidade do primeiro `Console.WriteLine` instrução de expressão está acessível porque essa instrução está acessível.
*  O `if` instrução está acessível porque o ponto de final do primeiro `Console.WriteLine` instrução de expressão está acessível.
*  A segunda `Console.WriteLine` instrução de expressão está acessível porque a expressão booliana do `if` instrução não tem o valor da constante `false`.

Há duas situações em que é um erro de tempo de compilação para o ponto de extremidade de uma instrução pode ser alcançado:

*  Porque o `switch` instrução não permite que uma seção switch para "passar" para a próxima seção switch, é um erro de tempo de compilação para o ponto de extremidade da lista de instruções de uma seção switch pode ser alcançado. Se esse erro ocorrer, ele costuma ser uma indicação de que um `break` instrução está ausente.
*  É um erro de tempo de compilação para o ponto de extremidade do bloco de um membro de função que calcula um valor para ser acessível. Se esse erro ocorrer, normalmente é uma indicação de que um `return` instrução está ausente.

## <a name="blocks"></a>Blocos

Um *bloco* permite a produção de várias instruções em contextos nos quais uma única instrução é permitida.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Um *bloco* consiste em um recurso opcional *statement_list* ([instrução lista](statements.md#statement-lists)), colocado entre chaves. Se a lista de instruções for omitida, o bloco deve ficar vazio.

Um bloco pode conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)). O escopo de um local constante ou variável declarada em um bloco é o bloco.

Um bloco é executado da seguinte maneira:

*  Se o bloco estiver vazio, o controle é transferido para o ponto de extremidade do bloco.
*  Se o bloco não estiver vazio, o controle é transferido para a lista de instrução. Quando e se o controle atinge o ponto de extremidade da lista de instrução, o controle é transferido para o ponto de extremidade do bloco.

A lista de instrução de um bloco é acessível se o bloco propriamente dito é acessível.

O ponto de extremidade de um bloco é acessível se o bloco estiver vazio ou se o ponto de extremidade da lista de instrução estiver acessível.

Um *bloco* que contém um ou mais `yield` instruções ([a instrução yield](statements.md#the-yield-statement)) é chamado de um bloco de iteradores. Blocos de iteradores são usados para implementar membros de função como iteradores ([iteradores](classes.md#iterators)). Algumas restrições adicionais se aplicam aos blocos de iterador:

*  É um erro de tempo de compilação para um `return` instrução para aparecer em um bloco de iteradores (mas `yield return` as instruções são permitidas).
*  É um erro de tempo de compilação para um bloco de iteradores conter um contexto sem segurança ([contextos não seguros](unsafe-code.md#unsafe-contexts)). Um bloco de iteradores sempre define o contexto de segurança, mesmo quando a sua declaração é aninhada em um contexto sem segurança.

### <a name="statement-lists"></a>Listas de instrução

Um ***lista de instruções*** consiste em uma ou mais instruções escritas em sequência. Listas de instrução ocorrem na *bloco*s ([blocos](statements.md#blocks)) e, na *switch_block*s ([da instrução switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Uma lista de instrução é executada, transferindo o controle para a primeira instrução. Quando e se o controle atinge o ponto de extremidade de uma instrução, o controle é transferido para a próxima instrução. Quando e se o controle atinge o ponto de extremidade da última instrução, o controle é transferido para o ponto de extremidade da lista de instruções.

Uma instrução em uma lista de instrução é acessível se pelo menos uma das seguintes opções for verdadeira:

*  A instrução for a primeira instrução e a lista de instrução em si está acessível.
*  O ponto de extremidade da instrução anterior está acessível.
*  A instrução é uma instrução rotulada e o rótulo é referenciado por uma acessível `goto` instrução.

O ponto de extremidade de uma lista de instrução é acessível se o ponto de extremidade da última instrução na lista puder ser acessado.

## <a name="the-empty-statement"></a>A instrução vazia

Uma *empty_statement* não faz nada.

```antlr
empty_statement
    : ';'
    ;
```

Uma instrução vazia é usada quando não houver nenhuma operação para executar em um contexto em que uma instrução é necessária.

Execução de uma instrução vazia simplesmente transfere o controle para o ponto de extremidade da instrução. Portanto, o ponto de extremidade de uma instrução vazia é acessível se a instrução vazia está acessível.

Uma instrução vazia pode ser usada ao gravar um `while` instrução com um corpo nulo:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Além disso, uma instrução vazia pode ser usada para declarar um rótulo antes do fechamento "`}`" de um bloco:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Instruções rotuladas

Um *labeled_statement* permite que uma instrução ser precedidos por um rótulo. Instruções rotuladas são permitidas em blocos, mas não são permitidas como instruções inseridas.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Uma instrução rotulada declara um rótulo com o nome fornecido o *identificador*. O escopo de um rótulo é o bloco inteiro no qual o rótulo é declarado, incluindo quaisquer blocos aninhados. É um erro de tempo de compilação para dois rótulos com o mesmo nome com escopos sobrepostos.

Um rótulo pode ser referenciado em `goto` instruções ([instrução goto](statements.md#the-goto-statement)) dentro do escopo do rótulo. Isso significa que `goto` instruções podem transferir o controle dentro de blocos e fora de blocos, mas nunca em blocos.

Rótulos tenham seu próprio espaço de declaração e não interfiram com outros identificadores. O exemplo
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
é válido e usa o nome `x` como um parâmetro e um rótulo.

Execução de uma instrução rotulada corresponde exatamente à execução da instrução a seguir o rótulo.

O problema de acessibilidade fornecido pelo fluxo de controle normal, além de uma instrução rotulada é acessível se o rótulo é referenciado por uma acessível `goto` instrução. (Exceção: se um `goto` instrução está dentro de uma `try` que inclui um `finally` é de bloco e a instrução rotulada fora o `try`e o ponto de extremidade do `finally` bloco está inacessível e, em seguida, a instrução rotulada não é acessível do que `goto` instrução.)

## <a name="declaration-statements"></a>Instruções de declaração

Um *declaration_statement* declara uma variável local ou uma constante. Instruções de declaração são permitidas em blocos, mas não são permitidas como instruções inseridas.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Declarações de variável local

Um *local_variable_declaration* declara um ou mais variáveis locais.

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

O *local_variable_type* de uma *local_variable_declaration* diretamente Especifica o tipo das variáveis introduzidas pela declaração, ou indica com o identificador `var` que o tipo deve ser inferido com base em um inicializador. O tipo é seguido por uma lista de *local_variable_declarator*s, cada uma delas apresenta uma nova variável. Um *local_variable_declarator* consiste em um *identificador* que nomeia a variável, opcionalmente seguida por um "`=`" token e um *local_variable_initializer* que fornece o valor inicial da variável.

No contexto de uma declaração de variável local, o var de identificador atua como uma palavra-chave contextual ([palavras-chave](lexical-structure.md#keywords)). Quando o *local_variable_type* é especificado como `var` e nenhum tipo denominado `var` está no escopo, a declaração é um ***declaração de variável local de tipo implícito***, cujo tipo é inferido do tipo da expressão de inicializador associado. Declarações de variável local digitadas implicitamente estão sujeitos às seguintes restrições:

*  O *local_variable_declaration* não é possível incluir vários *local_variable_declarator*s.
*  O *local_variable_declarator* deve incluir um *local_variable_initializer*.
*  O *local_variable_initializer* deve ser um *expressão*.
*  O inicializador *expressão* deve ter um tipo de tempo de compilação.
*  O inicializador *expressão* não pode se referir à variável declarada em si

A seguir está exemplos de declarações de variável local digitadas implicitamente incorretos:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

O valor de uma variável local é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)), e o valor de uma variável local é modificado usando um *atribuição* ( [Operadores de atribuição](expressions.md#assignment-operators)). Uma variável local deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) em cada local em que seu valor é obtido.

O escopo de uma variável local declarada em um *local_variable_declaration* é o bloco no qual ocorre a declaração. É um erro se referir a uma variável local em uma posição textual que precede o *local_variable_declarator* da variável local. Dentro do escopo de uma variável local, ele é um erro de tempo de compilação para declarar outro local, variável ou constante com o mesmo nome.

Uma declaração de variável local que declara várias variáveis é equivalente a várias declarações de variáveis únicas com o mesmo tipo. Além disso, um inicializador de variável em uma declaração de variável local corresponde exatamente a uma instrução de atribuição é inserida imediatamente após a declaração.

O exemplo
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
corresponde exatamente à
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

Em uma tipada implícito declaração de variável local, o tipo da variável local que está sendo declarado é feito para ser o mesmo que o tipo da expressão usada para inicializar a variável. Por exemplo:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

As tipadas implícito declarações de variável local acima são exatamente equivalentes às seguintes declarações digitadas explicitamente:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Declarações de constante local

Um *local_constant_declaration* declara um ou mais constantes locais.

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

O *tipo* de uma *local_constant_declaration* Especifica o tipo das constantes introduzidas pela declaração. O tipo é seguido por uma lista de *constant_declarator*s, cada uma delas apresenta uma nova constante. Um *constant_declarator* consiste em um *identificador* que indica que a constante, seguido por um "`=`" token, seguido por um *constant_expression* ([ Expressões de constante](expressions.md#constant-expressions)) que fornece o valor da constante.

O *tipo* e *constant_expression* de uma declaração de constante local deve seguir as mesmas regras que aquelas de uma declaração de membro constante ([constantes](classes.md#constants)).

O valor de uma constante de local é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)).

O escopo de uma constante local é o bloco no qual ocorre a declaração. É um erro para se referir a uma constante local em uma posição textual que precede seus *constant_declarator*. Dentro do escopo de uma constante local, ele é um erro de tempo de compilação para declarar outro local, variável ou constante com o mesmo nome.

Uma declaração de constante local que declara várias constantes é equivalente a várias declarações de constantes únicos com o mesmo tipo.

## <a name="expression-statements"></a>Instruções de expressão

Uma *expression_statement* avalia uma expressão fornecida. O valor computado pela expressão, se houver, serão descartados.

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

Nem todas as expressões são permitidas como instruções. Em particular, expressões, como `x + y` e `x == 1` que simplesmente calcular um valor (que será descartado), não são permitidos como instruções.

Execução de um *expression_statement* avalia a expressão independente e, em seguida, transfere o controle para o ponto de extremidade da *expression_statement*. O ponto de extremidade de uma *expression_statement* é acessível se esse *expression_statement* está acessível.

## <a name="selection-statements"></a>Instruções de seleção

Instruções de seleção Selecionar uma das várias instruções possíveis para execução com base no valor de alguma expressão.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If instrução

O `if` instrução seleciona uma instrução para execução com base no valor de uma expressão booliana.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Uma `else` part está associada precedente mais próximo lexicalmente `if` que é permitido pela sintaxe. Portanto, um `if` instrução do formulário
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

Um `if` instrução é executada da seguinte maneira:

*  O *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)) é avaliada.
*  Se a expressão booliana retorna `true`, o controle é transferido para a primeira instrução inserida. Quando e se o controle atinge o ponto de extremidade dessa instrução, o controle é transferido para o ponto de extremidade do `if` instrução.
*  Se a expressão booliana produz `false` e, se um `else` parte estiver presente, o controle é transferido para a segunda instrução inserida. Quando e se o controle atinge o ponto de extremidade dessa instrução, o controle é transferido para o ponto de extremidade do `if` instrução.
*  Se a expressão booliana produz `false` e, se um `else` parte não estiver presente, o controle é transferido para o ponto de extremidade do `if` instrução.

A primeira incorporado a declaração de um `if` instrução é acessível se o `if` instrução está acessível e se a expressão booliana não tem o valor da constante `false`.

O segundo inseridos instrução de um `if` instrução, se presente, é acessível se o `if` instrução está acessível e se a expressão booliana não tem o valor da constante `true`.

O ponto de extremidade de um `if` instrução é acessível se o ponto de extremidade de pelo menos uma das suas instruções inseridas está acessível. Além disso, do ponto final de uma `if` instrução sem nenhum `else` parte é acessível se o `if` instrução está acessível e se a expressão booliana não tem o valor da constante `true`.

### <a name="the-switch-statement"></a>A instrução switch

A instrução switch seleciona para execução de uma lista de instruções com um rótulo de comutador associado corresponde ao valor da expressão switch.

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

Um *switch_statement* consiste na palavra-chave `switch`, seguido por uma expressão entre parênteses (chamada expressão switch), seguido por um *switch_block*. O *switch_block* consiste de zero ou mais *switch_section*s, colocado entre chaves. Cada *switch_section* consiste em um ou mais *switch_label*s seguido por um *statement_list* ([instrução lista](statements.md#statement-lists)).

O ***que controlam o tipo*** de um `switch` instrução é estabelecida com a expressão switch.

*  Se for o tipo da expressão switch `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, ou um *enum_type*, ou se é o tipo que permite valor nulo correspondente a um desses tipos, que é o que controlam o tipo do `switch` instrução.
*  Caso contrário, exatamente um definido pelo usuário a conversão implícita ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)) deve existir do tipo da expressão switch para um dos seguintes possíveis que controlam tipos: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, ou então, um tipo anulável correspondente a um desses tipos.
*  Caso contrário, se nenhuma conversão implícita existe, ou se mais de uma conversão implícita existe, ocorrerá um erro de tempo de compilação.

A expressão de constante de cada `case` rótulo deve indicar um valor que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo que governam do `switch` instrução. Ocorrerá um erro de tempo de compilação se dois ou mais `case` rótulos na mesma `switch` instrução especificar o mesmo valor constante.

Pode haver no máximo um `default` rótulo em uma instrução switch.

Um `switch` instrução é executada da seguinte maneira:

*  A expressão de switch é avaliada e convertida no tipo que governam.
*  Se uma das constantes especificado em uma `case` rótulo na mesma `switch` instrução é igual ao valor da expressão switch, o controle é transferido para a lista de instrução correspondente a seguir `case` rótulo.
*  Se nenhuma das constantes especificadas na `case` rótulos na mesma `switch` instrução é igual ao valor da expressão switch e se um `default` rótulo estiver presente, o controle é transferido para o seguinte lista de instrução de `default` rótulo.
*  Se nenhuma das constantes especificadas na `case` rótulos na mesma `switch` instrução é igual ao valor da expressão switch e se nenhum `default` rótulo estiver presente, o controle é transferido para o ponto de extremidade do `switch` instrução.

Se o ponto de extremidade da lista de instruções de uma seção switch estiver acessível, ocorrerá um erro de tempo de compilação. Isso é conhecido como a regra "sem queda". O exemplo
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
é válido porque não há seção switch tem um ponto de extremidade acessível. Diferentemente do C e C++, execução de uma seção switch não é permitida para "passar" para a próxima seção de comutador e o exemplo
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
resultados em um erro de tempo de compilação. Quando a execução de uma seção switch está a ser seguido pela execução de outra seção de opção explícita `goto case` ou `goto default` declaração deve ser usada:
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

Vários rótulos são permitidos em uma *switch_section*. O exemplo
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
é válido. O exemplo não viola a regra "sem queda" porque os rótulos `case 2:` e `default:` fazem parte do mesmo *switch_section*.

A regra "sem queda" impede que uma classe comum de erros que ocorrem em C e C++ quando `break` instruções acidentalmente são omitidas. Além disso, devido a essa regra, as seções switch de uma `switch` instrução pode ser reorganizada arbitrariamente sem afetar o comportamento da instrução. Por exemplo, as seções do `switch` instrução acima pode ser revertida sem afetar o comportamento da instrução:
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

A lista de instrução de uma seção switch normalmente termina em um `break`, `goto case`, ou `goto default` instrução, mas qualquer construção que renderiza o ponto de extremidade da lista de instruções inacessível é permitido. Por exemplo, uma `while` instrução controlada por expressão booliana `true` é conhecida por nunca alcance seu ponto de extremidade. Da mesma forma, uma `throw` ou `return` declaração sempre será transferido em outro lugar do controle e nunca atinge seu ponto de extremidade. Assim, o exemplo a seguir é válido:
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

O tipo de regulador de uma `switch` instrução pode ser do tipo `string`. Por exemplo:
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

Como os operadores de igualdade de cadeia de caracteres ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)), o `switch` instrução diferencia maiusculas de minúsculas e executará uma seção de comutador específico somente se a cadeia de caracteres de expressão de switch corresponde exatamente um `case` rótulo constante.

Quando o controle de tipo de um `switch` instrução está `string`, o valor `null` é permitido como uma constante de rótulo de caso.

O *statement_list*s de uma *switch_block* pode conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)). O escopo de um local constante ou variável declarada em um bloco switch é o bloco de comutador.

A lista de instrução de uma seção de comutador específico é acessível se o `switch` instrução é acessível e pelo menos uma das seguintes opções for verdadeira:

*  A expressão de switch é um valor não constante.
*  A expressão switch é um valor constante que corresponde a um `case` rótulo na seção de opção.
*  A expressão switch é um valor constante que não corresponde a qualquer `case` label e seção switch contém o `default` rótulo.
*  Um rótulo de switch da seção switch é referenciado por uma acessível `goto case` ou `goto default` instrução.

O ponto de extremidade de um `switch` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:

*  O `switch` instrução contém uma acessível `break` instrução que sai do `switch` instrução.
*  O `switch` instrução é acessível, a expressão switch é um valor não constante e não `default` rótulo estiver presente.
*  O `switch` instrução é acessível, a expressão switch é um valor constante que não corresponde a qualquer `case` rótulo e não `default` rótulo estiver presente.

## <a name="iteration-statements"></a>Instruções de iteração

Instruções de iteração executar repetidamente uma instrução inserida.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>A instrução while

O `while` instrução executa condicionalmente uma declaração inserido zero ou mais vezes.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Um `while` instrução é executada da seguinte maneira:

*  O *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)) é avaliada.
*  Se a expressão booliana retorna `true`, o controle é transferido para a instrução inserida. Quando e se o controle atinge o ponto de extremidade da instrução inserida (possivelmente de execução de um `continue` instrução), o controle é transferido para o início do `while` instrução.
*  Se a expressão booliana produz `false`, o controle é transferido para o ponto de extremidade do `while` instrução.

Dentro da instrução inserida de um `while` instrução, uma `break` instrução ([a instrução break](statements.md#the-break-statement)) pode ser usado para transferir o controle para o ponto de extremidade do `while` instrução (terminando iteração do inserido instrução) e um `continue` instrução ([a instrução continue](statements.md#the-continue-statement)) pode ser usado para transferir o controle para o ponto de extremidade da instrução inserida (, além de executar outra iteração do `while` instrução).

A instrução inserida de um `while` instrução é acessível se o `while` instrução está acessível e se a expressão booliana não tem o valor da constante `false`.

O ponto de extremidade de um `while` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:

*  O `while` instrução contém uma acessível `break` instrução que sai do `while` instrução.
*  O `while` instrução está acessível e se a expressão booliana não tem o valor da constante `true`.

### <a name="the-do-statement"></a>A instrução do

O `do` instrução condicionalmente executa uma instrução inserida uma ou mais vezes.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Um `do` instrução é executada da seguinte maneira:

*  Controle é transferido para a instrução inserida.
*  Quando e se o controle atinge o ponto de extremidade da instrução inserida (possivelmente de execução de um `continue` instrução), o *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)) é avaliada. Se a expressão booliana produz `true`, o controle é transferido para o início do `do` instrução. Caso contrário, o controle é transferido para o ponto de extremidade do `do` instrução.

Dentro da instrução inserida de um `do` instrução, uma `break` instrução ([a instrução break](statements.md#the-break-statement)) pode ser usado para transferir o controle para o ponto de extremidade do `do` instrução (terminando iteração do inserido instrução) e um `continue` instrução ([a instrução continue](statements.md#the-continue-statement)) pode ser usado para transferir o controle para o ponto de extremidade da instrução inserida.

A instrução inserida de um `do` instrução é acessível se o `do` instrução é acessível.

O ponto de extremidade de um `do` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:

*  O `do` instrução contém uma acessível `break` instrução que sai do `do` instrução.
*  O ponto de extremidade da instrução inserida está acessível e se a expressão booliana não tem o valor da constante `true`.

### <a name="the-for-statement"></a>A instrução for

O `for` instrução avalia uma sequência de expressões de inicialização e em seguida, enquanto uma condição for true, repetidamente executa uma declaração inserido e avalia uma sequência de expressões de iteração.

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

O *for_initializer*, se presente, consiste em um *local_variable_declaration* ([declarações de variável Local](statements.md#local-variable-declarations)) ou uma lista de *statement_ expressão*s ([instruções de expressão](statements.md#expression-statements)) separados por vírgulas. O escopo de uma variável local declarada com um *for_initializer* começa na *local_variable_declarator* para a variável e se estende até o final da instrução inserida. O escopo inclui o *for_condition* e o *for_iterator*.

O *for_condition*, se presente, deve ser um *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)).

O *for_iterator*, se presente, consiste em uma lista de *statement_expression*s ([instruções de expressão](statements.md#expression-statements)) separados por vírgulas.

A instrução é executada da seguinte maneira:

*  Se um *for_initializer* está presente, os inicializadores de variável ou expressões de instrução são executadas na ordem em que elas são gravadas. Esta etapa é executada apenas uma vez.
*  Se um *for_condition* estiver presente, ela é avaliada.
*  Se o *for_condition* não estiver presente ou se a avaliação produz `true`, o controle é transferido para a instrução inserida. Quando e se o controle atinge o ponto de extremidade da instrução inserida (possivelmente de execução de um `continue` instrução), as expressões do *for_iterator*, se houver, são avaliadas em sequência, e, em seguida, outra iteração é executada, começando com a avaliação do *for_condition* na etapa anterior.
*  Se o *for_condition* está presente e a avaliação produz `false`, o controle é transferido para o ponto de extremidade do `for` instrução.

Dentro da instrução inserida de um `for` instrução, uma `break` instrução ([a instrução break](statements.md#the-break-statement)) pode ser usado para transferir o controle para o ponto de extremidade do `for` instrução (terminando iteração do inserido instrução) e um `continue` instrução ([a instrução continue](statements.md#the-continue-statement)) pode ser usado para transferir o controle para o ponto de extremidade da instrução incorporado (em execução, portanto, o *for_iterator* e executar outra iteração do `for` instrução, começando com o *for_condition*).

A instrução inserida de um `for` instrução é acessível se uma das seguintes opções for verdadeira:

*  O `for` instrução é acessível e não há *for_condition* está presente.
*  O `for` instrução é acessível e uma *for_condition* está presente e não tem o valor da constante `false`.

O ponto de extremidade de um `for` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:

*  O `for` instrução contém uma acessível `break` instrução que sai do `for` instrução.
*  O `for` instrução é acessível e uma *for_condition* está presente e não tem o valor da constante `true`.

### <a name="the-foreach-statement"></a>A instrução foreach

O `foreach` instrução enumera os elementos de uma coleção, executando uma instrução inserido para cada elemento da coleção.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

O *tipo* e *identificador* de um `foreach` instrução declare o ***variável de iteração*** da instrução. Se o `var` identificador é fornecido como o *local_variable_type*e nenhum tipo denominado `var` está no escopo, a variável de iteração deve ser um ***variável de iteração de tipo implícito***, e seu tipo será considerado como o tipo de elemento de `foreach` instrução, conforme especificado abaixo. A variável de iteração corresponde a uma variável local somente leitura com um escopo que se estende pela instrução inserida. Durante a execução de um `foreach` instrução, a variável de iteração representa o elemento da coleção para a qual uma iteração está sendo executada atualmente. Um erro de tempo de compilação ocorrerá se a instrução inserida tenta modificar a variável de iteração (por meio da atribuição ou o `++` e `--` operadores) ou passe a variável de iteração como uma `ref` ou `out` parâmetro.

A seguir, para fins de brevidade, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` e `IEnumerator<T>` consultar os tipos correspondentes nos namespaces `System.Collections` e `System.Collections.Generic`.

O processamento de tempo de compilação de uma instrução foreach primeiro determina a ***tipo de coleção***, ***tipo de enumerador*** e ***tipo de elemento*** da expressão. Essa determinação procede da seguinte maneira:

*  Se o tipo `X` dos *expressão* é um tipo de matriz, em seguida, há uma conversão de referência implícita de `X` para o `IEnumerable` interface (como `System.Array` implementa essa interface). O ***tipo de coleção*** é o `IEnumerable` interface, o ***tipo de enumerador*** é o `IEnumerator` interface e o ***tipo de elemento*** é o tipo de elemento da tipo de matriz `X`.
*  Se o tipo `X` dos *expressão* é `dynamic` e em seguida, há uma conversão implícita de *expressão* para o `IEnumerable` interface ([dinâmico implícito conversões](conversions.md#implicit-dynamic-conversions)). O ***tipo de coleção*** é o `IEnumerable` interface e o ***tipo de enumerador*** é o `IEnumerator` interface. Se o `var` identificador é fornecido como o *local_variable_type* o ***tipo de elemento*** é `dynamic`, caso contrário, será `object`.
*  Caso contrário, determinar se o tipo `X` tem um apropriado `GetEnumerator` método:
   * Executar a pesquisa de membro no tipo `X` com o identificador `GetEnumerator` sem nenhum argumento de tipo. Se a pesquisa de membro não produz uma correspondência, ou ele gera uma ambiguidade ou produz uma correspondência que não é um grupo de método, procure uma interface enumerável conforme descrito abaixo. É recomendável que um aviso ser emitido se a pesquisa de membro produz nada, exceto um grupo de métodos ou nenhuma correspondência.
   * Execute a resolução de sobrecarga usando o grupo resultante do método e uma lista de argumentos vazia. Se a resolução de sobrecarga resulta em nenhum método aplicável, resulta em uma ambiguidade ou resulta em um único método melhor, mas esse método é estático ou não público, procure uma interface enumerável, conforme descrito abaixo. É recomendável que um aviso ser emitido se a resolução de sobrecarga produzir qualquer coisa, exceto um método de instância pública não ambíguos ou nenhum método aplicável.
   * Se o tipo de retorno `E` do `GetEnumerator` método não é uma classe, struct ou interface, tipo, um erro é gerado e nenhuma outra etapa será efetuada.
   * Pesquisa de membro é executada em `E` com o identificador `Current` sem nenhum argumento de tipo. Se a pesquisa de membro não produz nenhuma correspondência, o resultado é um erro ou o resultado é que qualquer coisa, exceto uma propriedade de instância pública que permite a leitura, um erro é produzido e nenhuma outra etapa será efetuada.
   * Pesquisa de membro é executada em `E` com o identificador `MoveNext` sem nenhum argumento de tipo. Se a pesquisa de membro não produz nenhuma correspondência, o resultado é um erro ou o resultado é que qualquer coisa, exceto de um grupo de métodos, um erro é produzido e nenhuma outra etapa será efetuada.
   * Resolução de sobrecarga é executada no grupo de método com uma lista de argumentos vazia. Se os resultados da resolução de sobrecarga em nenhum métodos aplicáveis, resulta em uma ambiguidade ou resulta em um único método melhor, mas esse método é estático ou não público ou seu tipo de retorno não é `bool`, um erro é produzido e nenhuma outra etapa será efetuada.
   * O ***tipo de coleção*** é `X`, o ***tipo de enumerador*** é `E`e o ***tipo de elemento*** é o tipo dos `Current` propriedade.

*  Caso contrário, verifique uma interface enumerável:
   * Se entre todos os tipos `Ti` para as quais há uma conversão implícita de `X` ao `IEnumerable<Ti>`, há um tipo exclusivo `T` , de modo que `T` não é `dynamic` e para todos os outros `Ti` há um conversão implícita de `IEnumerable<T>` à `IEnumerable<Ti>`, em seguida, a ***tipo de coleção*** é a interface `IEnumerable<T>`, o ***tipo de enumerador*** é a interface `IEnumerator<T>`e o ***tipo de elemento*** é `T`.
   * Caso contrário, se houver mais de um tal tipo `T`, em seguida, um erro é produzido e nenhuma outra etapa será efetuada.
   * Caso contrário, se houver uma conversão implícita de `X` para o `System.Collections.IEnumerable` interface, em seguida, a ***tipo de coleção*** é essa interface, o ***tipo de enumerador*** é a interface `System.Collections.IEnumerator`e o ***tipo de elemento*** é `object`.
   * Caso contrário, um erro é produzido e nenhuma outra etapa será efetuada.

As etapas acima, se for bem-sucedido, produzirem um tipo de coleção inequivocamente `C`, tipo de enumerador `E` e o tipo de elemento `T`. Uma instrução foreach do formulário
```csharp
foreach (V v in x) embedded_statement
```
em seguida, é expandido para:
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

A variável `e` não é visível ou acessível para a expressão `x` ou a instrução inserida ou qualquer outro código-fonte do programa. A variável `v` é somente leitura na instrução inserida. Se não houver uma conversão explícita ([conversões explícitas](conversions.md#explicit-conversions)) do `T` (o tipo de elemento) para `V` (o *local_variable_type* na instrução foreach), um erro é produzido e nenhuma outra etapa será efetuada. Se `x` tem o valor `null`, um `System.NullReferenceException` é gerada em tempo de execução.

Uma implementação tem permissão para implementar uma determinada instrução foreach diferente, por exemplo, por motivos de desempenho, desde que o comportamento é consistente com a expansão acima.

O posicionamento dos `v` dentro do while loop é importante para como ele é capturado por qualquer função anônima que ocorrem na *embedded_statement*.

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
Se `v` foi declarado fora do while loop, seriam compartilhado entre todas as iterações e seu valor após o para loop seria o valor final, `13`, que é o que a invocação do `f` seria impressa. Em vez disso, porque cada iteração tem sua própria variável `v`, aquela capturados pelo `f` na primeira iteração continuará conter o valor `7`, que é o que será impresso. (Observação: as versões anteriores do c# é declarado `v` externa do while loop.)

O corpo da, por fim, o bloco é construído acordo com as seguintes etapas:

*  Se não houver uma conversão implícita da `E` para o `System.IDisposable` interface, em seguida,
   *  Se `E` é um tipo de valor não nulo, a, por fim, a cláusula é expandida para o equivalente a semântico:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  Caso contrário, o, por fim, a cláusula é expandida para o equivalente a semântico:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   exceto se `E` é um tipo de valor ou um parâmetro de tipo instanciado para um tipo de valor e, em seguida, a conversão de `e` para `System.IDisposable` não fará com que a conversão boxing ocorra.

*  Caso contrário, se `E` é um tipo selado, o, por fim, a cláusula é expandida para um bloco vazio:

   ```csharp
   finally {
   }
   ```

*  Caso contrário, o, por fim, a cláusula é expandida para:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   A variável local `d` não é visível ou acessível para qualquer código do usuário. Em particular, ele não entra em conflito com qualquer outra variável cujo escopo inclui o bloco finally.

A ordem na qual `foreach` percorre os elementos de uma matriz, é da seguinte maneira: para elementos de matrizes unidimensionais são percorridos em ordem de índice crescente, começando com o índice `0` e terminando com um índice `Length - 1`. Para matrizes multidimensionais, os elementos são percorridos, de modo que os índices da dimensão mais à direita são primeiro maior, em seguida, a próxima dimensão à esquerda, e assim por diante para a esquerda.

O exemplo a seguir imprime cada valor em uma matriz bidimensional, na ordem dos elementos:
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
A saída produzida é o seguinte:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

No exemplo
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
o tipo de `n` será inferida para ser `int`, o tipo de elemento `numbers`.

## <a name="jump-statements"></a>Instruções de hiperlink

Instruções de hiperlink incondicionalmente transferir o controle.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

O local para o qual uma instrução jump transfere o controle é chamado de ***destino*** da instrução de salto.

Quando uma instrução de salto ocorre dentro de um bloco, e o destino da instrução jump estiver fora desse bloco, a instrução de salto é considerada ***sair*** o bloco. Enquanto uma instrução de salto pode transferir o controle para fora de um bloco, ele nunca pode transferir o controle em um bloco.

Execução de instruções de salto é complicada pela presença de intervenção `try` instruções. Na ausência de tais `try` instruções, uma instrução jump transfere o controle incondicionalmente da instrução de salto para seu destino. Na presença de tais intermediária `try` instruções, a execução é mais complexa. Se a instrução de salto sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução. Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução. Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.

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
o `finally` blocos associados com dois `try` as instruções são executadas antes do controle é transferido para o destino da instrução de salto.

A saída produzida é o seguinte:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>A instrução break

O `break` instrução sai delimitadora mais próxima `switch`, `while`, `do`, `for`, ou `foreach` instrução.

```antlr
break_statement
    : 'break' ';'
    ;
```

O destino de uma `break` instrução é o ponto de extremidade de delimitadora mais próxima `switch`, `while`, `do`, `for`, ou `foreach` instrução. Se um `break` instrução não é incluída por um `switch`, `while`, `do`, `for`, ou `foreach` declaração, um erro de tempo de compilação ocorre.

Quando vários `switch`, `while`, `do`, `for`, ou `foreach` instruções estão aninhadas dentro uns dos outros, um `break` instrução se aplica somente à instrução mais interna. Para transferir o controle em vários níveis de aninhamento, uma `goto` instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usado.

Um `break` instrução não é possível sair um `finally` bloco ([a instrução try](statements.md#the-try-statement)). Quando um `break` instrução ocorre dentro de uma `finally` bloquear, o destino da `break` instrução deve ser dentro do mesmo `finally` bloquear; caso contrário, ocorrerá um erro de tempo de compilação.

Um `break` instrução é executada da seguinte maneira:

*  Se o `break` instrução sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução. Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução. Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.
*  O controle é transferido para o destino do `break` instrução.

Como uma `break` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `break` instrução nunca é acessível.

### <a name="the-continue-statement"></a>A instrução continue

O `continue` instrução inicia uma nova iteração do delimitadora mais próxima `while`, `do`, `for`, ou `foreach` instrução.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

O destino de uma `continue` instrução é o ponto de extremidade da instrução inserida de delimitadora mais próxima `while`, `do`, `for`, ou `foreach` instrução. Se um `continue` instrução não é incluída por um `while`, `do`, `for`, ou `foreach` declaração, um erro de tempo de compilação ocorre.

Quando vários `while`, `do`, `for`, ou `foreach` instruções estão aninhadas dentro uns dos outros, um `continue` instrução se aplica somente à instrução mais interna. Para transferir o controle em vários níveis de aninhamento, uma `goto` instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usado.

Um `continue` instrução não é possível sair um `finally` bloco ([a instrução try](statements.md#the-try-statement)). Quando um `continue` instrução ocorre dentro de uma `finally` bloquear, o destino da `continue` instrução deve ser dentro do mesmo `finally` bloquear; caso contrário, ocorrerá um erro de tempo de compilação.

Um `continue` instrução é executada da seguinte maneira:

*  Se o `continue` instrução sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução. Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução. Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.
*  O controle é transferido para o destino do `continue` instrução.

Como uma `continue` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `continue` instrução nunca é acessível.

### <a name="the-goto-statement"></a>A instrução goto

O `goto` transfere o controle para uma declaração que é marcada por um rótulo.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

O destino de uma `goto` *identificador* instrução é a instrução rotulada com o rótulo fornecido. Se um rótulo com o nome fornecido não existe no membro da função atual, ou se o `goto` instrução não está dentro do escopo do rótulo, ocorre um erro de tempo de compilação. Essa regra permite o uso de um `goto` instrução para transferir o controle fora de um escopo aninhado, mas não em um escopo aninhado. No exemplo
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
um `goto` instrução é usada para transferir o controle para fora de um escopo aninhado.

O destino de uma `goto case` instrução é a lista de instrução em imediatamente delimitador `switch` instrução ([da instrução switch](statements.md#the-switch-statement)), que contém um `case` rótulo com o valor constante fornecido. Se o `goto case` instrução não é incluída por um `switch` instrução, se o *constant_expression* não é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo que governam do mais próximo de circunscrição `switch` instrução, ou se delimitadora mais próxima `switch` instrução não contém um `case` rótulo com o valor constante fornecido, ocorre um erro de tempo de compilação.

O destino de uma `goto default` instrução é a lista de instrução em imediatamente delimitador `switch` instrução ([da instrução switch](statements.md#the-switch-statement)), que contém um `default` rótulo. Se o `goto default` instrução não é incluída por um `switch` instrução, ou se delimitadora mais próxima `switch` instrução não contém um `default` rotular, ocorre um erro de tempo de compilação.

Um `goto` instrução não é possível sair um `finally` bloco ([a instrução try](statements.md#the-try-statement)). Quando um `goto` instrução ocorre dentro de uma `finally` bloquear, o destino da `goto` instrução deve ser dentro do mesmo `finally` bloco, ou caso contrário, ocorrerá um erro de tempo de compilação.

Um `goto` instrução é executada da seguinte maneira:

*  Se o `goto` instrução sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução. Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução. Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.
*  O controle é transferido para o destino do `goto` instrução.

Como uma `goto` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `goto` instrução nunca é acessível.

### <a name="the-return-statement"></a>A instrução return

O `return` instrução retorna o controle para o chamador atual da função na qual o `return` instrução aparece.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Um `return` instrução com nenhuma expressão pode ser usada somente em um membro da função que não computa um valor, ou seja, um método com o tipo de resultado ([corpo do método](classes.md#method-body)) `void`, o `set` acessador de uma propriedade ou indexador, o `add` e `remove` acessadores de um evento, um construtor de instância, um construtor estático ou um destruidor.

Um `return` instrução com uma expressão somente pode ser usada em um membro da função que calcula um valor, ou seja, um método com um tipo de resultado não nulo, o `get` acessador de uma propriedade ou indexador ou um operador definido pelo usuário. Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão para o tipo de retorno do membro da função que contém.

Retornar declarações também podem ser usadas no corpo das expressões de função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)) e participar de determinar quais conversões existem para essas funções.

É um erro de tempo de compilação para um `return` instrução para aparecer em um `finally` bloco ([a instrução try](statements.md#the-try-statement)).

Um `return` instrução é executada da seguinte maneira:

*  Se o `return` declaração especifica uma expressão, a expressão é avaliada e o valor resultante é convertido para o tipo de retorno da função que contém por uma conversão implícita. O resultado da conversão torna-se o valor de resultado produzido pela função.
*  Se o `return` instrução é incluída por um ou mais `try` ou `catch` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução. Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução. Esse processo é repetido até que o `finally` blocos de inclusão de todos os `try` instruções foram executadas.
*  Se a função recipiente não é uma função assíncrona, o controle é retornado ao chamador da função que contém, juntamente com o valor do resultado, se houver.
*  Se a função recipiente é uma função assíncrona, controle é retornado ao chamador atual e o valor do resultado, se houver, será registrado no task de retorno conforme descrito em ([interfaces de enumerador](classes.md#enumerator-interfaces)).

Como uma `return` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `return` instrução nunca é acessível.

### <a name="the-throw-statement"></a>A instrução throw

O `throw` instrução gera uma exceção.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Um `throw` instrução com uma expressão gera o valor produzido pela avaliação da expressão. A expressão deve indicar um valor do tipo de classe `System.Exception`, de um tipo de classe que deriva de `System.Exception` ou de um tipo de parâmetro de tipo que tem `System.Exception` (ou uma subclasse disso) como sua classe base em vigor. Se a avaliação da expressão produz `null`, um `System.NullReferenceException` é acionada em vez disso.

Um `throw` instrução com nenhuma expressão pode ser usada somente em um `catch` bloquear, caso em que essa instrução gera novamente a exceção que está sendo manipulada no momento, por que `catch` bloco.

Como uma `throw` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `throw` instrução nunca é acessível.

Quando uma exceção é lançada, o controle é transferido para a primeira `catch` cláusula em um delimitador `try` instrução que pode lidar com a exceção. O processo que ocorre a partir do ponto da exceção sendo lançada para o ponto de transferência de controle para um manipulador de exceção adequada é conhecido como ***propagação de exceção***. Propagação de uma exceção consiste em avaliar repetidamente as etapas seguintes até que um `catch` cláusula que corresponde a exceção é encontrada. Nessa Descrição, o ***throw ponto*** inicialmente é o local em que a exceção é lançada.

*  No membro da função atual, cada `try` instrução que inclui o ponto de lançamento é examinada. Instrução for each `S`, começando com o mais interno `try` instrução e terminando com mais externo `try` instrução, as etapas a seguir são avaliadas:

   * Se o `try` bloco do `S` circunscreve o ponto de lançamento e se S tem um ou mais `catch` cláusulas, a `catch` cláusulas são examinadas em ordem de aparência para localizar um manipulador adequado para a exceção, de acordo com as regras especificadas na Seção [a instrução try](statements.md#the-try-statement). Se encontrar uma correspondência `catch` cláusula está localizada, a propagação de exceção é concluída, transferindo o controle para o bloco de que `catch` cláusula.

   * Caso contrário, se o `try` bloco ou uma `catch` block de `S` circunscreve o ponto de lançamento e se `S` tem um `finally` bloco, o controle é transferido para o `finally` bloco. Se o `finally` bloco lança a exceção de outro, o processamento da exceção atual será encerrado. Caso contrário, quando o controle atinge o ponto de extremidade do `finally` bloco, o processamento da exceção atual é continuado.

*  Se um manipulador de exceção não foi localizado na invocação da função atual, a invocação de função é encerrada e ocorre um dos seguintes:

   * Se a função atual é não assíncronas, as etapas acima são repetidas para o chamador da função com um ponto de lançamento correspondente à instrução a partir do qual o membro da função foi invocado.

   * Se a função atual é assíncrono e o retorno de tarefa, a exceção é registrada no task de retorno, que é colocado em um estado com falha ou cancelado, conforme descrito em [interfaces de enumerador](classes.md#enumerator-interfaces).

   * Se a função atual é assíncrono e retorna void, o contexto de sincronização do thread atual é notificado, conforme descrito em [interfaces enumeráveis](classes.md#enumerable-interfaces).

*  Se o processamento de exceção encerra todas as invocações de membro de função no thread atual, indicando que o thread não tem nenhum manipulador para a exceção, em seguida, o thread está em si encerrada. O impacto de tal rescisão é definido pela implementação.

## <a name="the-try-statement"></a>A instrução try

O `try` instrução fornece um mecanismo para capturar exceções que ocorrem durante a execução de um bloco. Além disso, o `try` instrução fornece a capacidade de especificar um bloco de código que sempre é executado quando o controle deixa o `try` instrução.

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

Há três formas possíveis de `try` instruções:

*  Um `try` bloco seguido por um ou mais `catch` blocos.
*  Um `try` bloco seguido por um `finally` bloco.
*  Um `try` bloco seguido por um ou mais `catch` blocos seguido por um `finally` bloco.

Quando um `catch` cláusula Especifica um *exception_specifier*, o tipo deve ser `System.Exception`, um tipo que deriva `System.Exception` ou um tipo de parâmetro de tipo que tem `System.Exception` (ou uma subclasse disso) como seu efetiva classe base.

Quando um `catch` cláusula especifica tanto uma *exception_specifier* com um *identificador*, uma ***variável de exceção*** do determinado nome e tipo é declarado. A variável de exceção corresponde a uma variável local com um escopo que se estende pelo `catch` cláusula. Durante a execução do *exception_filter* e *bloco*, a variável de exceção representa a exceção que está sendo manipulada no momento. Para fins de verificação de atribuição definitiva, a variável de exceção é considerada definitivamente atribuída no seu escopo inteiro.

A menos que um `catch` cláusula inclui um nome de variável de exceção, é impossível acessar o objeto de exceção no filtro e `catch` bloco.

Um `catch` cláusula que não especifica um *exception_specifier* é chamado um geral `catch` cláusula.

Algumas linguagens de programação podem dar suporte a exceções que não são representáveis como um objeto derivado de `System.Exception`, embora essas exceções nunca pudessem ser geradas pelo código em c#. Geral `catch` cláusula pode ser usada para capturar essas exceções. Assim, um general `catch` cláusula é semanticamente diferente dos que especifica o tipo `System.Exception`, em que o primeiro pode capturar exceções de outros idiomas de também.

Para localizar um manipulador para uma exceção, `catch` cláusulas são examinadas em ordem léxica. Se um `catch` cláusula Especifica um tipo, mas nenhum filtro de exceção, ele é um erro de tempo de compilação para uma posterior `catch` cláusula na mesma `try` instrução para especificar um tipo que é igual ou é derivado de, digite. Se um `catch` cláusula especifica nenhum tipo e nenhum filtro, ele deve ser o último `catch` cláusula para que `try` instrução.

Dentro de um `catch` bloco, um `throw` instrução ([a instrução throw](statements.md#the-throw-statement)) com nenhuma expressão pode ser usado para gerar novamente a exceção que foi detectada pelo `catch` bloco. As atribuições para uma variável de exceção não alteram a exceção que é lançada novamente.

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
o método `F` capturar uma exceção, grava algumas informações de diagnóstico no console, altera a variável de exceção e gera novamente a exceção. A exceção que é lançada novamente é a exceção original, portanto, a saída produzida é:
```
Exception in F: G
Exception in Main: G
```

Se o primeiro bloco catch tinha lançada `e` em vez de relançar a exceção atual, a saída produzida seria da seguinte maneira:
```csharp
Exception in F: G
Exception in Main: F
```

É um erro de tempo de compilação para um `break`, `continue`, ou `goto` instrução para transferir o controle de um `finally` bloco. Quando um `break`, `continue`, ou `goto` instrução ocorre em um `finally` bloco, o destino da instrução deve ser dentro do mesmo `finally` bloco, ou caso contrário, ocorrerá um erro de tempo de compilação.

É um erro de tempo de compilação para um `return` instrução ocorra em um `finally` bloco.

Um `try` instrução é executada da seguinte maneira:

*  O controle é transferido para o `try` bloco.
*  Quando e se o controle atinge o ponto de extremidade do `try` bloco:
   *  Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.
   *  O controle é transferido para o ponto de extremidade do `try` instrução.

*  Se uma exceção é propagada para o `try` instrução durante a execução do `try` bloco:
   *  O `catch` cláusulas, se houver, são examinadas em ordem de aparência para localizar um manipulador adequado para a exceção. Se um `catch` cláusula não especifica um tipo ou especifica o tipo de exceção ou um tipo base do tipo de exceção:
      *  Se o `catch` cláusula declara uma variável de exceção, o objeto de exceção é atribuído à variável de exceção.
      *  Se o `catch` cláusula declara um filtro de exceção, o filtro é avaliado. Se for avaliada como `false`, a cláusula catch não é uma correspondência e a pesquisa continua por meio de qualquer subsequentes `catch` cláusulas para um manipulador adequado.
      *  Caso contrário, o `catch` cláusula é considerada uma correspondência, e o controle é transferido para a correspondência `catch` bloco.
      *  Quando e se o controle atinge o ponto de extremidade do `catch` bloco:
         * Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.
         * O controle é transferido para o ponto de extremidade do `try` instrução.
      *  Se uma exceção é propagada para o `try` instrução durante a execução do `catch` bloco:
         *  Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.
         *  A exceção é propagada para a próxima delimitação `try` instrução.
   *  Se o `try` instrução não tem nenhum `catch` cláusulas ou se nenhum `catch` cláusula coincide com a exceção:
      *  Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.
      *  A exceção é propagada para a próxima delimitação `try` instrução.

As declarações de um `finally` bloco são sempre executadas quando o controle deixa uma `try` instrução. Isso é verdadeiro se a transferência de controle ocorre como resultado da execução normal, como resultado da execução de um `break`, `continue`, `goto`, ou `return` instrução, ou como resultado de propagar uma exceção do `try` instrução.

Se uma exceção for gerada durante a execução de um `finally` bloquear e não é capturada dentro do mesmo bloco finally, a exceção é propagada para a próxima delimitação `try` instrução. Se outra exceção estava no processo de propagação, essa exceção será perdida. O processo de propagação de uma exceção é discutido mais detalhes na descrição do `throw` instrução ([a instrução throw](statements.md#the-throw-statement)).

O `try` block de um `try` instrução é acessível se o `try` instrução é acessível.

Um `catch` block de um `try` instrução é acessível se o `try` instrução é acessível.

O `finally` block de um `try` instrução é acessível se o `try` instrução é acessível.

O ponto de extremidade de um `try` instrução é acessível se as duas seguintes forem verdadeiras:

*  O ponto de extremidade do `try` bloco está acessível ou ponto de final pelo menos um `catch` bloco está acessível.
*  Se um `finally` bloco estiver presente, o ponto de extremidade do `finally` bloco está acessível.

## <a name="the-checked-and-unchecked-statements"></a>As instruções checked e unchecked

O `checked` e `unchecked` instruções são usadas para controlar a ***contexto de verificação de estouro*** para conversões e operações aritméticas de tipo integral.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

O `checked` instrução faz com que todas as expressões na *bloco* a ser avaliada em um contexto verificado e o `unchecked` instrução faz com que todas as expressões no *bloco* a ser avaliada em um contexto desmarcado.

O `checked` e `unchecked` instruções são precisamente equivalentes para o `checked` e `unchecked` operadores ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)), exceto que eles operam em blocos, em vez de expressões .

## <a name="the-lock-statement"></a>A instrução lock

O `lock` instrução obtém o bloqueio de exclusão mútua para um determinado objeto, executa uma instrução e, em seguida, libera o bloqueio.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

A expressão de uma `lock` instrução deve indicar um valor de um tipo conhecido por ser um *reference_type*. Nenhuma conversão boxing implícita ([conversões Boxing](conversions.md#boxing-conversions)) nunca é executada para a expressão de uma `lock` instrução e, portanto, ele é um erro de tempo de compilação para a expressão indicar um valor de um *value_type*.

Um `lock` instrução do formulário
```csharp
lock (x) ...
```
em que `x` é uma expressão de uma *reference_type*, é precisamente equivalente a
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

Enquanto um bloqueio de exclusão mútua é mantido, código em execução no mesmo thread de execução pode também obter e liberar o bloqueio. No entanto, o código em execução em outros threads é impedido de obter o bloqueio até que o bloqueio seja liberado.

Bloqueio `System.Type` objetos para sincronizar o acesso a dados estáticos não é recomendado. Outro código pode bloquear no mesmo tipo, que pode resultar em deadlock. Uma abordagem melhor é sincronizar o acesso aos dados estáticos, bloqueando a um objeto estático privado. Por exemplo:
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

O `using` instrução obtém um ou mais recursos, executa uma instrução e, em seguida, descarta o recurso.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Um ***resource*** é uma classe ou struct que implementa `System.IDisposable`, que inclui um único método sem parâmetros chamado `Dispose`. O código que está usando um recurso pode chamar `Dispose` para indicar que o recurso não for mais necessário. Se `Dispose` não for chamado, em seguida, descarte automático ocorre, eventualmente, como consequência de coleta de lixo.

Se a forma de *resource_acquisition* é *local_variable_declaration* , em seguida, o tipo dos *local_variable_declaration* deve ser um `dynamic` ou um tipo que pode ser convertido implicitamente em `System.IDisposable`. Se o formulário da *resource_acquisition* é *expressão* e em seguida, essa expressão deve ser implicitamente conversível para `System.IDisposable`.

Variáveis locais declaradas em uma *resource_acquisition* são somente leitura e deve incluir um inicializador. Um erro de tempo de compilação ocorrerá se a instrução inserida tenta modificar essas variáveis locais (por meio da atribuição ou o `++` e `--` operadores), obter o endereço de-los ou passá-los como `ref` ou `out` parâmetros.

Um `using` instrução é convertida em três partes: aquisição, uso e descarte. Uso do recurso está contido implicitamente em um `try` instrução que inclui um `finally` cláusula. Isso `finally` cláusula descarta o recurso. Se um `null` adquirir o recurso, em seguida, nenhuma chamada para `Dispose` é feita, e nenhuma exceção é lançada. Se o recurso é do tipo `dynamic` dinamicamente, ele será convertido por meio de uma conversão implícita de dinâmica ([conversões implícitas de dinâmicas](conversions.md#implicit-dynamic-conversions)) para `IDisposable` durante a aquisição para garantir que a conversão seja concluída com êxito antes do uso e o descarte.

Um `using` instrução do formulário
```csharp
using (ResourceType resource = expression) statement
```
corresponde a um dos três possíveis expansões. Quando `ResourceType` é um tipo de valor não nulo, a expansão é
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

Caso contrário, quando `ResourceType` é um tipo de valor anulável ou um tipo de referência diferente de `dynamic`, a expansão é
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

Caso contrário, quando `ResourceType` é `dynamic`, a expansão é
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

Em qualquer expansão, o `resource` variável é somente leitura na instrução inserida e o `d` variável está inacessível no e é invisível para a instrução inserida.

Uma implementação tem permissão para implementar uma determinada usando-instrução de forma diferente, por exemplo, por motivos de desempenho, desde que o comportamento é consistente com a expansão acima.

Um `using` instrução do formulário
```csharp
using (expression) statement
```
tem as mesmas três possíveis expansões. Nesse caso `ResourceType` é implicitamente o tipo de tempo de compilação do `expression`, se ele tiver um. Caso contrário, a interface `IDisposable` em si é usada como o `ResourceType`. O `resource` variável está inacessível no e é invisível para a instrução inserida.

Quando um *resource_acquisition* assumem a forma de uma *local_variable_declaration*, é possível que você adquira vários recursos de um determinado tipo. Um `using` instrução do formulário
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
é precisamente equivalente a uma sequência de aninhado `using` instruções:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

O exemplo a seguir cria um arquivo chamado `log.txt` e grava duas linhas de texto no arquivo. O exemplo, em seguida, abre esse mesmo arquivo para leitura e copia as linhas independentes de texto para o console.
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

Uma vez que o `TextWriter` e `TextReader` classes implementam a `IDisposable` interface, o exemplo pode usar `using` instruções para garantir que o arquivo subjacente é fechado corretamente após a gravação ou operações de leitura.

## <a name="the-yield-statement"></a>A instrução yield

O `yield` instrução é usada em um bloco iterador ([blocos](statements.md#blocks)) para produzir um valor para o objeto de enumerador ([objetos de enumerador](classes.md#enumerator-objects)) ou o objeto enumerável ([objetos enumeráveis](classes.md#enumerable-objects)) de um iterador ou para sinalizar o final da iteração.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` não é uma palavra reservada; ele tem um significado especial somente quando usado imediatamente antes de uma `return` ou `break` palavra-chave. Em outros contextos, `yield` pode ser usado como um identificador.

Há várias restrições sobre onde um `yield` declaração pode aparecer, conforme descrito a seguir.

*  É um erro de tempo de compilação para um `yield` instrução (de qualquer forma) para aparecer fora de uma *method_body*, *operator_body* ou *accessor_body*
*  É um erro de tempo de compilação para um `yield` instrução (de qualquer forma) para aparecer dentro de uma função anônima.
*  É um erro de tempo de compilação para um `yield` instrução (de qualquer forma) apareçam na `finally` cláusula de um `try` instrução.
*  É um erro de tempo de compilação para um `yield return` instrução para aparecer em qualquer lugar em um `try` instrução que contém qualquer `catch` cláusulas.

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

Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão na `yield return` instrução para o tipo de yield ([geram tipo](classes.md#yield-type)) do iterador.

Um `yield return` instrução é executada da seguinte maneira:

*  A expressão fornecida na instrução é avaliada implicitamente convertida no tipo de rendimento e atribuída ao `Current` propriedade do objeto enumerador.
*  Execução do bloco iterador é suspenso. Se o `yield return` instrução está dentro de um ou mais `try` bloqueia associado `finally` blocos não são executados no momento.
*  O `MoveNext` método do objeto enumerador retornar `true` para seu chamador, que indica que o objeto de enumerador avançou com êxito para o próximo item.

A próxima chamada para o objeto de enumerador `MoveNext` método retoma a execução do bloco de iterador de onde ele foi suspenso pela última vez.

Um `yield break` instrução é executada da seguinte maneira:

*  Se o `yield break` instrução é incluída por um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução. Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução. Esse processo é repetido até que o `finally` blocos de inclusão de todos os `try` instruções foram executadas.
*  Controle é retornado ao chamador do bloco de iterador. Isso é o `MoveNext` método ou `Dispose` método do objeto enumerador.

Como uma `yield break` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `yield break` instrução nunca é acessível.
