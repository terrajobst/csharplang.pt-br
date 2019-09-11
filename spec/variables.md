---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876801"
---
# <a name="variables"></a>Variáveis

As variáveis representam locais de armazenamento. Cada variável tem um tipo que determina quais valores podem ser armazenados na variável. C#é um idioma de tipo seguro, e o C# compilador garante que os valores armazenados em variáveis sejam sempre do tipo apropriado. O valor de uma variável pode ser alterado por atribuição ou pelo uso dos `++` operadores e. `--`

Uma variável deve ser ***definitivamente atribuída*** ([atribuição definitiva](variables.md#definite-assignment)) antes que seu valor possa ser obtido.

Conforme descrito nas seções a seguir, as variáveis são ***inicialmente atribuídas*** ou ***inicialmente não atribuídas***. Uma variável atribuída inicialmente tem um valor inicial bem definido e é sempre considerada definitivamente atribuída. Uma variável inicialmente não atribuída não tem valor inicial. Para que uma variável inicialmente não atribuída seja considerada definitivamente atribuída em determinado local, uma atribuição para a variável deve ocorrer em cada caminho de execução possível, levando a esse local.

## <a name="variable-categories"></a>Categorias de variáveis

C#define sete categorias de variáveis: variáveis estáticas, variáveis de instância, elementos de matriz, parâmetros de valor, parâmetros de referência, parâmetros de saída e variáveis locais. As seções a seguir descrevem cada uma dessas categorias.

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
`x`é uma variável estática, `y` é uma variável de instância `v[0]` , é um elemento de `a` matriz, é um parâmetro `b` de valor, é um `c` parâmetro de referência, é um `i` parâmetro de saída e é uma variável local .

### <a name="static-variables"></a>Variáveis estáticas

Um campo declarado com o `static` modificador é chamado de uma ***variável estática***. Uma variável estática entra em existência antes da execução do construtor estático ([construtores estáticos](classes.md#static-constructors)) para seu tipo recipiente e deixa de existir quando o domínio do aplicativo associado deixar de existir.

O valor inicial de uma variável estática é o valor padrão ([valores padrão](variables.md#default-values)) do tipo da variável.

Para fins de verificação de atribuição definitiva, uma variável estática é considerada inicialmente atribuída.

### <a name="instance-variables"></a>Variáveis de instância

Um campo declarado sem o `static` modificador é chamado de ***variável de instância***.

#### <a name="instance-variables-in-classes"></a>Variáveis de instância em classes

Uma variável de instância de uma classe entra em existência quando uma nova instância dessa classe é criada e deixa de existir quando não há nenhuma referência a essa instância e o destruidor da instância (se houver) tiver sido executado.

O valor inicial de uma variável de instância de uma classe é o valor padrão ([valores padrão](variables.md#default-values)) do tipo da variável.

Para fins de verificação de atribuição definitiva, uma variável de instância de uma classe é considerada inicialmente atribuída.

#### <a name="instance-variables-in-structs"></a>Variáveis de instância em structs

Uma variável de instância de uma struct tem exatamente o mesmo tempo de vida que a variável de struct à qual ela pertence. Em outras palavras, quando uma variável de um tipo struct entra em existência ou deixa de existir, também as variáveis de instância do struct.

O estado de atribuição inicial de uma variável de instância de uma struct é o mesmo da variável struct que a contém. Em outras palavras, quando uma variável de struct é considerada inicialmente atribuída, também são suas variáveis de instância e, quando uma variável de struct é considerada inicialmente não atribuída, suas variáveis de instância são, da mesma forma, não atribuídas.

### <a name="array-elements"></a>Elementos da matriz

Os elementos de uma matriz entram em existência quando uma instância de matriz é criada e deixa de existir quando não há nenhuma referência a essa instância de matriz.

O valor inicial de cada um dos elementos de uma matriz é o valor padrão ([valores padrão](variables.md#default-values)) do tipo dos elementos da matriz.

Para fins de verificação de atribuição definitiva, um elemento de matriz é considerado inicialmente atribuído.

### <a name="value-parameters"></a>Parâmetros de valor

Um parâmetro declarado sem um `ref` modificador ou `out` é um ***parâmetro de valor***.

Um parâmetro de valor entra em existência na invocação do membro da função (método, Construtor de instância, acessador ou operador) ou função anônima à qual o parâmetro pertence, e é inicializado com o valor do argumento fornecido na invocação. Um parâmetro de valor normalmente deixa de existir no retorno do membro da função ou da função anônima. No entanto, se o parâmetro de valor for capturado por uma função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)), seu tempo de vida se estenderá pelo menos até que a árvore de delegação ou expressão criada a partir dessa função anônima esteja qualificada para a coleta de lixo.

Para fins de verificação de atribuição definitiva, um parâmetro value é considerado inicialmente atribuído.

### <a name="reference-parameters"></a>Parâmetros de referência

Um parâmetro declarado com um `ref` modificador é um ***parâmetro de referência***.

Um parâmetro de referência não cria um novo local de armazenamento. Em vez disso, um parâmetro de referência representa o mesmo local de armazenamento que a variável fornecida como o argumento no membro da função ou invocação de função anônima. Assim, o valor de um parâmetro de referência é sempre o mesmo que a variável subjacente.

As regras de atribuição definidas a seguir se aplicam aos parâmetros de referência. Observe as diferentes regras para os parâmetros de saída descritos em [parâmetros de saída](variables.md#output-parameters).

*  Uma variável deve ser definitivamente atribuída ([atribuição](variables.md#definite-assignment)definida) antes de poder ser passada como um parâmetro de referência em um membro de função ou uma invocação de delegado.
*  Dentro de um membro de função ou função anônima, um parâmetro de referência é considerado inicialmente atribuído.

Dentro de um método de instância ou acessador de instância de `this` um tipo struct, a palavra-chave se comporta exatamente como um parâmetro de referência do tipo struct ([esse acesso](expressions.md#this-access)).

### <a name="output-parameters"></a>Parâmetros de saída

Um parâmetro declarado com um `out` modificador é um ***parâmetro de saída***.

Um parâmetro de saída não cria um novo local de armazenamento. Em vez disso, um parâmetro de saída representa o mesmo local de armazenamento que a variável fornecida como o argumento no membro da função ou na invocação de delegado. Assim, o valor de um parâmetro de saída é sempre o mesmo que a variável subjacente.

As regras de atribuição definidas a seguir se aplicam aos parâmetros de saída. Observe as diferentes regras para parâmetros de referência descritos em [parâmetros de referência](variables.md#reference-parameters).

*  Uma variável não precisa ser definitivamente atribuída antes que possa ser passada como um parâmetro de saída em um membro de função ou invocação de delegado.
*  Após a conclusão normal de um membro de função ou de uma invocação de delegado, cada variável passada como um parâmetro de saída é considerada atribuída nesse caminho de execução.
*  Dentro de um membro de função ou função anônima, um parâmetro de saída é considerado inicialmente não atribuído.
*  Cada parâmetro de saída de um membro de função ou função anônima deve ser definitivamente atribuído ([atribuição definitiva](variables.md#definite-assignment)) antes que o membro de função ou função anônima retorne normalmente.

Dentro de um construtor de instância de um tipo struct `this` , a palavra-chave se comporta exatamente como um parâmetro de saída do tipo struct ([esse acesso](expressions.md#this-access)).

### <a name="local-variables"></a>Variáveis locais

Uma ***variável local*** é declarada por um *local_variable_declaration*, que pode ocorrer em um *bloco*, um *for_statement*, um *switch_statement* ou um *using_statement*; ou por um *foreach_statement* ou um *specific_catch_clause* para um *try_statement*.

O tempo de vida de uma variável local é a parte da execução do programa durante o qual o armazenamento tem a garantia de ser reservado para ele. Esse tempo de vida estende pelo menos da entrada para o *bloco*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*ou *specific_catch_clause* com o qual está associado, até a execução desse *bloco*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*ou *specific_catch_clause* termina de qualquer maneira. (Inserir um *bloco* anexado ou chamar um método suspende, mas não termina, execução do *bloco*atual, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*ou *specific_ catch_clause*.) Se a variável local for capturada por uma função anônima ([variáveis externas capturadas](expressions.md#captured-outer-variables)), seu tempo de vida se estenderá pelo menos até que a árvore de delegado ou expressão seja criada a partir da função anônima, juntamente com quaisquer outros objetos que venham a fazer referência ao a variável capturada está qualificada para a coleta de lixo.

Se o *bloco*pai, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*ou *specific_catch_clause* for inserido recursivamente, uma nova instância da variável local será criada cada o tempo e seu *local_variable_initializer*, se houver, serão avaliados a cada vez.

Uma variável local introduzida por um *local_variable_declaration* não é inicializada automaticamente e, portanto, não tem valor padrão. Para fins de verificação de atribuição definitiva, uma variável local introduzida por um *local_variable_declaration* é considerada inicialmente não atribuída. Um *local_variable_declaration* pode incluir um *local_variable_initializer*; nesse caso, a variável é considerada definitivamente atribuída somente após a expressão de inicialização ([instruções de declaração](variables.md#declaration-statements)).

Dentro do escopo de uma variável local introduzida por um *local_variable_declaration*, é um erro de tempo de compilação para se referir a essa variável local em uma posição textual que precede seu *local_variable_declarator*. Se a declaração de variável local for implícita ([declarações de variável local](statements.md#local-variable-declarations)), também será um erro para se referir à variável dentro de seu *local_variable_declarator*.

Uma variável local introduzida por um *foreach_statement* ou um *specific_catch_clause* é considerada definitivamente atribuída em seu escopo inteiro.

O tempo de vida real de uma variável local é dependente de implementação. Por exemplo, um compilador pode determinar estaticamente que uma variável local em um bloco seja usada apenas para uma pequena parte desse bloco. Usando essa análise, o compilador pode gerar código que resulta no armazenamento da variável que tem um tempo de vida menor do que o bloco que a contém.

O armazenamento referido por uma variável de referência local é recuperado independentemente do tempo de vida dessa variável de referência local ([Gerenciamento de memória automático](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Valores padrão

As seguintes categorias de variáveis são inicializadas automaticamente para seus valores padrão:

*  Variáveis estáticas.
*  Variáveis de instância de instâncias de classe.
*  Elementos da matriz.

O valor padrão de uma variável depende do tipo da variável e é determinado da seguinte maneira:

*  Para uma variável de um *value_type*, o valor padrão é o mesmo que o valor calculado pelo construtor padrão do *Value_type*([construtores padrão](types.md#default-constructors)).
*  Para uma variável de um *reference_type*, o valor padrão é `null`.

A inicialização para valores padrão normalmente é feita por ter o Gerenciador de memória ou o coletor de lixo inicializar a memória para todos os bits-zero antes que ele seja alocado para uso. Por esse motivo, é conveniente usar todos os bits-zero para representar a referência nula.

## <a name="definite-assignment"></a>Atribuição definitiva

Em um determinado local no código executável de um membro de função, uma variável é considerada ***definitivamente atribuída*** se o compilador pode provar, por uma análise de fluxo estático específica ([regras exatas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)), que é a variável foi inicializado automaticamente ou tem sido o destino de pelo menos uma atribuição. Indicado informalmente, as regras de atribuição definitiva são:

*  Uma variável atribuída inicialmente ([variáveis inicialmente atribuídas](variables.md#initially-assigned-variables)) é sempre considerada definitivamente atribuída.
*  Uma variável inicialmente não atribuída ([variáveis inicialmente não atribuídas](variables.md#initially-unassigned-variables)) será considerada definitivamente atribuída em um determinado local se todos os caminhos de execução possíveis que levam a esse local contiverem pelo menos um dos seguintes itens:
    * Uma atribuição simples ([atribuição simples](expressions.md#simple-assignment)) na qual a variável é o operando esquerdo.
    * Uma expressão de invocação ([expressões de invocação](expressions.md#invocation-expressions)) ou expressão de criação de objeto ([expressões de criação de objeto](expressions.md#object-creation-expressions)) que passa a variável como um parâmetro de saída.
    * Para uma variável local, uma declaração de variável local ([declarações de variável local](statements.md#local-variable-declarations)) que inclui um inicializador de variável.

A especificação formal subjacente às regras informais acima é descrita em [variáveis atribuídas inicialmente](variables.md#initially-assigned-variables), [variáveis inicialmente não atribuídas](variables.md#initially-unassigned-variables)e [regras precisas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment).

Os Estados de atribuição definitivos de variáveis de instância de uma variável *struct_type* são acompanhados individualmente, bem como coletivamente. Além das regras acima, as regras a seguir se aplicam a variáveis *struct_type* e suas variáveis de instância:

*  Uma variável de instância é considerada definitivamente atribuída se a variável que a contém *struct_type* é considerada definitivamente atribuída.
*  Uma variável *struct_type* será considerada definitivamente atribuída se cada uma de suas variáveis de instância for considerada definitivamente atribuída.

A atribuição definitiva é um requisito nos seguintes contextos:

*  Uma variável deve ser atribuída definitivamente em cada local em que seu valor é obtido. Isso garante que os valores indefinidos nunca ocorram. A ocorrência de uma variável em uma expressão é considerada para obter o valor da variável, exceto quando
    * a variável é o operando esquerdo de uma atribuição simples,
    * a variável é passada como um parâmetro de saída ou
    * a variável é uma variável *struct_type* e ocorre como o operando esquerdo de um acesso de membro.
*  Uma variável deve ser definitivamente atribuída em cada local em que é passada como um parâmetro de referência. Isso garante que o membro da função que está sendo invocado pode considerar o parâmetro de referência inicialmente atribuído.
*  Todos os parâmetros de saída de um membro de função devem ser definitivamente atribuídos em cada local em que o membro da `return` função retorna (por meio de uma instrução ou por meio da execução que chega ao final do corpo do membro da função). Isso garante que os membros da função não retornem valores indefinidos nos parâmetros de saída, permitindo que o compilador considere uma invocação de membro de função que usa uma variável como um parâmetro de saída equivalente a uma atribuição para a variável.
*  A `this` variável de um construtor de instância *struct_type* deve ser definitivamente atribuída em cada local em que o construtor de instância retorna.

### <a name="initially-assigned-variables"></a>Variáveis inicialmente atribuídas

As seguintes categorias de variáveis são classificadas como atribuídas inicialmente:

*  Variáveis estáticas.
*  Variáveis de instância de instâncias de classe.
*  Variáveis de instância de variáveis de struct inicialmente atribuídas.
*  Elementos da matriz.
*  Parâmetros de valor.
*  Parâmetros de referência.
*  Variáveis declaradas `catch` em uma cláusula `foreach` ou em uma instrução.

### <a name="initially-unassigned-variables"></a>Variáveis inicialmente não atribuídas

As seguintes categorias de variáveis são classificadas como inicialmente não atribuídas:

*  Variáveis de instância de variáveis struct inicialmente não atribuídas.
*  Parâmetros de saída, incluindo `this` a variável de construtores de instância de struct.
*  Variáveis locais, exceto aquelas declaradas `catch` em uma cláusula `foreach` ou uma instrução.

### <a name="precise-rules-for-determining-definite-assignment"></a>Regras precisas para determinar a atribuição definitiva

Para determinar se cada variável usada é definitivamente atribuída, o compilador deve usar um processo que seja equivalente ao descrito nesta seção.

O compilador processa o corpo de cada membro de função que tem uma ou mais variáveis não atribuídas inicialmente. Para cada variável *v*inicialmente não atribuída, o compilador determina um ***estado de atribuição definitivo*** para *v* em cada um dos seguintes pontos no membro da função:

*  No início de cada instrução
*  No ponto de extremidade ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)) de cada instrução
*  Em cada arco que transfere o controle para outra instrução ou para o ponto final de uma instrução
*  No início de cada expressão
*  No final de cada expressão

O estado de atribuição definitivo de *v* pode ser:

*  Definitivamente atribuído. Isso indica que em todos os fluxos de controle possíveis para esse ponto, *v* recebeu um valor.
*  Não é definitivamente atribuído. Para o estado de uma variável no final de uma expressão do tipo `bool`, o estado de uma variável que não está definitivamente atribuída pode (mas não necessariamente) se enquadrar em um dos seguintes subcaminhos:
    * Atribuído definitivamente após a expressão true. Esse estado indica que *v* é definitivamente atribuído se a expressão booliana for avaliada como true, mas não será necessariamente atribuído se a expressão booliana for avaliada como false.
    * Definitivamente atribuída após expressão false. Esse estado indica que *v* é definitivamente atribuído se a expressão booliana for avaliada como false, mas não será necessariamente atribuída se a expressão booliana for avaliada como true.

As regras a seguir regem como o estado de uma variável *v* é determinado em cada local.

#### <a name="general-rules-for-statements"></a>Regras gerais para instruções

*  *v* não é definitivamente atribuído no início de um corpo de membro de função.
*  o *v* é definitivamente atribuído no início de qualquer instrução inacessível.
*  O estado de atribuição definitivo de *v* no início de qualquer outra instrução é determinado verificando o estado de atribuição definido de *v* em todas as transferências de fluxo de controle direcionadas ao início dessa instrução. Se (e somente se) *v* for definitivamente atribuído a todas as transferências de fluxo de controle, *v* será definitivamente atribuído no início da instrução. O conjunto de possíveis transferências de fluxo de controle é determinado da mesma forma que para verificar a acessibilidade da instrução ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)).
*  O estado de atribuição definitivo de *v* no ponto de extremidade de um `checked`bloco `unchecked`, `if` `lock` `while` `foreach` `do` `for`,,,,,, `using`,, ou `switch`a instrução é determinada verificando o estado de atribuição definido de *v* em todas as transferências de fluxo de controle direcionadas ao ponto de extremidade dessa instrução. Se *v* for definitivamente atribuído a todas essas transferências de fluxo de controle, então *v* será definitivamente atribuído no ponto de extremidade da instrução. , *v* não é definitivamente atribuído no ponto de extremidade da instrução. O conjunto de possíveis transferências de fluxo de controle é determinado da mesma forma que para verificar a acessibilidade da instrução ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Instruções Block, instruções marcadas e desmarcadas

O estado de atribuição definitivo de *v* na transferência de controle para a primeira instrução da lista de instruções no bloco (ou até o ponto final do bloco, se a lista de instruções estiver vazia) é igual à instrução de atribuição definitiva de *v* antes do bloco instrução `checked`, ou `unchecked` .

#### <a name="expression-statements"></a>Instruções de expressão

Para uma instrução de expressão *stmt* que consiste na expressão *Expression:*

*  *v* tem o mesmo estado de atribuição definitivo no início de *expr* como no início de *stmt*.
*  Se *for* definitivamente atribuído ao final de *expr*, ele será definitivamente atribuído no ponto final de *stmt*; , Ele não é definitivamente atribuído no ponto de extremidade de *stmt*.

#### <a name="declaration-statements"></a>Instruções de declaração

*  Se *stmt* for uma instrução de declaração sem inicializadores, *v* terá o mesmo estado de atribuição definitivo no ponto de extremidade de *stmt* como no início de *stmt*.
*  Se *stmt* for uma instrução de declaração com inicializadores, o estado de atribuição definitivo para *v* será determinado como se *stmt* fosse uma lista de instruções, com uma instrução de atribuição para cada declaração com um inicializador (na ordem de declaração).

#### <a name="if-statements"></a>Instruções If

Para uma `if` instrução *stmt* do formulário:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* tem o mesmo estado de atribuição definitivo no início de *expr* como no início de *stmt*.
*  Se *v* for definitivamente atribuído ao final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para *then_stmt* e *else_stmt* ou para o ponto de extremidade de *stmt* se não houver nenhuma cláusula else.
*  Se *v* tiver o estado "definitivamente atribuído após expressão verdadeira" no final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para *then_stmt*e não definitivamente atribuído na transferência de fluxo de controle para *else_ stmt* ou para o ponto de extremidade de *stmt* se não houver nenhuma cláusula else.
*  Se *v* tiver o estado "definitivamente atribuído após expressão falsa" no final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para *else_stmt*e não definitivamente atribuído na transferência de fluxo de controle para *then_stmt* . Ele é definitivamente atribuído no ponto de extremidade de *stmt* se e somente se ele for definitivamente atribuído no ponto de extremidade de *then_stmt*.
*  Caso contrário, *v* é considerado não definitivamente atribuído na transferência de fluxo de controle para *then_stmt* ou *else_stmt*, ou para o ponto de extremidade de *stmt* se não houver nenhuma cláusula else.

#### <a name="switch-statements"></a>Instruções switch

Em uma `switch` instrução *stmt* com uma *expr*de expressão de controle:

*  O estado de atribuição definitivo de *v* no início de *expr* é o mesmo que o estado de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* na transferência de fluxo de controle para uma lista de instrução de bloqueio de switch acessível é igual ao estado de atribuição definido de *v* no final de *expr*.

#### <a name="while-statements"></a>Instruções while

Para uma `while` instrução *stmt* do formulário:
```csharp
while ( expr ) while_body
```

*  *v* tem o mesmo estado de atribuição definitivo no início de *expr* como no início de *stmt*.
*  Se *v* for definitivamente atribuído ao final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para *while_body* e para o ponto final de *stmt*.
*  Se *v* tiver o estado "definitivamente atribuído após expressão verdadeira" no final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para *while_body*, mas não definitivamente atribuído no ponto de extremidade de *stmt*.
*  Se *v* tiver o estado "definitivamente atribuído após expressão falsa" no final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para o ponto final de *stmt*, mas não definitivamente atribuído na transferência de fluxo de controle ao *while _body*.

#### <a name="do-statements"></a>Instruções do

Para uma `do` instrução *stmt* do formulário:
```csharp
do do_body while ( expr ) ;
```

*  *v* tem o mesmo estado de atribuição definitivo na transferência de fluxo de controle do início de *stmt* para *do_body* como no início de *stmt*.
*  *v* tem o mesmo estado de atribuição definitivo no início de *expr* como no ponto de extremidade de *do_body*.
*  Se *v* for definitivamente atribuído ao final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para o ponto final de *stmt*.
*  Se *v* tiver o estado "definitivamente atribuído após expressão falsa" no final de *expr*, ele será definitivamente atribuído na transferência de fluxo de controle para o ponto final de *stmt*.

#### <a name="for-statements"></a>Instruções for

Verificação de atribuição definitiva `for` para uma instrução do formulário:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
é feito como se a instrução fosse gravada:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Se o *for_condition* for omitido da `for` instrução, a avaliação da atribuição definitiva continuará como se *for_condition* fosse substituído `true` por na expansão acima.

#### <a name="break-continue-and-goto-statements"></a>Instruções Break, continue e goto

O estado de atribuição definitivo de *v* na transferência de fluxo de controle `break`causada `continue`por uma `goto` instrução, ou é igual ao estado de atribuição definido de *v* no início da instrução.

#### <a name="throw-statements"></a>Instruções throw

Para uma instrução *stmt* do formulário
```csharp
throw expr ;
```

O estado de atribuição definitivo de *v* no início de *expr* é o mesmo que o estado de atribuição definido de *v* no início de *stmt*.

#### <a name="return-statements"></a>Instruções de retorno

Para uma instrução *stmt* do formulário
```csharp
return expr ;
```

*  O estado de atribuição definitivo de *v* no início de *expr* é o mesmo que o estado de atribuição definido de *v* no início de *stmt*.
*  Se *v* for um parâmetro de saída, ele deverá ser definitivamente atribuído a:
    * Depois de *expr*
    * ou no final `finally` do bloco de um `try` ou`return` que incluiainstrução.- `try` - `finally` `catch` - `finally`

Para uma instrução stmt do formulário:
```csharp
return ;
```

*  Se *v* for um parâmetro de saída, ele deverá ser definitivamente atribuído a:
    * antes de *stmt*
    * ou no final `finally` do bloco de um `try` ou`return` que incluiainstrução.- `try` - `finally` `catch` - `finally`

#### <a name="try-catch-statements"></a>Instruções Try-Catch

Para uma instrução *stmt* do formulário:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  O estado de atribuição definitivo de *v* no início de *try_block* é o mesmo que o estado de atribuição definido de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* no início de *catch_block_i* (para qualquer *i*) é igual ao estado de atribuição definitivo de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* no ponto de extremidade de *stmt* é definitivamente atribuído se (e somente se) *v* for definitivamente atribuído no ponto de extremidade de *try_block* e a cada *catch_block_i* (para cada *i* de 1 a *n* ).

#### <a name="try-finally-statements"></a>Instruções try – finally

Para uma `try` instrução *stmt* do formulário:
```csharp
try try_block finally finally_block
```

*  O estado de atribuição definitivo de *v* no início de *try_block* é o mesmo que o estado de atribuição definido de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* no início de *finally_block* é o mesmo que o estado de atribuição definido de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* no ponto de extremidade de *stmt* é definitivamente atribuído se (e somente se) pelo menos uma das seguintes opções for verdadeira:
    * o *v* é definitivamente atribuído no ponto de extremidade de *try_block*
    * o *v* é definitivamente atribuído no ponto de extremidade de *finally_block*

Se uma transferência de fluxo de controle (por exemplo `goto` , uma instrução) for feita em *try_block*e terminar fora de *try_block*, então *v* também será considerado definitivamente atribuído nessa transferência de fluxo de controle se *v* for definitivamente atribuído no ponto de extremidade de *finally_block*. (Isso não é apenas se — se *v* for definitivamente atribuído por outro motivo nessa transferência de fluxo de controle, ele ainda será considerado definitivamente atribuído.)

#### <a name="try-catch-finally-statements"></a>Instruções try – catch-finally

Análise de atribuição definitiva `try` para uma - - `catch` instruçãodoformulário`finally` :
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
é feito como se a instrução fosse uma `try` - `try` - `finally` instrução delimitando uma `catch` instrução:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

O exemplo a seguir demonstra como os diferentes blocos de `try` uma instrução ([a instrução try](statements.md#the-try-statement)) afetam a atribuição definitiva.
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

#### <a name="foreach-statements"></a>Instruções Foreach

Para uma `foreach` instrução *stmt* do formulário:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  O estado de atribuição definitivo de *v* no início de *expr* é o mesmo que o estado de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* na transferência de fluxo de controle para *embedded_statement* ou para o ponto final de *stmt* é igual ao estado de *v* no final de *expr*.

#### <a name="using-statements"></a>Usando instruções

Para uma `using` instrução *stmt* do formulário:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  O estado de atribuição definitivo de *v* no início de *resource_acquisition* é o mesmo que o estado de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* na transferência de fluxo de controle para *embedded_statement* é o mesmo que o estado de *v* no final de *resource_acquisition*.

#### <a name="lock-statements"></a>Instruções Lock

Para uma `lock` instrução *stmt* do formulário:
```csharp
lock ( expr ) embedded_statement
```

*  O estado de atribuição definitivo de *v* no início de *expr* é o mesmo que o estado de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* na transferência de fluxo de controle para *embedded_statement* é o mesmo que o estado de *v* no final de *expr*.

#### <a name="yield-statements"></a>Instruções yield

Para uma `yield return` instrução *stmt* do formulário:
```csharp
yield return expr ;
```

*  O estado de atribuição definitivo de *v* no início de *expr* é o mesmo que o estado de *v* no início de *stmt*.
*  O estado de atribuição definitivo de *v* no final de *stmt* é igual ao estado de *v* no final de *expr*.
*  Uma `yield break` instrução não tem nenhum efeito sobre o estado de atribuição definitivo.

#### <a name="general-rules-for-simple-expressions"></a>Regras gerais para expressões simples

A regra a seguir se aplica a esses tipos de expressões: literais ([literais](expressions.md#literals)), nomes simples ([nomes simples](expressions.md#simple-names)), expressões de acesso de membro ([acesso de membro](expressions.md#member-access)), expressões de acesso base não indexadas ([acesso de base](expressions.md#base-access)), `typeof`expressões ([o operador typeof](expressions.md#the-typeof-operator)), expressões de valor padrão ([expressões de valor padrão](expressions.md#default-value-expressions)) e `nameof` expressões ([expressões nameof](expressions.md#nameof-expressions)).

*  O estado de atribuição definitivo de *v* no final de tal expressão é igual ao estado de atribuição definido de *v* no início da expressão.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Regras gerais para expressões com expressões inseridas

As regras a seguir se aplicam a esses tipos de expressões: expressões entre parênteses ([expressões entre parênteses](expressions.md#parenthesized-expressions)), expressões de acesso de elemento ([acesso de elemento](expressions.md#element-access)), expressões de acesso de base com indexação ([acesso de base](expressions.md#base-access)), incremento e decrementar expressões ([operadores de incremento e diminuição de sufixo](expressions.md#postfix-increment-and-decrement-operators), [incremento de prefixo e diminuir operadores](expressions.md#prefix-increment-and-decrement-operators)), expressões de conversão ( `~`expressões de[conversão](expressions.md#cast-expressions)), unário `+`, `-` `*`,,expressões, binary `+` `-` ,`%` ,,`>=`,, ,,`>>`,, ,`>`, `<<` `/` `*` `<` `<=` `==` `!=` expressões,`|`,,,, ,`^` ([operadores aritméticos](expressions.md#arithmetic-operators), [operadores de deslocamento](expressions.md#shift-operators), relacionais e de tipo-teste `is` `as` `&` [ operadores](expressions.md#relational-and-type-testing-operators), [operadores lógicos](expressions.md#logical-operators), expressões de atribuição compostas ( `checked` [atribuição composta](expressions.md#compound-assignment)) e `unchecked` expressões ([os operadores marcados e não verificados](expressions.md#the-checked-and-unchecked-operators)), além de matriz e delegado expressões de criação ([o novo operador](expressions.md#the-new-operator)).

Cada uma dessas expressões tem uma ou mais subexpressãos que são avaliadas incondicionalmente em uma ordem fixa. Por exemplo, o operador `%` binário avalia o lado esquerdo do operador e, em seguida, o lado direito. Uma operação de indexação avalia a expressão indexada e, em seguida, avalia cada uma das expressões de índice, na ordem da esquerda para a direita. Para uma *expr*de expressão, que tem subexpressãos *E1, E2,..., en*, avaliadas nessa ordem:

*  O estado de atribuição definitivo de *v* no início do *E1* é o mesmo que o estado de atribuição definido no início de *expr*.
*  O estado de atribuição definitivo de *v* no início de *Ei* (*i* maior que um) é o mesmo que o estado de atribuição definido no final da subexpressão anterior.
*  O estado de atribuição definitivo de *v* no final de *expr* é igual ao estado de atribuição definido no final de *en*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Expressões de invocação e expressões de criação de objeto

Para uma *expr* de expressão de invocação do formulário:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
ou uma expressão de criação de objeto do formulário:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Para uma expressão de invocação, o estado de atribuição definitivo de *v* antes de *primary_expression* é o mesmo que o estado de *v* antes de *expr*.
*  Para uma expressão de invocação, o estado de atribuição definitivo de *v* antes de *arg1* é o mesmo que o estado de *v* após *primary_expression*.
*  Para uma expressão de criação de objeto, o estado de atribuição definitivo de *v* antes de *arg1* é o mesmo que o estado de *v* antes de *expr*.
*  Para cada argumento *Argi*, o estado de atribuição definitivo de *v* após *Argi* é determinado pelas regras de expressão normal, `ref` ignorando quaisquer modificadores ou `out` .
*  Para cada argumento *Argi* para qualquer *i* maior que um, o estado de atribuição definitivo de *v* antes de *Argi* é o mesmo que o estado de *v* após o *ARG*anterior.
*  Se a variável *v* for passada como um `out` argumento (ou seja, um argumento do formulário `out v`) em qualquer um dos argumentos, o estado de *v* após *expr* será atribuído definitivamente. , o estado de *v* After *expr* é o mesmo que o estado de *v* após *argN*.
*  Para inicializadores de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)), inicializadores de objeto ([inicializadores de objeto](expressions.md#object-initializers)), inicializadores de coleção ([inicializadores de coleção](expressions.md#collection-initializers)) e inicializadores de objeto anônimos (criação de[objeto anônimo expressões](expressions.md#anonymous-object-creation-expressions)), o estado de atribuição definitivo é determinado pela expansão de que essas construções são definidas em termos de.

#### <a name="simple-assignment-expressions"></a>Expressões de atribuição simples

Para uma *expr* de expressão do formulário `w = expr_rhs`:

*  O estado de atribuição definitivo de *v* antes de *expr_rhs* é o mesmo que o estado de atribuição definido de *v* antes de *expr*.
*  O estado de atribuição definitivo de *v* após *expr* é determinado por:
   * Se *w* for a mesma variável que *v*, o estado de atribuição definitivo de *v* após *expr* será atribuído definitivamente.
   * Caso contrário, se a atribuição ocorrer dentro do construtor de instância de um tipo de struct, se *w* for um acesso de propriedade que designa uma propriedade automaticamente implementada *P* na instância que está sendo construída e *v* for o campo de apoio oculto de *P*. em seguida, o estado de atribuição definitiva de *v* após *expr* é atribuído definitivamente.
   * Caso contrário, o estado de atribuição definitivo de *v* após *expr* será o mesmo que o estado de atribuição definido de *v* após *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (condicional e) expressões

Para uma *expr* de expressão do formulário `expr_first && expr_second`:

*  O estado de atribuição definitivo de *v* antes de *expr_first* é o mesmo que o estado de atribuição definido de *v* antes de *expr*.
*  O estado de atribuição definitivo de *v* antes de *expr_second* será definitivamente atribuído se o estado de *v* após *expr_first* for definitivamente atribuído ou "definitivamente atribuído após a expressão verdadeira". Caso contrário, ele não é definitivamente atribuído.
*  O estado de atribuição definitivo de *v* após *expr* é determinado por:
    * Se *expr_first* for uma expressão constante com o valor `false`, o estado de atribuição definitivo de *v* após *expr* será o mesmo que o estado de atribuição definido de *v* após *expr_first*.
    * Caso contrário, se o estado de *v* após *expr_first* for definitivamente atribuído, o estado de *v* após *expr* será atribuído definitivamente.
    * Caso contrário, se o estado de *v* após *expr_second* for definitivamente atribuído, e o estado *de v* após *expr_first* for "definitivamente atribuído após expressão falsa", o estado de *v* após *expr* será definitivamente associada.
    * Caso contrário, se o estado de *v* após *expr_second* for definitivamente atribuído ou "definitivamente atribuído após a expressão verdadeira", o estado de *v* após *expr* será "definitivamente atribuído após a expressão verdadeira".
    * Caso contrário, se o estado de *v* após *expr_first* for "definitivamente atribuído após a expressão falsa", e o estado de *v* após *expr_second* for "definitivamente atribuído após a expressão falsa", o estado de *v* após  *expr* é "definitivamente atribuída após expressão falsa".
    * Caso contrário, o estado de *v* após *expr* não será atribuído definitivamente.

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
a variável `i` é considerada definitivamente atribuída em uma das instruções inseridas de uma `if` instrução, mas não no outro. Na instrução no método `F`, a variável `i` é definitivamente atribuída na primeira instrução inserida porque a execução da expressão `(i = y)` sempre precede a execução dessa instrução inserida. `if` Por outro lado, a `i` variável não é definitivamente atribuída na segunda instrução incorporada, `x >= 0` pois pode ter testado false, resultando na `i` não atribuição da variável.

#### <a name="-conditional-or-expressions"></a>|| expressões (condicionais ou)

Para uma *expr* de expressão do formulário `expr_first || expr_second`:

*  O estado de atribuição definitivo de *v* antes de *expr_first* é o mesmo que o estado de atribuição definido de *v* antes de *expr*.
*  O estado de atribuição definitivo de *v* antes de *expr_second* será definitivamente atribuído se o estado de *v* após *expr_first* for definitivamente atribuído ou "definitivamente atribuído após expressão falsa". Caso contrário, ele não é definitivamente atribuído.
*  A instrução de atribuição definitiva de *v* após *expr* é determinada por:
    * Se *expr_first* for uma expressão constante com o valor `true`, o estado de atribuição definitivo de *v* após *expr* será o mesmo que o estado de atribuição definido de *v* após *expr_first*.
    * Caso contrário, se o estado de *v* após *expr_first* for definitivamente atribuído, o estado de *v* após *expr* será atribuído definitivamente.
    * Caso contrário, se o estado de *v* após *expr_second* for definitivamente atribuído, e o estado *de v* após *expr_first* for "definitivamente atribuído após a expressão verdadeira", o estado de *v* após *expr* será definitivamente associada.
    * Caso contrário, se o estado de *v* após *expr_second* for definitivamente atribuído ou "definitivamente atribuído após a expressão falsa", o estado de *v* após *expr* será "definitivamente atribuído após a expressão falsa".
    * Caso contrário, se o estado de *v* após *expr_first* for "definitivamente atribuído após a expressão verdadeira", e o estado de *v* após *expr_second* for "definitivamente atribuído após a expressão verdadeira", o estado de *v* após *expr* é "definitivamente atribuído após expressão verdadeira".
    * Caso contrário, o estado de *v* após *expr* não será atribuído definitivamente.

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
a variável `i` é considerada definitivamente atribuída em uma das instruções inseridas de uma `if` instrução, mas não no outro. Na instrução no método `G`, a variável `i` é definitivamente atribuída na segunda instrução inserida porque a execução da expressão `(i = y)` sempre precede a execução dessa instrução inserida. `if` Por outro lado, a `i` variável não é definitivamente atribuída na primeira instrução inserida, `x >= 0` pois pode ter testado verdadeiro, resultando na `i` não atribuição da variável.

#### <a name="-logical-negation-expressions"></a>! (negação lógica) expressões

Para uma *expr* de expressão do formulário `! expr_operand`:

*  O estado de atribuição definitivo de *v* antes de *expr_operand* é o mesmo que o estado de atribuição definido de *v* antes de *expr*.
*  O estado de atribuição definitivo de *v* após *expr* é determinado por:
    * Se o estado de *v* After * expr_operand * for definitivamente atribuído, o estado de *v* após *expr* será atribuído definitivamente.
    * Se o estado de *v* After * expr_operand * não for definitivamente atribuído, o estado de *v* após *expr* não será atribuído definitivamente.
    * Se o estado de *v* After * expr_operand * for "definitivamente atribuído após a expressão falsa", o estado de *v* após *expr* será "definitivamente atribuído após a expressão verdadeira".
    * Se o estado de *v* After * expr_operand * for "definitivamente atribuído após a expressão verdadeira", o estado de *v* após *expr* será "definitivamente atribuído após a expressão falsa".

#### <a name="-null-coalescing-expressions"></a>?? (União nula) expressões

Para uma *expr* de expressão do formulário `expr_first ?? expr_second`:

*  O estado de atribuição definitivo de *v* antes de *expr_first* é o mesmo que o estado de atribuição definido de *v* antes de *expr*.
*  O estado de atribuição definitivo de *v* antes de *expr_second* é o mesmo que o estado de atribuição definido de *v* após *expr_first*.
*  A instrução de atribuição definitiva de *v* após *expr* é determinada por:
    * Se *expr_first* for uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) com o valor NULL, o estado de *v* After *expr* será o mesmo que o estado de *v* após *expr_second*.
*  Caso contrário, o estado de *v* After *expr* será o mesmo que o estado de atribuição definido de *v* após *expr_first*.

#### <a name="-conditional-expressions"></a>expressões?: (condicionais)

Para uma *expr* de expressão do formulário `expr_cond ? expr_true : expr_false`:

*  O estado de atribuição definitivo de *v* antes de *expr_cond* é o mesmo que o estado de *v* antes de *expr*.
*  O estado de atribuição definitivo de *v* antes de *expr_true* é definitivamente atribuído se e somente se uma das seguintes isenções:
    * *expr_cond* é uma expressão constante com o valor`false`
    * o estado de *v* após *expr_cond* é definitivamente atribuído ou "definitivamente atribuído após a expressão verdadeira".
*  O estado de atribuição definitivo de *v* antes de *expr_false* é definitivamente atribuído se e somente se uma das seguintes isenções:
    * *expr_cond* é uma expressão constante com o valor`true`
*  o estado de *v* após *expr_cond* é definitivamente atribuído ou "definitivamente atribuído após expressão falsa".
*  O estado de atribuição definitivo de *v* após *expr* é determinado por:
    * Se *expr_cond* for uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) com valor `true` , o estado de *v* após *expr* será o mesmo que o estado de *v* após *expr_true*.
    * Caso contrário, *se expr_cond* for uma expressão constante[(expressões constantes](expressions.md#constant-expressions)) com `false` valor, o estado *de v* após *expr* será o mesmo que o estado de *v* após *expr_false*.
    * Caso contrário, se o estado de *v* após *expr_true* for definitivamente atribuído e o estado de *v* depois de *expr_false* for definitivamente atribuído, o estado de *v* após *expr* será atribuído definitivamente.
    * Caso contrário, o estado de *v* após *expr* não será atribuído definitivamente.

#### <a name="anonymous-functions"></a>Funções anônimas

Para uma *expr* *lambda_expression* ou *anonymous_method_expression* com um corpo ( *bloco* ou *expressão*) *corpo*:

*  O estado de atribuição definitivo de uma variável externa *v* para o *corpo* é o mesmo que o estado de *v* antes de *expr*. Ou seja, o estado de atribuição definitivo de variáveis externas é herdado do contexto da função anônima.
*  O estado de atribuição definitivo de uma variável externa *v* depois de *expr* é o mesmo que o estado de *v* antes de *expr*.

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
gera um erro de tempo de compilação `max` porque não é definitivamente atribuído onde a função anônima é declarada. O exemplo
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
também gera um erro de tempo de compilação porque a atribuição `n` para na função anônima não tem nenhum efeito sobre o estado de atribuição `n` definitivo de fora da função anônima.

## <a name="variable-references"></a>Referências de variáveis

Um *variable_reference* é uma *expressão* que é classificada como uma variável. Um *variable_reference* denota um local de armazenamento que pode ser acessado para buscar o valor atual e para armazenar um novo valor.

```antlr
variable_reference
    : expression
    ;
```

Em C e C++, um *variable_reference* é conhecido como um *lvalue*.

## <a name="atomicity-of-variable-references"></a>Atomicidade de referências de variáveis

Leituras e gravações dos seguintes tipos de dados são Atomic: `bool`, `char`, `byte`, `sbyte` `int` `ushort` `short`,,, `uint`,, `float`e tipos de referência. Além disso, leituras e gravações de tipos de enumeração com um tipo subjacente na lista anterior também são atômicas. As leituras e gravações de outros tipos, `long`incluindo `ulong` `double`,, e `decimal`, bem como tipos definidos pelo usuário, não têm garantia de serem atômicas. Além das funções de biblioteca projetadas para essa finalidade, não há nenhuma garantia de leitura-modificação-gravação atômica, como no caso de incremento ou decréscimo.

