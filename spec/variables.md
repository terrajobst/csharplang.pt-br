# <a name="variables"></a>Variáveis

As variáveis representam os locais de armazenamento. Cada variável tem um tipo que determina quais valores podem ser armazenados na variável. C# é uma linguagem fortemente tipada e o compilador c# garante que os valores armazenados em variáveis são sempre do tipo apropriado. O valor de uma variável pode ser alterado por meio de atribuição ou por meio do uso do `++` e `--` operadores.

Uma variável deve ser ***definitivamente atribuído*** ([atribuição definitiva](variables.md#definite-assignment)) antes de seu valor pode ser obtido.

Conforme descrito nas seções a seguir, as variáveis são ***inicialmente atribuída*** ou ***atribuídas inicialmente***. Uma variável inicialmente atribuída tem um valor inicial bem definido e é sempre considerado definitivamente atribuído. Uma variável não atribuída inicialmente não tem nenhum valor inicial. Para uma variável inicialmente não atribuída a serem considerados definitivamente atribuído em um determinado local, uma atribuição à variável deve ocorrer em todos os caminhos possíveis de execução levando a nesse local.

## <a name="variable-categories"></a>Categorias variáveis

C# define sete categorias de variáveis: variáveis estáticas, variáveis de instância, elementos de matriz, parâmetros de valor, parâmetros de referência, parâmetros de saída e as variáveis locais. As seções a seguir descrevem cada uma dessas categorias.

No exemplo
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` é uma variável estática, `y` é uma variável de instância `v[0]` é um elemento de matriz `a` é um parâmetro de valor `b` é um parâmetro de referência `c` é um parâmetro de saída, e `i` é uma variável local.

### <a name="static-variables"></a>Variáveis estáticas

Um campo declarado com o `static` modificador é chamado um ***variável estática***. Uma variável estática entra em existência antes da execução do construtor estático ([construtores estáticos](classes.md#static-constructors)) para seu tipo recipiente e deixe de existir quando o domínio de aplicativo associado deixa de existir.

O valor inicial de uma variável estática é o valor padrão ([valores padrão](variables.md#default-values)) do tipo da variável.

Para fins de verificação de atribuição definitiva, uma variável estática é considerada inicialmente atribuída.

### <a name="instance-variables"></a>Variáveis de instância

Um campo declarado sem o `static` modificador é chamado um ***variável de instância***.

#### <a name="instance-variables-in-classes"></a>Variáveis de instância em classes

Uma variável de instância de uma classe entra em existência quando uma nova instância dessa classe é criada e deixa de existir quando nenhuma referência a essa instância e o destruidor da instância (se houver) foi executada.

O valor inicial de uma variável de instância de uma classe é o valor padrão ([valores padrão](variables.md#default-values)) do tipo da variável.

Para fins de verificação de atribuição definitiva, uma variável de instância de uma classe é considerada inicialmente atribuída.

#### <a name="instance-variables-in-structs"></a>Variáveis de instância em structs

Uma variável de instância de um struct tem exatamente a mesma duração como a variável de struct ao qual ele pertence. Em outras palavras, quando uma variável de um tipo de struct entra em existência ou deixa de existir, isso também fazer as variáveis de instância do struct.

O estado de atribuição inicial de uma variável de instância de um struct é o mesmo que a variável que contém de struct. Em outras palavras, quando uma variável de struct é considerada inicialmente atribuída, isso também são suas variáveis de instância e quando uma variável de struct é considerada inicialmente não atribuída, suas variáveis de instância da mesma forma são não atribuídos.

### <a name="array-elements"></a>Elementos de matriz

Os elementos de uma matriz passam a existir quando uma instância de matriz é criada e deixam de existir quando não houver nenhuma referência a essa instância de matriz.

O valor inicial de cada um dos elementos de uma matriz é o valor padrão ([valores padrão](variables.md#default-values)) do tipo dos elementos da matriz.

Para fins de verificação de atribuição definitiva, um elemento de matriz é considerado inicialmente atribuída.

### <a name="value-parameters"></a>Parâmetros de valor

Um parâmetro declarado sem um `ref` ou `out` modificador é um ***parâmetro value***.

Um parâmetro de valor entra em existência mediante invocação do membro da função (método, construtor de instância, acessador ou operador) ou da função anônima para o qual o parâmetro pertence e é inicializado com o valor do argumento fornecido na invocação. Um parâmetro de valor normalmente deixa de existir após o retorno do membro da função ou função anônima. No entanto, se o parâmetro de valor é capturado por uma função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)), seu tempo de vida se estende de pelo menos até que o delegado ou árvore de expressão criada a partir dessa função anônima é elegível para coleta de lixo.

Para fins de verificação de atribuição definitiva, um parâmetro de valor é considerado inicialmente atribuída.

### <a name="reference-parameters"></a>Parâmetros de referência

Um parâmetro declarado com um `ref` modificador é uma ***parâmetro de referência***.

Um parâmetro de referência não cria um novo local de armazenamento. Em vez disso, um parâmetro de referência representa o mesmo local de armazenamento como a variável fornecido como o argumento na chamada de função anônima ou membro da função. Portanto, o valor de um parâmetro de referência é sempre o mesmo que a variável subjacente.

As regras de atribuição definitiva a seguir se aplicam a parâmetros de referência. Observe as regras diferentes para parâmetros de saída, descritos em [parâmetros de saída](variables.md#output-parameters).

*  Uma variável deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes que ela pode ser passada como um parâmetro de referência em uma invocação de delegado ou de membro de função.
*  Dentro de um membro da função ou função anônima, um parâmetro de referência é considerado inicialmente atribuído.

Dentro de um método de instância ou o acessador de instância de um tipo de struct, o `this` palavra-chave se comporta exatamente como um parâmetro de referência do tipo struct ([esse acesso](expressions.md#this-access)).

### <a name="output-parameters"></a>Parâmetros de saída

Um parâmetro declarado com um `out` modificador é um ***parâmetro de saída***.

Um parâmetro de saída não cria um novo local de armazenamento. Em vez disso, um parâmetro de saída representa o mesmo local de armazenamento como a variável fornecido como o argumento na invocação de delegado ou de membro da função. Portanto, o valor de um parâmetro de saída é sempre o mesmo que a variável subjacente.

As regras de atribuição definitiva a seguir se aplicam a parâmetros de saída. Observe as regras diferentes para parâmetros de referência, descritos em [fazer referência a parâmetros](variables.md#reference-parameters).

*  Uma variável não precisa ser definitivamente atribuída antes que ele pode ser passado como um parâmetro de saída em um membro da função ou invocação de delegado.
*  Após a conclusão normal de uma invocação de delegado ou de membro de função, cada variável que foi passado como um parâmetro de saída é considerado atribuído nesse caminho de execução.
*  Dentro de um membro da função ou função anônima, um parâmetro de saída é considerado inicialmente não atribuído.
*  Cada parâmetro de saída de um membro da função ou função anônima deve ser definitivamente atribuído ([atribuição definitiva](variables.md#definite-assignment)) antes da função de membro ou função anônima retorna normalmente.

Dentro de um construtor de instância de um tipo de struct, o `this` palavra-chave se comporta exatamente como um parâmetro de saída do tipo struct ([esse acesso](expressions.md#this-access)).

### <a name="local-variables"></a>Variáveis locais

Um ***variável local*** é declarado por uma *local_variable_declaration*, que podem ocorrer em um *bloco*, um *for_statement*, um *switch_statement* ou um *using_statement*; ou por um *foreach_statement* ou uma *specific_catch_clause* para um *try_statement*.

O tempo de vida de uma variável local é a parte da execução do programa durante o qual o armazenamento é garantido para ser reservado para ele. Esse tempo de vida se estende de pelo menos de entrada para o *bloco*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, ou *specific_catch_clause* à qual ele está associado, até a execução desse *bloco*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, ou *specific_catch_clause* extremidades de qualquer forma. (Inserir um contido *bloco* ou chamar um método suspende, mas não termina a execução do atual *bloco*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, ou *specific_catch_clause*.) Se a variável local é capturada por uma função anônima ([capturados variáveis externas](expressions.md#captured-outer-variables)), seu tempo de vida se estende de pelo menos até que a árvore de expressão ou delegado criada a partir da função anônima, juntamente com quaisquer outros objetos que vêm para fazer referência à variável capturada, está qualificado para coleta de lixo.

Se o pai *bloco*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, ou *specific_catch_clause* é inserido recursivamente, uma nova instância da variável local é criada cada vez e seu *local_variable_initializer*, se houver, é avaliada cada vez.

Uma variável local introduzida por uma *local_variable_declaration* não é inicializado automaticamente e, portanto, não tem valor padrão. Para fins de verificação de atribuição definitiva, uma variável local introduzidos por uma *local_variable_declaration* é considerado inicialmente não atribuídas. Um *local_variable_declaration* pode incluir uma *local_variable_initializer*, caso em que a variável é considerada definitivamente atribuída apenas após a expressão de inicialização ([ Instruções de declaração](variables.md#declaration-statements)).

Dentro do escopo de uma variável local introduzido por uma *local_variable_declaration*, ele é um erro de tempo de compilação para se referir a essa variável local em uma posição textual que precede seu *local_variable_declarator*. Se a declaração de variável local é implícita ([declarações de variável Local](statements.md#local-variable-declarations)), também é um erro ao fazer referência à variável dentro do seu *local_variable_declarator*.

Uma variável local introduzida por uma *foreach_statement* ou um *specific_catch_clause* é considerado atribuído definitivamente em seu escopo inteiro.

O tempo de vida real de uma variável local é dependente de implementação. Por exemplo, um compilador estaticamente pode determinar que uma variável local em um bloco só é usada para uma pequena parte desse bloco. Usando essa análise, o compilador pode gerar código que resulta em ter um tempo de vida mais curto que o bloco de armazenamento da variável.

O armazenamento referenciado por uma variável de referência local seja recuperado, independentemente do tempo de vida da variável local de referência ([gerenciamento automático de memória](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Valores padrão

As seguintes categorias de variáveis são inicializadas automaticamente para seus valores padrão:

*  Variáveis estáticas.
*  Variáveis de instância de instâncias de classes.
*  Elementos da matriz.

O valor padrão de uma variável depende do tipo da variável e é determinado da seguinte maneira:

*  Para uma variável de um *value_type*, o valor padrão é o mesmo que o valor calculado pelo *value_type*do construtor padrão ([1&gt;construtores padrão](types.md#default-constructors)).
*  Para uma variável de um *reference_type*, o valor padrão é `null`.

Inicialização para valores padrão normalmente é feita fazendo com que o Gerenciador de memória ou o coletor de lixo inicializar a memória para todos os bits-zero antes que ele é alocado para uso. Por esse motivo, é conveniente usar zeros bits para representar a referência nula.

## <a name="definite-assignment"></a>Atribuição definida

Em um determinado local no código executável de um membro da função, uma variável é considerada ***definitivamente atribuído*** se o compilador pode comprovar a análise de fluxo estático particular ([precisas regras para determinar definitiva atribuição](variables.md#precise-rules-for-determining-definite-assignment)), que a variável foi inicializada automaticamente ou foi o destino de pelo menos uma atribuição. As regras de atribuição definitiva informalmente mencionado, são:

*  Uma variável inicialmente atribuída ([inicialmente atribuída variáveis](variables.md#initially-assigned-variables)) é sempre considerado atribuído definitivamente.
*  Uma variável não atribuída inicialmente ([inicialmente não atribuídos a variáveis](variables.md#initially-unassigned-variables)) é considerado definitivamente atribuído em um determinado local se todos os possíveis caminhos de execução levando a esse local contém pelo menos um dos seguintes:
    * Uma atribuição simples ([atribuição simples](expressions.md#simple-assignment)) no qual a variável é o operando esquerdo.
    * Uma expressão de invocação ([expressões de invocação](expressions.md#invocation-expressions)) ou uma expressão de criação de objeto ([expressões de criação do objeto](expressions.md#object-creation-expressions)) que passa a variável como um parâmetro de saída.
    * Para uma variável local, uma declaração de variável local ([declarações de variável Local](statements.md#local-variable-declarations)) que inclui um inicializador de variável.

A especificação formal subjacente informais regras acima é descrita em [inicialmente atribuída variáveis](variables.md#initially-assigned-variables), [inicialmente não atribuídos a variáveis](variables.md#initially-unassigned-variables), e [precisas regras para determinar atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment).

Os estados de atribuição definitiva de variáveis de instância de um *struct_type* variável são controladas individualmente, bem como coletivamente. Em adicional para as regras acima, as seguintes regras se aplicam a *struct_type* variáveis e suas variáveis de instância:

*  Uma variável de instância é considerada atribuída definitivamente se contendo seu *struct_type* variável é considerada atribuída definitivamente.
*  Um *struct_type* variável é considerada atribuída definitivamente se cada uma das suas variáveis de instância é considerada atribuída definitivamente.

Atribuição definida é um requisito nos seguintes contextos:

*  Uma variável deve ser definitivamente atribuída em cada local em que seu valor é obtido. Isso garante que nunca ocorrerem valores indefinidos. A ocorrência de uma variável em uma expressão é considerada para obter o valor da variável, exceto quando
    * a variável é o operando esquerdo de uma atribuição simples,
    * a variável é passada como um parâmetro de saída, ou
    * a variável é uma *struct_type* variável e ocorre como o operando esquerdo de um acesso de membro.
*  Uma variável deve ser definitivamente atribuída em cada local em que ele é passado como um parâmetro de referência. Isso garante que o membro da função que está sendo invocado pode considerar o parâmetro de referência que inicialmente atribuído.
*  Todos os parâmetros de saída de um membro da função devem ser definitivamente atribuídos em cada local em que retorna o membro da função (por meio de um `return` instrução ou por meio de atingir o final do corpo de membro da função de execução). Isso garante que os membros da função não retornam valores indefinidos em parâmetros de saída, permitindo que o compilador considere uma invocação de membro de função que usa uma variável como um parâmetro de saída equivalente a uma atribuição à variável.
*  O `this` variável de um *struct_type* construtor de instância deve ser definitivamente atribuído em cada local em que esse construtor de instância retorna.

### <a name="initially-assigned-variables"></a>Variáveis atribuídas inicialmente

As seguintes categorias de variáveis são classificadas como inicialmente atribuída:

*  Variáveis estáticas.
*  Variáveis de instância de instâncias de classes.
*  Variáveis de instância de variáveis de struct inicialmente atribuída.
*  Elementos da matriz.
*  Parâmetros de valor.
*  Parâmetros de referência.
*  Variáveis declaradas em uma `catch` cláusula ou um `foreach` instrução.

### <a name="initially-unassigned-variables"></a>Variáveis inicialmente não atribuídas

As seguintes categorias de variáveis são classificadas como inicialmente não atribuídas:

*  Variáveis de instância de struct atribuídas inicialmente variáveis.
*  Parâmetros de saída, incluindo o `this` variável dos construtores de instância de struct.
*  Variáveis locais, exceto aqueles declarados em uma `catch` cláusula ou um `foreach` instrução.

### <a name="precise-rules-for-determining-definite-assignment"></a>Precisas regras para determinar a atribuição definida

Para determinar o que cada variável usada é definitivamente atribuído, o compilador deve usar um processo que é equivalente ao descrito nesta seção.

O compilador processa o corpo de cada membro da função que tem uma ou mais variáveis atribuídas inicialmente. Para cada variável inicialmente não atribuída *v*, o compilador determina uma ***estado de atribuição definitiva*** para *v* em cada um dos pontos a seguir no membro da função:

*  No início de cada instrução
*  No ponto de extremidade ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)) de cada instrução
*  Em cada arco que transfere o controle para outra instrução ou para o ponto de extremidade de uma instrução
*  No início de cada expressão
*  No final de cada expressão

O estado de atribuição definitiva da *v* pode ser:

*  Definitivamente atribuído. Isso indica que em todos os fluxos de controle possíveis para esse ponto *v* foi atribuído um valor.
*  Não atribuído definitivamente. Para o estado de uma variável no final de uma expressão do tipo `bool`, o estado de uma variável que não é atribuída definitivamente maio (mas não necessariamente) se enquadram em um dos seguintes estados de subpropriedades:
    * Definitivamente atribuído após a expressão verdadeira. Este estado indica que *v* é atribuída definitivamente se a expressão booliana avaliada como verdadeira, mas não é necessariamente atribuída se a expressão booliana avaliada como false.
    * Definitivamente atribuído após a expressão de false. Este estado indica que *v* é atribuída definitivamente se a expressão booliana avaliada como false, mas não é necessariamente atribuída se a expressão booliana avaliada como verdadeira.

As seguintes regras regem como o estado de uma variável *v* é determinado em cada local.

#### <a name="general-rules-for-statements"></a>Regras gerais para instruções

*  *v* não está definitivamente atribuída no início do corpo da função membro.
*  *v* é definitivamente atribuída no início de qualquer instrução inacessível.
*  O estado de atribuição definitiva da *v* no início de qualquer outra instrução é determinada verificando o estado de atribuição definitiva da *v* em todas as transferências de fluxo de controle que direcionam o início do que instrução. Se (e somente se) *v* é atribuída definitivamente em todas essas transferências de fluxo de controle, em seguida, *v* é definitivamente atribuída no início da instrução. O conjunto de transferências de fluxo de controle possíveis é determinado da mesma forma que para verificação de acessibilidade de instrução ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)).
*  O estado de atribuição definitiva da *v* no ponto de extremidade de um bloco `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, ou `switch` é determinado pela verificação do estado de atribuição definitiva de *v* em todas as transferências de fluxo de controle que o ponto de extremidade dessa instrução de destino. Se *v* é atribuída definitivamente em todas essas transferências de fluxo de controle, em seguida, *v* será definitivamente atribuído no ponto de extremidade da instrução. Caso contrário, *v* não está definitivamente atribuída no ponto de extremidade da instrução. O conjunto de transferências de fluxo de controle possíveis é determinado da mesma forma que para verificação de acessibilidade de instrução ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Instruções de bloco, verificadas e instruções não verificadas

O estado de atribuição definitiva da *v* no controle de transferência para a primeira instrução da lista de instruções no bloco (ou até o ponto final do bloco, se a lista de instrução estiver vazia) é o mesmo que a instrução de atribuição definitiva de *v* antes do bloco `checked`, ou `unchecked` instrução.

#### <a name="expression-statements"></a>Instruções de expressão

Para uma instrução de expressão *stmt* que consiste a expressão *expr*:

*  *v* tem o mesmo estado de atribuição definitiva no início de *expr* assim como no início de *stmt*.
*  Se *v* se definitivamente atribuída no final da *expr*, será definitivamente atribuído no ponto de extremidade de *stmt*; caso contrário, ele não está definitivamente atribuído no ponto de extremidade de *stmt*.

#### <a name="declaration-statements"></a>Instruções de declaração

*  Se *stmt* é uma instrução de declaração sem inicializadores, em seguida, *v* tem o mesmo estado de atribuição definidas no ponto de extremidade de *stmt* assim como no início do *stmt*.
*  Se *stmt* é uma instrução de declaração com inicializadores, em seguida, o estado de atribuição definitiva para *v* é determinado como se *stmt* fosse uma lista de instrução, com uma atribuição instrução de cada declaração com um inicializador (na ordem de declaração).

#### <a name="if-statements"></a>Se as instruções

Para um `if` instrução *stmt* do formulário:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* tem o mesmo estado de atribuição definitiva no início de *expr* assim como no início de *stmt*.
*  Se *v* é definitivamente atribuída no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para *then_stmt* para *else_stmt*  ou para o ponto de extremidade da *stmt* se não houver nenhuma cláusula else.
*  Se *v* tem o estado "definitivamente atribuído após a expressão verdadeira" no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para *then_stmt*e não atribuído definitivamente sobre a transferência de fluxo de controle para o *else_stmt* ou para o ponto de extremidade da *stmt* se não houver nenhuma cláusula else.
*  Se *v* tem o estado "definitivamente atribuído após a expressão false" no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para *else_stmt*e não definitivamente atribuído a transferência de fluxo de controle para *then_stmt*. Ele é atribuído definitivamente no ponto de extremidade de *stmt* somente se ele é atribuído definitivamente no ponto de extremidade de *then_stmt*.
*  Caso contrário, *v* é considerada como não atribuído definitivamente sobre a transferência de fluxo de controle para qualquer um os *then_stmt* ou *else_stmt*, ou para o ponto de extremidade de  *stmt* se não houver nenhuma cláusula else.

#### <a name="switch-statements"></a>Instruções switch

Em um `switch` instrução *stmt* com uma expressão de controle *expr*:

*  O estado de atribuição definitiva da *v* no início da *expr* é o mesmo que o estado de *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* no fluxo de controle de transferência para uma lista de instruções do bloco switch acessível é o mesmo que o estado de atribuição definitiva da *v* no final do *expr*.

#### <a name="while-statements"></a>Enquanto as instruções

Para um `while` instrução *stmt* do formulário:
```csharp
while ( expr ) while_body
```

*  *v* tem o mesmo estado de atribuição definitiva no início de *expr* assim como no início de *stmt*.
*  Se *v* é definitivamente atribuída no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para *while_body* e para o ponto de extremidade de  *stmt*.
*  Se *v* tem o estado "definitivamente atribuído após a expressão verdadeira" no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para *while_body*, mas não definitivamente atribuídas no ponto de extremidade de *stmt*.
*  Se *v* tem o estado "definitivamente atribuído após a expressão false" no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para o ponto de extremidade de *stmt* , mas não tiver sido atribuído definitivamente sobre a transferência de fluxo de controle para *while_body*.

#### <a name="do-statements"></a>Siga as instruções

Para um `do` instrução *stmt* do formulário:
```csharp
do do_body while ( expr ) ;
```

*  *v* tem o mesmo estado de atribuição definitiva sobre a transferência de fluxo de controle do início da *stmt* à *do_body* assim como no início da *stmt*.
*  *v* tem o mesmo estado de atribuição definitiva no início de *expr* assim como acontece no ponto de extremidade do *do_body*.
*  Se *v* é definitivamente atribuída no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para o ponto de extremidade de *stmt*.
*  Se *v* tem o estado "definitivamente atribuído após a expressão false" no final da *expr*, em seguida, ele é atribuído definitivamente sobre a transferência de fluxo de controle para o ponto de extremidade de *stmt* .

#### <a name="for-statements"></a>Para instruções

Atribuição definitiva procurando um `for` instrução do formulário:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
é feito como se a instrução estivesse escrita:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Se o *for_condition* é omitido do `for` instrução e, em seguida, avaliação de atribuição definitiva continua como se *for_condition* foram substituídos por `true` na expansão acima .

#### <a name="break-continue-and-goto-statements"></a>Interromper, continuar e as instruções goto

O estado de atribuição definitiva da *v* sobre a transferência de fluxo de controle causada por uma `break`, `continue`, ou `goto` instrução é o mesmo que o estado de atribuição definitiva da *v* no início da instrução.

#### <a name="throw-statements"></a>Instruções throw

Para uma instrução *stmt* do formulário
```csharp
throw expr ;
```

O estado de atribuição definitiva da *v* no início da *expr* é o mesmo que o estado de atribuição definitiva da *v* no início do *stmt*.

#### <a name="return-statements"></a>Instruções de retorno

Para uma instrução *stmt* do formulário
```csharp
return expr ;
```

*  O estado de atribuição definitiva da *v* no início da *expr* é o mesmo que o estado de atribuição definitiva da *v* no início do *stmt*.
*  Se *v* é um parâmetro de saída, em seguida, ele deve ser definitivamente atribuído:
    * Depois de *expr*
    * ou no final do `finally` block de um `try` - `finally` ou `try` - `catch` - `finally` que circunscreve o `return` instrução.

Para uma instrução INSERT de instrução do formulário:
```csharp
return ;
```

*  Se *v* é um parâmetro de saída, em seguida, ele deve ser definitivamente atribuído:
    * antes de *stmt*
    * ou no final do `finally` block de um `try` - `finally` ou `try` - `catch` - `finally` que circunscreve o `return` instrução.

#### <a name="try-catch-statements"></a>Instruções try-catch

Para uma instrução *stmt* do formulário:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  O estado de atribuição definitiva da *v* no início da *try_block* é o mesmo que o estado de atribuição definitiva da *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* no início da *catch_block_i* (para qualquer *eu*) é o mesmo que o estado de atribuição definitiva de *v*no início de *stmt*.
*  O estado de atribuição definitiva da *v* no ponto de extremidade de *stmt* é atribuído definitivamente se (e somente se) *v* definitivamente é atribuído no ponto de extremidade de  *try_block* e cada *catch_block_i* (para cada *eu* de 1 a *n*).

#### <a name="try-finally-statements"></a>Instruções try-finally

Para um `try` instrução *stmt* do formulário:
```csharp
try try_block finally finally_block
```

*  O estado de atribuição definitiva da *v* no início da *try_block* é o mesmo que o estado de atribuição definitiva da *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* no início da *finally_block* é o mesmo que o estado de atribuição definitiva da *v* no início do *stmt* .
*  O estado de atribuição definitiva da *v* no ponto de extremidade de *stmt* é atribuído definitivamente se (e somente se) pelo menos uma das seguintes opções for verdadeira:
    * *v* será definitivamente atribuído no ponto de extremidade de *try_block*
    * *v* será definitivamente atribuído no ponto de extremidade de *finally_block*

Se uma transferência de fluxo de controle (por exemplo, uma `goto` instrução) que for feita começa dentro *try_block*e termina fora do *try_block*, em seguida, *v* também é considerado atribuído definitivamente nessa transferência de fluxo de controle se *v* é definitivamente atribuídas no ponto de extremidade de *finally_block*. (Isso não é um somente se — se *v* definitivamente atribuído por outro motivo nessa transferência de fluxo de controle, em seguida, ela ainda será considerada atribuído definitivamente.)

#### <a name="try-catch-finally-statements"></a>Instruções try-catch-finally

Análise de atribuição definitiva para um `try` - `catch` - `finally` instrução do formulário:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
é feito como se fosse a instrução de um `try` - `finally` instrução colocando um `try` - `catch` instrução:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

O exemplo a seguir demonstra como os blocos diferentes de um `try` instrução ([a instrução try](statements.md#the-try-statement)) afetam atribuição definitiva.
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Instruções de foreach

Para um `foreach` instrução *stmt* do formulário:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  O estado de atribuição definitiva da *v* no início da *expr* é o mesmo que o estado de *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* sobre a transferência de fluxo de controle para *embedded_statement* ou para o ponto de extremidade de *stmt* é o mesmo que o estado do *v* no final do *expr*.

#### <a name="using-statements"></a>Instruções de uso

Para um `using` instrução *stmt* do formulário:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  O estado de atribuição definitiva da *v* no início da *resource_acquisition* é o mesmo que o estado de *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* sobre a transferência de fluxo de controle para *embedded_statement* é o mesmo que o estado de *v* no final do *resource_ aquisição*.

#### <a name="lock-statements"></a>Instruções de bloqueio

Para um `lock` instrução *stmt* do formulário:
```csharp
lock ( expr ) embedded_statement
```

*  O estado de atribuição definitiva da *v* no início da *expr* é o mesmo que o estado de *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* sobre a transferência de fluxo de controle para *embedded_statement* é o mesmo que o estado de *v* no final do *expr*.

#### <a name="yield-statements"></a>Instruções yield

Para um `yield return` instrução *stmt* do formulário:
```csharp
yield return expr ;
```

*  O estado de atribuição definitiva da *v* no início da *expr* é o mesmo que o estado de *v* no início do *stmt*.
*  O estado de atribuição definitiva da *v* no final da *stmt* é o mesmo que o estado de *v* no final da *expr*.
*  Um `yield break` instrução não tem nenhum efeito sobre o estado de atribuição definitiva.

#### <a name="general-rules-for-simple-expressions"></a>Regras gerais para expressões simples

A regra a seguir se aplica a esses tipos de expressões: literais ([literais](expressions.md#literals)), os nomes simples ([nomes simples](expressions.md#simple-names)), expressões de acesso de membro ([acesso de membro](expressions.md#member-access)), expressões de acesso de base não indexada ([acesso de Base](expressions.md#base-access)), `typeof` expressões ([o operador typeof](expressions.md#the-typeof-operator)), expressões de valor padrão ([expressões de valor padrão ](expressions.md#default-value-expressions)) e `nameof` expressões ([expressões Nameof](expressions.md#nameof-expressions)).

*  O estado de atribuição definitiva da *v* no final de uma expressão é o mesmo que o estado de atribuição definitiva da *v* no início da expressão.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Regras gerais para expressões com expressões incorporadas

As seguintes regras se aplicam a esses tipos de expressões: expressões entre parênteses ([expressões entre parênteses](expressions.md#parenthesized-expressions)), expressões de acesso de elemento ([acesso de elemento](expressions.md#element-access)) Base acessar expressões com indexação ([acesso de Base](expressions.md#base-access)), o incremento e decréscimo expressões ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators), [incremento de prefixo e de decremento operadores](expressions.md#prefix-increment-and-decrement-operators)), expressões de conversão ([expressões de conversão](expressions.md#cast-expressions)), unário `+`, `-`, `~`, `*` expressões, binárias `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressões ([operadores aritméticos](expressions.md#arithmetic-operators), [operadores Shift](expressions.md#shift-operators), [operadores de teste de tipo e relacional](expressions.md#relational-and-type-testing-operators) [Operadores lógicos](expressions.md#logical-operators)), composta de expressões de atribuição ([atribuição composta](expressions.md#compound-assignment)), `checked` e `unchecked` expressões ([checked e unchecked operadores](expressions.md#the-checked-and-unchecked-operators)), além de expressões de criação de matriz e o delegado ([o novo operador](expressions.md#the-new-operator)).

Cada uma dessas expressões tem um ou mais subexpressões que incondicionalmente são avaliadas em uma ordem fixa. Por exemplo, o binário `%` operador avalia o lado esquerdo do operador e, em seguida, o lado direito. Uma operação de indexação avalia a expressão indexada e, em seguida, avalia cada uma das expressões de índice, em ordem da esquerda para a direita. Para uma expressão *expr*, que tem subexpressões *e1, e2,..., eN*, avaliadas nesta ordem:

*  O estado de atribuição definitiva da *v* no início da *e1* é o mesmo que o estado de atribuição definitiva no início da *expr*.
*  O estado de atribuição definitiva da *v* no início da *ei* (*eu* maior do que um) é o mesmo que o estado de atribuição definitiva no final a subexpressão anterior.
*  O estado de atribuição definitiva da *v* no final da *expr* é o mesmo que o estado de atribuição definitiva do final do *eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Expressões de invocação e expressões de criação de objeto

Para uma expressão de invocação *expr* do formulário:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
ou uma expressão de criação de objeto do formulário:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Para uma expressão de invocação, o estado de atribuição definitiva da *v* antes de *primary_expression* é o mesmo que o estado de *v* antes de *expr*.
*  Para uma expressão de invocação, o estado de atribuição definitiva da *v* antes de *arg1* é o mesmo que o estado de *v* depois *primary_expression*.
*  Para uma expressão de criação de objeto, o estado de atribuição definitiva da *v* antes de *arg1* é o mesmo que o estado de *v* antes *expr*.
*  Para cada argumento *argi*, o estado de atribuição definitiva da *v* após *argi* é determinado pelas regras de expressão normal, ignorando qualquer `ref` ou `out`modificadores.
*  Para cada argumento *argi* para qualquer *eu* maior do que um, o estado de atribuição definitiva da *v* antes *argi* é o mesmo que o estado do *v* após anterior *arg*.
*  Se a variável *v* é passado como um `out` argumento (ou seja, um argumento do formulário `out v`) em qualquer um dos argumentos, em seguida, o estado de *v* depois *expr* é atribuída definitivamente. Caso contrário, o estado de *v* após *expr* é o mesmo que o estado de *v* depois *argN*.
*  Para inicializadores de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)), inicializadores de objeto ([inicializadores de objeto](expressions.md#object-initializers)), inicializadores de coleção ([inicializadores de coleção](expressions.md#collection-initializers)) e inicializadores de objeto anônimos ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)), o estado de atribuição definitiva é determinado pelo que essas construções são definidas em termos de expansão.

#### <a name="simple-assignment-expressions"></a>Expressões de atribuição simples

Para uma expressão *expr* do formulário `w = expr_rhs`:

*  O estado de atribuição definitiva da *v* antes de *expr_rhs* é o mesmo que o estado de atribuição definitiva da *v* antes *expr*.
*  O estado de atribuição definitiva da *v* após *expr* é determinado por:
   * Se *w* é a mesma variável como *v*, em seguida, o estado de atribuição definitiva de *v* depois *expr* será definitivamente atribuído.
   * Caso contrário, se a atribuição ocorre dentro do construtor de instância de um tipo de struct, se *w* é um acesso de propriedade que designa uma propriedade implementada automaticamente *P* na instância que está sendo construída e *v* é o campo oculto de backup de *P*, em seguida, o estado de atribuição definitiva de *v* depois *expr* é definitivamente atribuído.
   * Caso contrário, o estado de atribuição definitiva da *v* após *expr* é o mesmo que o estado de atribuição definitiva da *v* depois *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (AND condicional) expressões

Para uma expressão *expr* do formulário `expr_first && expr_second`:

*  O estado de atribuição definitiva da *v* antes de *expr_first* é o mesmo que o estado de atribuição definitiva da *v* antes *expr*.
*  O estado de atribuição definitiva da *v* antes de *expr_second* é atribuída definitivamente se o estado de *v* depois *expr_first* é definitivamente atribuído ou "atribuído definitivamente após a expressão verdadeira". Caso contrário, ele não está definitivamente atribuído.
*  O estado de atribuição definitiva da *v* após *expr* é determinado por:
    * Se *expr_first* é uma expressão constante com o valor `false`, em seguida, o estado de atribuição definitiva de *v* depois *expr* é o mesmo que a atribuição definida estado de *v* após *expr_first*.
    * Caso contrário, se o estado de *v* após *expr_first* é definitivamente atribuído, em seguida, o estado de *v* depois *expr* será definitivamente atribuído.
    * Caso contrário, se o estado de *v* após *expr_second* definitivamente atribuído e o estado de *v* depois *expr_first* é "definitivamente atribuído após a expressão false", em seguida, o estado de *v* após *expr* será definitivamente atribuído.
    * Caso contrário, se o estado de *v* após *expr_second* será definitivamente atribuído ou "atribuído definitivamente após a expressão verdadeira", em seguida, o estado de *v* depois  *Expr* é "definitivamente atribuído após a expressão verdadeira".
    * Caso contrário, se o estado de *v* após *expr_first* é "definitivamente atribuído após a expressão false" e o estado de *v* depois *expr_second* é "definitivamente atribuído após a expressão false", em seguida, o estado do *v* depois *expr* é "definitivamente atribuído após a expressão false".
    * Caso contrário, o estado de *v* após *expr* definitivamente não foi atribuído.

No exemplo
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
a variável `i` é considerado definitivamente atribuído em uma das instruções inseridas de um `if` instrução, mas não na outra. No `if` instrução no método `F`, a variável `i` definitivamente atribuído na primeira instrução inserida porque a execução da expressão `(i = y)` sempre precede a execução desta instrução inserida. Por outro lado, a variável `i` não está definitivamente atribuída na segunda instrução inserida, desde `x >= 0` talvez testou falso, resultando na variável `i` sem atribuição.

#### <a name="-conditional-or-expressions"></a>|| (OR condicional) expressões

Para uma expressão *expr* do formulário `expr_first || expr_second`:

*  O estado de atribuição definitiva da *v* antes de *expr_first* é o mesmo que o estado de atribuição definitiva da *v* antes *expr*.
*  O estado de atribuição definitiva da *v* antes de *expr_second* é atribuída definitivamente se o estado de *v* depois *expr_first* é definitivamente atribuído ou "atribuído definitivamente depois expressão false". Caso contrário, ele não está definitivamente atribuído.
*  A instrução de atribuição definitiva de *v* após *expr* é determinado por:
    * Se *expr_first* é uma expressão constante com o valor `true`, em seguida, o estado de atribuição definitiva de *v* depois *expr* é o mesmo que a atribuição definida estado de *v* após *expr_first*.
    * Caso contrário, se o estado de *v* após *expr_first* é definitivamente atribuído, em seguida, o estado de *v* depois *expr* será definitivamente atribuído.
    * Caso contrário, se o estado de *v* após *expr_second* definitivamente atribuído e o estado de *v* depois *expr_first* é "definitivamente atribuído após a expressão verdadeira", em seguida, o estado de *v* após *expr* será definitivamente atribuído.
    * Caso contrário, se o estado de *v* após *expr_second* será definitivamente atribuído ou "atribuído definitivamente depois expressão false", em seguida, o estado de *v* após *expr* é "definitivamente atribuído após a expressão false".
    * Caso contrário, se o estado de *v* após *expr_first* é "definitivamente atribuído após a expressão verdadeira" e o estado de *v* depois *expr_second*é "definitivamente atribuído após a expressão verdadeira", em seguida, o estado da *v* após *expr* é "definitivamente atribuído após a expressão verdadeira".
    * Caso contrário, o estado de *v* após *expr* definitivamente não foi atribuído.

No exemplo
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
a variável `i` é considerado definitivamente atribuído em uma das instruções inseridas de um `if` instrução, mas não na outra. No `if` instrução no método `G`, a variável `i` definitivamente atribuído na segunda instrução inserida porque a execução da expressão `(i = y)` sempre precede a execução desta instrução inserida. Por outro lado, a variável `i` não está definitivamente atribuída na primeira instrução inserida, desde `x >= 0` talvez testou true, resultando na variável `i` sem atribuição.

#### <a name="-logical-negation-expressions"></a>! expressões (negação lógica)

Para uma expressão *expr* do formulário `! expr_operand`:

*  O estado de atribuição definitiva da *v* antes de *expr_operand* é o mesmo que o estado de atribuição definitiva da *v* antes *expr*.
*  O estado de atribuição definitiva da *v* após *expr* é determinado por:
    * Se o estado de *v* após * expr_operand * é definitivamente atribuído, em seguida, o estado do *v* após *expr* será definitivamente atribuído.
    * Se o estado de *v* após * expr_operand * não está definitivamente atribuída, em seguida, o estado do *v* após *expr* definitivamente não foi atribuído.
    * Se o estado de *v* após * expr_operand * é "definitivamente atribuído após a expressão false", em seguida, o estado do *v* após *expr* é "definitivamente atribuída após o verdadeiro expressão".
    * Se o estado de *v* após * expr_operand * é "definitivamente atribuído após a expressão verdadeira", em seguida, o estado do *v* após *expr* é "definitivamente atribuída após o falso expressão".

#### <a name="-null-coalescing-expressions"></a>?? expressões (união de nulo)

Para uma expressão *expr* do formulário `expr_first ?? expr_second`:

*  O estado de atribuição definitiva da *v* antes de *expr_first* é o mesmo que o estado de atribuição definitiva da *v* antes *expr*.
*  O estado de atribuição definitiva da *v* antes de *expr_second* é o mesmo que o estado de atribuição definitiva da *v* depois *expr_first*.
*  A instrução de atribuição definitiva de *v* após *expr* é determinado por:
    * Se *expr_first* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) com valor nulo, em seguida, ao estado de *v* depois *expr* é o mesmo como o estado de *v* após *expr_second*.
*  Caso contrário, o estado de *v* após *expr* é o mesmo que o estado de atribuição definitiva da *v* depois *expr_first*.

#### <a name="-conditional-expressions"></a>?: (condicionais) expressões

Para uma expressão *expr* do formulário `expr_cond ? expr_true : expr_false`:

*  O estado de atribuição definitiva da *v* antes de *expr_cond* é o mesmo que o estado de *v* antes *expr*.
*  O estado de atribuição definitiva da *v* antes de *expr_true* é atribuída definitivamente se e somente se uma das opções a seguir contém:
    * *expr_cond* é uma expressão constante com o valor `false`
    * o estado de *v* após *expr_cond* definitivamente atribuído ou "definitivamente atribuído após a expressão verdadeira".
*  O estado de atribuição definitiva da *v* antes de *expr_false* é atribuída definitivamente se e somente se uma das opções a seguir contém:
    * *expr_cond* é uma expressão constante com o valor `true`
*  o estado de *v* após *expr_cond* definitivamente atribuído ou "definitivamente atribuído após a expressão false".
*  O estado de atribuição definitiva da *v* após *expr* é determinado por:
    * Se *expr_cond* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) com o valor `true` , em seguida, o estado do *v* depois *expr* é o mesmo que o estado de *v* após *expr_true*.
    * Caso contrário, se *expr_cond* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) com o valor `false` , em seguida, o estado do *v* depois *expr* é o mesmo que o estado do *v* depois *expr_false*.
    * Caso contrário, se o estado de *v* após *expr_true* será definitivamente atribuído e o estado de *v* depois *expr_false* é definitivamente atribuído, em seguida, o estado de *v* após *expr* será definitivamente atribuído.
    * Caso contrário, o estado de *v* após *expr* definitivamente não foi atribuído.

#### <a name="anonymous-functions"></a>Funções anônimas

Para um *lambda_expression* ou *anonymous_method_expression* *expr* com um corpo (ambos *bloco* ou *expressão* ) *corpo*:

*  O estado de atribuição definitiva de uma variável externa *v* antes de *corpo* é o mesmo que o estado de *v* antes *expr*. Ou seja, estado de atribuição definitiva de variáveis externas é herdado do contexto da função anônima.
*  O estado de atribuição definitiva de uma variável externa *v* após *expr* é o mesmo que o estado de *v* antes *expr*.

O exemplo
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
gera um erro de tempo de compilação desde `max` não está definitivamente atribuída no qual a função anônima é declarada. O exemplo
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
também gera um erro de tempo de compilação desde a atribuição ao `n` na função anônima que não tem nenhum efeito sobre o estado de atribuição definitiva da `n` fora da função anônima.

## <a name="variable-references"></a>Referências de variável

Um *variable_reference* é um *expressão* que é classificado como uma variável. Um *variable_reference* denota um local de armazenamento que pode ser acessado para buscar o valor atual e para armazenar um novo valor.

```antlr
variable_reference
    : expression
    ;
```

Em C e C++, uma *variable_reference* é conhecido como um *lvalue*.

## <a name="atomicity-of-variable-references"></a>Atomicidade de referências de variáveis

Leituras e gravações dos seguintes tipos de dados são atômicas: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`e tipos de referência. Além disso, leituras e gravações de tipos de enumeração com um tipo subjacente na lista anterior também são atômicas. Leituras e gravações de outros tipos, incluindo `long`, `ulong`, `double`, e `decimal`, bem como tipos definidos pelo usuário, não há garantia de ser atômicas. Além das funções de biblioteca criadas para essa finalidade, há nenhuma garantia de atômica modificar-leitura, como no caso de incremento ou decremento.

