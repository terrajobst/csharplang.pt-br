# <a name="expressions"></a>Expressões

Uma expressão é uma sequência de operadores e operandos. Este capítulo define a sintaxe, a ordem de avaliação dos operandos e operadores e o significado das expressões.

## <a name="expression-classifications"></a>Classificações de expressão

Uma expressão é classificada como um dos seguintes:

*  Um valor. Cada valor tem um tipo associado.
*  Uma variável. Cada variável tem um tipo associado, ou seja, o tipo declarado da variável.
*  Um namespace. Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)). Em qualquer outro contexto, uma expressão classificada como um namespace causa um erro de tempo de compilação.
*  Um tipo. Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)), ou como um operando para o `as` operador ([o operador ](expressions.md#the-as-operator)), o `is` operador ([o operador is](expressions.md#the-is-operator)), ou o `typeof` operador ([o operador typeof](expressions.md#the-typeof-operator)). Em qualquer outro contexto, uma classificado como um tipo de expressão causa um erro de tempo de compilação.
*  Um grupo de método, que é um conjunto de métodos sobrecarregados resultante de uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)). Um grupo de método pode ter uma expressão de instância associada e uma lista de argumentos de tipo associado. Quando um método de instância é invocado, o resultado da avaliação da expressão de instância torna-se a instância representada pelo `this` ([esse acesso](expressions.md#this-access)). Um grupo de métodos é permitido em uma *invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)), uma *delegate_creation_expression* ([delegar a criação expressões](expressions.md#delegate-creation-expressions)) e como o lado esquerdo de um é o operador e pode ser convertido implicitamente em um tipo de delegado compatível ([conversões de grupo de método](conversions.md#method-group-conversions)). Em qualquer outro contexto, uma expressão classificada como um grupo de métodos causa um erro de tempo de compilação.
*  Um literal nulo. Uma expressão com essa classificação pode ser convertida implicitamente para um tipo de referência ou tipo anulável.
*  Uma função anônima. Uma expressão com essa classificação pode ser implicitamente convertida para um tipo de delegado compatível ou tipo de árvore de expressão.
*  Um acesso de propriedade. Todo acesso de propriedade tem um tipo associado, ou seja, o tipo da propriedade. Além disso, um acesso de propriedade pode ter uma expressão de instância associada. Quando um acessador (o `get` ou `set` bloco) de uma instância de acesso de propriedade é invocado, o resultado da avaliação da expressão de instância torna-se a instância representada pelo `this` ([esse acesso](expressions.md#this-access)).
*  Acesso de um evento. Acesso de cada evento tem um tipo associado, ou seja, o tipo do evento. Além disso, o acesso de um evento pode ter uma expressão de instância associada. Um acesso de evento pode aparecer como o operando à esquerda do `+=` e `-=` operadores ([atribuição de evento](expressions.md#event-assignment)). Em qualquer outro contexto, uma expressão classificada como um acesso de evento causa um erro de tempo de compilação.
*  Acesso de um indexador. Cada acesso do indexador tem um tipo associado, ou seja, o tipo de elemento do indexador. Além disso, o acesso de um indexador tem uma expressão de instância associada e uma lista de argumentos associados. Quando um acessador (o `get` ou `set` bloco) de um indexador acesso é invocado, o resultado da avaliação da expressão de instância torna-se a instância representada pelo `this` ([esse acesso](expressions.md#this-access)) e o resultado de Avaliando a lista de argumentos torna-se a lista de parâmetros de invocação.
*  nada. Isso ocorre quando a expressão é uma invocação de um método com um tipo de retorno `void`. Uma expressão classificada como nada só é válido no contexto de um *statement_expression* ([instruções de expressão](statements.md#expression-statements)).

O resultado final de uma expressão nunca é um namespace, tipo, o grupo de método ou acesso ao evento. Em vez disso, conforme observado acima, essas categorias de expressões são construções intermediárias que são permitidas somente em determinados contextos.

Um acesso de propriedade ou o acesso do indexador é sempre reclassificado como um valor pela execução de uma invocação do *acessador get* ou o *acessador set*. O acessador particular é determinado pelo contexto de acesso de propriedade ou indexador: se o acesso é o destino de uma atribuição, o *acessador set* é chamado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)) . Caso contrário, o *acessador get* é chamado para obter o valor atual ([valores das expressões](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Valores de expressões

A maioria das construções que envolvem uma expressão, por fim, exige que a expressão para denotar uma ***valor***. Nesses casos, se a expressão real denota um namespace, um tipo, um grupo de métodos ou nada, ocorrerá um erro de tempo de compilação. No entanto, se a expressão denota um acesso de propriedade, um acesso de indexador ou uma variável, o valor da propriedade, indexador ou variável implicitamente é substituído:

*  O valor de uma variável é simplesmente o valor armazenado atualmente no local de armazenamento identificado pela variável. Uma variável deve ser considerada atribuída definitivamente ([atribuição definitiva](variables.md#definite-assignment)) antes de seu valor pode ser obtido ou, caso contrário, ocorrerá um erro de tempo de compilação.
*  O valor de uma expressão de acesso de propriedade é obtido invocando o *acessador get* da propriedade. Se a propriedade não tiver nenhuma *acessador get*, ocorre um erro de tempo de compilação. Caso contrário, uma invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é executada, e o resultado da invocação se tornará o valor da expressão de acesso de propriedade.
*  O valor de uma expressão de acesso do indexador é obtido invocando o *acessador get* do indexador. Se o indexador não tiver nenhuma *acessador get*, ocorre um erro de tempo de compilação. Caso contrário, uma invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é executada com o argumento de lista associado com a expressão de acesso do indexador e o resultado da invocação se tornará o valor da expressão de acesso do indexador.

## <a name="static-and-dynamic-binding"></a>Associação estática e dinâmica

O processo de determinar o significado de uma operação com base no tipo ou valor de expressões constituintes (argumentos, operandos, receptores) é geralmente denominado ***associação***. Por exemplo o significado de uma chamada de método é determinado com base no tipo dos argumentos e o receptor. O significado de um operador é determinado com base no tipo de seus operandos.

Em c# o significado de uma operação normalmente é determinada em tempo de compilação, com base no tipo de tempo de compilação das suas expressões constituintes. Da mesma forma, se uma expressão contiver um erro, o erro é detectado e reportado pelo compilador. Essa abordagem é conhecida como ***vinculação estática***.

No entanto, se uma expressão é uma expressão dinâmica (ou seja, tem o tipo `dynamic`) indica que qualquer ligação que participa do deve ser baseada em seu tipo de tempo de execução (ou seja, o tipo real do objeto, ele indicará em tempo de execução) em vez do tipo tem no tempo de compilação. A associação dessa operação, portanto, é adiada até o momento em que a operação deve ser executado durante a execução do programa. Isso é conhecido como ***vinculação dinâmica***.

Quando uma operação é vinculada dinamicamente, pouca ou nenhuma verificação é executada pelo compilador. Em vez disso, se a associação de tempo de execução falhar, os erros são relatados como exceções em tempo de execução.

As seguintes operações em c# estão sujeitos a associação:

*  Acesso de membro: `e.M`
*  Invocação de método: `e.M(e1, ..., eN)`
*  Invocação de delegado:`e(e1, ..., eN)`
*  Acesso de elemento: `e[e1, ..., eN]`
*  Criação do objeto: `new C(e1, ..., eN)`
*  Operadores unários sobrecarregados: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Sobrecarga dos operadores binários: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Operadores de atribuição: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Conversões implícitas e explícitas

Quando não há expressões dinâmicas estão envolvidas, c# assume como padrão a vinculação estática, o que significa que os tipos de tempo de compilação de expressões constituintes são usados no processo de seleção. No entanto, quando uma das expressões constituintes nas operações listadas acima é uma expressão dinâmica, a operação em vez disso, vinculada dinamicamente.

### <a name="binding-time"></a>Tempo de associação

Vinculação estática ocorre em tempo de compilação, ao passo que a vinculação dinâmica ocorre em tempo de execução. Nas seções a seguir, o termo ***tempo de associação*** refere-se ao tempo de compilação ou tempo de execução, dependendo de quando ocorre a associação.

O exemplo a seguir ilustra as noções de associação estática e dinâmica e de tempo de associação:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

As duas primeiras chamadas são vinculadas estaticamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de compilação do seu argumento. Assim, o tempo de associação é o tempo de compilação.

A terceira chamada associada dinamicamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de execução do seu argumento. Isso acontece porque o argumento é uma expressão dinâmica – seu tipo de tempo de compilação é `dynamic`. Portanto, o tempo de associação para a terceira chamada é o tempo de execução.

### <a name="dynamic-binding"></a>Associação dinâmica

A finalidade de associação dinâmica é permitir que programas em c# interagir com ***objetos dinâmicos***, ou seja, sistema de tipos de objetos que não seguem as regras normais do c#. Objetos dinâmicos podem ser objetos de outras linguagens de programação com sistemas de tipos diferentes, ou podem ser objetos por meio de programação são configuradas para implementar suas próprias semânticas de associação para operações diferentes.

O mecanismo pelo qual um objeto dinâmico implementa seu próprio semântica é definido pela implementação. Uma determinada interface – novamente definido pela implementação – é implementada por objetos dinâmicos para sinalizar para o c# tempo de execução que eles têm semântica especial. Assim, sempre que as operações em um objeto dinâmico são vinculadas dinamicamente, suas próprias semânticas de associação, em vez da linguagem c#, conforme especificado neste documento, assumir.

Enquanto a finalidade de associação dinâmica é permitir a interoperação com objetos dinâmicos, c# permite a associação dinâmica em todos os objetos, independentemente de estarem dinâmicos ou não. Isso permite uma integração mais suave de objetos dinâmicos, como os resultados das operações neles talvez por si próprios ser objetos dinâmicos, mas são ainda de um tipo desconhecido para o programador em tempo de compilação. Também a vinculação dinâmica pode ajudar a eliminar o código de baseados em reflexão propenso a erro mesmo quando não há objetos envolvidos são objetos dinâmicos.

As seções a seguir descrevem cada constructo no idioma for exatamente quando vinculação dinâmica é aplicada, o que compilar a verificação de tempo – se qualquer um-- é aplicada e qual classificação de resultados e a expressão de tempo de compilação é.

### <a name="types-of-constituent-expressions"></a>Tipos de expressões constituintes

Quando uma operação é vinculada estaticamente, o tipo de uma expressão constituinte (por exemplo, um receptor e argumento, um índice ou um operando) é sempre considerado como o tipo de tempo de compilação dessa expressão.

Quando uma operação dinamicamente é vinculada, o tipo de uma expressão constituinte é determinado de maneiras diferentes dependendo do tipo de tempo de compilação da expressão constituinte:

*  Uma expressão constituinte do tipo de tempo de compilação `dynamic` é considerado como tendo o tipo do valor real que a expressão é avaliada em tempo de execução
*  Uma expressão constituinte cujo tipo de tempo de compilação é um parâmetro de tipo é considerada como tendo o tipo que o parâmetro de tipo é associado em tempo de execução
*  Caso contrário, a expressão constituinte é considerada como tendo seu tipo de tempo de compilação.

## <a name="operators"></a>Operadores

Expressões são construídas a partir ***operandos*** e ***operadores***. Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos. Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`. Exemplos de operandos incluem literais, campos, variáveis locais e expressões.

Há três tipos de operadores:

*  Operadores unários. Os operadores unários usam um operando e usar a notação de prefixo (como `--x`) ou notação de sufixo (como `x++`).
*  Operadores binários. Os operadores binários usam dois operandos e todas usam a notação de infixo (como `x + y`).
*  operador ternário. Apenas um operador ternário `?:`, existe; ele usa três operandos e usa a notação de infixo (`c ? x : y`).

A ordem de avaliação de operadores em uma expressão é determinada pelo ***precedência*** e ***associatividade*** dos operadores ([precedência e associatividade](expressions.md#operator-precedence-and-associativity)) .

Os operandos em uma expressão são avaliados da esquerda para a direita. Por exemplo, na `F(i) + G(i++) * H(i)`, método `F` for chamado usando o valor antigo da `i`, em seguida, o método `G` é chamado com o valor antigo da `i`e, finalmente, o método `H` é chamado com o novo valor de `i`. Isso é separada e não relacionadas à precedência de operador.

Determinados operadores podem ser ***sobrecarregados***. Sobrecarga de operador permite que implementações de operador definido pelo usuário deve ser especificado para operações em que um ou ambos os operandos são de um tipo de classe ou estrutura definida pelo usuário ([sobrecarga de operador](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Precedência e associatividade do operador

Quando uma expressão contiver vários operadores, a ***precedência*** dos operadores controla a ordem na qual os operadores individuais são avaliados. Por exemplo, a expressão `x + y * z` é avaliado como `x + (y * z)` porque o `*` operador tem precedência maior do que o binário `+` operador. A precedência de um operador é estabelecida pela definição de sua produção de gramática associado. Por exemplo, um *additive_expression* consiste em uma sequência de *multiplicative_expression*s separados por `+` ou `-` operadores, proporcionando assim a `+` e `-` operadores menor precedência que o `*`, `/`, e `%` operadores.

A tabela a seguir resume todos os operadores na ordem de precedência da mais alta para a mais baixa:

| __Section__                                                                                   | __Categoria__                | __Operadores__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Expressões primárias](expressions.md#primary-expressions)                                     | Primária                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Operadores unários](expressions.md#unary-operators)                                             | Unário                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Operadores aritméticos](expressions.md#arithmetic-operators)                                   | Multiplicativo              | `*`  `/`  `%` | 
| [Operadores aritméticos](expressions.md#arithmetic-operators)                                   | Aditivo                    | `+`  `-`      | 
| [Operadores shift](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [Operadores de teste de tipo e relacional](expressions.md#relational-and-type-testing-operators) | Teste de tipo e relacional | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Operadores de teste de tipo e relacional](expressions.md#relational-and-type-testing-operators) | Igualdade                    | `==`  `!=`    | 
| [Operadores lógicos](expressions.md#logical-operators)                                         | AND lógico                 | `&`           | 
| [Operadores lógicos](expressions.md#logical-operators)                                         | XOR lógico                 | `^`           | 
| [Operadores lógicos](expressions.md#logical-operators)                                         | OR lógico                  | <code>&#124;</code>           |
| [Operadores lógicos condicionais](expressions.md#conditional-logical-operators)                 | AND condicional             | `&&`          | 
| [Operadores lógicos condicionais](expressions.md#conditional-logical-operators)                 | OR condicional              | <code>&#124;&#124;</code>          | 
| [O operador de coalescência nula](expressions.md#the-null-coalescing-operator)                   | Coalescência nula             | `??`          | 
| [Operador condicional](expressions.md#conditional-operator)                                   | Condicional                 | `?:`          | 
| [Operadores de atribuição](expressions.md#assignment-operators), [expressões de função anônima](expressions.md#anonymous-function-expressions)  | Atribuição e expressão lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Quando ocorre um operando entre dois operadores com a mesma precedência, a associatividade dos operadores controla a ordem na qual as operações são executadas:

*  Exceto para os operadores de atribuição e o operador de união nulo, todos os operadores binários são ***associativos à esquerda***, que significa que operações são executadas da esquerda para a direita. Por exemplo, `x + y + z` é avaliado como `(x + y) + z`.
*  Os operadores de atribuição, o operador de união nulo e o operador condicional (`?:`) estão ***associativo***, que significa que as operações são executadas da direita para esquerda. Por exemplo, `x = y = z` é avaliado como `x = (y = z)`.

Precedência e associatividade podem ser controladas usando parênteses. Por exemplo, `x + y * z` primeiro multiplica `y` por `z` e, em seguida, adiciona o resultado a `x`, mas `(x + y) * z` primeiro adiciona `x` e `y` e, em seguida, multiplica o resultado por `z`.

### <a name="operator-overloading"></a>Sobrecarga de operador

Todos os operadores unários e binários têm predefinidos implementações que ficam automaticamente disponíveis em qualquer expressão. As implementações predefinidas, além de implementações definidas pelo usuário podem ser introduzidas, incluindo `operator` declarações em classes e structs ([operadores](classes.md#operators)). Implementações de operador definido pelo usuário sempre têm precedência sobre implementações de operadores predefinidos: somente quando não há implementações aplicável operador definido pelo usuário existem serão as implementações de operador pré-definido ser considerado, conforme descrito em [ Resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution).

O ***operadores sobrecarregáveis unários*** são:
```csharp
+   -   !   ~   ++   --   true   false
```

Embora `true` e `false` não são explicitamente usados em expressões (e, portanto, não são incluídos na tabela precedência [precedência e associatividade](expressions.md#operator-precedence-and-associativity)), eles são considerados operadores porque eles são invocado em vários contextos de expressão: expressões Boolianas ([expressões Boolianas](expressions.md#boolean-expressions)) e expressões que envolvem a condicional ([operador condicional](expressions.md#conditional-operator)) e condicional lógico operadores ([operadores lógicos condicionais](expressions.md#conditional-logical-operators)).

O ***sobrecarregáveis operadores binários*** são:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Somente os operadores listados acima podem ser sobrecarregados. Em particular, não é possível sobrecarregar o acesso de membro de invocação de método, ou o `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, e `is` operadores.

Quando um operador binário está sobrecarregado, o operador de atribuição correspondente, se houver, também estará implicitamente sobrecarregado. Por exemplo, uma sobrecarga de operador `*` também é uma sobrecarga de operador `*=`. Isso é descrito posteriormente em [atribuição composta](expressions.md#compound-assignment). Observe que o operador de atribuição (`=`) não pode ser sobrecarregado. Uma atribuição sempre executa uma simple cópia bit a bit de um valor em uma variável.

Converter operações, como `(T)x`, estão sobrecarregados, fornecendo conversões definidas pelo usuário ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)).

Acesso de elemento, como `a[x]`, não é considerado um operador que pode ser sobrecarregado. Em vez disso, a indexação de definida pelo usuário tem suporte por meio de indexadores ([indexadores](classes.md#indexers)).

Em expressões, operadores são referenciados usando a notação do operador e em declarações, operadores são referenciados usando a notação funcional. A tabela a seguir mostra a relação entre o operador e notações funcionais para o operador unário ou binário. Na primeira entrada, *op* denota qualquer operador de prefixo unário que pode ser sobrecarregado. Na segunda entrada, *op* denota o sufixo unário `++` e `--` operadores. Na entrada do terceiro *op* denota qualquer operador binário que pode ser sobrecarregado.


| __Notação do operador__ | __Notação funcional__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Declarações de operador definido pelo usuário sempre exigem pelo menos um dos parâmetros para ser do tipo de classe ou struct que contém a declaração do operador. Portanto, não é possível para um operador definido pelo usuário ter a mesma assinatura como um operador predefinido.

Declarações de operador definido pelo usuário não é possível modificar a sintaxe, a precedência ou a associatividade do operador. Por exemplo, o `/` operador é sempre um operador binário, sempre tem o nível de precedência especificada no [precedência e associatividade](expressions.md#operator-precedence-and-associativity)e é sempre associativos à esquerda.

Embora seja possível para um operador definido pelo usuário executar qualquer cálculo, o que desejar, implementações que produzem resultados diferentes daqueles que intuitivamente são esperados são altamente desaconselhável. Por exemplo, uma implementação de `operator ==` deve comparar os dois operandos quanto à igualdade e retornar um apropriado `bool` resultado.

As descrições dos operadores individuais na [expressões primárias](expressions.md#primary-expressions) por meio [operadores lógicos condicionais](expressions.md#conditional-logical-operators) especificar as implementações predefinidas de operadores e todas as regras adicionais que se aplicam para cada operador. Verifique as descrições de usar os termos ***resolução de sobrecarga de operador unário***, ***resolução de sobrecarga de operador binário***, e ***promoção numérica***, definições de quais são encontrado nas seções a seguir.

### <a name="unary-operator-overload-resolution"></a>Resolução de sobrecarga de operador unário

Uma operação do formulário `op x` ou `x op`, onde `op` é um operador unário que pode ser sobrecarregado, e `x` é uma expressão do tipo `X`, é processado da seguinte maneira:

*  O conjunto de operadores definidos pelo usuário de candidato fornecidos pelo `X` para a operação `operator op(x)` é determinado usando as regras da [operadores definidos pelo usuário do candidato](expressions.md#candidate-user-defined-operators).
*  Se o conjunto de operadores definidos pelo usuário do candidato não estiver vazio, em seguida, isso se torna o conjunto de operadores de candidato para a operação. Caso contrário, o operador unário de predefinidos `operator op` implementações, incluindo seus formulários elevados, tornam-se o conjunto de operadores de candidato para a operação. As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([expressões primárias](expressions.md#primary-expressions) e [operadores unários](expressions.md#unary-operators)).
*  As regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores de candidato para selecionar o operador de melhor em relação à lista de argumentos `(x)`, e este operador torna-se o resultado da sobrecarga processo de resolução. Se a resolução de sobrecarga não selecionar um único operador melhor, ocorrerá um erro de tempo de associação.

### <a name="binary-operator-overload-resolution"></a>Resolução de sobrecarga de operador binário

Uma operação do formulário `x op y`, onde `op` é um operador binário que pode ser sobrecarregado, `x` é uma expressão do tipo `X`, e `y` é uma expressão do tipo `Y`, é processado da seguinte maneira:

*  O conjunto de operadores definidos pelo usuário de candidato fornecidos pelo `X` e `Y` para a operação `operator op(x,y)` é determinado. O conjunto consiste na união dos operadores candidato fornecidos pelo `X` e os operadores de candidato fornecidos pelo `Y`, cada determinado usando as regras da [operadores definidos pelo usuário do candidato](expressions.md#candidate-user-defined-operators). Se `X` e `Y` são do mesmo tipo, ou se `X` e `Y` são derivados de um tipo de base comum, em seguida, operadores de candidato compartilhado ocorrerem somente em conjunto combinado uma vez.
*  Se o conjunto de operadores definidos pelo usuário do candidato não estiver vazio, em seguida, isso se torna o conjunto de operadores de candidato para a operação. Caso contrário, o binário predefinido `operator op` implementações, incluindo seus formulários elevados, tornam-se o conjunto de operadores de candidato para a operação. As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([operadores aritméticos](expressions.md#arithmetic-operators) por meio [operadores lógicos condicionais](expressions.md#conditional-logical-operators)). Para os operadores predefinidos para enum e delegado, os operadores apenas considerados são aquelas definidas por um tipo de enum ou delegado que é o tipo de tempo de associação de um dos operandos.
*  As regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores de candidato para selecionar o operador de melhor em relação à lista de argumentos `(x,y)`, e este operador torna-se o resultado da sobrecarga processo de resolução. Se a resolução de sobrecarga não selecionar um único operador melhor, ocorrerá um erro de tempo de associação.

### <a name="candidate-user-defined-operators"></a>Operadores definidos pelo usuário do candidato

Dado um tipo `T` e uma operação `operator op(A)`, onde `op` é um operador que pode ser sobrecarregado e `A` é uma lista de argumentos, o conjunto de candidatos operadores definidos pelo usuário fornecidos pelo `T` para `operator op(A)` é determinada da seguinte maneira:

*  Determinar o tipo `T0`. Se `T` é um tipo anulável, `T0` é o seu tipo subjacente, caso contrário `T0` é igual a `T`.
*  Para todos os `operator op` declarações na `T0` e todas as retiradas formas desses operadores, se pelo menos um operador é aplicável ([membro da função aplicável](expressions.md#applicable-function-member)) em relação à lista de argumentos `A`, em seguida, o conjunto de operadores de candidato consiste em todos os tais operadores aplicáveis na `T0`.
*  Caso contrário, se `T0` é `object`, o conjunto de operadores de release candidate está vazio.
*  Caso contrário, o conjunto de operadores de candidato fornecidos pelo `T0` é o conjunto de operadores de candidato fornecidos pela classe base direta de `T0`, ou a classe base efetiva de `T0` se `T0` é um parâmetro de tipo.

### <a name="numeric-promotions"></a>Promoções numéricas

Promoção numérica consiste em realizar automaticamente determinadas conversões implícitas de operandos dos operadores numéricos binários e unária predefinida. Promoção numérica não é um mecanismo distinto, mas em vez disso, um efeito de aplicar a resolução de sobrecarga para os operadores predefinidos. Promoção numérica especificamente não afeta a avaliação dos operadores definidos pelo usuário, embora os operadores definidos pelo usuário podem ser implementados para apresentar efeitos semelhantes.

Como um exemplo de promoção numérico, considere as implementações predefinidas do binário `*` operador:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Quando as regras de resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)) são aplicadas a esse conjunto de operadores, o efeito é selecionar o primeiro dos operadores para o qual existem conversões implícitas entre os tipos de operando. Por exemplo, para a operação `b * s`, onde `b` é um `byte` e `s` é um `short`, seleciona de resolução de sobrecarga `operator *(int,int)` como o operador melhor. Portanto, o efeito é que `b` e `s` são convertidos em `int`, e o tipo do resultado é `int`. Da mesma forma, para a operação `i * d`, onde `i` é um `int` e `d` é um `double`, seleciona de resolução de sobrecarga `operator *(double,double)` como o operador melhor.

#### <a name="unary-numeric-promotions"></a>Promoções de numérico unário

Promoção de unário numérica ocorre para os operandos de predefinida `+`, `-`, e `~` operadores unários. Promoção de numérico unária consiste apenas em operandos do tipo de conversão `sbyte`, `byte`, `short`, `ushort`, ou `char` digitar `int`. Além disso, para o operador unário `-` operador unário de promoção numérico converte operandos do tipo `uint` digitar `long`.

#### <a name="binary-numeric-promotions"></a>Promoções numéricas binárias

Promoção de numérica binária ocorre para os operandos de predefinida `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, e `<=` operadores binários. Promoção numérica binária implicitamente converte ambos os operandos em um tipo comum que, no caso dos operadores relacionais, também se tornará o tipo de resultado da operação. Promoção de numérica binária consiste em Aplicar regras a seguir, na ordem em que eles aparecem aqui:

*  Se qualquer operando for do tipo `decimal`, o outro operando será convertido ao tipo `decimal`, ou um erro em tempo de vinculação ocorrerá se o outro operando for do tipo `float` ou `double`.
*  Caso contrário, se qualquer operando for do tipo `double`, o outro operando será convertido ao tipo `double`.
*  Caso contrário, se qualquer operando for do tipo `float`, o outro operando será convertido ao tipo `float`.
*  Caso contrário, se qualquer operando for do tipo `ulong`, o outro operando será convertido ao tipo `ulong`, ou um erro em tempo de vinculação ocorrerá se o outro operando for do tipo `sbyte`, `short`, `int`, ou `long`.
*  Caso contrário, se qualquer operando for do tipo `long`, o outro operando será convertido ao tipo `long`.
*  Caso contrário, se qualquer operando for do tipo `uint` e o outro operando for do tipo `sbyte`, `short`, ou `int`, ambos os operandos serão convertidos ao tipo `long`.
*  Caso contrário, se qualquer operando for do tipo `uint`, o outro operando será convertido ao tipo `uint`.
*  Caso contrário, os dois operandos serão convertidos ao tipo `int`.

Observe que a primeira regra não permite todas as operações que combinam os `decimal` tipo com o `double` e `float` tipos. A regra segue do fato de que não há nenhuma conversão implícita entre o `decimal` tipo e o `double` e `float` tipos.

Observe também que não é possível que um operando para ser do tipo `ulong` quando o outro operando for de um tipo integral com sinal. O motivo é que existe nenhum tipo integral que pode representar a gama completa de `ulong` , bem como os tipos integrais com sinal.

Em ambos os casos acima, uma expressão de conversão pode ser usada para converter explicitamente um operando em um tipo que é compatível com o outro operando.

No exemplo
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
um erro de tempo de associação ocorre porque uma `decimal` não pode ser multiplicada por uma `double`. O erro seja resolvido com a conversão explícita para o segundo operando `decimal`, da seguinte maneira:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Operadores canceladas

***Eliminada operadores*** permitir que operadores predefinidos e definidos pelo usuário que operam em tipos de valor não anulável para também ser usados com formulários que permitem valor nulos desses tipos. Operadores elevadas são construídos a partir de operadores predefinidos e definidos pelo usuário que atendem a certos requisitos, conforme descrito a seguir:

*   Para os operadores unários

    ```csharp
    +  ++  -  --  !  ~
    ```

    existe em um formato elevado de um operador se os tipos de operando e o resultado são os dois tipos de valor não anulável. O formulário elevado é construído com a adição de um único `?` modificador para os tipos de operando e resultado. O operador elevado produz um valor nulo se o operando for nulo. Caso contrário, o operador cancelado ou não, o operando, aplica o operador subjacente e encapsula o resultado.

*   Para os operadores binários

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    existe em um formato elevado de um operador se os tipos de operando e o resultado são todos os tipos de valor não anulável. O formulário elevado é construído com a adição de um único `?` modificador para cada tipo de operando e o resultado. O operador elevado produz um valor nulo se um ou ambos os operandos forem nulos (uma exceção que está sendo a `&` e `|` operadores da `bool?` de tipo, conforme descrito em [boolianos operadores lógicos](expressions.md#boolean-logical-operators)). Caso contrário, o operador cancelado ou não, os operandos, aplica o operador subjacente e encapsula o resultado.

*   Para os operadores de igualdade

    ```csharp
    ==  !=
    ```

    um formulário elevado de um operador existe se os tipos de operando forem tipos de valor não anulável e se o tipo de resultado é `bool`. O formulário elevado é construído com a adição de um único `?` modificador para cada tipo de operando. O operador elevado considera iguais de dois valores nulos e um valor nulo desiguais para qualquer valor não nulo. Se ambos os operandos forem não nulos, o operador de cancelada ou não, os operandos e aplica o operador de base para produzir o `bool` resultado.

*   Para os operadores relacionais

    ```csharp
    <  >  <=  >=
    ```

    um formulário elevado de um operador existe se os tipos de operando forem tipos de valor não anulável e se o tipo de resultado é `bool`. O formulário elevado é construído com a adição de um único `?` modificador para cada tipo de operando. O operador elevado produz o valor `false` se um ou ambos os operandos são nulos. Caso contrário, o operador cancelado ou não, os operandos e aplica o operador de base para produzir o `bool` resultado.

## <a name="member-lookup"></a>Pesquisa de membro

Uma pesquisa de membro é o processo pelo qual o significado de um nome no contexto de um tipo é determinado. Uma pesquisa de membro pode ocorrer como parte da avaliação de uma *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)) em um expressão. Se o *simple_name* ou *member_access* ocorre como o *primary_expression* de uma *invocation_expression* ([ As invocações de método](expressions.md#method-invocations)), o membro deve ser invocado.

Se um membro é um método ou evento, ou se for uma constante, campo ou propriedade de um tipo de delegado ([delegados](delegates.md)) ou o tipo `dynamic` ([tipo dinâmico](types.md#the-dynamic-type)), em seguida, o membro deve ser *invocable*.

Pesquisa de membro considera não apenas o nome de um membro, mas também o número de parâmetros de tipo que do membro e se o membro for acessível. Para fins de pesquisa de membro, métodos genéricos e tipos genéricos aninhados têm o número de parâmetros de tipo indicado em suas respectivas declarações e todos os outros membros têm zero parâmetros de tipo.

Uma pesquisa de um nome de membro `N` com `K` parâmetros de tipo em um tipo `T` é processado da seguinte maneira:

*  Primeiro, um conjunto de membros acessíveis chamado `N` é determinado:
    * Se `T` é um parâmetro de tipo, em seguida, o conjunto é a união dos conjuntos de membros acessíveis nomeados `N` em cada um dos tipos especificados como uma restrição primária ou restrição secundária ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) para `T`, juntamente com o conjunto de membros acessíveis nomeados `N` em `object`.
    * Caso contrário, o conjunto consiste em todos os acessível ([acesso de membro](basic-concepts.md#member-access)) membros nomeados `N` na `T`, incluindo membros herdados e os membros acessíveis nomeados `N` em `object`. Se `T` é um tipo construído, o conjunto de membros é obtido, substituindo os argumentos de tipo, conforme descrito em [membros de tipos construídos](classes.md#members-of-constructed-types). Os membros que incluem um `override` modificador são excluídos do conjunto.
*  Em seguida, se `K` for zero, todos os aninhados de tipos cujas declarações incluem parâmetros de tipo são removidos. Se `K` for diferente de zero, todos os membros com um número diferente de tipo de parâmetros são removidos. Observe que, quando `K` é zero, métodos com parâmetros não forem removidos, desde que o processo de inferência de tipo do tipo ([inferência de tipo](expressions.md#type-inference)) pode ser capaz de inferir os argumentos de tipo.
*  Em seguida, se o membro é *invocado*, todos os não-*invocable* os membros são removidos do conjunto.
*  Em seguida, os membros que estão ocultos por outros membros são removidos do conjunto. Para cada membro `S.M` no conjunto, onde `S` é o tipo no qual o membro `M` for declarado, as seguintes regras são aplicadas:
    * Se `M` é uma constante, campo, propriedade, evento ou membro de enumeração, em seguida, todos os membros declarados em um tipo base do `S` são removidas do conjunto.
    * Se `M` é uma declaração de tipo e, em seguida, todos os tipos que não são declarados em um tipo base de `S` são removidas do conjunto, e todas as declarações com o mesmo número de parâmetros de tipo como de tipo `M` declarado em um tipo base do `S` são removidos do conjunto.
    * Se `M` é um método, em seguida, todos os membros não-método declarado em um tipo base do `S` são removidas do conjunto.
*  Em seguida, os membros de interface que ficam ocultos por membros de classe são removidos do conjunto. Esta etapa só terá efeito se `T` é um parâmetro de tipo e `T` tem os dois uma classe base efetivada diferente de `object` e uma interface de efetivada vazio definido ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Para cada membro `S.M` no conjunto, onde `S` é o tipo no qual o membro `M` for declarado, as seguintes regras são aplicadas se `S` é uma declaração de classe diferente de `object`:
    * Se `M` é uma constante, campo, propriedade, evento, membro de enumeração ou declaração de tipo, em seguida, todos os membros declarados na declaração de interface são removidos do conjunto.
    * Se `M` é um método, em seguida, todos os membros não-método declarados em uma declaração de interface são removidos do conjunto e todos os métodos com a mesma assinatura que `M` declarado em uma interface de declaração são removidos do conjunto.
*  Por fim, o resultado da pesquisa de ter removido a membros ocultos, é determinado:
    * Se o conjunto consiste em um único membro que não é um método, esse membro é o resultado da pesquisa.
    * Caso contrário, se o conjunto contém apenas os métodos, esse grupo de métodos é o resultado da pesquisa.
    * Caso contrário, a pesquisa é ambígua e ocorre um erro em tempo de vinculação.

Para pesquisas de membro em tipos diferentes de parâmetros de tipo e interfaces e pesquisas de membro em interfaces que são estritamente herança única (tem exatamente zero ou uma interface direta da base de cada interface na cadeia de herança), é o efeito das regras de pesquisa simplesmente que derivado membros ocultar membros base com o mesmo nome ou assinatura. Essas pesquisas de herança única nunca são ambíguas. As ambiguidades que possivelmente podem surgir de pesquisas de membro em interfaces de herança múltipla são descritas em [acesso de membro de Interface](interfaces.md#interface-member-access).

### <a name="base-types"></a>Tipos base

Para fins de pesquisa de membro, um tipo `T` é considerado como tendo os seguintes tipos de base:

*  Se `T` está `object`, em seguida, `T` não tem nenhum tipo de base.
*  Se `T` é um *enum_type*, os tipos base do `T` são os tipos de classe `System.Enum`, `System.ValueType`, e `object`.
*  Se `T` é um *struct_type*, os tipos base do `T` são os tipos de classe `System.ValueType` e `object`.
*  Se `T` é um *class_type*, os tipos base do `T` são as classes base `T`, incluindo o tipo de classe `object`.
*  Se `T` é um *interface_type*, os tipos base do `T` são as interfaces base dos `T` e o tipo de classe `object`.
*  Se `T` é um *array_type*, os tipos base do `T` são os tipos de classe `System.Array` e `object`.
*  Se `T` é um *delegate_type*, os tipos base do `T` são os tipos de classe `System.Delegate` e `object`.

## <a name="function-members"></a>Membros da função

Membros da função são membros que contêm instruções executáveis. Membros da função sempre são membros de tipos e não podem ser membros de namespaces. C# define as seguintes categorias de membros da função:

*  Métodos
*  Propriedades
*  Eventos
*  Indexadores
*  Operadores definidos pelo usuário
*  Construtores de instância
*  Construtores estáticos
*  Destruidores

Exceto para os destruidores e construtores estáticos (que não podem ser invocados explicitamente), as instruções contidas em membros de função são executadas por meio de invocações de função de membro. A sintaxe real para a gravação de uma invocação de membro de função depende a categoria de membro de função específica.

A lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de um membro da função chamada fornece os valores reais ou referências de variável para os parâmetros do membro da função.

As invocações de métodos genéricos podem empregar a inferência de tipo para determinar o conjunto de argumentos de tipo para passar para o método. Esse processo é descrito em [inferência de tipo](expressions.md#type-inference).

As invocações de métodos, indexadores, operadores e construtores de instância empregam a resolução de sobrecarga para determinar quais de um conjunto de candidatos de membros da função para invocar. Esse processo é descrito em [resolução de sobrecarga](expressions.md#overload-resolution).

Depois que um membro da função específica tiver sido identificado em tempo de associação, possivelmente por meio da resolução de sobrecarga, o processo de tempo de execução real de invocar o membro da função é descrito no [verificação de resolução de sobrecarga dinâmicostempodecompilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

A tabela a seguir resume o processamento que ocorre em construções que envolvem as seis categorias de membros da função que podem ser chamados explicitamente. Na tabela, `e`, `x`, `y`, e `value` indicar classificadas como variáveis ou valores, as expressões `T` indica uma expressão classificada como um tipo, `F` é o nome simple de um método e `P` é o nome simple de uma propriedade.


| __Construir__     | __Exemplo__    | __Descrição__ |
|-------------------|----------------|-----------------|
| Invocação de método | `F(x,y)`       | Resolução de sobrecarga é aplicada para selecionar o melhor método `F` na classe ou struct recipiente. O método é invocado com a lista de argumentos `(x,y)`. Se o método não for `static`, a expressão de instância é `this`. | 
|                   | `T.F(x,y)`     | Resolução de sobrecarga é aplicada para selecionar o melhor método de `F` na classe ou struct `T`. Um erro em tempo de vinculação ocorrerá se o método não é `static`. O método é invocado com a lista de argumentos `(x,y)`. | 
|                   | `e.F(x,y)`     | Resolução de sobrecarga é aplicada para selecionar o melhor método F na classe, struct ou interface fornecido pelo tipo de `e`. Um erro em tempo de vinculação ocorrerá se o método é `static`. O método é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`. | 
| Acesso de propriedade   | `P`            | O `get` acessador da propriedade `P` na classe ou struct recipiente é invocado. Ocorrerá um erro de tempo de compilação se `P` é somente gravação. Se `P` não é `static`, a expressão de instância é `this`. | 
|                   | `P = value`    | O `set` acessador da propriedade `P` na classe ou struct recipiente é invocado com a lista de argumentos `(value)`. Ocorrerá um erro de tempo de compilação se `P` é somente leitura. Se `P` não é `static`, a expressão de instância é `this`. | 
|                   | `T.P`          | O `get` acessador da propriedade `P` na classe ou struct `T` é invocado. Ocorrerá um erro de tempo de compilação se `P` não é `static` ou se `P` é somente gravação. | 
|                   | `T.P = value`  | O `set` acessador da propriedade `P` na classe ou struct `T` é invocado com a lista de argumentos `(value)`. Ocorrerá um erro de tempo de compilação se `P` não é `static` ou se `P` é somente leitura. | 
|                   | `e.P`          | O `get` acessador da propriedade `P` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e`. Um erro em tempo de vinculação ocorrerá se `P` está `static` ou se `P` é somente gravação. | 
|                   | `e.P = value`  | O `set` acessador da propriedade `P` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e` e a lista de argumentos `(value)`. Um erro em tempo de vinculação ocorrerá se `P` está `static` ou se `P` é somente leitura. | 
| Acesso ao evento      | `E += value`   | O `add` acessador do evento `E` na classe ou struct recipiente é invocado. Se `E` é não estático, a expressão de instância é `this`. | 
|                   | `E -= value`   | O `remove` acessador do evento `E` na classe ou struct recipiente é invocado. Se `E` é não estático, a expressão de instância é `this`. | 
|                   | `T.E += value` | O `add` acessador do evento `E` na classe ou struct `T` é invocado. Ocorrerá um erro de tempo de associação se `E` não é estático. | 
|                   | `T.E -= value` | O `remove` acessador do evento `E` na classe ou struct `T` é invocado. Ocorrerá um erro de tempo de associação se `E` não é estático. | 
|                   | `e.E += value` | O `add` acessador do evento `E` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e`. Ocorrerá um erro de tempo de associação se `E` é estático. | 
|                   | `e.E -= value` | O `remove` acessador do evento `E` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e`. Ocorrerá um erro de tempo de associação se `E` é estático. | 
| Acesso do indexador    | `e[x,y]`       | Resolução de sobrecarga é aplicada para selecionar o indexador melhor na classe, struct ou interface fornecido pelo tipo de e. O `get` acessador do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`. Um erro em tempo de vinculação ocorrerá se o indexador é somente gravação. | 
|                   | `e[x,y] = value` | Resolução de sobrecarga é aplicada para selecionar o indexador melhor na classe, struct ou interface fornecido pelo tipo de `e`. O `set` acessador do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y,value)`. Um erro em tempo de vinculação ocorrerá se o indexador é somente leitura. | 
| Invocação de operador | `-x`         | Resolução de sobrecarga é aplicada para selecionar o operador unário melhor na classe ou struct fornecido pelo tipo de `x`. O operador selecionado é invocado com a lista de argumentos `(x)`. | 
|                     | `x + y`      | Resolução de sobrecarga é aplicada para selecionar o operador binário melhor nas classes ou structs fornecido pelos tipos de `x` e `y`. O operador selecionado é invocado com a lista de argumentos `(x,y)`. | 
| Invocação do construtor de instância | `new T(x,y)` | Resolução de sobrecarga é aplicada para selecionar o melhor construtor de instância na classe ou struct `T`. O construtor de instância é invocado com a lista de argumentos `(x,y)`. | 

### <a name="argument-lists"></a>Listas de argumentos

Cada invocação de membro e o delegado de função inclui uma lista de argumentos que fornece os valores reais ou referências de variável para os parâmetros do membro da função. A sintaxe para especificar a lista de argumentos de uma invocação de membro de função depende a categoria de membro da função:

*  Por exemplo construtores, métodos, indexadores e delegados, os argumentos são especificados como um *argument_list*, conforme descrito abaixo. Para indexadores, ao invocar o `set` acessador, a lista de argumentos Além disso, inclui a expressão especificada como o operando à direita do operador de atribuição.
*  Para propriedades, a lista de argumentos está vazia ao invocar o `get` acessador e consiste na expressão especificada como o operando à direita do operador de atribuição ao invocar o `set` acessador.
*  Para eventos, a lista de argumentos é formada pela expressão especificada como o operando direito dos `+=` ou `-=` operador.
*  Operadores definidos pelo usuário, a lista de argumentos consiste em único operando do operador unário ou os dois operandos do operador binário.

Os argumentos de propriedades ([propriedades](classes.md#properties)), eventos ([eventos](classes.md#events)) e operadores definidos pelo usuário ([operadores](classes.md#operators)) sempre são passados como parâmetros de valor ([ Parâmetros de valores](classes.md#value-parameters)). Os argumentos de indexadores ([indexadores](classes.md#indexers)) sempre são passados como parâmetros de valor ([parâmetros de valores](classes.md#value-parameters)) ou matrizes de parâmetros ([matrizes de parâmetro](classes.md#parameter-arrays)). Não há suporte para parâmetros de referência e de saída para essas categorias de membros da função.

Os argumentos de uma invocação de construtor, método, indexador ou delegado de instância são especificados como um *argument_list*:

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

Uma *argument_list* consiste em um ou mais *argumento*s, separados por vírgulas. Cada argumento consiste em um recurso opcional *argument_name* seguido por um *argument_value*. Uma *argumento* com um *argument_name* é conhecido como um ***argumento nomeado***, enquanto um *argumento* sem um  *argument_name* é um ***argumento posicional***. É um erro para um argumento posicional aparecer após um argumento nomeado em uma *argument_list*.

O *argument_value* pode assumir um dos seguintes formatos:

*  Uma *expressão*, indicando que o argumento é passado como um parâmetro de valor ([parâmetros de valores](classes.md#value-parameters)).
*  A palavra-chave `ref` seguido por um *variable_reference* ([referências variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de referência ([fazer referência a parâmetros ](classes.md#reference-parameters)). Uma variável deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes que ele pode ser passado como um parâmetro de referência. A palavra-chave `out` seguido por um *variable_reference* ([referências variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de saída ([deparâmetrosdesaída](classes.md#output-parameters)). Uma variável é considerada atribuída definitivamente ([atribuição definitiva](variables.md#definite-assignment)) após uma invocação de membro de função em que a variável é passada como um parâmetro de saída.

#### <a name="corresponding-parameters"></a>Parâmetros correspondentes

Para cada argumento na lista de argumentos deve ser um parâmetro correspondente no membro da função ou delegado que está sendo invocado.

A lista de parâmetros usada no seguinte é determinada da seguinte maneira:

*  Para indexadores definidos nas classes e métodos virtuais, a lista de parâmetros separado da declaração mais específica ou substituição do membro da função, começando com o tipo estático do receptor e a pesquisa por meio de suas classes base.
*  Para métodos de interface e indexadores, a lista de parâmetros é escolhida formam a definição mais específica do membro, começando com o tipo de interface e a pesquisa por meio de interfaces base. Se nenhuma lista de parâmetro exclusivo é encontrado, uma lista de parâmetros com nomes inacessíveis e sem parâmetros opcionais é construída, para que as invocações não é possível usar parâmetros nomeados ou omitir argumentos opcionais.
*  Para métodos parciais, lista de parâmetros de declaração de definição de método parcial é usada.
*  Para todos os outros membros da função e representantes, há apenas uma lista de parâmetro único, que é usada.

A posição de um argumento ou um parâmetro é definida como o número de argumentos ou parâmetros que precedem-lo na lista de argumentos ou lista de parâmetros.

Os parâmetros para argumentos da função membro correspondentes são estabelecidos da seguinte maneira:

*  Argumentos de *argument_list* de construtores de instância, métodos, indexadores e delegados:
    * Um argumento posicional em que um parâmetro fixado ocorre na mesma posição na lista de parâmetros corresponde a esse parâmetro.
    * Um argumento posicional de um membro da função com uma matriz de parâmetros invocado em sua forma normal corresponde à matriz de parâmetros, que deve ocorrer na mesma posição na lista de parâmetros.
    * Um argumento posicional de um membro da função com uma matriz de parâmetros invocado em sua forma expandida, onde não ocorre a nenhum parâmetro fixo na mesma posição na lista de parâmetros, corresponde a um elemento na matriz de parâmetros.
    * Um argumento nomeado corresponde ao parâmetro de mesmo nome na lista de parâmetros.
    * Para indexadores, ao invocar o `set` acessador, a expressão especificada como o operando à direita do operador de atribuição corresponde ao implícito `value` parâmetro do `set` declaração do acessador.
*  Para propriedades, ao invocar o `get` acessador lá estão sem argumentos. Ao invocar o `set` acessador, a expressão especificada como o operando à direita do operador de atribuição corresponde ao implícito `value` parâmetro do `set` declaração do acessador.
*  Operadores de unário definido pelo usuário (incluindo conversões), o único operando corresponde ao parâmetro único da declaração do operador.
*  Operadores binários definidos pelo usuário, o operando esquerdo corresponde ao primeiro parâmetro e o operando direito corresponde ao segundo parâmetro da declaração do operador.

#### <a name="run-time-evaluation-of-argument-lists"></a>Avaliação do tempo de execução das listas de argumentos

Durante o processamento de tempo de execução de uma invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), as expressões ou referências de variável de uma lista de argumentos são avaliadas em ordem, da esquerda para a direita, como a seguir:

*  Para um parâmetro de valor, a expressão de argumento é avaliada e uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para o parâmetro correspondente tipo é executado. O valor resultante se torna o valor inicial do parâmetro value na invocação de membro da função.
*  Para um parâmetro de referência ou de saída, a referência de variável é avaliada e o local de armazenamento resultante se torna o local de armazenamento, representado pelo parâmetro na invocação de membro da função. Se a referência de variável fornecida como um referência ou parâmetro de saída é um elemento de matriz de um *reference_type*, é realizada uma verificação de tempo de execução para garantir que o tipo de elemento da matriz é idêntico ao tipo do parâmetro. Se essa verificação falha, um `System.ArrayTypeMismatchException` é gerada.

Métodos, indexadores e construtores de instância podem declarar o parâmetro mais à direita como uma matriz de parâmetro ([matrizes de parâmetro](classes.md#parameter-arrays)). Esses membros de função são invocados em sua forma normal ou em seus formulários expandidos, dependendo do que é aplicável ([membro da função aplicável](expressions.md#applicable-function-member)):

*  Quando um membro de função com uma matriz de parâmetros é invocado em sua forma normal, o argumento fornecido para a matriz de parâmetros deve ser uma única expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetro. Nesse caso, a matriz de parâmetro Age exatamente como um parâmetro de valor.
*  Quando um membro de função com uma matriz de parâmetros é invocado em sua forma expandida, a invocação deve especificar zero ou mais argumentos posicionais para a matriz de parâmetros, onde cada argumento é uma expressão que é implicitamente conversível ([implícitas conversões](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros. Nesse caso, a invocação cria uma instância do tipo de parâmetro de matriz com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecido e usa a instância de matriz recém-criado como o real argumento.

As expressões de uma lista de argumentos sempre são avaliadas na ordem em que elas são gravadas. Portanto, o exemplo
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
produz a saída
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

As regras de variação conjunta de matriz ([covariância de matriz](arrays.md#array-covariance)) permite um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que existe uma conversão de referência implícita de `B` para `A`. Devido a essas regras, quando um elemento de matriz de um *reference_type* é passado como um parâmetro de saída ou de referência, uma verificação de tempo de execução é necessária para garantir que o tipo de elemento real da matriz é idêntico do parâmetro. No exemplo
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
a segunda chamada de `F` faz com que um `System.ArrayTypeMismatchException` seja lançada porque o tipo de elemento real do `b` é `string` e não `object`.

Quando um membro de função com uma matriz de parâmetros é invocado em sua forma expandida, a invocação é processada exatamente como se uma expressão de criação de matriz com um inicializador de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)) foi inserido em torno de parâmetros expandidos. Por exemplo, dada a declaração
```csharp
void F(int x, int y, params object[] args);
```
as seguintes invocações da forma expandida do método
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
corresponder exatamente à
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Em particular, observe que uma matriz vazia é criada quando há zero argumentos fornecidos para a matriz de parâmetros.

Quando os argumentos forem omitidos de um membro da função com parâmetros opcionais correspondentes, os argumentos padrão da declaração de membro da função são passados implicitamente. Porque eles sempre são constantes, sua avaliação não afetará a ordem de avaliação dos argumentos restantes.

### <a name="type-inference"></a>Inferência de tipo

Quando um método genérico é chamado sem especificar argumentos de tipo, uma ***inferência de tipo*** processo tenta inferir argumentos de tipo para a chamada. A presença de inferência de tipo permite uma sintaxe mais conveniente a ser usado para chamar um método genérico e permite ao programador Evite especificar informações de tipo redundantes. Por exemplo, dada a declaração de método:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
é possível invocar o `Choose` método sem especificar explicitamente um argumento de tipo:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Por meio de inferência de tipo, os argumentos de tipo `int` e `string` são determinados dos argumentos para o método.

Inferência de tipo ocorre como parte do processamento de tempo de associação de uma invocação de método ([invocações de método](expressions.md#method-invocations)) e ocorre antes da etapa de resolução de sobrecarga da invocação. Quando um grupo de método em particular for especificado em uma invocação de método e nenhum argumento de tipo é especificado como parte da invocação do método, a inferência de tipos é aplicada a cada método genérico no grupo de método. Se a inferência de tipo for bem-sucedida, os argumentos de tipo inferidos são usados para determinar os tipos de argumentos para a resolução de sobrecarga subsequentes. Se a resolução de sobrecarga escolhe um método genérico que o usado para invocar, os argumentos de tipo inferidos são usados como os argumentos de tipo real para a invocação. Se a falha de inferência de tipo para um método específico, esse método não participará na resolução de sobrecarga. A falha de inferência de tipo, por si só, não causa um erro em tempo de vinculação. No entanto, isso geralmente leva a um erro em tempo de vinculação quando a resolução de sobrecarga, em seguida, consegue encontrar nenhum método aplicável.

Se o número fornecido de argumentos é diferente do número de parâmetros no método, em seguida, inferência imediatamente falhará. Caso contrário, suponha que o método genérico tem a seguinte assinatura:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Com uma chamada de método do formulário `M(E1...Em)` a tarefa de inferência de tipo é encontrar os argumentos de tipo exclusivo `S1...Sn` para cada um dos parâmetros de tipo `X1...Xn` para que a chamada `M<S1...Sn>(E1...Em)` se torna válido.

Durante o processo de inferência de cada parâmetro de tipo `Xi` seja *fixo* a um determinado tipo `Si` ou *unfixed* com um conjunto associado de *limites*. Cada um dos limites é algum tipo `T`. Inicialmente, cada variável de tipo `Xi` é unfixed com um conjunto vazio de limites.

Inferência de tipos ocorre em fases. Cada fase tentará inferir argumentos de tipo para obter mais variáveis de tipo com base nas descobertas da fase anterior. A primeira fase faz algumas inferências inicias dos limites, enquanto a segunda fase variáveis de tipo para tipos específicos de correções e infere o dos limites. A segunda fase pode ter que ser repetido um número de vezes.

*Observação:* tipo inferência ocorre não apenas quando um método genérico é chamado. Inferência de tipo para conversão de grupos de método é descrita em [inferência de tipos para a conversão de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) e encontrar o melhor tipo comum de um conjunto de expressões é descrita em [localizando o melhor tipo comum de um conjunto expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>A primeira fase

Para cada um dos argumentos de método `Ei`:

*   Se `Ei` é uma função anônima, uma *inferência de tipo de parâmetro explícito* ([inferências de tipo de parâmetro explícito](expressions.md#explicit-parameter-type-inferences)) é feita do `Ei` para `Ti`
*   Caso contrário, se `Ei` tem um tipo `U` e `xi` é um parâmetro de valor, uma *inferência de tipos de limite inferior* é feita *do* `U` *para* `Ti`.
*   Caso contrário, se `Ei` tem um tipo `U` e `xi` é uma `ref` ou `out` parâmetro, uma *exato inferência* é feita *de* `U` *à* `Ti`.
*   Caso contrário, nenhuma inferência é feita para esse argumento.


#### <a name="the-second-phase"></a>A segunda fase

A segunda fase procede da seguinte maneira:

*   Todos os *unfixed* variáveis do tipo `Xi` quais não *dependem* ([dependência](expressions.md#dependence)) qualquer `Xj` são fixos ([corrigindo](expressions.md#fixing)).
*   Se essas variáveis de tipo não existir, todos os *unfixed* variáveis do tipo `Xi` são *fixa* para o qual todos os itens a seguir manter:
    *   Há pelo menos uma variável de tipo `Xj` depende do que `Xi`
    *   `Xi` tem um conjunto não vazio de limites
*   Se essas variáveis de tipo não existem e ainda houver *unfixed* variáveis do tipo, falha de inferência de tipo.
*   Caso contrário, se nenhuma outra *unfixed* as variáveis de tipo existem, inferência de tipo é bem-sucedida.
*   Caso contrário, para todos os argumentos `Ei` com o tipo de parâmetro correspondente `Ti` em que o *tipos de saída* ([tipos de saída](expressions.md#output-types)) contêm *unfixed* variáveis do tipo `Xj` , mas o *tipos de entrada* ([tipos de entrada](expressions.md#input-types)) não fizer isso, uma *inferência de tipo de saída* ([inferências de tipo de saída ](expressions.md#output-type-inferences)) é feita *partir* `Ei` *para* `Ti`. Em seguida, a segunda fase é repetida.

#### <a name="input-types"></a>Tipos de entrada

Se `E` é um grupo de método ou função anônima de tipo implícito e `T` é um delegado, tipo ou tipo de árvore de expressão e todos os tipos de parâmetro `T` são *tipos de entrada* de `E` *com tipo* `T`.

####  <a name="output-types"></a>Tipos de saída

Se `E` é um grupo de método ou uma função anônima e `T` é um delegado, tipo ou tipo de árvore de expressão e o tipo de retorno `T` é uma *tipo de saída* `E` *com tipo*  `T`.

#### <a name="dependence"></a>Dependência

Uma *unfixed* variável de tipo `Xi` *depende diretamente* uma variável do tipo unfixed `Xj` se algum argumento `Ek` com tipo `Tk` `Xj` ocorre em um *tipo de entrada* de `Ek` com tipo `Tk` e `Xi` ocorre em um *tipo de saída* de `Ek` com tipo `Tk`.

`Xj` *depende* `Xi` se `Xj` *depende diretamente* `Xi` ou se `Xi` *depende diretamente* `Xk` e `Xk` *depende* `Xj`. Portanto, "depende" é o fechamento transitivo, mas não reflexivo de "depende diretamente".

#### <a name="output-type-inferences"></a>Inferências de tipo de saída

Uma *inferência de tipo de saída* é feita *partir* uma expressão `E` *para* um tipo `T` da seguinte maneira:

*  Se `E` é uma função anônima com o tipo de retorno deduzido `U` ([tipo de retorno inferido](expressions.md#inferred-return-type)) e `T` é um tipo de delegado ou tipo de árvore de expressão com o tipo de retorno `Tb`, em seguida, um *inferência de tipos de limite inferior* ([inferências de limite inferior](expressions.md#lower-bound-inferences)) é feita *do* `U` *para* `Tb`.
*  Caso contrário, se `E` é um grupo de método e `T` é um tipo de delegado ou tipo de árvore de expressão com tipos de parâmetro `T1...Tk` e o tipo de retorno `Tb`e a resolução de sobrecarga de `E` com os tipos de `T1...Tk` produz um um único método com o tipo de retorno `U`, em seguida, um *inferência de tipos de limite inferior* é feita *da* `U` *para* `Tb`.
*  Caso contrário, se `E` é uma expressão com o tipo `U`, em seguida, um *inferência de tipos de limite inferior* é feita *do* `U` *para* `T`.
*  Caso contrário, nenhum inferências são feitas.

#### <a name="explicit-parameter-type-inferences"></a>Inferências de tipo de parâmetro explícito

Uma *inferência de tipo de parâmetro explícito* é feita *partir* uma expressão `E` *para* um tipo `T` da seguinte maneira:

*  Se `E` é uma função anônima digitada explicitamente com tipos de parâmetro `U1...Uk` e `T` é um tipo de delegado ou tipo de árvore de expressão com tipos de parâmetro `V1...Vk` , em seguida, para cada `Ui` um *exata inferência* ([exato inferências](expressions.md#exact-inferences)) é feita *da* `Ui` *para* correspondente `Vi`.

#### <a name="exact-inferences"></a>Inferências exatas

Uma *exato inferência* *de* um tipo `U` *para* um tipo `V` é feita da seguinte maneira:

*  Se `V` é um dos *unfixed* `Xi` , em seguida, `U` é adicionado ao conjunto de limites exatos para `Xi`.

*  Caso contrário, define `V1...Vk` e `U1...Uk` são determinadas pela verificação se qualquer um dos casos a seguir se aplicam:

   *  `V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` da mesma classificação
   *  `V` é o tipo `V1?` e `U` é do tipo `U1?`
   *  `V` é um tipo construído `C<V1...Vk>`e `U` é um tipo construído `C<U1...Uk>`

   Se qualquer um desses casos, em seguida, aplicar um *exato inferência* é feita *de* cada `Ui` *para* correspondente `Vi`.

*  Caso contrário, nenhum inferências são feitas.

#### <a name="lower-bound-inferences"></a>Limite inferior inferências

Um *inferência de tipos de limite inferior* *partir* um tipo `U` *para* um tipo `V` é feita da seguinte maneira:

*  Se `V` é um dos *unfixed* `Xi` , em seguida, `U` é adicionado ao conjunto de limites inferiores do `Xi`.
*  Caso contrário, se `V` é o tipo `V1?`e `U` é o tipo `U1?` uma inferência de limite inferior é feita da `U1` para `V1`.
*  Caso contrário, define `U1...Uk` e `V1...Vk` são determinadas pela verificação se qualquer um dos casos a seguir se aplicam:
   *  `V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` (ou um parâmetro de tipo cujo tipo base eficaz é `U1[...]`) da mesma classificação
   *  `V` é um dos `IEnumerable<V1>`, `ICollection<V1>` ou `IList<V1>` e `U` é um tipo de matriz unidimensional `U1[]`(ou um parâmetro de tipo cujo tipo base eficaz é `U1[]`)
   *  `V` é um tipo de classe, struct, interface ou delegado construído `C<V1...Vk>` e há um tipo exclusivo `C<U1...Uk>` , de modo que `U` (ou, se `U` é um parâmetro de tipo, sua classe base efetivo ou qualquer membro do seu conjunto efetivo de interface) é idêntico ao, herda (direta ou indiretamente) ou implementa (direta ou indiretamente) `C<U1...Uk>`.

      (A restrição "exclusividade" significa que, na interface do case `C<T> {} class U: C<X>, C<Y> {}`, nenhuma inferência é feita quando a inferência de `U` à `C<T>` porque `U1` poderia ser `X` ou `Y`.)

   Se qualquer um desses casos aplicar uma inferência é feita *partir* cada `Ui` *para* correspondente `Vi` da seguinte maneira:

   *  Se `Ui` não é conhecido como um tipo de referência, uma *exato inferência* é feita
   *  Caso contrário, se `U` é um tipo de matriz, um *inferência de tipos de limite inferior* é feita
   *  Caso contrário, se `V` está `C<V1...Vk>` e em seguida, depende de inferência de tipos no parâmetro de tipo i-th `C`:
      *  Se ele é covariante, uma *inferência de tipos de limite inferior* é feita.
      *  Se ele é contravariante, então um *inferência de tipos de limite superior* é feita.
      *  Se ele for invariável, uma *exato inferência* é feita.
*  Caso contrário, nenhum inferências são feitas.

#### <a name="upper-bound-inferences"></a>Limite superior inferências

Uma *inferência de tipos de limite superior* *de* um tipo `U` *para* um tipo `V` é feita da seguinte maneira:

*  Se `V` é um dos *unfixed* `Xi` , em seguida, `U` é adicionado ao conjunto de limites superiores do `Xi`.
*  Caso contrário, define `V1...Vk` e `U1...Uk` são determinadas pela verificação se qualquer um dos casos a seguir se aplicam:
   *  `U` é um tipo de matriz `U1[...]` e `V` é um tipo de matriz `V1[...]` da mesma classificação
   *  `U` é um dos `IEnumerable<Ue>`, `ICollection<Ue>` ou `IList<Ue>` e `V` é um tipo de matriz unidimensional `Ve[]`
   *  `U` é o tipo `U1?` e `V` é do tipo `V1?`
   *  `U` é construído de classe, struct, tipo de interface ou delegado `C<U1...Uk>` e `V` é uma classe, struct, tipo de delegado ou interface que é idêntico a herda (direta ou indiretamente) ou implementa (direta ou indiretamente) um tipo exclusivo `C<V1...Vk>`

      (A restrição "exclusividade" significa que, se tivermos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, nenhuma inferência é feita quando a inferência de `C<U1>` para `V<Q>`. Inferências não são feitas de `U1` para um `X<Q>` ou `Y<Q>`.)

   Se qualquer um desses casos aplicar uma inferência é feita *partir* cada `Ui` *para* correspondente `Vi` da seguinte maneira:
   *  Se `Ui` não é conhecido como um tipo de referência, uma *exato inferência* é feita
   *  Caso contrário, se `V` é um tipo de matriz, uma *inferência de tipos de limite superior* é feita
   *  Caso contrário, se `U` está `C<U1...Uk>` e em seguida, depende de inferência de tipos no parâmetro de tipo i-th `C`:
      *  Se ele é covariante, uma *inferência de tipos de limite superior* é feita.
      *  Se ele é contravariante, em seguida, um *inferência de tipos de limite inferior* é feita.
      *  Se ele for invariável, uma *exato inferência* é feita.
*  Caso contrário, nenhum inferências são feitas.   

#### <a name="fixing"></a>Corrigir

Uma *unfixed* variável de tipo `Xi` com um conjunto de limites é *fixa* da seguinte maneira:

*  O conjunto de *tipos de candidatos* `Uj` começa como o conjunto de todos os tipos no conjunto de limites para `Xi`.
*  Em seguida, examinaremos cada limite para `Xi` por sua vez: cada limite exato `U` de `Xi` todos os tipos `Uj` que não são idêntico ao `U` são removidas do conjunto de candidatas. Cada limite inferior `U` dos `Xi` todos os tipos `Uj` para o qual existe é *não* uma conversão implícita de `U` são removidas do conjunto de candidatas. Para cada limite superior `U` de `Xi` todos os tipos `Uj` daí que está *não* uma conversão implícita para `U` são removidas do conjunto de candidatos.
*  Se entre os demais tipos de candidato `Uj` há um tipo exclusivo `V` de que há uma conversão implícita para todos os outros candidatos tipos, em seguida, `Xi` está fixado em `V`.
*  Caso contrário, a inferência de tipo falhará.

#### <a name="inferred-return-type"></a>Tipo de retorno deduzido

O inferido do tipo de retorno de uma função anônima `F` é usado durante a resolução de sobrecarga e de inferência de tipo. O tipo de retorno deduzido só pode ser determinado para uma função anônima onde o parâmetro todos os tipos são conhecidos, ou porque eles são atribuídos explicitamente, fornecido por meio de uma conversão de função anônima ou inferido durante a inferência de tipo em um delimitador genérica invocação de método.

O ***inferido do tipo de resultado*** é determinado da seguinte maneira:

*  Se o corpo da `F` é um *expressão* que tem um tipo e, em seguida, o tipo inferido do resultado de `F` é o tipo dessa expressão.
*  Se o corpo da `F` é um *bloco* e o conjunto de expressões no bloco de `return` instruções tem um tipo comum melhor `T` ([localizando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), em seguida, o tipo de resultado inferido de `F` é `T`.
*  Caso contrário, um tipo de resultado não pode ser inferido para `F`.

O ***inferido do tipo de retorno*** é determinado da seguinte maneira:

*  Se `F` é assíncrono e o corpo da `F` é uma expressão classificada como nada ([classificações de expressão](expressions.md#expression-classifications)), ou um bloco de instruções onde não há instruções return têm expressões, é do tipo de retorno inferido `System.Threading.Tasks.Task`
*  Se `F` é assíncrono e tem um tipo de resultado inferido `T`, é do tipo de retorno inferido `System.Threading.Tasks.Task<T>`.
*  Se `F` é não assíncronas e tem um tipo de resultado inferido `T`, é do tipo de retorno inferido `T`.
*  Caso contrário, um tipo de retorno não pode ser inferido para `F`.

Como um exemplo de inferência de tipo que envolvem funções anônimas, considere a `Select` método de extensão declarados no `System.Linq.Enumerable` classe:
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

Supondo que o `System.Linq` namespace foi importado com um `using` cláusula e dada a classe `Customer` com um `Name` propriedade do tipo `string`, o `Select` método pode ser usado para selecionar os nomes de uma lista de clientes:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

A invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)) de `Select` é processada por reescrever a chamada para uma invocação de método estático:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Como argumentos de tipo não foram especificados explicitamente, a inferência de tipo é usada para inferir os argumentos de tipo. Primeiro, o `customers` argumento está relacionado à `source` parâmetro, inferir `T` ser `Customer`. Em seguida, usando a função anônima digite descrito acima, do processo de inferência `c` recebe o tipo `Customer`e a expressão `c.Name` está relacionado ao tipo de retorno dos `selector` parâmetro, inferindo `S` ser `string`. Portanto, a invocação é equivalente a
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
e o resultado é do tipo `IEnumerable<string>`.

O exemplo a seguir demonstra o tipo de função anônima como inferência de tipos permite que as informações de tipo "fluir" entre os argumentos em uma invocação de método genérico. Com o método:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Inferência de tipo para a invocação:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
procede da seguinte maneira: primeiro, o argumento `"1:15:30"` está relacionado à `value` parâmetro, inferir `X` ser `string`. Então, o parâmetro da primeira função anônima `s`, recebe o tipo inferido `string`e a expressão `TimeSpan.Parse(s)` está relacionado ao tipo de retorno de `f1`, inferindo `Y` ser `System.TimeSpan`. Por fim, o parâmetro da segunda função anônima, `t`, recebe o tipo inferido `System.TimeSpan`e a expressão `t.TotalSeconds` está relacionado ao tipo de retorno de `f2`, inferindo `Z` ser `double`. Portanto, o resultado da invocação é do tipo `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Inferência de tipo para conversão de grupos de método

Semelhante às chamadas de métodos genéricos, inferência de tipo também deve ser aplicada quando um grupo de métodos `M` que contém um método genérico é convertido em um tipo de delegado fornecidos `D` ([conversões de grupo de método](conversions.md#method-group-conversions)). Um método
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
e o grupo de métodos `M` que está sendo atribuído ao tipo de delegado `D` a tarefa de inferência de tipo é encontrar os argumentos de tipo `S1...Sn` , de modo que a expressão:
```csharp
M<S1...Sn>
```
se torna compatível ([declarações de delegado](delegates.md#delegate-declarations)) com `D`.

Diferente do algoritmo de inferência de tipo para chamadas de método genérico, nesse caso, há apenas o argumento *tipos*, nenhum argumento *expressões*. Em particular, não há nenhuma função anônima e, portanto, sem a necessidade de várias fases de inferência de tipos.

Em vez disso, todos os `Xi` são considerados *unfixed*e uma *inferência de tipos de limite inferior* é feita *do* cada tipo de argumento `Uj` de `D` *à* o tipo de parâmetro correspondente `Tj` de `M`. Se qualquer uma da `Xi` não há limites foram encontrados, falha de inferência de tipo. Caso contrário, todos os `Xi` estão *fixo* correspondente `Si`, que são o resultado de inferência de tipo.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Localizando o melhor tipo comum de um conjunto de expressões

Em alguns casos, um tipo comum deve ser deduzida para um conjunto de expressões. Em particular, os tipos de elementos de matrizes de tipo implícito e os tipos de retorno de funções anônimas com *bloco* corpos encontram-se dessa maneira.

Intuitivamente, dado um conjunto de expressões `E1...Em` essa inferência de tipos deve ser equivalente a chamar um método
```csharp
Tr M<X>(X x1 ... X xm)
```
com o `Ei` como argumentos.

Mais precisamente, a inferência começa com um *unfixed* variável de tipo `X`. *Inferências de tipo de saída* , em seguida, são feitas *partir* cada `Ei` *para* `X`. Por fim, `X` está *fixo* e, se for bem-sucedido, resultante de tipo `S` é o melhor tipo comum resultante para as expressões. Se nenhum `S` existir, as expressões não têm nenhum tipo comum melhor.

### <a name="overload-resolution"></a>Resolução de sobrecarga

Resolução de sobrecarga é um mecanismo de tempo de associação para selecionar o membro da função melhor invocar dada uma lista de argumentos e um conjunto de membros da função de candidato. Resolução de sobrecarga seleciona o membro da função para invocar nos seguintes contextos distintos em c#:

*  Invocação de um método chamado em um *invocation_expression* ([invocações de método](expressions.md#method-invocations)).
*  Invocação de um construtor de instância nomeada em um *object_creation_expression* ([expressões de criação do objeto](expressions.md#object-creation-expressions)).
*  Invocação de um acessador de indexador por meio de um *element_access* ([acesso de elemento](expressions.md#element-access)).
*  Invocação de um operador predefinido ou definidos pelo usuário referenciada em uma expressão ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)).

Cada um desses contextos define o conjunto de membros da função release candidate e a lista de argumentos em sua própria maneira exclusiva, conforme descrito em detalhes nas seções listados acima. Por exemplo, o conjunto de candidatos para uma invocação de método não inclui métodos marcados `override` ([pesquisa de membro](expressions.md#member-lookup)), e métodos em uma classe base não são candidatos se qualquer método em uma classe derivada é aplicável ([ As invocações de método](expressions.md#method-invocations)).

Depois que foram identificados os membros da função release candidate e a lista de argumentos, a seleção do membro da função recomendada é a mesma em todos os casos:

*  Dado o conjunto de membros da função candidato aplicável, a melhor função membro nesse conjunto é localizado. Se o conjunto contém apenas um membro da função, esse membro da função é o membro da função melhor. Caso contrário, o membro da função melhor será o membro de uma função que é melhor do que todos os outros membros da função em relação à lista de argumento fornecido, desde que cada membro da função é comparado com todos os outros membros da função usando as regras em [ Membro de função melhor](expressions.md#better-function-member). Se não houver exatamente um membro da função que é melhor do que todos os outros membros da função, em seguida, a invocação de membro da função é ambígua e ocorre um erro em tempo de vinculação.

As seções a seguir definem o significado exato dos termos ***membro da função aplicáveis*** e ***melhor membro da função***.

#### <a name="applicable-function-member"></a>Membro de função aplicáveis

Um membro da função deve ser um ***membro da função aplicáveis*** em relação a uma lista de argumentos `A` quando todos os itens a seguir forem verdadeiras:

*  Cada argumento na `A` corresponde a um parâmetro na declaração de membro da função, conforme descrito em [correspondente parâmetros](expressions.md#corresponding-parameters), e qualquer parâmetro não correspondente ao nenhum argumento é um parâmetro opcional.
*  Para cada argumento na `A`, o modo do argumento de passagem de parâmetro (ou seja, valor, `ref`, ou `out`) é idêntico para o modo de passagem de parâmetro do parâmetro correspondente, e
   *  para um parâmetro de valor ou uma matriz de parâmetros, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe do argumento para o tipo do parâmetro correspondente, ou
   *  para um `ref` ou `out` parâmetro, o tipo do argumento é idêntico ao tipo do parâmetro correspondente. Afinal de contas, uma `ref` ou `out` parâmetro é um alias para o argumento passado.

Para um membro da função que inclui uma matriz de parâmetros, se o membro da função for aplicável, as regras acima, ele deve ser aplicável em sua ***forma normal***. Se um membro da função que inclui uma matriz de parâmetro não for aplicável em sua forma normal, o membro da função em vez disso, pode ser aplicável em sua ***expandido formulário***:

*  A forma expandida é construída, substituindo a matriz de parâmetros na declaração de membro da função com zero ou mais parâmetros de valor do tipo de elemento do parâmetro de matriz, que o número de argumentos na lista de argumentos `A` corresponde ao total número de parâmetros. Se `A` tem menos argumentos que o número de parâmetros fixos na declaração de membro da função, a forma expandida do membro da função não pode ser construída e, portanto, não é aplicável.
*  Caso contrário, a forma expandida é aplicável se cada argumento na `A` o modo de passagem de parâmetro do argumento é idêntico para o modo de passagem de parâmetro do parâmetro correspondente, e
   *  para um parâmetro de valor fixo ou criados pela expansão, uma conversão implícita de um parâmetro de valor ([conversões implícitas](conversions.md#implicit-conversions)) existe do tipo do argumento para o tipo do parâmetro correspondente, ou
   *  para um `ref` ou `out` parâmetro, o tipo do argumento é idêntico ao tipo do parâmetro correspondente.

#### <a name="better-function-member"></a>Membro de função melhor

A fim de determinar o membro da função melhor, uma lista de argumentos simplificada um é construída que contém apenas as expressões do argumento em si na ordem em que eles aparecem na lista de argumentos original.

Listas de parâmetros para cada função candidato membros são construídos da seguinte maneira:

*  A forma expandida será usada se o membro da função foi aplicável apenas a forma expandida.
*  Parâmetros opcionais sem argumentos correspondentes são removidos da lista de parâmetros
*  Os parâmetros são reordenados para que eles ocorram na mesma posição como o argumento correspondente na lista de argumentos.

Dada uma lista de argumentos `A` com um conjunto de expressões de argumento `{E1, E2, ..., En}` e dois membros da função aplicável `Mp` e `Mq` com tipos de parâmetro `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}`, `Mp` é definido como um ***melhor membro da função*** que `Mq` se

*  para cada argumento, a conversão implícita da `Ex` à `Qx` não é melhor do que a conversão implícita de `Ex` para `Px`, e
*  para pelo menos um argumento, a conversão de `Ex` à `Px` é melhor do que a conversão de `Ex` para `Qx`.

Ao fazer isso, se `Mp` ou `Mq` é aplicável em sua forma expandida, em seguida, `Px` ou `Qx` refere-se a um parâmetro no formato expandido da lista de parâmetros.

Caso o parâmetro de tipo sequências `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}` são equivalentes (ou seja, cada `Pi` tem uma conversão de identidade para os respectivos `Qi`), as seguintes regras de desempate são aplicadas em ordem, para determinar o melhor membro de função.

*  Se `Mp` é um método não genérico e `Mq` é um método genérico, em seguida, `Mp` é melhor do que `Mq`.
*  Caso contrário, se `Mp` é aplicável em sua forma normal e `Mq` tem um `params` de matriz e é aplicável apenas em sua forma expandida, em seguida, `Mp` é melhor do que `Mq`.
*  Caso contrário, se `Mp` mais declarou parâmetros que `Mq`, em seguida, `Mp` é melhor do que `Mq`. Isso pode ocorrer se os dois métodos têm `params` matrizes e são aplicável apenas em seus formulários expandidos.
*  Caso contrário se todos os parâmetros de `Mp` tem um argumento correspondente, enquanto os argumentos padrão precisam ser substituídos pelo menos um parâmetro opcional nas `Mq` , em seguida, `Mp` é melhor do que `Mq`.
*  Caso contrário, se `Mp` tem tipos de parâmetro mais específicos que `Mq`, em seguida, `Mp` é melhor do que `Mq`. Deixe `{R1, R2, ..., Rn}` e `{S1, S2, ..., Sn}` representam os tipos de parâmetro não instanciado e não expandidas de `Mp` e `Mq`. `Mp`do tipos de parâmetro são mais específicos que `Mq`do se, para cada parâmetro, `Rx` não é menos específicas `Sx`e, pelo menos um parâmetro, `Rx` é mais específico que `Sx`:
   *  Um parâmetro de tipo é menos específico um parâmetro sem tipo.
   *  Recursivamente, um tipo construído é mais específico do que outro tipo construído (com o mesmo número de argumentos de tipo) se pelo menos um argumento de tipo é mais específico e nenhum argumento de tipo é menos específico do argumento de tipo correspondente no outro.
   *  Um tipo de matriz é mais específico do que outro tipo de matriz (com o mesmo número de dimensões), se o tipo de elemento da primeira é mais específico do que o tipo de elemento da segunda.
*  Caso contrário, se um membro é um operador sem comparação de precisão e a outra é um operador elevado, a sem comparação de precisão é melhor.
*  Caso contrário, nenhum membro da função é melhor.

#### <a name="better-conversion-from-expression"></a>Melhor conversão de expressão

Dada uma conversão implícita `C1` que converte de uma expressão `E` a um tipo `T1`e uma conversão implícita `C2` que converte de uma expressão `E` a um tipo `T2`, `C1` é um ***melhor conversão*** que `C2` se `E` corresponder exatamente a `T2` e mantém pelo menos um dos seguintes:

* `E` corresponde exatamente `T1` ([correspondem exatamente a expressão](expressions.md#exactly-matching-expression))
* `T1` é um destino de conversão melhor que `T2` ([melhor destino da conversão](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Expressão exatamente correspondente

Considerando uma expressão `E` e um tipo `T`, `E` corresponda exatamente ao `T` se tiver mantido por um dos seguintes:

*  `E` tem um tipo `S`, e existe uma conversão de identidade de `S` para `T`
*  `E` é uma função anônima, `T` é um tipo de delegado `D` ou um tipo de árvore de expressão `Expression<D>` e mantém por um dos seguintes:
   *  Um tipo de retorno inferido `X` existe para o `E` no contexto da lista de parâmetros de `D` ([tipo de retorno inferido](expressions.md#inferred-return-type)), e existe uma conversão de identidade de `X` para o tipo de retorno `D`
   *  Qualquer um dos `E` é não assíncronas e `D` tem um tipo de retorno `Y` ou `E` é assíncrono e `D` tem um tipo de retorno `Task<Y>`, e uma das opções a seguir contém:
      * O corpo da `E` é uma expressão que exatamente correspondências `Y`
      * O corpo da `E` é um bloco de instrução onde cada instrução return retorna uma expressão que exatamente correspondências `Y`

#### <a name="better-conversion-target"></a>Melhor destino da conversão

Dois tipos diferentes de dado `T1` e `T2`, `T1` é um destino de conversão melhor que `T2` se nenhuma conversão implícita de `T2` para `T1` existir, e pelo menos um dos seguintes contém:

*  Uma conversão implícita da `T1` para `T2` existe
*  `T1` é um tipo de delegado `D1` ou um tipo de árvore de expressão `Expression<D1>`, `T2` é um tipo de delegado `D2` ou um tipo de árvore de expressão `Expression<D2>`, `D1` tem um tipo de retorno `S1` e uma da contém a seguinte:
   * `D2` void está retornando
   * `D2` tem um tipo de retorno `S2`, e `S1` é um destino de conversão melhor que `S2`
*  `T1` está `Task<S1>`, `T2` é `Task<S2>`, e `S1` é um destino de conversão melhor que `S2`
*  `T1` está `S1` ou `S1?` onde `S1` é um tipo integral com sinal, e `T2` está `S2` ou `S2?` onde `S2` é um tipo integral sem sinal. Especificamente:
   * `S1` está `sbyte` e `S2` é `byte`, `ushort`, `uint`, ou `ulong`
   * `S1` está `short` e `S2` é `ushort`, `uint`, ou `ulong`
   * `S1` está `int` e `S2` é `uint`, ou `ulong`
   * `S1` está `long` e `S2` é `ulong`

#### <a name="overloading-in-generic-classes"></a>Sobrecarga em classes genéricas

Embora assinaturas conforme declarado devem ser exclusivas, é possível que a substituição de argumentos de tipo resulta em assinaturas idênticas. Nesses casos, as regras de resolução de sobrecarga acima de desempate escolherá o membro mais específico.

Os exemplos a seguir mostram as sobrecargas que são válidas e inválidas de acordo com essa regra:

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Verificação de resolução de sobrecarga de dinâmica de tempo de compilação

Para operações de ligação mais dinamicamente o conjunto de possíveis candidatos para a resolução é desconhecido no tempo de compilação. Em alguns casos, no entanto o conjunto de candidato é conhecido no tempo de compilação:

*  Chamadas de método estático com argumentos dinâmicos
*  Chamadas de método de instância em que o receptor não for uma expressão dinâmica
*  Chamadas de indexador em que o receptor não for uma expressão dinâmica
*  Chamadas de construtor com argumentos dinâmicos

Nesses casos, uma verificação limitada de tempo de compilação é executada para cada candidato ver se qualquer um deles, possivelmente, pode aplicar em tempo de execução. Essa verificação consiste as seguintes etapas:

*  Inferência de tipo parcial: qualquer tipo de argumento que não depende diretamente ou indiretamente um argumento do tipo `dynamic` é inferido usando as regras da [inferência de tipo](expressions.md#type-inference). Os argumentos de tipo restantes são desconhecidos.
*  Verificação de aplicabilidade parcial: aplicabilidade é verificada de acordo com a [membro da função aplicável](expressions.md#applicable-function-member), mas ignorando os parâmetros cujos tipos são desconhecidos.
*  Se nenhum candidato passar nesse teste, ocorrerá um erro de tempo de compilação.

### <a name="function-member-invocation"></a>Invocação de membro de função

Esta seção descreve o processo que ocorre em tempo de execução para invocar um membro de função específica. Supõe-se que um processo de tempo de associação já determinou que o membro específico para invocar, possivelmente, aplicando resolução de sobrecarga para um conjunto de membros da função de candidato.

Para fins de descrever o processo de invocação, os membros da função são divididos em duas categorias:

*  Membros de função estática. Esses são métodos estáticos, construtores de instância, os acessadores de propriedade estática e operadores definidos pelo usuário. Membros da função estáticos são sempre não virtual.
*  Membros da função de instância. Esses são métodos de instância, os acessadores de propriedade de instância e acessadores de indexador. Membros da função de instância são não virtual ou virtual e sempre são invocados em uma instância específica. A instância é calculada por uma expressão de instância, e se torna acessível dentro do membro da função como `this` ([esse acesso](expressions.md#this-access)).

O processamento de tempo de execução de uma invocação de membro de função consiste em etapas a seguir, onde `M` é o membro da função e, se `M` é um membro de instância, `E` é a expressão da instância:

*  Se `M` é um membro da função estática:
   * A lista de argumentos é avaliada, conforme descrito em [listas de argumentos](expressions.md#argument-lists).
   * `M` é invocado.

*  Se `M` é um membro da função de instância declarado em um *value_type*:
   * `E` é avaliado. Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.
   * Se `E` não é classificado como uma variável e, em seguida, uma variável local temporária do `E`do tipo é criado e o valor de `E` é atribuído a essa variável. `E` em seguida, ser reclassificado como uma referência para a variável local temporário. Variável temporária é acessível como `this` dentro de `M`, mas não em qualquer outra forma. Assim, somente quando `E` é uma variável true é possível para o chamador observar as alterações que `M` faz com `this`.
   * A lista de argumentos é avaliada, conforme descrito em [listas de argumentos](expressions.md#argument-lists).
   * `M` é invocado. A variável referenciada por `E` torna-se a variável referenciada por `this`.

*  Se `M` é um membro da função de instância declarado em um *reference_type*:
   * `E` é avaliado. Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.
   * A lista de argumentos é avaliada, conforme descrito em [listas de argumentos](expressions.md#argument-lists).
   * Se o tipo de `E` é um *value_type*, uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) é executada para converter `E` para o tipo `object`, e `E` é considerado para ser do tipo `object` nas etapas a seguir. Nesse caso, `M` só pode ser um membro de `System.Object`.
   * O valor de `E` é verificado para ser válido. Se o valor de `E` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.
   * A implementação de membro da função para invocar é determinada:
     * Se o tempo de associação de tipo de `E` é uma interface, o membro da função para invocar é a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`. Este membro da função é determinado pela aplicação das regras de mapeamento de interface ([mapeamento de Interface](interfaces.md#interface-mapping)) para determinar a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.
     * Caso contrário, se `M` é um membro da função virtual, o membro da função para invocar é a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`. Este membro da função é determinado pela aplicação das regras para determinar a implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) de `M` em relação ao tipo de tempo de execução da instância referenciada por `E`.
     * Caso contrário, `M` é um membro da função não virtual e é o membro da função para invocar `M` em si.
   * A implementação de membro da função determinada na etapa anterior é invocada. O objeto referenciado por `E` torna-se o objeto referenciado por `this`.

#### <a name="invocations-on-boxed-instances"></a>Invocações de instâncias do box

Um membro da função implementada em uma *value_type* pode ser invocado por meio de uma instância demarcada desse *value_type* nas seguintes situações:

*  Quando o membro da função é um `override` de um método herdado do tipo `object` e é invocado por meio de uma expressão de instância do tipo `object`.
*  Quando o membro da função é uma implementação de um membro da função de interface e é invocado por meio de uma expressão de instância de um *interface_type*.
*  Quando o membro da função é invocado por meio de um delegado.

Nessas situações, a instância do box é considerada como tendo uma variável do *value_type*, e essa variável se torna a variável referenciada por `this` dentro a invocação de membro da função. Em particular, isso significa que, quando um membro da função é invocado em uma instância do box, é possível que o membro da função modificar o valor contido na instância do box.

## <a name="primary-expressions"></a>Expressões primárias

Expressões primárias incluem as formas mais simples de expressões.

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

Expressões primárias são divididas entre *array_creation_expression*s e *primary_no_array_creation_expression*s. Tratar a expressão de criação de matriz dessa forma, em vez de listá-la junto com as outras formas de expressão simples, permite que a gramática para não permitir código potencialmente confuso, como
```csharp
object o = new int[3][1];
```
Caso contrário, que poderiam ser interpretados como
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literais

Um *primary_expression* que consiste em um *literal* ([literais](lexical-structure.md#literals)) é classificado como um valor.


### <a name="interpolated-strings"></a>Cadeias de caracteres interpoladas

Uma *interpolated_string_expression* consiste em um `$` sinal seguido por uma regular ou textual cadeia de caracteres literal, no qual os buracos, delimitadas por `{` e `}`, inclua expressões e formatação especificações. Uma expressão de cadeia de caracteres interpolada é o resultado de uma *interpolated_string_literal* que foi dividido em tokens individuais, conforme descrito em [interpoladas literais de cadeia de caracteres](lexical-structure.md#interpolated-string-literals).

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

O *constant_expression* uma interpolação deve ter uma conversão implícita em `int`.

Uma *interpolated_string_expression* é classificado como um valor. Se ele imediatamente é convertido em `System.IFormattable` ou `System.FormattableString` com uma conversão implícita de cadeia de caracteres interpolada ([implícitas interpoladas conversões de cadeia de caracteres](conversions.md#implicit-interpolated-string-conversions)), a expressão de cadeia de caracteres interpolada tem esse tipo. Caso contrário, ele tem o tipo `string`.

Se for o tipo de uma cadeia de caracteres interpolada `System.IFormattable` ou `System.FormattableString`, o significado é uma chamada para `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Se o tipo for `string`, o significado da expressão é uma chamada para `string.Format`. Em ambos os casos, a lista de argumentos da chamada consiste em uma cadeia de caracteres de formato literal com espaços reservados para cada interpolação e um argumento para cada expressão correspondente para os espaços reservados.

A cadeia de caracteres de formato literal é construída da seguinte maneira, onde `N` é o número de interpolações em de *interpolated_string_expression*:

*  Se um *interpolated_regular_string_whole* ou um *interpolated_verbatim_string_whole* segue o `$` entrar, em seguida, o literal de cadeia de caracteres de formato é esse token.
*  Caso contrário, a cadeia de caracteres de formato literal consiste em: 
   *  Primeiro o *interpolated_regular_string_start* ou *interpolated_verbatim_string_start*
   *  Em seguida, para cada número `I` partir `0` para `N-1`: 
      * A representação decimal de `I`
      * Então, se correspondente *interpolação* tem um *constant_expression*, um `,` (vírgula) seguida pela representação decimal do valor da *constant_expression*
      * Em seguida, a *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* ou *interpolated_ verbatim_string_end* imediatamente após a interpolação correspondente.

Os argumentos subsequentes são simplesmente o *expressões* da *interpolações* (se houver), na ordem.

TODO: exemplos.


### <a name="simple-names"></a>Nomes simples

Um *simple_name* consiste em um identificador, opcionalmente seguido por uma lista de argumentos de tipo:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

Um *simple_name* é do formulário `I` ou do formulário `I<A1,...,Ak>`, onde `I` é um identificador único e `<A1,...,Ak>` é um recurso opcional *type_argument_list*. Quando nenhum *type_argument_list* é especificado, considere `K` seja zero. O *simple_name* é avaliado e classificados da seguinte maneira:

*  Se `K` é zero e o *simple_name* aparece dentro de uma *bloco* e, se a *bloco*do (ou um delimitador *bloco*do) local espaço de declaração de variável ([declarações](basic-concepts.md#declarations)) contém uma variável local, parâmetro ou constante com nome `I`, em seguida, o *simple_name* refere-se para a variável local, parâmetro ou constante e é classificado como uma variável ou um valor.
*  Se `K` é zero e o *simple_name* é exibida dentro do corpo de uma declaração de método genérico e se essa declaração inclui um parâmetro de tipo com nome `I`, em seguida, a *simple_name*se refere a esse parâmetro de tipo.
*  Caso contrário, para cada tipo de instância `T` ([o tipo de instância](classes.md#the-instance-type)), começando com o tipo de instância de declaração de tipo de delimitador imediatamente e continuando com o tipo de instância de cada classe delimitadora ou struct declaração (se houver):
   *  Se `K` é zero e a declaração de `T` inclui um parâmetro de tipo com nome `I`, em seguida, o *simple_name* refere-se a esse parâmetro de tipo.
   *  Caso contrário, se uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` na `T` com `K` argumentos de tipo produz uma correspondência:
      * Se `T` é o tipo de instância do tipo de classe ou struct imediatamente delimitador e a pesquisa identifica um ou mais métodos, o resultado é um grupo de método com uma expressão de instância associada do `this`. Se uma lista de argumentos de tipo foi especificada, ele será usado na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).
      * Caso contrário, se `T` é o tipo de instância do tipo imediatamente delimitador de classe ou struct, se a pesquisa identifica um membro de instância, e se a referência ocorre dentro do corpo de um construtor de instância, um método de instância ou um acessador de instância, o resultado é o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `this.I`. Isso só pode acontecer quando `K` é zero.
      * Caso contrário, o resultado é o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `T.I` ou `T.I<A1,...,Ak>`. Nesse caso, ele é um erro de tempo de associação para o *simple_name* para se referir a um membro de instância.

*  Caso contrário, para cada namespace `N`, começando com o namespace no qual o *simple_name* ocorre, continuando com cada delimitador namespace (se houver) e terminando com o namespace global, são as seguintes etapas avaliada até que uma entidade está localizada:
   *  Se `K` é zero e `I` é o nome de um namespace em `N`, então:
      * Se o local onde o *simple_name* ocorre estiver envolvido por uma declaração de namespace `N` e a declaração de namespace contém uma *extern_alias_directive* ou  *using_alias_directive* que associa o nome `I` com um namespace ou tipo, em seguida, a *simple_name* é ambíguo e ocorrer um erro de tempo de compilação.
      * Caso contrário, o *simple_name* refere-se ao namespace chamado `I` em `N`.
   *  Caso contrário, se `N` contém um tipo acessível com o nome `I` e `K` , em seguida, os parâmetros de tipo:
      * Se `K` é zero e o local onde o *simple_name* ocorre é incluído por uma declaração de namespace `N` e a declaração de namespace contém um *extern_alias_directive*ou *using_alias_directive* que associa o nome `I` com um namespace ou tipo, em seguida, a *simple_name* é ambíguo e ocorrer um erro de tempo de compilação.
      * Caso contrário, o *namespace_or_type_name* refere-se para o tipo construído com os argumentos de tipo em questão.
   *  Caso contrário, se o local em que o *simple_name* ocorre é incluído por uma declaração de namespace para `N`:
      * Se `K` é zero e a declaração de namespace contém uma *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` com um namespace importado ou tipo, em seguida, a *simple_name* refere-se a esse namespace ou tipo.
      * Caso contrário, se as declarações de namespaces e o tipo importado pela *using_namespace_directive*s e *using_static_directive*s da declaração do namespace conter exatamente um tipo acessível ou tendo o nome de membro estático sem extensão `I` e `K` parâmetros de tipo, o *simple_name* refere-se a esse tipo ou membro construído com os argumentos de tipo em questão.
      * Caso contrário, se os tipos e namespaces importados pelo *using_namespace_directive*s da declaração do namespace contêm mais de um tipo acessível ou ter um nome de membro estático do método de extensão não `I` e `K` parâmetros de tipo, o *simple_name* é ambíguo e ocorre um erro.

   Observe que toda essa etapa é exatamente paralela para a etapa correspondente no processamento de uma *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)).

*  Caso contrário, o *simple_name* é indefinido e ocorrer um erro de tempo de compilação.


### <a name="parenthesized-expressions"></a>Expressões entre parênteses

Um *parenthesized_expression* consiste em um *expressão* entre parênteses.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Um *parenthesized_expression* é avaliada por avaliar o *expressão* dentro dos parênteses. Se o *expressão* dentro dos parênteses denota um namespace ou tipo, ocorre um erro de tempo de compilação. Caso contrário, o resultado do *parenthesized_expression* é o resultado da avaliação de independente *expressão*.

### <a name="member-access"></a>Acesso de membros

Um *member_access* consiste em um *primary_expression*, um *predefined_type*, ou um *qualified_alias_member*, seguido por um"`.`"token, seguido por um *identificador*, opcionalmente seguido por um *type_argument_list*.

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

O *qualified_alias_member* produção é definida no [qualificadores alias de Namespace](namespaces.md#namespace-alias-qualifiers).

Um *member_access* é do formulário `E.I` ou do formulário `E.I<A1, ..., Ak>`, onde `E` é uma expressão primária `I` é um identificador único e `<A1, ..., Ak>` é um recurso opcional  *type_argument_list*. Quando nenhum *type_argument_list* é especificado, considere `K` seja zero.

Um *member_access* com um *primary_expression* do tipo `dynamic` associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o compilador classifica o acesso de membro como um acesso de propriedade do tipo `dynamic`. As regras abaixo para determinar o significado do *member_access* , em seguida, são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação da *primary_expression*. Se essa classificação de tempo de execução leva a um grupo de métodos, o acesso de membro deve ser o *primary_expression* de uma *invocation_expression*.

O *member_access* é avaliado e classificados da seguinte maneira:

*  Se `K` é zero e `E` é um namespace e `E` contém um namespace aninhado com nome `I`, o resultado será desse namespace.
*  Caso contrário, se `E` é um namespace e `E` contém um tipo acessível com nome `I` e `K` parâmetros de tipo, em seguida, o resultado será aquele tipo construído com os argumentos de tipo em questão.
*  Se `E` é um *predefined_type* ou uma *primary_expression* classificado como um tipo, se `E` não é um parâmetro de tipo e se uma pesquisa de membro ([depesquisademembro](expressions.md#member-lookup)) dos `I` na `E` com `K` parâmetros de tipo produz uma correspondência, em seguida, `E.I` é avaliado e classificados da seguinte maneira:
   *  Se `I` identifica um tipo, em seguida, o resultado será aquele tipo construído com os argumentos de tipo em questão.
   *  Se `I` identifica um ou mais métodos, o resultado será um grupo de métodos sem expressão de instância associada. Se uma lista de argumentos de tipo foi especificada, ele será usado na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).
   *  Se `I` identifica um `static` propriedade e, em seguida, o resultado é um acesso de propriedade sem expressão de instância associada.
   *  Se `I` identifica um `static` campo:
      * Se o campo estiver `readonly` e a referência ocorre fora do construtor estático da classe ou struct em que o campo é declarado e, em seguida, o resultado é um valor, ou seja, o valor do campo estático `I` em `E`.
      * Caso contrário, o resultado é uma variável, ou seja, o campo estático `I` em `E`.
   *  Se `I` identifica um `static` eventos:
      * Se a referência ocorre dentro da classe ou struct em que o evento é declarado e o evento foi declarado sem *event_accessor_declarations* ([eventos](classes.md#events)), em seguida, `E.I` é processado exatamente como se `I` foram um campo estático.
      * Caso contrário, o resultado é um acesso de evento sem expressão de instância associada.
   *  Se `I` identifica uma constante, o resultado será um valor, ou seja, o valor dessa constante.
    * Se `I` identifica um membro de enumeração, o resultado será um valor, ou seja, o valor desse membro de enumeração.
    * Caso contrário, `E.I` é uma referência de membro inválido e ocorre um erro de tempo de compilação.
*  Se `E` é um acesso de propriedade, acesso de indexador, variável ou valor, o tipo do qual é `T`e uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) do `I` na `T` com `K` argumentos de tipo produz uma correspondência, em seguida, `E.I` é avaliado e classificados da seguinte maneira:
   *  Primeiro, se `E` é uma propriedade ou acesso do indexador, então o valor da propriedade ou indexador acesso é obtido ([valores de expressões](expressions.md#values-of-expressions)) e `E` é reclassificado como um valor.
   *  Se `I` identifica um ou mais métodos, o resultado será um grupo de métodos com uma expressão de instância associada do `E`. Se uma lista de argumentos de tipo foi especificada, ele será usado na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).
   *  Se `I` identifica uma propriedade de instância
      * Se `E` está `this`, `I` identifica uma propriedade implementada automaticamente ([implementada automaticamente propriedades](classes.md#automatically-implemented-properties)) sem um setter e a referência ocorre dentro de um construtor de instância para um tipo de classe ou struct `T`, em seguida, o resultado é uma variável, ou seja, o campo de suporte oculto para a propriedade automática fornecida pelo `I` na instância do `T` fornecido pela `this`.
      * Caso contrário, o resultado é um acesso de propriedade com uma expressão de instância associada do `E`.
   *  Se `T` é um *class_type* e `I` identifica um campo de instância disso *class_type*:
      * Se o valor de `E` está `null`, em seguida, um `System.NullReferenceException` é gerada.
      * Caso contrário, se o campo estiver `readonly` e a referência ocorre fora de um construtor de instância da classe na qual o campo é declarado e, em seguida, o resultado é um valor, ou seja, o valor do campo `I` no objeto referenciado pelo `E`.
      * Caso contrário, o resultado é uma variável, ou seja, o campo `I` no objeto referenciado pelo `E`.
   *  Se `T` é um *struct_type* e `I` identifica um campo de instância disso *struct_type*:
      * Se `E` é um valor, ou se o campo estiver `readonly` e a referência ocorre fora de um construtor de instância do struct em que o campo é declarado e, em seguida, o resultado é um valor, ou seja, o valor do campo `I` na instância do struct fornecida por `E`.
      * Caso contrário, o resultado é uma variável, ou seja, o campo `I` na instância do struct fornecida pelo `E`.
   *  Se `I` identifica um evento da instância:
      * Se a referência ocorre dentro da classe ou struct em que o evento é declarado e o evento foi declarado sem *event_accessor_declarations* ([eventos](classes.md#events)), e a referência não ocorre como o lado esquerdo de uma `+=` ou `-=` operador, em seguida, `E.I` é processado exatamente como se `I` foi um campo de instância.
      * Caso contrário, o resultado é um evento de acesso com uma expressão de instância associada do `E`.
*  Caso contrário, é feita uma tentativa para processar `E.I` como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)). Se isso falhar, `E.I` é uma referência de membro inválido e ocorre um erro em tempo de vinculação.

#### <a name="identical-simple-names-and-type-names"></a>Nomes idênticos de simples e nomes de tipo

Em um acesso de membro do formulário `E.I`, se `E` é um identificador único e se o significado dos `E` como um *simple_name* ([nomes simples](expressions.md#simple-names)) é uma constante, campo, propriedade, variável local ou parâmetro com o mesmo tipo que o significado dos `E` como um *type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)), em seguida, os dois significados possíveis de `E` são permitido. Os dois significados possíveis das `E.I` nunca são ambíguas, desde `I` necessariamente deve ser um membro do tipo `E` em ambos os casos. Em outras palavras, a regra simplesmente permite acesso a membros estáticos e tipos aninhados de `E` onde caso contrário, seria ter ocorrido um erro de tempo de compilação. Por exemplo:
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a>Ambiguidades de gramática

As produções para *simple_name* ([nomes simples](expressions.md#simple-names)) e *member_access* ([acesso de membro](expressions.md#member-access)) pode dar origem a ambiguidades no gramática de expressões. Por exemplo, a instrução:
```
F(G<A,B>(7));
```
pode ser interpretado como uma chamada para `F` com dois argumentos, `G < A` e `B > (7)`. Como alternativa, ele poderia ser interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.

Se uma sequência de tokens pode ser analisada (no contexto) como um *simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro](expressions.md#member-access)), ou *pointer_member_access* ([acesso de membro do ponteiro](unsafe-code.md#pointer-member-access)) terminando com um *type_argument_list* ([argumentos de tipo](types.md#type-arguments)), o imediatamente após o fechamento do token `>` token é examinado. Se ele for um dos
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
em seguida, a *type_argument_list* é mantido como parte do *simple_name*, *member_access* ou *pointer_member_access* e qualquer a análise da sequência de tokens de outra possíveis será descartada. Caso contrário, o *type_argument_list* não é considerado parte do *simple_name*, *member_access* ou *pointer_member_access*, mesmo se não houver nenhuma análise da sequência de tokens de outro possíveis. Observe que essas regras não são aplicadas durante a análise de um *type_argument_list* em um *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)). A instrução
```csharp
F(G<A,B>(7));
```
será, de acordo com a esta regra, interpretada como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular. As instruções
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
cada uma ser interpretada como uma chamada para `F` com dois argumentos. A instrução
```csharp
x = F < A > +y;
```
será interpretado como um operador menor que, maior que o operador e operador de adição unário, como se tivesse sido escrita a instrução `x = (F < A) > (+y)`, em vez de como uma *simple_name* com um *type_argument_list* seguido por um operador de adição binária. Na instrução
```csharp
x = y is C<T> + z;
```
os tokens `C<T>` são interpretados como uma *namespace_or_type_name* com um *type_argument_list*.

### <a name="invocation-expressions"></a>Expressões de invocação

Uma *invocation_expression* é usado para invocar um método.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Uma *invocation_expression* associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)) se tiver mantido pelo menos um dos seguintes:

* O *primary_expression* tem o tipo de tempo de compilação `dynamic`.
* Pelo menos um argumento de opcional *argument_list* tem o tipo de tempo de compilação `dynamic` e o *primary_expression* não tem um tipo de delegado.

Nesse caso, o compilador classifica os *invocation_expression* como um valor do tipo `dynamic`. As regras abaixo para determinar o significado do *invocation_expression* , em seguida, são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação do *primary_expression* e argumentos que têm o tipo de tempo de compilação `dynamic`. Se o *primary_expression* não tem o tipo de tempo de compilação `dynamic`, em seguida, a invocação do método passa por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de resolução de sobrecarga de dinâmica de tempo de compilação ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

O *primary_expression* de uma *invocation_expression* deve ser um grupo de método ou um valor de um *delegate_type*. Se o *primary_expression* é um grupo de método, o *invocation_expression* é uma invocação de método ([invocações de método](expressions.md#method-invocations)). Se o *primary_expression* é um valor de uma *delegate_type*, o *invocation_expression* é uma invocação de delegado ([delegar invocações](expressions.md#delegate-invocations)). Se o *primary_expression* não é um grupo de métodos nem um valor de uma *delegate_type*, ocorrerá um erro em tempo de associação.

Opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) fornece os valores ou referências de variáveis para os parâmetros do método.

O resultado da avaliação de uma *invocation_expression* são classificados da seguinte maneira:

*  Se o *invocation_expression* invoca um método ou um delegado que retorna `void`, o resultado é que nada. Uma expressão que é classificada como nada é permitido apenas no contexto de um *statement_expression* ([instruções de expressão](statements.md#expression-statements)) ou como o corpo de um *lambda_expression*([Expressões de função anônima](expressions.md#anonymous-function-expressions)). Caso contrário, ocorrerá um erro em tempo de vinculação.
*  Caso contrário, o resultado é um valor do tipo retornado pelo método ou delegate.

#### <a name="method-invocations"></a>Invocações de método

Para uma invocação de método, o *primary_expression* da *invocation_expression* deve ser um grupo de método. O grupo de método identifica um método para invocar ou o conjunto de métodos sobrecarregados para escolher um método específico para invocar. No último caso, a determinação do método específico para invocar se baseia o contexto fornecido pelos tipos dos argumentos de *argument_list*.

O processamento de tempo de associação de uma invocação de método do formulário `M(A)`, onde `M` é um grupo de métodos (incluindo, possivelmente, um *type_argument_list*), e `A` é um recurso opcional *argument_ lista*, consiste as seguintes etapas:

*  O conjunto de métodos de candidato para a invocação do método é construído. Para cada método de `F` associada ao grupo de método `M`:
   *  Se `F` é não genérica, `F` é um candidato quando:
      * `M` não tem nenhuma lista de argumentos de tipo, e
      * `F` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).
   *  Se `F` é genérico e `M` não tem nenhuma lista de argumentos de tipo `F` é um candidato quando:
      * Inferência de tipo ([inferência de tipo](expressions.md#type-inference)) for bem-sucedida, inferir uma lista de argumentos de tipo para a chamada, e
      * Depois que os argumentos de tipo inferidos são substituídos por parâmetros de tipo correspondente do método, todos os tipos construídos na lista de parâmetros de F atender às suas restrições ([atendendo às restrições de](types.md#satisfying-constraints)) e a lista de parâmetro de `F` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).
   *  Se `F` é genérico e `M` inclui uma lista de argumentos de tipo `F` é um candidato quando:
      * `F` tem o mesmo número de parâmetros de tipo de método como foram fornecidos na lista de argumentos de tipo, e
      * Depois que os argumentos de tipo são substituídos por parâmetros de tipo correspondente do método, todos os tipos construídos na lista de parâmetros de F atender às suas restrições ([atendendo às restrições](types.md#satisfying-constraints)) e a lista de parâmetros de `F` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).
*  O conjunto de métodos de candidato é reduzido para conter apenas métodos de tipos mais derivados: para cada método de `C.F` no conjunto, onde `C` é o tipo no qual o método `F` for declarado, todos os métodos declarados em um tipo base do `C`são removidos do conjunto. Além disso, se `C` é um tipo de classe diferente de `object`, todos os métodos declarados em um tipo de interface são removidos do conjunto. (Essa última regra só tem efeito quando o grupo de método foi o resultado de uma pesquisa de membro em um parâmetro de tipo que tem uma classe base efetivada que não seja o objeto e uma interface de efetivada vazio definido.)
*  Se o conjunto de métodos de candidato resultante está vazio, processamento adicional ao longo das etapas a seguir são abandonados e, em vez disso, é feita uma tentativa para processar a invocação de uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)). Se isso falhar, então nenhum método aplicável existe e ocorre um erro em tempo de vinculação.
*  O melhor método do conjunto de métodos de candidato é identificado usando as regras de resolução de sobrecarga de [resolução de sobrecarga](expressions.md#overload-resolution). Se um único método melhor não pode ser identificado, a invocação do método é ambígua e ocorre um erro em tempo de vinculação. Ao executar a resolução de sobrecarga, os parâmetros de um método genérico são considerados depois de substituir os argumentos de tipo (fornecida ou inferido) para os parâmetros de tipo correspondente do método.
*  Validação final do método melhor escolhido é executada:
   * O método é validado no contexto do grupo de método: se o melhor método é um método estático, o grupo de método deve ter resultado de uma *simple_name* ou um *member_access* por meio de um tipo. Se o melhor método é um método de instância, o grupo de método deve ter resultado de uma *simple_name*, um *member_access* por meio de uma variável ou um valor, ou uma *base_access*. Se nenhum desses requisitos for true, ocorrerá um erro de tempo de associação.
   * Se o melhor método é um método genérico, os argumentos de tipo (fornecida ou inferido) são verificados em relação às restrições ([atendendo às restrições de](types.md#satisfying-constraints)) declarado no método genérico. Se qualquer argumento de tipo não satisfaz a ões correspondente no parâmetro de tipo, ocorrerá um erro de tempo de associação.

Depois que um método foi selecionado e validado no tempo de associação, as etapas acima, a invocação de tempo de execução real é processada de acordo com as regras da invocação de membro de função descrito na [verificação de resolução de sobrecarga de dinâmica de tempo de compilação ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

O efeito intuitivo das regras de resolução descrito acima é da seguinte maneira: para localizar o método específico, invocado por uma invocação de método, comece com o tipo indicado pela invocação do método e continuar a cadeia de herança até que pelo menos um aplicável, declaração de método acessíveis, a substituição não for encontrada. Em seguida, realizar a inferência de tipo e resolução no conjunto de métodos aplicáveis, acessíveis e não-override declaradas nesse tipo de sobrecarga e invocar o método selecionado, portanto. Se nenhum método foi encontrado, tente em vez disso processar a invocação de uma invocação de método de extensão.

#### <a name="extension-method-invocations"></a>Invocações de método de extensão

Em uma invocação de método ([invocações em instâncias demarcadas](expressions.md#invocations-on-boxed-instances)) de uma das formas
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Se o processamento normal de invocação não encontrar nenhum método aplicável, é feita uma tentativa para processar a construção de uma invocação de método de extensão. Se *expr* ou qualquer um dos *args* tem o tipo de tempo de compilação `dynamic`, métodos de extensão não serão aplicada.

O objetivo é encontrar a melhor *type_name* `C`, de modo que a invocação de método estático correspondentes podem ocorrer:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Um método de extensão `Ci.Mj` está ***qualificados*** se:

*  `Ci` é uma classe não genérica, não aninhadas
*  O nome da `Mj` é *identificador*
*  `Mj` é aplicável quando aplicado aos argumentos como um método estático, como mostrado acima e acessível
*  Existe uma identidade implícitas, referência ou conversão boxing a partir *expr* para o tipo do primeiro parâmetro de `Mj`.

A pesquisa de `C` procede da seguinte maneira:

*  Começando com a mais próxima declaração namespace delimitador, continuando com cada declaração de namespace delimitador e terminando com a unidade de compilação que contém, tentativas sucessivas são feitas para localizar um conjunto de candidatos de métodos de extensão:
   * Se a unidade de compilação ou de namespace determinada diretamente contém declarações de tipo não genérico `Ci` com métodos de extensão qualificados `Mj`, em seguida, o conjunto desses métodos de extensão é o conjunto de candidatos.
   * Se tipos `Ci` importados pelo *using_static_declarations* e diretamente declarados em namespaces importados pelo *using_namespace_directive*s na unidade de especificada namespace ou de compilação diretamente contém métodos de extensão qualificados `Mj`, em seguida, o conjunto desses métodos de extensão é o conjunto de candidatos.
*  Se nenhum conjunto de candidato for encontrado em qualquer unidade de compilação ou de declaração de namespace delimitador, ocorrerá um erro de tempo de compilação.
*  Caso contrário, a resolução de sobrecarga é aplicada ao candidato definido conforme descrito em ([resolução de sobrecarga](expressions.md#overload-resolution)). Se nenhum método único de melhor for encontrado, ocorrerá um erro de tempo de compilação.
*  `C` é o tipo no qual o melhor método é declarado como um método de extensão.

Usando o `C` como um destino, a chamada de método é processada como uma invocação de método estático ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

As regras acima significam que os métodos de instância têm precedência sobre métodos de extensão, que os métodos de extensão disponíveis em declarações de namespace interno têm precedência sobre métodos de extensão disponíveis em declarações de namespace externo e essa extensão métodos declarados diretamente em um namespace têm precedência sobre métodos de extensão, importados para o mesmo namespace com o uso de uma diretiva de namespace. Por exemplo:
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

No exemplo, `B`do método tem precedência sobre o primeiro método de extensão, e `C`do método tem precedência sobre os dois métodos de extensão.

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

A saída deste exemplo é:
```
E.F(1)
D.G(2)
C.H(3)
```
`D.G` tem precedência sobre `C.G`, e `E.F` prevalece sobre ambos `D.F` e `C.F`.

#### <a name="delegate-invocations"></a>Invocações delegadas

Para uma invocação de delegado, o *primary_expression* da *invocation_expression* deve ser um valor de um *delegate_type*. Além disso, considerando o *delegate_type* ser um membro da função com a mesma lista de parâmetros como o *delegate_type*, o *delegate_type* deve ser aplicável ( [Membro da função aplicáveis](expressions.md#applicable-function-member)) com relação ao *argument_list* dos *invocation_expression*.

O processamento de tempo de execução de uma invocação de delegado do formulário `D(A)`, onde `D` é um *primary_expression* de um *delegate_type* e `A` é um opcional*argument_list*, consiste as seguintes etapas:

*  `D` é avaliado. Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.
*  O valor de `D` é verificado para ser válido. Se o valor de `D` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.
*  Caso contrário, `D` é uma referência a uma instância de delegado. Invocações de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) são executadas em cada uma das entidades que pode ser chamadas na lista de invocação do delegado. Para entidades que pode ser chamadas consiste em uma instância e o método de instância, a instância para a invocação é a instância contida na entidade que pode ser chamada.

### <a name="element-access"></a>Acesso de elemento

Uma *element_access* consiste em um *primary_no_array_creation_expression*, seguido por um "`[`" token, seguido por um *argument_list*, seguido por um " `]`"token. O *argument_list* consiste em um ou mais *argumento*s, separados por vírgulas.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

O *argument_list* de uma *element_access* não pode conter `ref` ou `out` argumentos.

Uma *element_access* associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)) se tiver mantido pelo menos um dos seguintes:

* O *primary_no_array_creation_expression* tem o tipo de tempo de compilação `dynamic`.
* Pelo menos uma expressão do *argument_list* tem o tipo de tempo de compilação `dynamic` e o *primary_no_array_creation_expression* não tem um tipo de matriz.

Nesse caso, o compilador classifica os *element_access* como um valor do tipo `dynamic`. As regras abaixo para determinar o significado do *element_access* , em seguida, são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação do *primary_no_array_creation_expression*e *argument_list* expressões que têm o tipo de tempo de compilação `dynamic`. Se o *primary_no_array_creation_expression* não tem o tipo de tempo de compilação `dynamic`, em seguida, o acesso de elemento passa por uma verificação de tempo de compilação limitada, conforme descrito em [verificando dinâmico de tempo de compilação resolução de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Se o *primary_no_array_creation_expression* de uma *element_access* é um valor de um *array_type*, o *element_access* é um acesso de matriz ([acesso de matriz](expressions.md#array-access)). Caso contrário, o *primary_no_array_creation_expression* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface que tem um ou mais membros de indexador, nesse caso, o *element_access* é um acesso do indexador ([acesso do indexador](expressions.md#indexer-access)).

#### <a name="array-access"></a>Acesso à matriz

Para obter um acesso de matriz, o *primary_no_array_creation_expression* da *element_access* deve ser um valor de um *array_type*. Além disso, o *argument_list* de uma matriz acesso não é permitido para conter argumentos nomeados. O número de expressões na *argument_list* deve ser o mesmo que a classificação do *array_type*, e cada expressão deve ser do tipo `int`, `uint`, `long`, `ulong`, ou deve ser implicitamente conversível para um ou mais desses tipos.

O resultado da avaliação de um acesso à matriz é uma variável do tipo de elemento da matriz, ou seja, o elemento de matriz selecionado pelos valores das expressões na *argument_list*.

O processamento de tempo de execução de um acesso de matriz no formato `P[A]`, onde `P` é um *primary_no_array_creation_expression* de um *array_type* e `A` é um *argument_list*, consiste as seguintes etapas:

*  `P` é avaliado. Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.
*  As expressões de índice do *argument_list* são avaliados na ordem da esquerda para a direita. Após a avaliação de cada expressão de índice, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) é executada para um dos seguintes tipos: `int`, `uint`, `long`, `ulong`. O primeiro tipo nessa lista para o qual existe uma conversão implícita é escolhido. Por exemplo, se a expressão de índice é do tipo `short` , em seguida, uma conversão implícita para `int` é executada, desde a conversões implícitas de `short` para `int` bidirecionalmente `short` para `long` são possíveis. Se a avaliação de uma expressão de índice ou a conversão implícita subsequente faz com que uma exceção, nenhuma expressão de índice adicional é avaliado, e nenhuma outra etapas são executadas.
*  O valor de `P` é verificado para ser válido. Se o valor de `P` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.
*  O valor de cada expressão na *argument_list* é verificado em relação os limites reais de cada dimensão da instância de matriz referenciada por `P`. Se um ou mais valores estão fora do intervalo, um `System.IndexOutOfRangeException` é lançada e nenhuma outra etapa é executada.
*  O local do elemento da matriz fornecido pelas expressões de índice é computado, e esse local se tornará o resultado do acesso de matriz.

#### <a name="indexer-access"></a>Acesso do indexador

Para acessar um indexador, o *primary_no_array_creation_expression* da *element_access* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface, e esse tipo deve implementar uma ou mais indexadores são aplicáveis em relação à *argument_list* da *element_access*.

O processamento de tempo de associação de um acesso do indexador do formulário `P[A]`, onde `P` é um *primary_no_array_creation_expression* de uma classe, struct ou tipo de interface `T`, e `A` é um *argument_list*, consiste as seguintes etapas:

*  O conjunto de indexadores fornecidos pelo `T` é construído. O conjunto consiste em todos os indexadores declarados na `T` ou um tipo base `T` que não estão `override` declarações e pode ser acessada no contexto atual ([acesso de membro](basic-concepts.md#member-access)).
*  O conjunto é reduzido para esses indexadores são aplicáveis e não ocultos por outros indexadores. As seguintes regras são aplicadas a cada indexador `S.I` no conjunto, onde `S` é o tipo no qual o indexador `I` é declarado:
   * Se `I` não é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)), em seguida, `I` é removido do conjunto.
   * Se `I` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)), em seguida, todos os indexadores declarados em um tipo base de `S` são removidas do conjunto de.
   * Se `I` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)) e `S` é um tipo de classe diferente de `object`, declarados em uma interface de todos os indexadores são removidos do conjunto.
*  Se o conjunto resultante de indexadores de candidato estiver vazio, em seguida, sem indexadores aplicáveis existirem e ocorre um erro em tempo de vinculação.
*  O indexador melhor do conjunto de indexadores do candidato é identificado usando as regras de resolução de sobrecarga de [resolução de sobrecarga](expressions.md#overload-resolution). Se um único indexador melhor não pode ser identificado, o acesso do indexador é ambíguo e ocorre um erro em tempo de vinculação.
*  As expressões de índice do *argument_list* são avaliados na ordem da esquerda para a direita. O resultado do processamento de acesso do indexador é uma expressão classificada como um acesso do indexador. A expressão de acesso do indexador referencia o indexador determinado na etapa anterior e tem uma expressão de instância associada do `P` e uma lista de argumentos associados de `A`.

Dependendo do contexto no qual ele é usado, um acesso de indexador faz com que a invocação de um a *acessador get* ou o *acessador set* do indexador. Se o acesso do indexador é o destino de uma atribuição, o *acessador set* é chamado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)). Em todos os outros casos, o *acessador get* é chamado para obter o valor atual ([valores das expressões](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Esse acesso

Um *this_access* consiste em uma palavra reservada `this`.

```antlr
this_access
    : 'this'
    ;
```

Um *this_access* é permitido somente na *bloco* de um construtor de instância, um método de instância ou um acessador de instância. Ele tem um dos seguintes significados:

*  Quando `this` é usado em uma *primary_expression* dentro de um construtor de instância de uma classe, ele é classificado como um valor. O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe na qual ocorre o uso e o valor é uma referência ao objeto que está sendo construído.
*  Quando `this` é usado em uma *primary_expression* dentro de um método de instância ou o acessador de instância de uma classe, ele é classificado como um valor. O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe na qual ocorre o uso e o valor é uma referência ao objeto para o qual o método ou o acessador foi invocado.
*  Quando `this` é usado em uma *primary_expression* dentro de um construtor de instância de um struct, ele é classificado como uma variável. O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) do struct no qual ocorre o uso e a variável representa a estrutura que está sendo construída. O `this` variável de um construtor de instância de um struct se comporta exatamente como um `out` parâmetro do tipo struct — em particular, isso significa que a variável deve ser definitivamente atribuída em cada caminho de execução da instância construtor.
*  Quando `this` é usado em uma *primary_expression* dentro de um método de instância ou o acessador de instância de um struct, ele é classificado como uma variável. O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) do struct no qual ocorre o uso.
   * Se o método ou o acessador não é um iterador ([iteradores](classes.md#iterators)), o `this` variável representa a estrutura para o qual o acessador ou método foi chamado e se comporta exatamente como um `ref` parâmetro do tipo struct.
   * Se o método ou o acessador é um iterador, o `this` variável representa uma cópia da estrutura para o qual o acessador ou método foi chamado e se comporta exatamente como um parâmetro de valor do tipo struct.

Uso de `this` em um *primary_expression* em um contexto diferente daqueles listados acima é um erro de tempo de compilação. Em particular, não é possível se referir `this` em um método estático, um acessador de propriedade estática, ou em um *variable_initializer* de uma declaração de campo.

### <a name="base-access"></a>Acesso de base

Um *base_access* consiste em uma palavra reservada `base` seguida por um "`.`" token e um identificador ou um *argument_list* entre colchetes:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Um *base_access* é usado para acessar membros de classe base que estão ocultos, da mesma forma nomeados membros na classe atual ou struct. Um *base_access* é permitido somente na *bloco* de um construtor de instância, um método de instância ou um acessador de instância. Quando `base.I` ocorre em uma classe ou struct, `I` deve indicar um membro da classe base dessa classe ou struct. Da mesma forma, quando `base[E]` ocorre em uma classe, um indexador aplicável deve existir na classe base.

Em tempo de vinculação *base_access* expressões do formulário `base.I` e `base[E]` são avaliadas exatamente como se eles fossem escritos `((B)this).I` e `((B)this)[E]`, onde `B` é a classe base da classe ou struct no qual ocorre a construção. Portanto, `base.I` e `base[E]` correspondem aos `this.I` e `this[E]`, exceto `this` é visto como uma instância da classe base.

Quando um *base_access* faz referência a um membro de função virtual (um método, propriedade ou indexador), a determinação de qual função de membro a ser invocado em tempo de execução ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é alterado. O membro da função que é invocado é determinado pelo Localizando a implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) do membro da função em relação às `B` (em vez de em relação ao tipo de tempo de execução de `this`, como seria normal em um acesso de base não). Portanto, dentro de um `override` de um `virtual` membro da função, um *base_access* pode ser usado para chamar a implementação herdada do membro da função. Se o membro da função referenciado por uma *base_access* é abstrato, ocorrerá um erro em tempo de associação.

### <a name="postfix-increment-and-decrement-operators"></a>Incremento de sufixo e operadores de decremento

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

O operando de um incremento ou decremento pós-fixada operação deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso do indexador. O resultado da operação é um valor do mesmo tipo como o operando.

Se o *primary_expression* tem o tipo de tempo de compilação `dynamic` e em seguida, o operador está dinamicamente vinculado ([vinculação dinâmica](expressions.md#dynamic-binding)), o *post_increment_expression*ou *post_decrement_expression* tem o tipo de tempo de compilação `dynamic` e as seguintes regras são aplicadas em tempo de execução usando o tipo de tempo de execução do *primary_expression*.

Se o operando de um sufixo incrementar ou operação de decremento é uma propriedade ou o acesso do indexador, propriedade ou indexador deve ter uma `get` e um `set` acessador. Se isso não for o caso, ocorre um erro em tempo de vinculação.

Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Predefinidas `++` e `--` operadores existem para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`e qualquer tipo de enumeração. Predefinido `++` operadores retornam o valor produzido pela adição de 1 para o operando e predefinido `--` operadores retornam o valor produzido pela subtração de 1 de operando. Em um `checked` contexto, se o resultado dessa adição ou subtração está fora do intervalo do tipo de resultado e o tipo de resultado é um tipo integral ou um tipo de enumeração, um `System.OverflowException` é gerada.

O processamento de tempo de execução de um incremento ou decremento a operação do formulário `x++` ou `x--` consiste as seguintes etapas:

*   Se `x` é classificado como uma variável:
    * `x` é avaliado para produzir a variável.
    * O valor de `x` é salvo.
    * O operador selecionado é invocado com o valor salvo do `x` como seu argumento.
    * O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.
    * O valor salvo do `x` se tornará o resultado da operação.
*   Se `x` é classificado como um acesso de propriedade ou indexador:
    * A expressão de instância (se `x` não é `static`) e a lista de argumentos (se `x` é um indexador de acesso) associadas com `x` são avaliados, e os resultados são usados em subsequente `get` e `set` invocações do acessador.
    * O `get` acessador de `x` é invocado e o valor retornado é salvo.
    * O operador selecionado é invocado com o valor salvo do `x` como seu argumento.
    * O `set` acessador da `x` é invocado com o valor retornado pelo operador como seu `value` argumento.
    * O valor salvo do `x` se tornará o resultado da operação.

O `++` e `--` operadores também dão suporte a notação de prefixo ([incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)). Normalmente, o resultado de `x++` ou `x--` é o valor da `x` antes da operação, enquanto que o resultado da `++x` ou `--x` é o valor do `x` após a operação. Em ambos os casos, `x` em si tem o mesmo valor após a operação.

Uma `operator ++` ou `operator --` implementação pode ser invocada usando a notação de prefixo ou sufixo. Não é possível ter implementações de operadores separados para as duas notações.

### <a name="the-new-operator"></a>O novo operador

O `new` operador é usado para criar novas instâncias de tipos.

Há três formas de `new` expressões:

*  Expressões de criação de objeto são usadas para criar novas instâncias de tipos de classes e tipos de valor.
*  Expressões de criação de matriz são usadas para criar novas instâncias dos tipos de matriz.
*  Expressões de criação de delegado são usadas para criar novas instâncias do representante de tipos.

O `new` operador implica a criação de uma instância de um tipo, mas não implica necessariamente alocação dinâmica de memória. Em particular, instâncias de tipos de valor não exigem nenhuma memória adicional, além de variáveis em que eles residem, e nenhuma alocação de dinâmica ocorre quando `new` é usado para criar instâncias de tipos de valor.

#### <a name="object-creation-expressions"></a>Expressões de criação de objeto

Uma *object_creation_expression* é usado para criar uma nova instância de um *class_type* ou uma *value_type*.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

O *tipo* de uma *object_creation_expression* deve ser um *class_type*, um *value_type* ou uma *type_parameter* . O *tipo* não pode ser um `abstract` *class_type*.

Opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) é permitido somente se o *tipo* é um *class_type* ou uma *struct_ tipo*.

Uma expressão de criação de objeto pode omitir a lista de argumentos de construtor e colocar parênteses fornecido inclui um inicializador de objeto ou um inicializador de coleção. Omitir a lista de argumentos de construtor e colocando parênteses são equivalente a especificar uma lista de argumentos vazia.

Processamento de uma expressão de criação de objeto que inclui um inicializador de objeto ou um inicializador de coleção consiste em processamento pela primeira vez o construtor de instância e, em seguida, processar as inicializações de membro ou o elemento especificadas pelo inicializador de objeto ([ Inicializadores de objeto](expressions.md#object-initializers)) ou o inicializador de coleção ([inicializadores de coleção](expressions.md#collection-initializers)).

Se qualquer um dos argumentos opcionais *argument_list* tem o tipo de tempo de compilação `dynamic` o *object_creation_expression* associado dinamicamente ([deassociaçãodinâmica](expressions.md#dynamic-binding)) e as seguintes regras são aplicadas em tempo de execução usando o tipo de tempo de execução desses argumentos do *argument_list* que têm o tipo de tempo de compilação `dynamic`. No entanto, a criação do objeto sofre uma verificação de tempo de compilação limitada, conforme descrito em [verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

O processamento de tempo de associação de um *object_creation_expression* do formulário `new T(A)`, onde `T` é um *class_type* ou uma *value_type* e `A` é um recurso opcional *argument_list*, consiste as seguintes etapas:

*   Se `T` é um *value_type* e `A` não está presente:
    * O *object_creation_expression* é uma invocação do construtor padrão. O resultado do *object_creation_expression* é um valor do tipo `T`, ou seja, o valor padrão para `T` conforme definido na [ValueType o tipo](types.md#the-systemvaluetype-type).
*   Caso contrário, se `T` é um *type_parameter* e `A` não está presente:
    * Se nenhuma restrição de tipo de valor ou uma restrição de construtor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) foi especificado para `T`, ocorrerá um erro em tempo de associação.
    * O resultado do *object_creation_expression* é um valor do tipo de tempo de execução que o parâmetro de tipo foi associado, ou seja, o resultado de chamar o construtor padrão desse tipo. O tipo de tempo de execução pode ser um tipo de referência ou um tipo de valor.
*   Caso contrário, se `T` é um *class_type* ou uma *struct_type*:
    * Se `T` é um `abstract` *class_type*, ocorre um erro de tempo de compilação.
    * Para invocar o construtor de instância é determinado usando as regras de resolução de sobrecarga de [resolução de sobrecarga](expressions.md#overload-resolution). O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessível declarados em `T` que são aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)). Se o conjunto de construtores de instância de release candidate está vazio ou se um construtor de instância única melhor não pode ser identificado, ocorrerá um erro de tempo de associação.
    * O resultado do *object_creation_expression* é um valor do tipo `T`, ou seja, o valor produzido por invocar o construtor de instância determinado na etapa anterior.
*  Caso contrário, o *object_creation_expression* for inválido, um erro de tempo de associação.

Mesmo se o *object_creation_expression* é dinamicamente vinculado, o tipo de tempo de compilação é ainda `T`.

O processamento de tempo de execução de um *object_creation_expression* do formulário `new T(A)`, onde `T` está *class_type* ou uma *struct_type* e `A` é um recurso opcional *argument_list*, consiste as seguintes etapas:

*   Se `T` é um *class_type*:
    * Uma nova instância da classe `T` é alocado. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.
    * Todos os campos da nova instância são inicializados com seus valores padrão ([valores padrão](variables.md#default-values)).
    * O construtor de instância é invocado de acordo com as regras da invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Uma referência à instância recentemente alocada automaticamente é passada para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.
*   Se `T` é um *struct_type*:
    * Uma instância do tipo `T` é criado ao alocar uma variável local temporária. Desde um construtor de instância de um *struct_type* é necessária, definitivamente, atribuir um valor para cada campo da instância que está sendo criado, nenhuma inicialização da variável temporária é necessária.
    * O construtor de instância é invocado de acordo com as regras da invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Uma referência à instância recentemente alocada automaticamente é passada para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.

#### <a name="object-initializers"></a>Inicializadores de objeto

Uma ***inicializador de objeto*** especifica valores para zero ou mais campos, propriedades ou elementos indexados de um objeto.

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

Um inicializador de objeto consiste em uma sequência de inicializadores de membro, delimitados por `{` e `}` tokens e separados por vírgulas. Cada *member_initializer* designa um destino para a inicialização. Uma *identificador* deve nomear um campo acessível ou uma propriedade do objeto que está sendo inicializado, enquanto um *argument_list* colocados entre colchetes deve especificar argumentos para um indexador acessível no objeto que está sendo inicializado. É um erro para um inicializador de objeto incluir mais de um inicializador de membro para o mesmo campo ou propriedade.

Cada *initializer_target* é seguido por um sinal de igual e uma expressão, um inicializador de objeto ou um inicializador de coleção. Não é possível para expressões no inicializador de objeto para fazer referência ao objeto recém-criado que está sendo inicializado.

Um inicializador de membro que especifica uma expressão depois que o sinal de igual é processado da mesma maneira que uma atribuição ([atribuição simples](expressions.md#simple-assignment)) para o destino.

Um inicializador de membro que especifica um inicializador de objeto após o sinal de igual é um ***inicializador de objeto aninhado***, ou seja, uma inicialização de um objeto inserido. Em vez de atribuir um novo valor para o campo ou propriedade, as atribuições no inicializador de objeto aninhado são tratadas como atribuições aos membros do campo ou propriedade. Inicializadores de objeto aninhado não podem ser aplicados a propriedades com um tipo de valor, ou a campos somente leitura com um tipo de valor.

Um inicializador de membro que especifica um inicializador de coleção após o sinal de igual é uma inicialização de uma coleção inserida. Em vez de atribuir uma nova coleção para o campo de destino, propriedade ou indexador, os elementos fornecidos no inicializador são adicionados à coleção referenciada pelo destino. O destino deve ser de um tipo de coleção que atende aos requisitos especificados em [inicializadores de coleção](expressions.md#collection-initializers).

Os argumentos para um inicializador de índice sempre serão avaliados exatamente uma vez. Portanto, mesmo se os argumentos acabam nunca sendo usado (por exemplo, devido a um inicializador vazio aninhado), eles serão avaliados por seus efeitos colaterais.

A classe a seguir representa um ponto com duas coordenadas:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Uma instância de `Point` pode ser criado e inicializado da seguinte maneira:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
que tem o mesmo efeito que
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
onde `__a` é uma variável temporária caso contrário, invisível e inacessível. A classe a seguir representa um retângulo criado a partir de dois pontos:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Uma instância de `Rectangle` pode ser criado e inicializado da seguinte maneira:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
que tem o mesmo efeito que
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
em que `__r`, `__p1` e `__p2` são variáveis temporárias que estariam invisíveis e inacessíveis.

Se `Rectangle`do construtor aloca os dois inseridos `Point` instâncias
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
a construção a seguir pode ser usada para inicializar o embedded `Point` instâncias em vez de atribuir novas instâncias:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
que tem o mesmo efeito que
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

Dada uma definição de apropriada de C, o exemplo a seguir:
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
é equivalente a esta série de atribuições de:
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
onde `__c`, etc., são geradas variáveis que são invisíveis e inacessíveis para o código-fonte. Observe que os argumentos para `[0,0]` são avaliada apenas uma vez e os argumentos para `[1,2]` são avaliadas uma vez, mesmo que eles nunca são usados.

#### <a name="collection-initializers"></a>Inicializadores de coleção

Um inicializador de coleção Especifica os elementos de uma coleção.

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

Um inicializador de coleção consiste em uma sequência de inicializadores de elemento, entre `{` e `}` tokens e separados por vírgulas. Cada inicializador de elemento Especifica um elemento a ser adicionado ao objeto de coleção que está sendo inicializado e consiste em uma lista de expressões entre `{` e `}` tokens e separados por vírgulas.  Um inicializador de elemento de expressão única pode ser escrito sem as chaves, mas, em seguida, não pode ser uma expressão de atribuição, para evitar ambiguidade com inicializadores de membro. O *non_assignment_expression* produção é definida no [expressão](expressions.md#expression).

Este é um exemplo de uma expressão de criação de objeto que inclui um inicializador de coleção:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

O objeto de coleção ao qual um inicializador de coleção é aplicado deve ser de um tipo que implementa `System.Collections.IEnumerable` ou ocorre um erro de tempo de compilação. Para cada elemento na ordem, o inicializador de coleção invoca um `Add` método no destino do objeto com a lista de expressões do inicializador de elemento como a lista de argumentos, aplicação de pesquisa de membro normal e resolução para cada invocação de sobrecarga. Portanto, o objeto de coleção deve ter um método de instância ou extensão aplicável, com o nome `Add` para cada inicializador de elemento.

A classe a seguir representa um contato com um nome e uma lista de números de telefone:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

Um `List<Contact>` pode ser criado e inicializado da seguinte maneira:
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
que tem o mesmo efeito que
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
em que `__clist`, `__c1` e `__c2` são variáveis temporárias que estariam invisíveis e inacessíveis.

#### <a name="array-creation-expressions"></a>Expressões de criação de matriz

Uma *array_creation_expression* é usado para criar uma nova instância de um *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Uma expressão de criação de matriz do primeiro formulário aloca uma instância de matriz do tipo que é o resultado da exclusão de cada uma das expressões individuais da lista de expressões. Por exemplo, a expressão de criação de matriz `new int[10,20]` produz uma instância de matriz do tipo `int[,]`e a expressão de criação de matriz `new int[10][,]` produz uma matriz do tipo `int[][,]`. Cada expressão na lista da expressão deve ser do tipo `int`, `uint`, `long`, ou `ulong`, ou implicitamente conversível em um ou mais desses tipos. O valor de cada expressão determina o comprimento da dimensão correspondente na instância de matriz recém-alocada. Como o comprimento de uma dimensão de matriz deve ser não negativo, ele é um erro de tempo de compilação para ter uma *constant_expression* com um valor negativo na lista de expressões.

Exceto em um contexto sem segurança ([contextos não seguros](unsafe-code.md#unsafe-contexts)), o layout de matrizes não é especificado.

Se uma expressão de criação de matriz do primeiro formulário inclui um inicializador de matriz, cada expressão na lista da expressão deve ser uma constante e os comprimentos de dimensão e de classificação especificados pela lista de expressão devem corresponder do inicializador de matriz.

Em uma expressão de criação de matriz no formato de segundo ou terceiro, a classificação do especificador de tipo ou a classificação de matriz especificado deve corresponder do inicializador de matriz. Os comprimentos de dimensão individuais são inferidos do número de elementos em cada um dos níveis de aninhamento correspondentes do inicializador de matriz. Assim, a expressão
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
corresponde exatamente ao
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Uma expressão de criação de matriz do terceiro formulário é conhecida como um ***expressão de criação de matriz de tipo implícito***. Ele é semelhante ao segundo formato, exceto que o tipo de elemento da matriz não for explicitamente fornecido, mas é determinado como o tipo mais comum ([localizando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) do conjunto de expressões na matriz inicializador. Para uma matriz multidimensional, ou seja, aquele em que o *rank_specifier* contém pelo menos uma vírgula, este conjunto consiste em todos *expressão*s encontrado no aninhados *array_initializer*s.

Inicializadores de matriz são descritos mais detalhadamente em [inicializadores de matriz](arrays.md#array-initializers).

O resultado da avaliação de uma expressão de criação de matriz é classificado como um valor, ou seja, uma referência para a instância de matriz recém-alocada. O processamento de tempo de execução de uma expressão de criação de matriz consiste as seguintes etapas:

*  As expressões de comprimento da dimensão do *lista_de_expressão* são avaliados na ordem da esquerda para a direita. Após a avaliação de cada expressão, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) é executada para um dos seguintes tipos: `int`, `uint`, `long`, `ulong`. O primeiro tipo nessa lista para o qual existe uma conversão implícita é escolhido. Se a avaliação de uma expressão ou a conversão implícita subsequente faz com que uma exceção, não há mais expressões são avaliadas, e nenhuma outra etapa é executada.
*  Os valores computados para os tamanhos da dimensão são validados da seguinte maneira. Se um ou mais dos valores são menor que zero, um `System.OverflowException` é lançada e nenhuma outra etapa é executada.
*  Uma instância de matriz com os comprimentos de determinada dimensão é alocada. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.
*  Todos os elementos da nova instância de matriz são inicializados com seus valores padrão ([valores padrão](variables.md#default-values)).
*  Se a expressão de criação de matriz contiver um inicializador de matriz, cada expressão de inicializador de matriz é avaliada e atribuída ao seu elemento correspondente da matriz. As atribuições e avaliações são executadas na ordem em que as expressões são gravadas no inicializador de matriz — em outras palavras, os elementos são inicializados em índice ordem crescente, com a dimensão mais à direita, aumentando pela primeira vez. Se a avaliação de uma determinada expressão ou atribuição subsequente para o elemento de matriz correspondente faz com que uma exceção, em seguida, sem elementos adicionais são inicializados (e os elementos restantes, portanto, terão seus valores padrão).

Uma expressão de criação de matriz permite a instanciação de uma matriz com elementos de um tipo de matriz, mas os elementos de como uma matriz devem ser inicializados manualmente. Por exemplo, a instrução
```csharp
int[][] a = new int[100][];
```
cria uma matriz unidimensional com 100 elementos do tipo `int[]`. O valor inicial de cada elemento é `null`. Não é possível para a mesma expressão de criação de matriz para também criar uma instância de submatrizes e a instrução
```csharp
int[][] a = new int[100][5];        // Error
```
resultados em um erro de tempo de compilação. Instanciação de subpropriedades matrizes em vez disso, deve ser executada manualmente, como em
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Quando uma matriz de matrizes tem um formato "retangular", ou seja, quando as matrizes de subpropriedades são todos do mesmo comprimento, é mais eficiente usar uma matriz multidimensional. No exemplo acima, a instanciação da matriz de matrizes cria 101 objetos — uma matriz externa e submatrizes de 100. Por outro lado,
```csharp
int[,] = new int[100, 5];
```
cria apenas um único objeto, uma matriz bidimensional e realiza a alocação em uma única instrução.

A seguir estão exemplos de expressões de criação de matriz de tipo implícito:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

A última expressão causa um erro de tempo de compilação porque nem `int` nem `string` é implicitamente conversível para o outro e, em seguida, digite aqui é não é mais comum. Uma expressão de criação de matriz tipada explicitamente deve ser usada nesse caso, por exemplo, especificando o tipo a ser `object[]`. Como alternativa, um dos elementos pode ser convertido em um tipo de base comum, que, em seguida, iria se tornar o tipo do elemento deduzido.

Expressões de criação de matriz implícita podem ser combinadas com inicializadores de objeto anônimos ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) criar anonimamente digitado estruturas de dados. Por exemplo:
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a>Expressões de criação de delegado

Um *delegate_creation_expression* é usado para criar uma nova instância de um *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

O argumento de uma expressão de criação de delegado deve ser um grupo de método, uma função anônima ou um valor de tipo de tempo de compilação `dynamic` ou um *delegate_type*. Se o argumento for um grupo de métodos, ele identifica o método e, para um método de instância, o objeto para o qual criar um delegado. Se o argumento for uma função anônima diretamente define os parâmetros e o corpo do método de destino de delegado. Se o argumento for um valor que identifica uma instância de delegado do qual criar uma cópia.

Se o *expressão* tem o tipo de tempo de compilação `dynamic`, o *delegate_creation_expression* associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)) e as regras abaixo são aplicadas em tempo de execução usando o tipo de tempo de execução do *expressão*. Caso contrário, as regras são aplicadas em tempo de compilação.

O processamento de tempo de associação de um *delegate_creation_expression* do formulário `new D(E)`, onde `D` é um *delegate_type* e `E` é uma *expressão* , consiste as seguintes etapas:

*  Se `E` é um grupo de método, a expressão de criação de delegado é processada da mesma forma como a conversão de grupos de método ([conversões de grupo de método](conversions.md#method-group-conversions)) de `E` para `D`.
*  Se `E` é uma função anônima, a expressão de criação de delegado é processada da mesma forma como uma conversão de função anônima ([conversões de função anônima](conversions.md#anonymous-function-conversions)) de `E` para `D`.
*  Se `E` é um valor `E` devem ser compatíveis ([declarações de delegado](delegates.md#delegate-declarations)) com `D`, e o resultado é uma referência a um delegado recém-criado do tipo `D` que se refere à invocação do mesma Listar como `E`. Se `E` não é compatível com `D`, ocorre um erro de tempo de compilação.

O processamento de tempo de execução de um *delegate_creation_expression* do formulário `new D(E)`, onde `D` é um *delegate_type* e `E` é uma *expressão* , consiste as seguintes etapas:

*   Se `E` é um grupo de método, a expressão de criação de delegado é avaliada como uma conversão de grupo de método ([conversões de grupo de método](conversions.md#method-group-conversions)) de `E` para `D`.
*   Se `E` é uma função anônima, a criação de delegado é avaliada como uma conversão de função anônima da `E` à `D` ([conversões de função anônima](conversions.md#anonymous-function-conversions)).
*   Se `E` é um valor de uma *delegate_type*:
    * `E` é avaliado. Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.
    * Se o valor de `E` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.
    * Uma nova instância do tipo de delegado `D` é alocado. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.
    * A nova instância de delegado é inicializada com a mesma lista de invocação que a instância do representante fornecida pelo `E`.

A lista de invocação de um delegado é determinada quando o delegado é instanciado e, em seguida, é uma constante para o tempo de vida inteiro do delegado. Em outras palavras, não é possível alterar as entidades de destino que pode ser chamado de um delegado após ele ter sido criado. Quando dois delegados são combinados ou um for removido de outra ([declarações de delegado](delegates.md#delegate-declarations)), faz com que um novo delegado; nenhum delegado existente tem seu conteúdo alterado.

Não é possível criar um delegado que faz referência a uma propriedade, indexador, operador definido pelo usuário, construtor de instância, destruidor ou construtor estático.

Conforme descrito acima, quando um delegado é criado de um grupo de método, a lista de parâmetros formais e tipo de retorno do delegado de determinar quais métodos sobrecarregados para selecionar. No exemplo
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
o `A.f` campo é inicializado com um delegado que se refere à segunda `Square` método porque esse método corresponde exatamente a lista de parâmetros formais e o tipo de retorno de `DoubleFunc`. Tinha o segundo `Square` método não estado presente, um erro de tempo de compilação teria ocorrido.

#### <a name="anonymous-object-creation-expressions"></a>Expressões de criação de objeto anônimo

Uma *anonymous_object_creation_expression* é usado para criar um objeto de um tipo anônimo.

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

Um inicializador de objeto anônimos declara um tipo anônimo e retorna uma instância desse tipo. Um tipo anônimo é um tipo sem nome de classe que herda diretamente de `object`. Os membros de um tipo anônimo são uma sequência de propriedades somente leitura inferidos do inicializador de objeto anônimo usado para criar uma instância do tipo. Especificamente, um inicializador de objeto anônimo do formulário
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
declara um tipo anônimo do formulário
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
em que cada `Tx` é o tipo da expressão correspondente `ex`. A expressão usada em uma *member_declarator* deve ter um tipo. Portanto, é um erro de tempo de compilação para uma expressão em uma *member_declarator* ser null ou uma função anônima. Também é um erro de tempo de compilação para a expressão ter um tipo que não é seguro.

Os nomes de um tipo anônimo e do parâmetro a seu `Equals` método são gerados automaticamente pelo compilador e não pode ser referenciado no texto do programa.

Dentro do mesmo programa, dois inicializadores de objeto anônimos que especificam uma sequência de propriedades dos mesmos nomes e tipos de tempo de compilação na mesma ordem produzirá instâncias do mesmo tipo anônimo.

No exemplo
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
a atribuição na última linha é permitida porque `p1` e `p2` são do mesmo tipo anônimo.

O `Equals` e `GetHashcode` métodos em tipos anônimos substituir os métodos herdados da `object`e são definidos em termos do `Equals` e `GetHashcode` das propriedades, para que duas instâncias do mesmo tipo anônimo são iguais se e apenas se todas as suas propriedades são iguais.

Um Declarador de membro pode ser abreviado como um nome simples ([inferência](expressions.md#type-inference)), um acesso de membro ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), um acesso de base ([deacessodeBase](expressions.md#base-access)) ou um acesso de membro condicional nulo ([expressões condicionais nulos como inicializadores de projeção](expressions.md#null-conditional-expressions-as-projection-initializers)). Isso é chamado de um ***inicializadores de projeção*** e é uma abreviação para uma declaração de e a atribuição a uma propriedade com o mesmo nome. Especificamente, os declaradores de membro dos formulários
```csharp
identifier
expr.identifier
```
são exatamente equivalentes às seguintes, respectivamente:
```csharp
identifier = identifier
identifier = expr.identifier
```

Assim, em inicializadores de projeção a *identificador* seleciona tanto o valor e o campo ou propriedade à qual o valor é atribuído. Intuitivamente, um inicializador de projeção projetos não apenas um valor, mas também o nome do valor.

### <a name="the-typeof-operator"></a>O operador typeof

O `typeof` operador é usado para obter o `System.Type` objeto para um tipo.

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

A primeira forma de *typeof_expression* consiste em um `typeof` seguido por um parênteses de palavra-chave *tipo*. O resultado de uma expressão desse formulário é o `System.Type` objeto para o tipo indicado. Há apenas um `System.Type` objeto para qualquer tipo determinado. Isso significa que, para um tipo `T`, `typeof(T) == typeof(T)` é sempre verdadeiro. O *tipo* não pode ser `dynamic`.

A segunda forma de *typeof_expression* consiste em um `typeof` seguido por um parênteses de palavra-chave *unbound_type_name*. Uma *unbound_type_name* é muito semelhante a um *type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)), exceto que um *unbound_type_name* contém *generic_dimension_specifier*s em que um *type_name* contém *type_argument_list*s. Quando o operando de um *typeof_expression* é uma sequência de tokens que satisfaça as gramáticas de ambos *unbound_type_name* e *type_name*, ou seja, quando ele não contém nem uma *generic_dimension_specifier* nem um *type_argument_list*, a sequência de tokens é considerada um *type_name*. O significado de um *unbound_type_name* é determinado da seguinte maneira:

*  Converter a sequência de tokens para um *type_name* , substituindo cada *generic_dimension_specifier* com um *type_argument_list* tendo o mesmo número de vírgulas e o palavra-chave `object` à medida que cada *type_argument*.
*  Avaliar resultante *type_name*, ignorando todas as restrições de parâmetro de tipo.
*  O *unbound_type_name* resolve para o tipo genérico não associado, associado com o tipo construído resultante ([associado e não associados a tipos](types.md#bound-and-unbound-types)).

O resultado do *typeof_expression* é o `System.Type` objeto resultante não associado de tipo genérico.

O terceiro formulário da *typeof_expression* consiste em um `typeof` palavra-chave, seguido por um entre parênteses `void` palavra-chave. O resultado de uma expressão desse formulário é o `System.Type` objeto que representa a ausência de um tipo. O objeto do tipo retornado por `typeof(void)` é diferente do tipo objeto retornado para qualquer tipo. Esse objeto de tipo especial é útil em bibliotecas de classes que permitem que a reflexão para métodos no idioma, em que esses métodos deverão ter uma forma de representar o tipo de retorno de qualquer método, incluindo métodos void, com uma instância do `System.Type`.

O `typeof` operador pode ser usado em um parâmetro de tipo. O resultado é o `System.Type` objeto para o tipo de tempo de execução que estava associado ao parâmetro de tipo. O `typeof` operador também pode ser usado em um tipo construído ou um tipo genérico não associado ([ligado e não associados a tipos de](types.md#bound-and-unbound-types)). O `System.Type` do objeto para um tipo genérico não associado não é igual a `System.Type` objeto do tipo de instância. O tipo de instância é sempre um tipo construído fechado em tempo de execução para sua `System.Type` objeto depende dos argumentos de tipo de tempo de execução em uso, enquanto o tipo genérico não associado não tiver nenhum argumento de tipo.

O exemplo
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
produz a seguinte saída:
```
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

Observe que `int` e `System.Int32` são do mesmo tipo.

Observe também que o resultado de `typeof(X<>)` não requer que o argumento de tipo, mas o resultado de `typeof(X<T>)` faz.

### <a name="the-checked-and-unchecked-operators"></a>Os operadores checked e unchecked

O `checked` e `unchecked` operadores são usados para controlar a ***contexto de verificação de estouro*** para conversões e operações aritméticas de tipo integral.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

O `checked` operador avalia a expressão contida em um contexto verificado e o `unchecked` operador avalia a expressão contida em um contexto desmarcado. Um *checked_expression* ou *unchecked_expression* corresponde exatamente um *parenthesized_expression* ([asexpressõesentreparênteses](expressions.md#parenthesized-expressions)), exceto que a expressão contida é avaliada no contexto de verificação de estouro determinado.

O contexto de verificação de estouro também pode ser controlado por meio de `checked` e `unchecked` instruções ([as instruções checked e unchecked](statements.md#the-checked-and-unchecked-statements)).

As seguintes operações são afetadas pelo contexto estabelecido de verificação de estouro de `checked` e `unchecked` operadores e as instruções:

*  Predefinido `++` e `--` operadores unários ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators) e [incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)), quando o operando for do integral tipo.
*  Predefinido `-` operador unário ([operador de subtração unário](expressions.md#unary-minus-operator)), quando o operando for do tipo integral.
*  Predefinido `+`, `-`, `*`, e `/` operadores binários ([operadores aritméticos](expressions.md#arithmetic-operators)), quando ambos os operandos forem de tipos integrais.
*  Conversões numéricas explícitas ([conversões numéricas explícitas](conversions.md#explicit-numeric-conversions)) de um tipo integral para outro tipo integral ou de `float` ou `double` para um tipo integral.

Quando uma das operações acima produzir um resultado que é muito grande para ser representado no tipo de destino, o contexto no qual a operação é executados controles o comportamento resultante:

*  Em um `checked` contexto, se a operação for uma expressão de constante ([expressões constantes](expressions.md#constant-expressions)), ocorre um erro de tempo de compilação. Caso contrário, quando a operação é executada em tempo de execução, um `System.OverflowException` é gerada.
*  Em um `unchecked` contexto, o resultado é truncado descartando quaisquer bits de ordem superior que não cabem no tipo de destino.

Para expressões não constantes (expressões são avaliadas em tempo de execução) que não são incluídos por qualquer `checked` ou `unchecked` operadores ou mais instruções, o contexto de verificação de estouro de padrão é `unchecked` , a menos que os fatores de externo (por exemplo, o compilador comutadores e configuração do ambiente de execução) chamam para `checked` avaliação.

Para expressões de constante (expressões que podem ser completamente avaliadas em tempo de compilação), o contexto de verificação de estouro de padrão sempre é `checked`. A menos que uma expressão de constante seja explicitamente colocada em um `unchecked` contexto, estouros que ocorrem durante a avaliação do tempo de compilação da expressão sempre causam erros de tempo de compilação.

O corpo de uma função anônima não é afetado pelas `checked` ou `unchecked` contextos em que a função anônima ocorre.

No exemplo
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
Nenhum erro de tempo de compilação é relatado, pois nenhuma das expressões pode ser avaliada em tempo de compilação. Em tempo de execução, o `F` método lança um `System.OverflowException`e o `G` método retorna-727379968 (os 32 bits inferiores do resultado fora do intervalo). O comportamento do `H` método depende do contexto para a compilação de verificação de estouro de padrão, mas é o mesmo que `F` ou o mesmo que `G`.

No exemplo
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
os estouros ocorridos durante a avaliação das expressões constantes na `F` e `H` causar erros de tempo de compilação a ser relatado porque as expressões são avaliadas em um `checked` contexto. Um estouro também ocorre ao avaliar a expressão de constante `G`, mas já que a avaliação é feita em um `unchecked` contexto, o estouro não será relatado.

O `checked` e `unchecked` operadores afetam apenas o contexto para as operações que estão contidos textualmente de verificação de estouro de "`(`"e"`)`" tokens. Os operadores não têm efeito nos membros da função que são invocados como resultado da avaliação da expressão independente. No exemplo
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
o uso de `checked` na `F` não afeta a avaliação de `x * y` na `Multiply`, então `x * y` é avaliada no contexto de verificação de estouro de padrão.

O `unchecked` operador é conveniente ao escrever constantes dos tipos integrais com sinal em notação hexadecimal. Por exemplo:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Ambas as constantes hexadecimais acima são do tipo `uint`. Como as constantes são fora o `int` de intervalo, sem a `unchecked` operador, as conversões para `int` geraria erros de tempo de compilação.

O `checked` e `unchecked` operadores e as instruções permitem que os programadores controlar determinados aspectos de alguns cálculos numéricos. No entanto, o comportamento de alguns operadores numéricos depende de tipos de dados de seus operandos. Por exemplo, a multiplicação de duas casas decimais sempre resulta em uma exceção no estouro, mesmo dentro um explicitamente `unchecked` construir. Da mesma forma, a multiplicação de dois floats nunca resultados em uma exceção no estouro até mesmo dentro um explicitamente `checked` construir. Além disso, outros operadores nunca são afetados pelo modo de verificação, se padrão ou explícita.

### <a name="default-value-expressions"></a>Expressões de valor padrão

Uma expressão de valor padrão é usada para obter o valor padrão ([valores padrão](variables.md#default-values)) de um tipo. Normalmente, uma expressão de valor padrão é usada para parâmetros de tipo, desde que ele pode não ser conhecido se o parâmetro de tipo é um tipo de valor ou um tipo de referência. (Não existe conversão do `null` literal a um parâmetro de tipo, a menos que o parâmetro de tipo é conhecido por ser um tipo de referência.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Se o *tipo* em um *default_value_expression* é avaliada em tempo de execução para um tipo de referência, o resultado é `null` convertido nesse tipo. Se o *tipo* em um *default_value_expression* é avaliada em tempo de execução para um tipo de valor, o resultado é o *value_type*do valor padrão ([padrão construtores](types.md#default-constructors)).

Um *default_value_expression* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) se o tipo é um tipo de referência ou um parâmetro de tipo é conhecido por ser um tipo de referência ([parâmetro de tipo restrições de](classes.md#type-parameter-constraints)). Além disso, uma *default_value_expression* é uma expressão constante, se o tipo é um dos seguintes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, ou qualquer tipo de enumeração.


### <a name="nameof-expressions"></a>Expressões Nameof

Um *nameof_expression* é usado para obter o nome de uma entidade programa como uma cadeia de caracteres constante.

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

Gramaticalmente falando, o *named_entity* operando sempre é uma expressão. Porque `nameof` não é uma palavra-chave reservada, a expressão nameof sempre é sintaticamente ambígua com uma invocação do nome simple `nameof`. Por razões de compatibilidade, se uma pesquisa de nome ([nomes simples](expressions.md#simple-names)) de nome `nameof` for bem-sucedida, a expressão é tratada como um *invocation_expression* – independentemente de se a invocação é legal. Caso contrário, ele é um *nameof_expression*.

O significado do *named_entity* de uma *nameof_expression* é o significado de-lo como uma expressão; ou seja, tanto como um *simple_name*, um *base_access*  ou um *member_access*. No entanto, em que a pesquisa descrito em [nomes simples](expressions.md#simple-names) e [acesso de membro](expressions.md#member-access) resulta em um erro porque um membro de instância foi encontrado em um contexto estático, uma *nameof_expression*não produz nenhum esse erro.

É um erro de tempo de compilação para um *named_entity* designar um grupo de métodos para ter um *type_argument_list*. É um erro de tempo de compilação para um *named_entity_target* para ter o tipo `dynamic`.

Um *nameof_expression* é uma expressão constante do tipo `string`, e não tem nenhum efeito em tempo de execução. Especificamente, sua *named_entity* não será avaliada e é ignorada para fins de análise de atribuição definitiva ([regras gerais para expressões simples](variables.md#general-rules-for-simple-expressions)). Seu valor é o último identificador do *named_entity* antes do final opcional *type_argument_list*, transformada da seguinte maneira:

* O prefixo "`@`", se usado, é removido.
* Cada *unicode_escape_sequence* é transformado em seu caractere Unicode correspondente.
* Qualquer *formatting_characters* são removidos.

Essas são as mesmas transformações aplicadas na [identificadores](lexical-structure.md#identifiers) ao testar a igualdade entre identificadores.

TODO: exemplos

### <a name="anonymous-method-expressions"></a>Expressões de método anônimo

Uma *anonymous_method_expression* é uma das duas maneiras de definir uma função anônima. Além disso, elas são descritas em [expressões de função anônima](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Operadores unários

O `?`, `+`, `-`, `!`, `~`, `++`, `--`, converta, e `await` os operadores unários são chamados de operadores.

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

Se o operando de um *unary_expression* tem o tipo de tempo de compilação `dynamic`, ele está vinculado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tempo de compilação de tipo dos *unary_expression* é `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução do operando.

### <a name="null-conditional-operator"></a>Operador nulo condicional

O operador nulo condicional se aplica uma lista das operações ao operando somente se o operando não for nulo. Caso contrário, o resultado da aplicação do operador é `null`.

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

A lista de operações pode incluir acesso de membro e operações de acesso de elemento (que podem estar nulo-condicional), bem como invocação.

Por exemplo, a expressão `a.b?[0]?.c()` é um *null_conditional_expression* com um *primary_expression* `a.b` e *null_conditional_operations* `?[0]` (acesso de elemento nulo-condicional), `?.c` (acesso de membro nulo condicional) e `()` (invocação).

Para um *null_conditional_expression* `E` com um *primary_expression* `P`, permitem que `E0` ser a expressão obtida removendo textualmente o entrelinhamento `?`de cada um dos *null_conditional_operations* de `E` que tiver uma. Conceitualmente, `E0` é a expressão que será avaliada se nenhum das verificações nulas representado pela `?`s encontrar um `null`.

Além disso, permitir que `E1` ser a expressão obtida removendo textualmente o entrelinhamento `?` de apenas o primeiro a *null_conditional_operations* em `E`. Isso pode levar a um *primary-expression* (se houver apenas um `?`) ou para outro *null_conditional_expression*.

Por exemplo, se `E` é a expressão `a.b?[0]?.c()`, em seguida, `E0` é a expressão `a.b[0].c()` e `E1` é a expressão `a.b[0]?.c()`.

Se `E0` é classificado como nothing, em seguida, `E` é classificado como nada. Caso contrário, E é classificado como um valor.

`E0` e `E1` são usados para determinar o significado de `E`:

*  Se `E` ocorre como uma *statement_expression* o significado de `E` é o mesmo que a instrução

   ```csharp
   if ((object)P != null) E1;
   ```

   exceto que P é avaliada apenas uma vez.

*  Caso contrário, se `E0` é classificado como nada ocorre um erro de tempo de compilação.

*  Caso contrário, deixe `T0` ser o tipo de `E0`.

   *  Se `T0` é um parâmetro de tipo que não sejam conhecido por ser um tipo de referência ou um tipo de valor não anulável, ocorre um erro de tempo de compilação.

   *  Se `T0` é um tipo de valor não nulo e, em seguida, o tipo de `E` é `T0?`e o significado de `E` é o mesmo que

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      exceto pelo fato de `P` é avaliada apenas uma vez.

   *  Caso contrário, o tipo de E é T0 e o significado E é o mesmo que

      ```csharp
      ((object)P == null) ? null : E1
      ```

      exceto pelo fato de `P` é avaliada apenas uma vez.

Se `E1` em si um *null_conditional_expression*, em seguida, essas regras são aplicadas novamente, os testes para de aninhamento `null` até que não haja nenhuma outra `?`do, e a expressão foi reduzida todo o caminho para baixo a expressão de primário `E0`.

Por exemplo, se a expressão `a.b?[0]?.c()` ocorre como uma instrução de expressão, como a instrução:
```csharp
a.b?[0]?.c();
```
seu significado é equivalente a:
```csharp
if (a.b != null) a.b[0]?.c();
```
que novamente é equivalente a:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.

Se ele ocorrer em um contexto em que seu valor é usado, como em:
```csharp
var x = a.b?[0]?.c();
```
e supondo que o tipo da invocação do final não é um tipo de valor não anulável, seu significado é equivalente a:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Expressões de nulo-condicional como inicializadores de projeção

Uma expressão condicional nulo é permitida somente como uma *member_declarator* em um *anonymous_object_creation_expression* ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) se ele termina com um acesso de membro (opcionalmente nulo-condicional). Gramaticalmente, esse requisito pode ser expresso como:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Esse é um caso especial da gramática para *null_conditional_expression* acima. Produção *member_declarator* na [expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions) , em seguida, inclua apenas *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Expressões de nulo-condicional como expressões de instrução

Uma expressão condicional nulo é permitida somente como uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)) se ele termina com uma invocação. Gramaticalmente, esse requisito pode ser expresso como:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Esse é um caso especial da gramática para *null_conditional_expression* acima. Produção *statement_expression* na [instruções de expressão](statements.md#expression-statements) , em seguida, inclua apenas *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Operador unário de adição

Para uma operação do formulário `+x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador. Os operadores de mais unária predefinida são:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Para cada um desses operadores, o resultado é simplesmente o valor do operando.

### <a name="unary-minus-operator"></a>Operador de subtração unário

Para uma operação do formulário `-x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador. Os operadores de negação predefinidos são:

*  Negação de inteiro:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   O resultado é calculado subtraindo `x` de zero. Se o valor da `x` é o menor valor representável do tipo de operando (-2 ^ 31 para `int` ou -2 ^ 63 para `long`), em seguida, a negação matemática do `x` não for representável no tipo de operando. Se isso ocorrer dentro de um `checked` contexto, uma `System.OverflowException` gerada; se ele ocorrer dentro de um `unchecked` contexto, o resultado é o valor do operando, e o estouro não será relatado.

   Se o operando do operador de negação é do tipo `uint`, ele será convertido ao tipo `long`, e o tipo do resultado é `long`. Uma exceção é a regra que permite que o `int` valor de -2147483648 (-2 ^ 31) a ser gravado como um literal de inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).

   Se o operando do operador de negação é do tipo `ulong`, ocorre um erro de tempo de compilação. Uma exceção é a regra que permite que o `long` valor de -9223372036854775808 (-2 ^ 63) a ser gravado como um literal de inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).

*  Negação de ponto flutuante:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   O resultado é o valor de `x` com o sinal invertido. Se `x` é NaN, o resultado também é NaN.

*  Negação decimal:

   ```csharp
   decimal operator -(decimal x);
   ```

   O resultado é calculado subtraindo `x` de zero. Negação decimal é equivalente a usar o operador do tipo de menos unário `System.Decimal`.

### <a name="logical-negation-operator"></a>Operador de negação lógica

Para uma operação do formulário `!x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador. Existe apenas um operador de negação lógica predefinidos:
```csharp
bool operator !(bool x);
```

Esse operador calcula a negação lógica do operando: se o operando `true`, o resultado é `false`. Se o operando `false`, o resultado é `true`.

### <a name="bitwise-complement-operator"></a>Operador de complemento bit a bit

Para uma operação do formulário `~x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador. Os operadores de complemento bit a bit predefinidos são:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Para cada um desses operadores, o resultado da operação é o complemento bit a bit de `x`.

Cada tipo de enumeração `E` implicitamente fornece o operador de complemento bit a bit a seguir:

```csharp
E operator ~(E x);
```

O resultado da avaliação `~x`, onde `x` é uma expressão de um tipo de enumeração `E` com um tipo subjacente `U`, é exatamente o mesmo que avaliar `(E)(~(U)x)`, exceto que a conversão em `E` é sempre é executada como se estiver em um `unchecked` contexto ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Incremento de prefixo e operadores de decremento

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

O operando de um incremento de prefixo ou decremento operação deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso do indexador. O resultado da operação é um valor do mesmo tipo como o operando.

Se o operando de um prefixo de incrementar ou operação de decremento é uma propriedade ou o acesso do indexador, propriedade ou indexador deve ter uma `get` e um `set` acessador. Se isso não for o caso, ocorre um erro em tempo de vinculação.

Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Predefinidas `++` e `--` operadores existem para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`e qualquer tipo de enumeração. Predefinido `++` operadores retornam o valor produzido pela adição de 1 para o operando e predefinido `--` operadores retornam o valor produzido pela subtração de 1 de operando. Em um `checked` contexto, se o resultado dessa adição ou subtração está fora do intervalo do tipo de resultado e o tipo de resultado é um tipo integral ou um tipo de enumeração, um `System.OverflowException` é gerada.

O processamento de tempo de execução de um incremento de prefixo ou operação do formulário de decremento `++x` ou `--x` consiste as seguintes etapas:

*   Se `x` é classificado como uma variável:
    * `x` é avaliado para produzir a variável.
    * O operador selecionado é invocado com o valor de `x` como seu argumento.
    * O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.
    * O valor retornado pelo operador torna-se o resultado da operação.
*   Se `x` é classificado como um acesso de propriedade ou indexador:
    * A expressão de instância (se `x` não é `static`) e a lista de argumentos (se `x` é um indexador de acesso) associadas com `x` são avaliados, e os resultados são usados em subsequente `get` e `set` invocações do acessador.
    * O `get` acessador de `x` é invocado.
    * O operador selecionado é invocado com o valor retornado pelo `get` acessador como seu argumento.
    * O `set` acessador da `x` é invocado com o valor retornado pelo operador como seu `value` argumento.
    * O valor retornado pelo operador torna-se o resultado da operação.

O `++` e `--` operadores também dão suporte a pós-fixação notação ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators)). Normalmente, o resultado de `x++` ou `x--` é o valor da `x` antes da operação, enquanto que o resultado da `++x` ou `--x` é o valor do `x` após a operação. Em ambos os casos, `x` em si tem o mesmo valor após a operação.

Uma `operator++` ou `operator--` implementação pode ser invocada usando a notação de prefixo ou sufixo. Não é possível ter implementações de operadores separados para as duas notações.

### <a name="cast-expressions"></a>Expressões de conversão

Um *cast_expression* é usado para converter explicitamente uma expressão para um determinado tipo.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

Um *cast_expression* do formulário `(T)E`, onde `T` é um *tipo* e `E` é uma *unary_expression*, executa um explícito conversão ([conversões explícitas](conversions.md#explicit-conversions)) do valor de `E` digitar `T`. Se nenhuma conversão explícita existe a partir `E` para `T`, ocorrerá um erro em tempo de associação. Caso contrário, o resultado é o valor produzido pela conversão explícita. O resultado sempre é classificado como um valor, mesmo se `E` denota uma variável.

A gramática para um *cast_expression* leva a determinados ambiguidades sintáticas. Por exemplo, a expressão `(x)-y` pode ser interpretado como um *cast_expression* (uma conversão de `-y` digitar `x`) ou como um *additive_expression* combinado com um *parenthesized_expression* (que calcula o valor `x - y)`.

Para resolver *cast_expression* ambiguidades, a regra a seguir existir: uma sequência de um ou mais *token*s ([espaço em branco](lexical-structure.md#white-space)) entre parênteses é considerado o início de um *cast_expression* apenas se pelo menos um dos seguintes forem verdadeiro:

*  A sequência de tokens é a gramática correta para uma *tipo*, mas não para um *expressão*.
*  A sequência de tokens é a gramática correta para uma *tipo*, e o token imediatamente após os parênteses de fechamento é o token "`~`", o token "`!`", o token "`(`", um  *identificador* ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)), uma *literal* ([literais](lexical-structure.md#literals)), ou qualquer *palavra-chave*([Palavras-chave](lexical-structure.md#keywords)), exceto `as` e `is`.

O termo "gramática correta" acima significa apenas que a sequência de tokens deve estar de acordo com a produção gramatical específica. Especificamente, ele não considera o significado real de todos os identificadores constituintes. Por exemplo, se `x` e `y` são os identificadores, em seguida, `x.y` é a gramática correta para um tipo, mesmo se `x.y` , na verdade, não denota um tipo.

Da regra de Desambiguidade segue que, se `x` e `y` são identificadores, `(x)y`, `(x)(y)`, e `(x)(-y)` são *cast_expression*s, mas `(x)-y` é não, mesmo se `x` identifica um tipo. No entanto, se `x` é uma palavra-chave que identifica um tipo predefinido (como `int`), em seguida, todos os quatro formulários são *cast_expression*s (porque tal uma palavra-chave não pode ser uma expressão, possivelmente, por si só).

### <a name="await-expressions"></a>Expressões await

O operador de espera é usado para suspender a avaliação da função assíncrona até que a operação assíncrona representada pelo operando seja concluída.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Uma *await_expression* só é permitida no corpo de uma função assíncrona ([iteradores](classes.md#iterators)). Dentro do delimitador mais próximo função assíncrona, um *await_expression* pode não ocorrer nesses locais:

*  Dentro de uma função anônima do aninhada (não assíncronas)
*  Dentro do bloco de um *lock_statement*
*  Em um contexto inseguro

Observe que um *await_expression* não pode ocorrer na maioria dos lugares dentro de uma *query_expression*, porque aquelas sintaticamente são transformadas para usar as expressões lambda não assíncronas.

Dentro de uma função assíncrona, `await` não pode ser usado como um identificador. Portanto, não há nenhuma ambiguidade sintática entre expressões await e várias expressões que envolvem identificadores. Fora de funções assíncronas, `await` atua como um identificador normal.

O operando de um *await_expression* é chamado de ***tarefa***. Ele representa uma operação assíncrona que pode ou não pode ser concluída no momento a *await_expression* é avaliada. É a finalidade do operador await suspender a execução da função assíncrona até que a tarefa aguardada seja concluída e, em seguida, obter seu resultado.

#### <a name="awaitable-expressions"></a>Expressões de awaitable

A tarefa de uma expressão await é necessária para ser ***aguardável***. Uma expressão `t` é awaitable, se tiver mantido por um dos seguintes:

*  `t` é do tipo de tempo de compilação `dynamic`
*  `t` tem um método de extensão ou de instância acessível chamado `GetAwaiter` sem parâmetros e nenhum parâmetro de tipo e um tipo de retorno `A` para o qual todos os itens a seguir manter:
   * `A` implementa a interface `System.Runtime.CompilerServices.INotifyCompletion` (daqui em diante, conhecido como `INotifyCompletion` para fins de brevidade)
   * `A` tem uma propriedade de instância acessível e legível `IsCompleted` do tipo `bool`
   * `A` tem um método de instância acessível `GetResult` sem parâmetros e nenhum parâmetro de tipo

A finalidade de `GetAwaiter` método é obter uma ***awaiter*** para a tarefa. O tipo `A` é chamado de ***tipo awaiter*** para a expressão await.

A finalidade de `IsCompleted` é de propriedade determinar se a tarefa já foi concluída. Nesse caso, não é necessário para suspender a avaliação.

A finalidade do `INotifyCompletion.OnCompleted` método é inscrever-se de "continuação" para a tarefa; ou seja, um delegado (do tipo `System.Action`) que será invocado quando a tarefa for concluída.

A finalidade de `GetResult` método é obter o resultado da tarefa quando ela for concluída. Esse resultado pode ser a conclusão bem-sucedida, possivelmente com um valor de resultado, ou pode ser uma exceção que é lançada pelo `GetResult` método.

#### <a name="classification-of-await-expressions"></a>Expressões de classificação de await

A expressão `await t` é classificado da mesma forma que a expressão `(t).GetAwaiter().GetResult()`. Portanto, se o tipo de retorno de `GetResult` está `void`, o *await_expression* é classificado como nada. Se ele tiver um tipo de retorno não nulo `T`, o *await_expression* é classificado como um valor do tipo `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Expressões de avaliação do tempo de execução do await

Em tempo de execução, a expressão `await t` é avaliada como segue:

*  Um awaiter `a` é obtido avaliando a expressão `(t).GetAwaiter()`.
*  Um `bool` `b` é obtido avaliando a expressão `(a).IsCompleted`.
*  Se `b` está `false` e em seguida, avaliação depende `a` implementa a interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (daqui em diante, conhecido como `ICriticalNotifyCompletion` para fins de brevidade). Essa verificação é feita no tempo; de ligação ou seja, em tempo de execução se `a` tem o tipo de tempo de compilação `dynamic`e em tempo de compilação caso contrário. Deixe `r` denotam o delegado de continuação ([iteradores](classes.md#iterators)):
    * Se `a` não implementa `ICriticalNotifyCompletion`, em seguida, a expressão `(a as (INotifyCompletion)).OnCompleted(r)` é avaliada.
    * Se `a` implementar `ICriticalNotifyCompletion`, em seguida, a expressão `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` é avaliada.
    * Avaliação, em seguida, é suspenso e controle é retornado ao chamador da função assíncrona atual.
*  Qualquer um dos imediatamente após (se `b` foi `true`), ou mediante posterior invocação de delegado de continuação (se `b` foi `false`), a expressão `(a).GetResult()` é avaliada. Se ela retorna um valor, esse valor é o resultado do *await_expression*. Caso contrário, o resultado é que nada.

A implementação de um awaiter métodos da interface `INotifyCompletion.OnCompleted` e `ICriticalNotifyCompletion.UnsafeOnCompleted` deve fazer com que o delegado `r` a ser invocado no máximo uma vez. Caso contrário, o comportamento da função async será indefinido.

## <a name="arithmetic-operators"></a>Operadores aritméticos

O `*`, `/`, `%`, `+`, e `-` operadores são chamados de operadores aritméticos.

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

Se um operando do operador aritmético tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.

### <a name="multiplication-operator"></a>Operador de multiplicação

Para uma operação do formulário `x * y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores de multiplicação predefinidos estão listados abaixo. Todos os operadores compute o produto dos `x` e `y`.

*  Multiplicação de inteiro:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   Em um `checked` contexto, se o produto está fora do intervalo do tipo de resultado, um `System.OverflowException` é gerada. Em um `unchecked` contexto, estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.


*  Multiplicação de ponto flutuante:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   O produto é calculado de acordo com as regras de aritmética do IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN. Na tabela, `x` e `y` são valores positivos de finitos. `z` é o resultado de `x * y`. Se o resultado for muito grande para o tipo de destino, `z` é infinito. Se o resultado for muito pequeno para o tipo de destino, `z` é zero.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | + y   | -y   | +0  | -0  | +inf | -inf | NaN | 
   | + x   | + z   | -z   | +0  | -0  | +inf | -inf | NaN | 
   | -x   | -z   | + z   | -0  | +0  | -inf | +inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | +inf | +inf | -inf | NaN | NaN | +inf | -inf | NaN | 
   | -inf | -inf | +inf | NaN | NaN | -inf | +inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Multiplicação decimal:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada. Se o valor do resultado for muito pequeno para representar o `decimal` formato, o resultado é zero. A escala do resultado, antes de qualquer arredondamento, é a soma das escalas dos dois operandos.

   Multiplicação decimal é equivalente a usar o operador de multiplicação do tipo `System.Decimal`.


### <a name="division-operator"></a>Operador de divisão

Para uma operação do formulário `x / y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores de divisão predefinidos estão listados abaixo. O quociente de computação de todos os operadores `x` e `y`.

*  Divisão de inteiros:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Se o valor do operando à direita for zero, um `System.DivideByZeroException` é gerada.

   A divisão Arredonda o resultado em direção a zero. Assim, o valor absoluto do resultado é o maior inteiro possível que seja menor ou igual ao valor absoluto do quociente de dois operandos. O resultado é zero ou positivo quando os dois operandos têm o mesmo sinal e zero ou negativo quando os dois operandos tiverem oposta sinais.

   Se o operando esquerdo for menor representável `int` ou `long` valor e o operando direito é `-1`, ocorre um estouro. Em um `checked` contexto, isso faz com que um `System.ArithmeticException` (ou uma subclasse disso) seja lançada. Em um `unchecked` contexto, ele é definido pela implementação sobre se um `System.ArithmeticException` (ou uma subclasse disso) é gerada ou o estouro é relatado com o valor resultante, sendo que a do operando à esquerda.

*  Divisão de ponto flutuante:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   O quociente é calculado de acordo com as regras de aritmética do IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN. Na tabela, `x` e `y` são valores positivos de finitos. `z` é o resultado de `x / y`. Se o resultado for muito grande para o tipo de destino, `z` é infinito. Se o resultado for muito pequeno para o tipo de destino, `z` é zero.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | + x   | + z   | -z   | +inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | + z   | -inf | +inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | +inf | +inf | -inf | +inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | +inf | -inf | +inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Divisão decimal:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Se o valor do operando à direita for zero, um `System.DivideByZeroException` é gerada. Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada. Se o valor do resultado for muito pequeno para representar o `decimal` formato, o resultado é zero. A escala do resultado é a escala menor que irá preservar um resultado igual ao valor decimal representável para o resultado matemático true mais próximo.

   Divisão decimal é equivalente a usar o operador de divisão do tipo `System.Decimal`.


### <a name="remainder-operator"></a>Operador restante

Para uma operação do formulário `x % y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores de restante predefinidos estão listados abaixo. Todos os operadores compute o resto da divisão entre `x` e `y`.

*  Resto inteiro:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   O resultado de `x % y` é o valor produzido por `x - (x / y) * y`. Se `y` for zero, um `System.DivideByZeroException` é gerada.

   Se o operando esquerdo for menor `int` ou `long` é de valor e o operando direito `-1`, um `System.OverflowException` é gerada. Em nenhum caso faz `x % y` lançar uma exceção onde `x / y` não geraria uma exceção.

*  Ponto flutuante restante:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN. Na tabela, `x` e `y` são valores positivos de finitos. `z` é o resultado de `x % y` e é computado como `x - n * y`, onde `n` é o maior inteiro possíveis que é menor que ou igual a `x / y`. Esse método de computação o restante é análogo àquela utilizada para operandos de inteiro, mas é diferente da definição de IEEE 754 (no qual `n` é o inteiro mais próximo ao `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | + x   | + z   | + z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | +inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Decimal restante:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Se o valor do operando à direita for zero, um `System.DivideByZeroException` é gerada. A escala do resultado, antes de qualquer arredondamento, é o maior entre as escalas de dois operandos, e o sinal do resultado, se diferente de zero, é o mesmo da `x`.

   Decimal restante é equivalente a usar o operador de resto do tipo `System.Decimal`.


### <a name="addition-operator"></a>Operador de adição

Para uma operação do formulário `x + y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores de adição predefinidas são listados abaixo. Para tipos numéricos e de enumeração, os operadores de adição predefinidos calcular a soma dos dois operandos. Quando um ou ambos os operandos são do tipo cadeia de caracteres, os operadores de adição predefinidos concatenar a representação de cadeia de caracteres dos operandos.

*  Adição de inteiro:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   Em um `checked` contexto, se a soma está fora do intervalo do tipo de resultado, um `System.OverflowException` é gerada. Em um `unchecked` contexto, estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.

*  Adição de ponto flutuante:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   A soma é calculada de acordo com as regras de aritmética do IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN. Na tabela, `x` e `y` são valores Finitas diferente de zero, e `z` é o resultado de `x + y`. Se `x` e `y` têm a mesma magnitude mas oposta sinais, `z` for zero positivo. Se `x + y` é muito grande para ser representado no tipo de destino, `z` é infinito com o mesmo sinal que `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | +inf | -inf | NaN  | 
   | x    | z    | x    | x    | +inf | -inf | NaN  | 
   | +0   | y    | +0   | +0   | +inf | -inf | NaN  | 
   | -0   | y    | +0   | -0   | +inf | -inf | NaN  | 
   | +inf | +inf | +inf | +inf | +inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Adição de decimal:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada. A escala do resultado, antes de qualquer arredondamento, é o maior entre as escalas de dois operandos.

   Adição de decimal é equivalente a usar o operador de adição do tipo `System.Decimal`.

*  Adição de enumeração. Implicitamente, a cada tipo de enumeração fornece o seguinte predefinidas operadores, onde `E` é o tipo de enumeração, e `U` é o tipo subjacente de `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   Em tempo de execução, esses operadores são avaliados exatamente como `(E)((U)x + (U)y)`.

*  Concatenação de cadeia de caracteres:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Essas sobrecargas do binário `+` operador realizam a concatenação de cadeia de caracteres. Se um operando de concatenação de cadeia de caracteres é `null`, uma cadeia de caracteres vazia é substituída. Caso contrário, qualquer argumento de cadeia de caracteres não é convertido em sua representação de cadeia de caracteres com a invocação virtual `ToString` método herdado do tipo `object`. Se `ToString` retorna `null`, uma cadeia de caracteres vazia é substituída.

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   O resultado do operador de concatenação de cadeia de caracteres é uma cadeia de caracteres que consiste em caracteres do operando à esquerda seguido pelos caracteres do operando à direita. O operador de concatenação de cadeia de caracteres nunca retorna um `null` valor. Um `System.OutOfMemoryException` poderá ser gerada se não há memória suficiente disponível para alocar a cadeia de caracteres resultante.

*  Combinação de delegado. Cada tipo de delegado implicitamente fornece o operador pré-definido seguinte, onde `D` é o tipo de delegado:

   ```csharp
   D operator +(D x, D y);
   ```

   O binário `+` operador executa a combinação de delegados quando ambos os operandos são de algum tipo de delegado `D`. (Se os operandos tiverem tipos diferentes de delegado, ocorrerá um erro em tempo de associação.) Se for o primeiro operando `null`, o resultado da operação é o valor do segundo operando (mesmo que seja também `null`). Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando. Caso contrário, o resultado da operação é uma nova instância delegada que, quando invocado, invoca o primeiro operando e, em seguida, invoca o segundo operando. Para obter exemplos de combinação de delegados, consulte [operador de subtração](expressions.md#subtraction-operator) e [invocação de delegado](delegates.md#delegate-invocation). Uma vez que `System.Delegate` não é um tipo de delegado `operator` `+` não está definido para ele.

### <a name="subtraction-operator"></a>Operador de subtração

Para uma operação do formulário `x - y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores de subtração predefinidas são listados abaixo. Os operadores todos subtrair `y` de `x`.

*  Subtração de inteiro:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   Em um `checked` contexto, se a diferença está fora do intervalo do tipo de resultado, um `System.OverflowException` é gerada. Em um `unchecked` contexto, estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.

*  Subtração de ponto flutuante:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   A diferença é calculada de acordo com as regras de aritmética do IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e NaNs. Na tabela, `x` e `y` são valores Finitas diferente de zero, e `z` é o resultado de `x - y`. Se `x` e `y` forem iguais, `z` for zero positivo. Se `x - y` é muito grande para ser representado no tipo de destino, `z` é infinito com o mesmo sinal que `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | NaN  | y    | +0   | -0   | +inf | -inf | NaN | 
   | x    | z    | x    | x    | -inf | +inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | +inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | +inf | NaN | 
   | +inf | +inf | +inf | +inf | NaN  | +inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Subtração decimal:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada. A escala do resultado, antes de qualquer arredondamento, é o maior entre as escalas de dois operandos.

   Subtração de decimal é equivalente a usar o operador de subtração do tipo `System.Decimal`.

*  Subtração de enumeração. Cada tipo de enumeração implicitamente fornece o operador pré-definido seguinte, onde `E` é o tipo de enumeração, e `U` é o tipo subjacente de `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Esse operador é avaliado exatamente como `(U)((U)x - (U)y)`. Em outras palavras, o operador calcula a diferença entre os valores ordinais das `x` e `y`, e o tipo do resultado é o tipo subjacente da enumeração.

   ```csharp
   E operator -(E x, U y);
   ```

   Esse operador é avaliado exatamente como `(E)((U)x - y)`. Em outras palavras, o operador subtrai um valor do tipo subjacente da enumeração, que rende um valor da enumeração.

*  Remoção de delegado. Cada tipo de delegado implicitamente fornece o operador pré-definido seguinte, onde `D` é o tipo de delegado:

   ```csharp
   D operator -(D x, D y);
   ```

   O binário `-` operador executa a remoção de delegado quando ambos os operandos são de algum tipo de delegado `D`. Se os operandos tiverem tipos diferentes de delegado, ocorrerá um erro de tempo de associação. Se for o primeiro operando `null`, o resultado da operação é `null`. Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando. Caso contrário, ambos os operandos representam listas de invocação ([declarações de delegado](delegates.md#delegate-declarations)) ter uma ou mais entradas e o resultado é uma nova lista de invocação, consistindo de lista do primeiro operando com entradas do segundo operando removidas do forneceu a lista do segundo operando é uma sublista contígua adequada do primeiro.     (Para determinar igualdade sublista, as entradas correspondentes são comparadas como para o operador de igualdade de delegado ([delegar operadores de igualdade](expressions.md#delegate-equality-operators)).) Caso contrário, o resultado é o valor do operando à esquerda. Nenhuma das listas dos operandos é alterada no processo. Se a lista do segundo operando corresponder a várias sublistas contíguos entradas da lista do primeiro operando, a sublista de correspondência mais à direita das entradas contíguas é removida. Se os resultados da remoção em uma lista vazia, o resultado será `null`. Por exemplo:

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a>Operadores shift

O `<<` e `>>` operadores são usados para executar operações de mudança de bits.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Se um operando de um *shift_expression* tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.

Para uma operação do formulário `x << count` ou `x >> count`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Ao declarar um operador de deslocamento sobrecarregado, o tipo do primeiro operando sempre deve ser a classe ou struct que contém a declaração do operador e o tipo do segundo operando deve ser sempre `int`.

Os operadores shift predefinidas são listados abaixo.

*  Usar SHIFT-left:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   O `<<` turnos de operador `x` à esquerda por um número de bits calculado conforme descrito abaixo.

   Os bits de ordem superior fora do intervalo do tipo de resultado `x` são descartados, os bits restantes são deslocados para a esquerda e as posições de bits vazios de ordem inferior são definidas como zero.

*  SHIFT direito:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   O `>>` turnos de operador `x` à direita por um número de bits calculado conforme descrito abaixo.

   Quando `x` é do tipo `int` ou `long`, os bits de ordem inferior do `x` são descartados, os bits restantes são deslocados para a direita, e as posições de bits vazios de ordem superior são definidas como zero se `x` é não negativo e será definida para um, se `x` é negativo.

   Quando `x` é do tipo `uint` ou `ulong`, os bits de ordem inferior de `x` são descartados, os bits restantes são deslocados para a direita, e as posições de bits vazios de ordem superior são definidas como zero.

Para os operadores predefinidos, o número de bits a deslocar é calculado da seguinte maneira:

*  Quando o tipo de `x` está `int` ou `uint`, a contagem de shift é determinada pelos cinco bits inferiores de `count`. Em outras palavras, a contagem de deslocamento é computada a partir `count & 0x1F`.
*  Quando o tipo de `x` está `long` ou `ulong`, a contagem de deslocamento será determinada pelos seis bits inferiores de `count`. Em outras palavras, a contagem de deslocamento é computada a partir `count & 0x3F`.

Se a contagem de deslocamento resultante for zero, os operadores shift simplesmente retornam o valor de `x`.

Operações de deslocamento nunca causam estouros e produzem os mesmos resultados no `checked` e `unchecked` contextos.

Quando o operando da esquerda a `>>` operador é de um tipo integral com sinal, o operador executa um deslocamento aritmético à direita no qual o valor do bit mais significativo (o bit de sinal) do operando é propagado para as posições de bit de ordem superior vazio. Quando o operando da esquerda a `>>` operador é de um tipo integral sem sinal, o operador executa um deslocamento lógico à direita no qual as posições de bit de ordem superior vazias são sempre definidas como zero. Para executar a operação oposta do que é inferido do tipo de operando, conversões explícitas podem ser usados. Por exemplo, se `x` é uma variável do tipo `int`, a operação `unchecked((int)((uint)x >> y))` executa um deslocamento lógico à direita do `x`.

## <a name="relational-and-type-testing-operators"></a>Operadores relacionais e de teste de tipo

O `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` e `as` operadores são chamados de operadores relacionais e de teste de tipo.

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

O `is` operador é descrito em [a é o operador](expressions.md#the-is-operator) e o `as` operador é descrito na [o operador](expressions.md#the-as-operator).

O `==`, `!=`, `<`, `>`, `<=` e `>=` operadores são ***operadores de comparação***.

Se um operando do operador de comparação tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.

Para uma operação do formulário `x` *op* `y`, onde *op* é um operador de comparação, a resolução de sobrecarga ([deresoluçãodesobrecargadeoperadorbinário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores de comparação predefinidos são descritos nas seções a seguir. Todos os operadores de comparação predefinido retornam um resultado do tipo `bool`, conforme descrito na tabela a seguir.


| __operação__ | __Result__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` Se `x` é igual a `y`, `false` caso contrário,                 | 
| `x != y`      | `true` Se `x` não é igual a `y`, `false` caso contrário,             | 
| `x < y`       | `true` Se `x` é menor que `y`, `false` caso contrário,                | 
| `x > y`       | `true` Se `x` é maior que `y`, `false` caso contrário,             | 
| `x <= y`      | `true` Se `x` é menor que ou igual a `y`, `false` caso contrário,    | 
| `x >= y`      | `true` Se `x` é maior que ou igual a `y`, `false` caso contrário, | 

### <a name="integer-comparison-operators"></a>Operadores de comparação de inteiro

Os operadores de comparação de inteiro predefinidos são:
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

Cada um desses operadores compara os valores numéricos dos operandos dois inteiros e retorna um `bool` valor que indica se a relação específica `true` ou `false`.

### <a name="floating-point-comparison-operators"></a>Operadores de comparação de ponto flutuante

Os operadores de comparação de ponto flutuante predefinidos são:
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

Os operadores comparam os operandos de acordo com as regras do padrão IEEE 754:

*  Se qualquer operando for NaN, o resultado será `false` para todos os operadores exceto `!=`, para que o resultado é `true`. Para qualquer dois operandos, `x != y` sempre produz o mesmo resultado que `!(x == y)`. No entanto, quando um ou ambos os operandos são NaN, a `<`, `>`, `<=`, e `>=` operadores não produzem os mesmos resultados que a negação lógica do operador oposto. Por exemplo, se qualquer uma de `x` e `y` for NaN, em seguida, `x < y` é `false`, mas `!(x >= y)` é `true`.
*  Quando nenhum dos operandos for NaN, os operadores comparam os valores dos dois operandos de ponto flutuantes em relação a ordenação

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   em que `min` e `max` são os menores e maiores finitos valores positivos que podem ser representados no formato de ponto flutuante fornecido. Efeitos notáveis essa ordenação são:
   * Zeros positivos e negativos são considerados iguais.
   * Um número infinito negativo é considerado menor do que todos os outros valores, mas igual a outro infinito negativo.
   * Um número infinito positivo é considerado maior do que todos os outros valores, mas igual ao outro infinito positivo.

### <a name="decimal-comparison-operators"></a>Operadores de comparação decimal

Os operadores de comparação de decimal predefinidos são:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Cada um desses operadores compara os valores numéricos dos dois operandos de decimais e retorna um `bool` valor que indica se a relação específica `true` ou `false`. Cada comparação decimal é equivalente a usar o operador de igualdade do tipo ou correspondente relacional `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operadores de igualdade booliana

Os operadores de igualdade booliana predefinidos são:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

O resultado de `==` está `true` se ambos `x` e `y` são `true` ou se ambos os `x` e `y` são `false`. Caso contrário, o resultado é `false`.

O resultado de `!=` está `false` se ambos `x` e `y` são `true` ou se ambos os `x` e `y` são `false`. Caso contrário, o resultado é `true`. Quando os operandos forem do tipo `bool`, o `!=` operador produz o mesmo resultado que o `^` operador.

### <a name="enumeration-comparison-operators"></a>Operadores de comparação de enumeração

Cada tipo de enumeração implicitamente fornece os seguintes operadores de comparação predefinidos:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

O resultado da avaliação `x op y`, onde `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`, e `op` é um dos operadores de comparação, é exatamente o mesmo que Avaliando `((U)x) op ((U)y)`. Em outras palavras, os operadores de comparação de tipo de enumeração simplesmente comparam os valores integrais subjacentes dos dois operandos.

### <a name="reference-type-equality-operators"></a>Operadores de igualdade de tipo de referência

Os operadores de igualdade do tipo de referência predefinidos são:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Os operadores retornam o resultado de comparar as duas referências para igualdade ou não igualdade.

Uma vez que os operadores de igualdade do tipo de referência predefinidos aceitam operandos do tipo `object`, eles se aplicam a todos os tipos que não declaram aplicáveis `operator ==` e `operator !=` membros. Por outro lado, qualquer operadores de igualdade aplicável definidos pelo usuário efetivamente ocultam os operadores de igualdade do tipo de referência predefinidos.

Os operadores de igualdade do tipo de referência predefinidos exigem um destes procedimentos:

*  Ambos os operandos forem um valor de um tipo conhecido por ser um *reference_type* ou o literal `null`. Além disso, uma conversão de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)) existe do tipo de ambos os operandos para o tipo de outro operando.
*  Um operando for um valor do tipo `T` onde `T` é um *type_parameter* e o outro operando é o literal `null`. Além disso `T` não tem a restrição de tipo de valor.

A menos que uma das seguintes condições forem verdadeiras, ocorrerá um erro de tempo de associação. Implicações importantes dessas regras são:

*  Ele é um erro de tempo de associação a usar os operadores de igualdade do tipo de referência predefinidos para comparar duas referências que são conhecidas por ser diferente em tempo de associação. Por exemplo, se os tipos de tempo de associação de operandos são dois tipos de classe `A` e `B`e caso nem `A` nem `B` deriva a outra, em seguida, seria impossível para os dois operandos referenciar o mesmo objeto. Portanto, a operação é considerada um erro de tempo de associação.
*  Os operadores de igualdade do tipo de referência predefinidos não permitem valor operandos do tipo a ser comparado. Portanto, a menos que um tipo de struct declara seus próprios operadores de igualdade, não é possível comparar os valores desse tipo de struct.
*  Os operadores de igualdade do tipo de referência predefinidos nunca causam operações de conversão boxing ocorra para seus operandos. Ele não teria sentido executar essas operações de conversão boxing, já que as referências às instâncias recém-alocada demarcadas necessariamente seriam diferente de todas as outras referências.
*  Se um operando de um tipo de parâmetro de tipo `T` é comparado ao `null`e o tipo de tempo de execução do `T` é um tipo de valor, o resultado da comparação é `false`.

O exemplo a seguir verifica se um argumento de um tipo de parâmetro de tipo sem restrição é `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

O `x == null` constructo é permitido, embora `T` pode representar um tipo de valor e o resultado é simplesmente definido para ser `false` quando `T` é um tipo de valor.

Para uma operação do formulário `x == y` ou `x != y`, se qualquer aplicável `operator ==` ou `operator !=` existir, a resolução de sobrecarga de operador ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) selecionará as regras que operador em vez do operador de igualdade de tipo de referência predefinidos. No entanto, sempre é possível selecionar o operador de igualdade do tipo de referência predefinidos convertendo explicitamente um ou ambos os operandos para o tipo `object`. O exemplo
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
produz a saída
```
True
False
False
False
```

O `s` e `t` variáveis se referirem a dois diferentes `string` instâncias que contém os mesmos caracteres. A primeira comparação gera `True` porque o operador de igualdade de cadeia de caracteres predefinida ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)) é selecionado quando ambos os operandos forem do tipo `string`. Todas as comparações restantes de saída `False` porque o operador de igualdade do tipo de referência predefinidos é selecionado quando um ou ambos os operandos forem do tipo `object`.

Observe que a técnica acima não é significativa para tipos de valor. O exemplo
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
gera `False` porque as conversões criem referências para duas instâncias separadas do box `int` valores.

### <a name="string-equality-operators"></a>Operadores de igualdade de cadeia de caracteres

Os operadores de igualdade de cadeia de caracteres predefinidas são:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dois `string` valores são considerados iguais quando uma das seguintes opções for verdadeira:

*  Os dois valores forem `null`.
*  Ambos os valores são nulos referências às instâncias de cadeia de caracteres que têm caracteres idênticos e comprimentos idênticos em cada posição do caractere.

Os operadores de igualdade de cadeia de caracteres comparam valores de cadeia de caracteres em vez de referências de cadeia de caracteres. Quando duas instâncias de cadeia de caracteres separada contêm a mesma sequência exata de caracteres, os valores das cadeias de caracteres são iguais, mas as referências são diferentes. Conforme descrito em [operadores de igualdade de tipo de referência](expressions.md#reference-type-equality-operators), os operadores de igualdade do tipo de referência podem ser usados para comparar as referências de cadeia de caracteres em vez de valores de cadeia de caracteres.

### <a name="delegate-equality-operators"></a>Operadores de igualdade de delegado

Cada tipo de delegado implicitamente fornece os seguintes operadores de comparação predefinidos:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Delegado duas instâncias são consideradas iguais da seguinte maneira:

*  Se qualquer uma das instâncias de delegado for `null`, eles são iguais se e somente se ambos forem `null`.
*  Se os delegados tem um tipo diferente de tempo de execução, eles nunca são iguais.
*  Se ambas as instâncias de delegado têm uma lista de invocação ([declarações de delegado](delegates.md#delegate-declarations)), essas instâncias são iguais se e somente se suas listas de invocação têm o mesmo comprimento, e cada entrada na lista de invocação de um é igual a (conforme definido abaixo) para a entrada correspondente, em ordem, na lista de invocação do outro.

As seguintes regras regem a igualdade de entradas da lista de invocação:

*  Se dois invocação entradas da lista os dois se referirem ao mesmo estático método e em seguida, as entradas são iguais.
*  Se dois invocação entradas da lista os dois se referirem ao mesmo método não estático no mesmo objeto de destino (conforme definido pelos operadores de igualdade de referência), em seguida, as entradas são iguais.
*  Entradas da lista de invocação produzido da avaliação de semanticamente idêntico *anonymous_method_expression*s ou *lambda_expression*s com o mesmo conjunto (possivelmente vazio) de variável externa capturada instâncias são permitidas (mas não obrigatórios) são iguais.

### <a name="equality-operators-and-null"></a>NULL e operadores de igualdade

O `==` e `!=` operadores permitem um operando para ser um valor de um tipo anulável e outra para ser o `null` literal, mesmo se não existe nenhum operador predefinido ou definidos pelo usuário (no unlifted ou retirados de formulário) para a operação.

Para uma operação de uma das formas
```csharp
x == null
null == x
x != null
null != x
```
em que `x` é uma expressão de um tipo que permite valor nulo, se a resolução de sobrecarga de operador ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) Falha ao localizar um operador aplicável, o resultado em vez disso, é computada a partir de `HasValue` propriedade de `x`. Especificamente, os primeiros dois formulários são convertidos em `!x.HasValue`, e os últimos dois formulários são convertidos em `x.HasValue`.

### <a name="the-is-operator"></a>O operador is

O `is` operador é usado para verificar dinamicamente se o tipo de tempo de execução de um objeto é compatível com um determinado tipo. O resultado da operação `E is T`, onde `E` é uma expressão e `T` é um tipo, é um booleano valor que indica se `E` pode ser convertido com êxito para o tipo `T` por uma conversão de referência, uma conversão boxing conversão, ou uma conversão unboxing. A operação é avaliada como a seguir, depois de argumentos de tipo tiverem sido substituídos para todos os parâmetros de tipo:

*  Se `E` é uma função anônima, ocorre um erro de tempo de compilação
*  Se `E` é um grupo de método ou o `null` literal, se o tipo de `E` é um tipo de referência ou um tipo anulável e o valor de `E` é nulo, o resultado é false.
*  Caso contrário, deixe `D` representam o tipo dinâmico de `E` da seguinte maneira:
   * Se o tipo de `E` é um tipo de referência `D` é o tipo de tempo de execução da referência de instância por `E`.
   * Se o tipo de `E` é um tipo anulável, `D` é o tipo subjacente desse tipo que permite valor nulo.
   * Se o tipo de `E` é um tipo de valor não anulável `D` é o tipo de `E`.
*  O resultado da operação depende `D` e `T` da seguinte maneira:
   * Se `T` é um tipo de referência, o resultado será true se `D` e `T` são do mesmo tipo, se `D` é um tipo de referência e uma conversão de referência implícita da `D` para `T` existe, ou se `D` é um tipo de valor e uma conversão de uma conversão boxing `D` para `T` existe.
   * Se `T` é um tipo anulável, o resultado será true se `D` é o tipo subjacente de `T`.
   * Se `T` é um tipo de valor não anulável, o resultado será true se `D` e `T` são do mesmo tipo.
   * Caso contrário, o resultado é false.

Observe que as conversões definidas pelo usuário, não são consideradas pelo `is` operador.

### <a name="the-as-operator"></a>O operador

O `as` operador é usado para converter explicitamente um valor para um tipo de referência fornecida ou tipo que permite valor nulo. Ao contrário de uma expressão de conversão ([expressões de conversão](expressions.md#cast-expressions)), o `as` operador nunca gera uma exceção. Em vez disso, se a conversão indicada não for possível, o valor resultante é `null`.

Em uma operação do formulário `E as T`, `E` deve ser uma expressão e `T` deve ser um tipo de referência, um parâmetro de tipo conhecido por ser um tipo de referência ou um tipo anulável. Além disso, pelo menos uma das seguintes opções deve ser verdadeira ou, caso contrário, ocorrerá um erro de tempo de compilação:

*  Uma identidade ([conversão de identidade](conversions.md#identity-conversion)), implícita que permitem valor nulo ([conversões implícitas de anuláveis](conversions.md#implicit-nullable-conversions)), a referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)), conversão boxing ([ Conversões boxing](conversions.md#boxing-conversions)), explícita que permite valor nulo ([conversões que permitem valor nulas explícitas](conversions.md#explicit-nullable-conversions)), referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)), ou conversão unboxing ([Conversões de conversão Unboxing](conversions.md#unboxing-conversions)) existe conversão de `E` para `T`.
*  O tipo de `E` ou `T` é um tipo aberto.
*  `E` é o `null` literal.

Se o tipo de tempo de compilação do `E` não é `dynamic`, a operação `E as T` produz o mesmo resultado que
```csharp
E is T ? (T)(E) : (T)null
```
exceto que `E` é avaliado apenas uma vez. O compilador pode ser esperado para otimizar `E as T` para executar no máximo uma verificação de tipo dinâmico em vez das duas verificações de tipo dinâmico implicado pela expansão acima.

Se o tipo de tempo de compilação do `E` é `dynamic`, diferentemente do operador cast as `as` operador não está vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Portanto, a expansão nesse caso é:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Observe que algumas conversões, como conversões definidas pelo usuário, não são possíveis com o `as` operador e, em vez disso, deve ser executada usando expressões de conversão.

No exemplo
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
o parâmetro de tipo `T` de `G` é conhecido por ser um tipo de referência, porque ele tem a restrição de classe. O parâmetro de tipo `U` dos `H` não está no entanto; portanto, o uso das `as` operador em `H` não é permitido.

## <a name="logical-operators"></a>Operadores lógicos

O `&`, `^`, e `|` operadores são chamados de operadores lógicos.

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

Se um operando de um operador lógico tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.

Para uma operação do formulário `x op y`, onde `op` é um dos operadores lógicos, resolução de sobrecarga ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.

Os operadores lógicos predefinidos são descritos nas seções a seguir.

### <a name="integer-logical-operators"></a>Operadores lógicos de inteiro

Os operadores lógicos de inteiro predefinidos são:
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

O `&` operador calcula o bit a bit lógicos `AND` dos dois operandos, o `|` operador calcula o bit a bit lógicos `OR` dos dois operandos e o `^` operador calcula o exclusivo lógico bit a bit `OR` dos dois operandos. Não há estouros são possíveis dessas operações.

### <a name="enumeration-logical-operators"></a>Operadores lógicos de enumeração

Cada tipo de enumeração `E` implicitamente oferece operadores lógicos de predefinidas a seguir:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

O resultado da avaliação `x op y`, onde `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`, e `op` é um dos operadores lógicos, é exatamente o mesmo que Avaliando `(E)((U)x op (U)y)`. Em outras palavras, os operadores lógicos de tipo de enumeração simplesmente executam a operação lógica no tipo subjacente dos dois operandos.

### <a name="boolean-logical-operators"></a>Operadores lógicos boolianos

Os operadores lógicos boolianos predefinidos são:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

O resultado de `x & y` está `true` se ambos `x` e `y` são `true`. Caso contrário, o resultado é `false`.

O resultado de `x | y` está `true` se qualquer um dos `x` ou `y` é `true`. Caso contrário, o resultado é `false`.

O resultado de `x ^ y` está `true` se `x` é `true` e `y` é `false`, ou `x` é `false` e `y` é `true`. Caso contrário, o resultado é `false`. Quando os operandos forem do tipo `bool`, o `^` operador calcula o mesmo resultado que o `!=` operador.

### <a name="nullable-boolean-logical-operators"></a>Operadores lógicos boolianos que permitem valor nulos

O tipo de booliano anulável `bool?` pode representar os três valores, `true`, `false`, e `null`e é conceitualmente semelhante ao tipo de três valores usado para expressões Boolianas em SQL. Para garantir que os resultados produzidos pelos `&` e `|` operadores para `bool?` operandos são consistentes com a lógica de três valores do SQL, os seguintes operadores predefinidos são fornecidos:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

A tabela a seguir lista os resultados produzidos por esses operadores para todas as combinações dos valores `true`, `false`, e `null`.

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a>Operadores lógicos condicionais

O `&&` e `||` operadores são chamados de operadores lógicos condicionais. Eles também são chamados de operadores lógicos "curto-circuito".

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

O `&&` e `||` operadores são versões condicionais da `&` e `|` operadores:

*  A operação `x && y` corresponde à operação `x & y`, exceto pelo fato `y` é avaliada apenas se `x` não é `false`.
*  A operação `x || y` corresponde à operação `x | y`, exceto pelo fato `y` é avaliada apenas se `x` não é `true`.

Se um operando de um operador lógico condicional tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.

Uma operação do formulário `x && y` ou `x || y` é processada por meio da aplicação de resolução de sobrecarga ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação foi escrita `x & y` ou `x | y`. Em seguida,

*  Se a resolução de sobrecarga não consegue localizar um único operador melhor, ou se a resolução de sobrecarga seleciona um dos operadores lógicos de inteiro predefinidos, ocorrerá um erro de tempo de associação.
*  Caso contrário, se o operador selecionado é um dos operadores lógicos predefinidos booleanos ([operadores lógicos boolianos](expressions.md#boolean-logical-operators)) ou os operadores lógicos boolianos anuláveis ([operadores lógicos de boolianos anuláveis](expressions.md#nullable-boolean-logical-operators)), o a operação é processada, conforme descrito em [Boolean operadores lógicos condicionais](expressions.md#boolean-conditional-logical-operators).
*  Caso contrário, o operador selecionado é um operador definido pelo usuário e a operação é processada, conforme descrito em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).

Não é possível sobrecarregar diretamente os operadores lógicos condicionais. No entanto, porque os operadores lógicos condicionais são avaliados em termos de operadores lógicos regulares, sobrecargas dos operadores lógicos regulares, com algumas restrições, também são consideradas sobrecargas dos operadores lógicos condicionais. Isso é descrito posteriormente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Boolianos operadores lógicos condicionais

Quando os operandos de `&&` ou `||` são do tipo `bool`, ou quando os operandos forem de tipos que não definem um aplicável `operator &` ou `operator |`, mas definir conversões implícitas para `bool`, a operação é processado da seguinte maneira:

*  A operação `x && y` é avaliada como `x ? y : false`. Em outras palavras, `x` é avaliado primeiro e convertido no tipo `bool`. Então, se `x` está `true`, `y` é avaliada e convertido no tipo `bool`, e isso se torna o resultado da operação. Caso contrário, o resultado da operação é `false`.
*  A operação `x || y` é avaliada como `x ? true : y`. Em outras palavras, `x` é avaliado primeiro e convertido no tipo `bool`. Então, se `x` está `true`, o resultado da operação é `true`. Caso contrário, `y` é avaliado e convertido no tipo `bool`, e isso se torna o resultado da operação.

### <a name="user-defined-conditional-logical-operators"></a>Operadores lógicos condicionais definidos pelo usuário

Quando os operandos de `&&` ou `||` são de tipos que declaram um aplicável definidos pelo usuário `operator &` ou `operator |`, ambos os seguintes procedimentos devem ser verdadeiras, onde `T` é o tipo no qual o operador selecionado é declarado:

*  O tipo de retorno e o tipo de cada parâmetro do operador selecionado devem ser `T`. Em outras palavras, o operador deve calcular o lógico `AND` ou a lógica `OR` de dois operandos do tipo `T`e deve retornar um resultado do tipo `T`.
*  `T` deve conter declarações de `operator true` e `operator false`.

Ocorrerá um erro de tempo de associação se esses requisitos não for atendida. Caso contrário, o `&&` ou `||` operação é calculada pela combinação de definido pelo usuário `operator true` ou `operator false` com o operador selecionado definido pelo usuário:

*  A operação `x && y` é avaliado como `T.false(x) ? x : T.&(x, y)`, onde `T.false(x)` é uma invocação dos `operator false` declarado no `T`, e `T.&(x, y)` é uma invocação do selecionado `operator &`. Em outras palavras, `x` é avaliado primeiro e `operator false` é invocado no resultado para determinar se `x` é, definitivamente, false. Então, se `x` é definitivamente false, o resultado da operação é o valor calculado anteriormente para `x`. Caso contrário, `y` é avaliada e selecionado `operator &` é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.
*  A operação `x || y` é avaliado como `T.true(x) ? x : T.|(x, y)`, onde `T.true(x)` é uma invocação dos `operator true` declarado no `T`, e `T.|(x,y)` é uma invocação do selecionado `operator|`. Em outras palavras, `x` é avaliado primeiro e `operator true` é invocado no resultado para determinar se `x` é definitivamente verdade. Então, se `x` é definitivamente verdade, o resultado da operação é o valor calculado anteriormente para `x`. Caso contrário, `y` é avaliada e selecionado `operator |` é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.

Em qualquer uma dessas operações, a expressão fornecida pelo `x` só é avaliada uma vez e a expressão fornecida pelo `y` não é avaliada ou avaliada apenas uma vez.

Para obter um exemplo de um tipo que implementa `operator true` e `operator false`, consulte [banco de dados tipo booliano](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>O operador de união nulo

O `??` operador é chamado de operador de união nulo.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Uma expressão de união nula do formulário `a ?? b` requer `a` para ser de um tipo que permite valor nulo de tipo ou referência. Se `a` não for nulo, o resultado da `a ?? b` é `a`; caso contrário, o resultado é `b`. A operação avalia `b` somente se `a` é nulo.

O operador de união nulo é associativo à direita, o que significa que as operações são agrupadas da direita para esquerda. Por exemplo, uma expressão do formulário `a ?? b ?? c` é avaliada como `a ?? (b ?? c)`. Termos em geral, uma expressão do formulário `E1 ?? E2 ?? ... ?? En` retorna o primeiro dos operandos for não nulo, ou nulo se todos os operandos são nulos.

O tipo da expressão `a ?? b` depende de quais conversões implícitas estão disponíveis nos operandos. Ordem de preferência, o tipo de `a ?? b` está `A0`, `A`, ou `B`, onde `A` é o tipo de `a` (desde que `a` tem um tipo), `B` é o tipo de `b` ( desde que `b` tem um tipo), e `A0` é o tipo subjacente de `A` se `A` é um tipo anulável, ou `A` caso contrário. Especificamente, `a ?? b` é processado da seguinte maneira:

*  Se `A` existe e não é um tipo anulável ou um tipo de referência, ocorre um erro de tempo de compilação.
*  Se `b` é uma expressão dinâmica, o tipo de resultado é `dynamic`. Em tempo de execução, `a` é avaliada primeiro. Se `a` não for nulo, `a` é convertido para dinâmico, e isso se torna o resultado. Caso contrário, `b` é avaliada, e isso se torna o resultado.
*  Caso contrário, se `A` existe e é um tipo anulável e existe uma conversão implícita da `b` à `A0`, o tipo de resultado é `A0`. Em tempo de execução, `a` é avaliada primeiro. Se `a` não for null, `a` é desencapsulado para o tipo `A0`, e isso se torna o resultado. Caso contrário, `b` é avaliado e convertido no tipo `A0`, e isso se torna o resultado.
*  Caso contrário, se `A` existe e existe uma conversão implícita da `b` à `A`, o tipo de resultado é `A`. Em tempo de execução, `a` é avaliada primeiro. Se `a` não for nulo, `a` se tornará o resultado. Caso contrário, `b` é avaliado e convertido no tipo `A`, e isso se torna o resultado.
*  Caso contrário, se `b` tem um tipo `B` e existe uma conversão implícita de `a` à `B`, o tipo de resultado é `B`. Em tempo de execução, `a` é avaliada primeiro. Se `a` não for null, `a` é desencapsulado para o tipo `A0` (se `A` existe e é anulável) e convertido para o tipo `B`, e isso se torna o resultado. Caso contrário, `b` é avaliada e torna-se o resultado.
*  Caso contrário, `a` e `b` são incompatíveis e um erro de tempo de compilação ocorre.

## <a name="conditional-operator"></a>Operador condicional

O `?:` operador é chamado de operador condicional. Ele às vezes também é chamado de operador ternário.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Uma expressão condicional do formulário `b ? x : y` primeiro avalia a condição `b`. Então, se `b` está `true`, `x` é avaliada e torna-se o resultado da operação. Caso contrário, `y` é avaliada e torna-se o resultado da operação. Uma expressão condicional é avaliada nunca ambos `x` e `y`.

O operador condicional é associativo à direita, o que significa que as operações são agrupadas da direita para esquerda. Por exemplo, uma expressão do formulário `a ? b : c ? d : e` é avaliada como `a ? b : (c ? d : e)`.

O primeiro operando do `?:` operador deve ser uma expressão que pode ser convertida implicitamente em `bool`, ou uma expressão de um tipo que implementa `operator true`. Se nenhum desses requisitos é atendido, ocorrerá um erro de tempo de compilação.

O segundo e terceiro operandos, `x` e `y`, da `?:` operador controlam o tipo da expressão condicional.

*  Se `x` tem o tipo `X` e `y` tem o tipo `Y` , em seguida,
   * Se uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe a partir de `X` para `Y`, mas não de `Y` para `X`, em seguida, `Y` é o tipo da expressão condicional.
   * Se uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe a partir de `Y` para `X`, mas não de `X` para `Y`, em seguida, `X` é o tipo da expressão condicional.
   * Caso contrário, nenhum tipo de expressão pode ser determinado, e ocorre um erro de tempo de compilação.
*  Se apenas um dos `x` e `y` tem um tipo e ambos `x` e `y`, de são implicitamente conversíveis para esse tipo, em seguida, o que é o tipo da expressão condicional.
*  Caso contrário, nenhum tipo de expressão pode ser determinado, e ocorre um erro de tempo de compilação.

O processamento de tempo de execução de uma expressão condicional do formulário `b ? x : y` consiste as seguintes etapas:

*  Primeiro, `b` é avaliada e o `bool` valor `b` é determinado:
   * Se uma conversão implícita do tipo de `b` à `bool` existir, essa conversão implícita é executada para produzir um `bool` valor.
   * Caso contrário, o `operator true` definidos pelo tipo de `b` é invocado para produzir um `bool` valor.
*  Se o `bool` valor produzido pela etapa anterior está `true`, em seguida, `x` é avaliada e convertida no tipo de expressão condicional, e isso se torna o resultado da expressão condicional.
*  Caso contrário, `y` é avaliada e convertida no tipo de expressão condicional, e isso se torna o resultado da expressão condicional.

## <a name="anonymous-function-expressions"></a>Expressões de função anônima

Uma ***função anônima*** é uma expressão que representa uma definição de método "na linha". Uma função anônima não tem um valor ou tipo por si só, mas pode ser convertido em um tipo de árvore de expressão ou delegado compatível. A avaliação de uma conversão de função anônima depende do tipo de destino da conversão: se for um tipo de delegado, a conversão é avaliada como um valor de delegado fizer referência ao método que define a função anônima. Se for um tipo de árvore de expressão, a conversão é avaliada como uma árvore de expressão que representa a estrutura do método como uma estrutura de objeto.

Por razões históricas existem dois tipos sintáticos de funções anônimas, ou seja, *lambda_expression*s e *anonymous_method_expression*s. Para fins de quase todos os *lambda_expression*s são mais concisas e expressivo que *anonymous_method_expression*s, que permanecem no idioma para compatibilidade com versões anteriores.

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

O `=>` operador tem a mesma precedência que a atribuição (`=`) e é associativo à direita.

Uma função anônima com o `async` modificador é uma função assíncrona e segue as regras descritas em [iteradores](classes.md#iterators).

Os parâmetros de uma função anônima na forma de um *lambda_expression* podem ser explicitamente ou implicitamente digitados. Em uma lista de parâmetros de tipo explícito, o tipo de cada parâmetro é explicitamente declarado. Em uma lista de parâmetros de tipo implícito, os tipos dos parâmetros são inferidos do contexto no qual ocorre a função anônima — especificamente, quando a função anônima é convertida em um tipo de delegado compatível ou tipo de árvore de expressão, que fornece o tipo os tipos de parâmetro ([conversões de função anônima](conversions.md#anonymous-function-conversions)).

Em uma função anônima com um parâmetro de tipo implícito, único, os parênteses que podem ser omitidos da lista de parâmetros. Em outras palavras, uma função anônima do formulário
```csharp
( param ) => expr
```
pode ser abreviado como
```csharp
param => expr
```

Lista de parâmetros de uma função anônima na forma de um *anonymous_method_expression* é opcional. Se for especificado, os parâmetros devem ser digitados explicitamente. Se não, a função anônima é convertida em um delegado com qualquer parâmetro de lista não contêm `out` parâmetros.

Um *bloco* corpo de uma função anônima é acessível ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)), a menos que a função anônima ocorre dentro de uma instrução inacessível.

Seguem alguns exemplos de funções anônimas abaixo:

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

O comportamento de *lambda_expression*s e *anonymous_method_expression*s é o mesmo, exceto para os seguintes pontos:

*  *anonymous_method_expression*s permitir que a lista de parâmetros a serem omitidos totalmente, produzindo a convertibilidade de tipos de qualquer lista de parâmetros com valor de delegado.
*  *lambda_expression*s permite que os tipos de parâmetro seja omitido e inferido, enquanto *anonymous_method_expression*s exigem tipos de parâmetro a ser declarado explicitamente.
*  O corpo de uma *lambda_expression* pode ser uma expressão ou um bloco de instruções enquanto o corpo de uma *anonymous_method_expression* deve ser um bloco de instruções.
*  Somente *lambda_expression*s têm conversões para tipos de árvore de expressão compatível ([tipos de árvore de expressão](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Assinaturas de função anônima

Opcional *anonymous_function_signature* de uma função anônima define os nomes e, opcionalmente, os tipos dos parâmetros formais para a função anônima. O escopo dos parâmetros de função anônima que é o *anonymous_function_body*. ([Escopos](basic-concepts.md#scopes)) junto com a lista de parâmetros (se fornecida) anônima-corpo do método-constitui um espaço de declaração ([declarações](basic-concepts.md#declarations)). Ele é, portanto, um erro de tempo de compilação para o nome de um parâmetro da função anônima para corresponder ao nome de uma variável local, a constante local ou parâmetro cujo escopo inclui o *anonymous_method_expression* ou *lambda_ expressão*.

Se uma função anônima tem uma *explicit_anonymous_function_signature*, em seguida, o conjunto de tipos de delegado compatível e tipos de árvore de expressão é restrito para aqueles que têm os mesmos tipos de parâmetro e modificadores na mesma ordem. Em contraste com conversões de grupo de método ([conversões de grupo de método](conversions.md#method-group-conversions)), não há suporte para a contravariância dos tipos de parâmetro de função anônima. Se uma função anônima não tem um *anonymous_function_signature*, em seguida, o conjunto de tipos de delegado compatível e tipos de árvore de expressão é restrito para aqueles que não têm nenhum `out` parâmetros.

Observe que um *anonymous_function_signature* não pode incluir atributos ou uma matriz de parâmetros. No entanto, uma *anonymous_function_signature* podem ser compatíveis com um tipo de delegado cuja lista de parâmetros contém uma matriz de parâmetros.

Observe também que conversão para um tipo de árvore de expressão, mesmo se compatível, ainda pode falhar em tempo de compilação ([tipos de árvore de expressão](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Corpos de função anônima

O corpo (*expressão* ou *bloco*) de uma função anônima é sujeito às seguintes regras:

*  Se a função anônima inclui uma assinatura, os parâmetros especificados na assinatura estão disponíveis no corpo. Se a função anônima não tiver uma assinatura pode ser convertido para um tipo de delegado ou ter parâmetros de tipo de expressão ([conversões de função anônima](conversions.md#anonymous-function-conversions)), mas os parâmetros não podem ser acessados no corpo.
*  Exceto para `ref` ou `out` parâmetros especificados na assinatura (se houver) do delimitador mais próximo função anônima, ele é um erro de tempo de compilação para o corpo acessar um `ref` ou `out` parâmetro.
*  Quando o tipo de `this` é um tipo de struct, ele é um erro de tempo de compilação para o corpo acessar `this`. Isso é verdadeiro se o acesso é explícito (como em `this.x`) ou implícita (como na `x` onde `x` é um membro de instância do struct). Essa regra simplesmente proíbe tal acesso e não afeta se a pesquisa de membro resulta em um membro do struct.
*  O corpo da tem acesso a variáveis externas ([Outer variáveis](expressions.md#outer-variables)) da função anônima. Acesso de uma variável externa fará referência a instância da variável que está ativo no momento a *lambda_expression* ou *anonymous_method_expression* é avaliada ([avaliação do expressões de função anônima](expressions.md#evaluation-of-anonymous-function-expressions)).
*  É um erro de tempo de compilação para o corpo conter um `goto` instrução `break` instrução, ou `continue` cujo destino está fora do corpo ou dentro do corpo de uma função anônima contida.
*  Um `return` no corpo da declaração de uma invocação de delimitadora mais próxima função anônima, não do membro da função de circunscrição. Uma expressão especificada em uma `return` instrução deve ser implicitamente conversível para o tipo de retorno do tipo de delegado ou tipo de árvore de expressão para o qual delimitadora mais próxima *lambda_expression* ou *anonymous_ method_expression* é convertido ([conversões de função anônima](conversions.md#anonymous-function-conversions)).

Ele não é especificado explicitamente se há alguma maneira de executar o bloco de uma função anônima diferente por meio de avaliação e invocação do *lambda_expression* ou *anonymous_method_expression*. Em particular, o compilador pode optar por implementar uma função anônima por sintetizar um ou mais nomeadas tipos ou métodos. Os nomes de todos esses elementos sintetizados devem ser de um formulário reservado para uso pelo compilador.

### <a name="overload-resolution-and-anonymous-functions"></a>Resolução de sobrecarga e funções anônimas

Funções anônimas em uma lista de argumentos participarem de inferência de tipo e resolução de sobrecarga. Consulte a [inferência](expressions.md#type-inference) e [resolução de sobrecarga](expressions.md#overload-resolution) para as regras exatas.

O exemplo a seguir ilustra o efeito de funções anônimas em resolução de sobrecarga.

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

O `ItemList<T>` classe tem duas `Sum` métodos. Cada um leva um `selector` argumento, que extrai o valor a soma ao longo de um item de lista. O valor extraído pode ser um `int` ou um `double` e a soma resultante é da mesma forma uma `int` ou um `double`.

O `Sum` métodos, por exemplo poderiam ser usados para calcular somas em uma lista de linhas de detalhes em uma ordem.

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

Na primeira invocação de `orderDetails.Sum`, ambos `Sum` métodos são aplicáveis porque a função anônima `d => d. UnitCount` é compatível com ambos `Func<Detail,int>` e `Func<Detail,double>`. No entanto, a resolução de sobrecarga seleciona a primeira `Sum` método porque a conversão para o `Func<Detail,int>` é melhor do que a conversão em `Func<Detail,double>`.

Na segunda chamada de `orderDetails.Sum`, apenas o segundo `Sum` método é aplicável porque a função anônima `d => d.UnitPrice * d.UnitCount` produz um valor do tipo `double`. Portanto, sobrecarregar resolução escolhe o segundo `Sum` método para essa invocação.

### <a name="anonymous-functions-and-dynamic-binding"></a>Funções anônimas e vinculação dinâmica

Uma função anônima não pode ser um receptor, o argumento ou operando de uma operação dinâmica associada.

### <a name="outer-variables"></a>Variáveis externas

Qualquer variável local, um parâmetro de valor ou uma matriz de parâmetros cujo escopo inclui o *lambda_expression* ou *anonymous_method_expression* é chamado um ***variável externa*** da função anônima. Em um membro da função de instância de uma classe, o `this` valor é considerado um parâmetro de valor e é uma variável externa de qualquer função anônima contida dentro do membro da função.

#### <a name="captured-outer-variables"></a>Capturado variáveis externas

Quando uma variável externa é referenciada por uma função anônima, a variável externa deve ter sido ***capturados*** pela função anônima. Normalmente, o tempo de vida de uma variável local é limitado à execução de instrução ao qual ele está associado ou bloco ([variáveis locais](variables.md#local-variables)). No entanto, o tempo de vida de uma variável externa capturada é estendido pelo menos até que o delegado ou árvore de expressão criada a partir da função anônima se qualifique para coleta de lixo.

No exemplo
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
a variável local `x` for capturada, a função anônima e a vida útil do `x` é estendido pelo menos até que o delegado retornado `F` se qualifique para coleta de lixo (que não acontece até o final do o programa). Uma vez que cada invocação de função anônima que opera na mesma instância de `x`, a saída do exemplo é:
```
1
2
3
```

Quando uma variável local ou um parâmetro de valor é capturado por uma função anônima, o parâmetro ou variável local não é mais considerado para ser uma variável fixa ([fixo e variáveis moveable](unsafe-code.md#fixed-and-moveable-variables)), mas em vez disso, é considerado como um moveable variável. Assim, qualquer `unsafe` código que usa o endereço de uma variável externa capturada primeiro deve usar o `fixed` instrução para corrigir a variável.

Observe que, ao contrário de uma variável uncaptured, uma variável de local capturada pode ser exposta simultaneamente para vários threads de execução.

#### <a name="instantiation-of-local-variables"></a>Instanciação de variáveis locais

Uma variável local é considerada ***instanciado*** quando execução entra no escopo da variável. Por exemplo, quando o método a seguir é invocado, a variável local `x` é instanciada e inicializado três vezes — uma vez para cada iteração do loop.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

No entanto, mover a declaração de `x` fora os resultados de loop em uma única instanciação de `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Quando não capturada, não há nenhuma maneira de observar exatamente a frequência com que uma variável local é instanciada — como os tempos de vida as instanciações são não contíguos, é possível para cada instanciação simplesmente usar o mesmo local de armazenamento. No entanto, quando uma função anônima captura uma variável local, os efeitos de instanciação se tornam aparentes.

O exemplo
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
produz a saída:
```
1
3
5
```

No entanto, quando a declaração de `x` é movido para fora do loop:
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
A saída é:
```
5
5
5
```

Se um loop for declarar uma variável de iteração, essa variável em si é considerado declarado fora do loop. Portanto, se o exemplo é modificado para capturar a variável de iteração:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
apenas uma instância de variável de iteração é capturada, que produz a saída:
```
3
3
3
```

É possível que os delegados de função anônima para compartilhar algumas variáveis capturadas ainda tem instâncias separadas de outras pessoas. Por exemplo, se `F` é alterado para
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
os três delegados capturam a mesma instância do `x` mas instâncias separadas do `y`, e a saída é:
```
1 1
2 1
3 1
```

Funções anônimas separadas podem capturar a mesma instância de uma variável externa. No exemplo:
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
as duas funções anônimas capturam a mesma instância da variável local `x`, e eles podem, portanto, "se comunicar" por meio dessa variável. A saída do exemplo é:
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Avaliação de expressões de função anônima

Uma função anônima `F` sempre deve ser convertido em um tipo de delegado `D` ou um tipo de árvore de expressão `E`, diretamente ou por meio da execução de uma expressão de criação de delegado `new D(F)`. Essa conversão determina o resultado da função anônima, conforme descrito na [conversões de função anônima](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Expressões de consulta

***Expressões de consulta*** fornecem uma sintaxe de linguagem integrada para consultas que é semelhante a linguagens de consulta relacionais e hierárquicas, como SQL e XQuery.

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

Uma expressão de consulta começa com um `from` cláusula e termina com um uma `select` ou `group` cláusula. Inicial `from` cláusula pode ser seguida por zero ou mais `from`, `let`, `where`, `join` ou `orderby` cláusulas. Cada `from` cláusula é um gerador de introduzir uma ***variável de intervalo*** que abrange os elementos de uma ***sequência***. Cada `let` cláusula introduz uma variável de intervalo que representa um valor calculado por meio de variáveis de intervalo anterior. Cada `where` cláusula é um filtro que exclui itens de resultado. Cada `join` cláusula compara as chaves especificadas da sequência de origem com chaves de outra sequência, resultando em pares correspondentes. Cada `orderby` cláusula reordena os itens de acordo com os critérios especificados. O último `select` ou `group` cláusula Especifica a forma do resultado em termos de variáveis de intervalo. Por fim, um `into` cláusula pode ser usada para "unir" consultas, tratando os resultados de uma consulta como um gerador em uma consulta subsequente.

### <a name="ambiguities-in-query-expressions"></a>Ambiguidades em expressões de consulta

Expressões de consulta contêm um número de "palavras-chave contextuais", ou seja, os identificadores que têm significado especial em um determinado contexto. Especificamente, esses são `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` e `by`. Para evitar ambiguidades em expressões de consulta causadas pelo uso misto desses identificadores como nomes simples ou palavras-chave, esses identificadores são considerados palavras-chave quando que ocorrem em qualquer lugar dentro de uma expressão de consulta.

Para essa finalidade, uma expressão de consulta é qualquer expressão que começa com "`from identifier`"seguido por qualquer token exceto"`;`","`=`"ou"`,`".

Para usar essas palavras como identificadores em uma expressão de consulta, elas podem ser prefixadas com "`@`" ([identificadores](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Conversão de expressão de consulta

A linguagem c# não especifica a semântica de execução de expressões de consulta. Em vez disso, as expressões de consulta são traduzidas para invocações de métodos que seguem a *padrão de expressão de consulta* ([o padrão de expressão de consulta](expressions.md#the-query-expression-pattern)). Especificamente, as expressões de consulta são convertidas em chamadas de métodos chamados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, e `Cast`. Esses métodos devem ter assinaturas específicas e tipos de resultado, conforme descrito em [o padrão de expressão de consulta](expressions.md#the-query-expression-pattern). Esses métodos podem ser métodos de instância do objeto que está sendo consultada ou métodos de extensão que são externos ao objeto, e elas implementam a execução real da consulta.

A tradução de expressões de consulta para invocações de método é um mapeamento sintático que ocorre antes de qualquer tipo de associação ou resolução de sobrecarga foi executada. A tradução é garantida para ser sintaticamente correto, mas não é garantido para produzir o código c# semanticamente correto. Após a conversão de expressões de consulta, invocações de método resultantes são processadas como invocações de método normal, e isso por sua vez pode revelar erros, por exemplo, se os métodos não existem, se os argumentos têm tipos errados ou se os métodos são genéricos e Falha de inferência de tipo.

Uma expressão de consulta é processada aplicando as seguintes traduções repetidamente até que nenhum reduções ainda maiores são possíveis. As traduções estão listadas na ordem do aplicativo: cada seção pressupõe que as traduções nas seções anteriores foram executadas exaustivamente e depois que esgotar, uma seção será não mais tarde revista no processamento da mesma expressão de consulta.

Atribuição a variáveis de intervalo não é permitida em expressões de consulta. No entanto, uma implementação c# é permitida para nem sempre impõe esta restrição, uma vez que isso pode às vezes, não ser possível com o esquema de tradução sintática apresentado aqui.

Determinadas conversões inserir variáveis de intervalo com identificadores transparentes indicados por `*`. As propriedades especiais de transparentes identificadores são discutidas mais detalhadamente em [transparentes identificadores](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Cláusulas SELECT e groupby com continuações

Uma expressão de consulta com uma continuação
```csharp
from ... into x ...
```
é convertido em
```csharp
from x in ( from ... ) ...
```

As traduções nas seções a seguir supõem que não têm consultas `into` continuações.

O exemplo
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
é convertido em
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
a tradução final que é
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Tipos de variável de intervalo explícito

Um `from` cláusula que especifica explicitamente um tipo de variável de intervalo
```csharp
from T x in e
```
é convertido em
```csharp
from x in ( e ) . Cast < T > ( )
```

Um `join` cláusula que especifica explicitamente um tipo de variável de intervalo
```
join T x in e on k1 equals k2
```
é convertido em
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

As traduções nas seções a seguir pressupõem que o consultas não tem nenhum tipo de variável de intervalo explícito.

O exemplo
```csharp
from Customer c in customers
where c.City == "London"
select c
```
é convertido em
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
a tradução final que é
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Tipos de variável de intervalo explícitas são úteis para consultar coleções que implementam não genéricas `IEnumerable` interface, mas não genérica `IEnumerable<T>` interface. No exemplo acima, isso seria o caso se `customers` eram do tipo `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Expressões de consulta de degeneração

Uma expressão de consulta do formulário
```csharp
from x in e select x
```
é convertido em
```csharp
( e ) . Select ( x => x )
```

O exemplo
```csharp
from c in customers
select c
```
é convertido em
```csharp
customers.Select(c => c)
```

Uma expressão de consulta degenerado é aquele que trivialmente seleciona os elementos da fonte. Uma fase posterior da tradução remove degeneradas consultas introduzidas por outras etapas de tradução, substituindo-os à sua origem. É importante no entanto, para garantir que o resultado de uma consulta de expressão nunca é o objeto de origem em si, como o que poderia revelar o tipo e a identidade da origem para o cliente da consulta. Nesta etapa, portanto, protege os degenerado consultas gravadas diretamente no código-fonte chamando explicitamente `Select` na origem. Ele cabe, em seguida, os implementadores de `Select` e outros operadores de consulta para garantir que esses métodos nunca retornam o objeto de origem.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Do, where, orderby e junção cláusulas let

Uma expressão de consulta com uma segunda `from` cláusula seguido por um `select` cláusula
```csharp
from x1 in e1
from x2 in e2
select v
```
é convertido em
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Uma expressão de consulta com uma segunda `from` cláusula seguido por algo diferente de um `select` cláusula:

```csharp
from x1 in e1
from x2 in e2
...
```
é convertido em
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Uma expressão de consulta com um `let` cláusula
```csharp
from x in e
let y = f
...
```
é convertido em
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Uma expressão de consulta com um `where` cláusula
```csharp
from x in e
where f
...
```
é convertido em
```csharp
from x in ( e ) . Where ( x => f )
...
```

Uma expressão de consulta com um `join` cláusula sem uma `into` seguido por um `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
é convertido em
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Uma expressão de consulta com um `join` cláusula sem uma `into` seguido por algo diferente de um `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
é convertido em
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Uma expressão de consulta com um `join` cláusula com um `into` seguido por um `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
é convertido em
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Uma expressão de consulta com um `join` cláusula com um `into` seguido por algo diferente de um `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
é convertido em
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Uma expressão de consulta com um `orderby` cláusula
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
é convertido em
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Se uma ordenação cláusula Especifica um `descending` indicador de direção, uma invocação de `OrderByDescending` ou `ThenByDescending` é produzido em vez disso.

As traduções a seguir supõem que não há nenhum `let`, `where`, `join` ou `orderby` cláusulas e não mais do que aquele inicial `from` cláusula em cada expressão de consulta.

O exemplo
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
é convertido em
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

O exemplo
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
é convertido em
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
a tradução final que é
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
onde `x` é um identificador gerado pelo compilador que de outra forma inacessíveis e invisível.

O exemplo
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
é convertido em
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
a tradução final que é
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
onde `x` é um identificador gerado pelo compilador que de outra forma inacessíveis e invisível.

O exemplo
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
é convertido em
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

O exemplo
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
é convertido em
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
a tradução final que é
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
em que `x` e `y` são identificadores gerado pelo compilador que estariam invisíveis e inacessíveis.

O exemplo
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
tem a tradução final
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Cláusulas SELECT

Uma expressão de consulta do formulário
```csharp
from x in e select v
```
é convertido em
```csharp
( e ) . Select ( x => v )
```
exceto quando v é o identificador de x, a tradução é simplesmente
```csharp
( e )
```

Por exemplo
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
simplesmente é traduzido em
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Cláusulas de GroupBy

Uma expressão de consulta do formulário
```csharp
from x in e group v by k
```
é convertido em
```csharp
( e ) . GroupBy ( x => k , x => v )
```
exceto quando v é o identificador de x, a tradução é
```csharp
( e ) . GroupBy ( x => k )
```

O exemplo
```csharp
from c in customers
group c.Name by c.Country
```
é convertido em
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Identificadores transparentes

Determinadas conversões de injetar as variáveis de intervalo com ***identificadores transparentes*** indicada por `*`. Identificadores transparentes não são um recurso de idioma apropriados; elas existem somente como uma etapa intermediária no processo de conversão de expressão de consulta.

Quando uma conversão de consulta injeta um identificador transparente, ainda mais etapas de conversão propagam o identificador transparente para funções anônimas e inicializadores de objeto anônimos. Nesses contextos, transparentes identificadores têm o seguinte comportamento:

*  Quando ocorre um identificador transparente como um parâmetro em uma função anônima, os membros do tipo anônimo associado são automaticamente no escopo no corpo da função anônima.
*  Quando um membro com um identificador transparente está no escopo, os membros desse membro estão no escopo também.
*  Quando ocorre um identificador transparente como um Declarador de membro de um inicializador de objeto anônimo, ele introduz um membro com um identificador transparente.
*  As etapas de conversão descritas acima, sempre são introduzidos identificadores transparentes junto com os tipos anônimos, com a intenção de capturar várias variáveis de intervalo como membros de um único objeto. Uma implementação da linguagem c# tem permissão para usar um mecanismo diferente de tipos anônimos para agrupar diversas variáveis de intervalo. Os exemplos de conversão a seguir pressupõem que tipos anônimos são usados e mostram como transparentes identificadores podem ser convertidos imediatamente.

O exemplo
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
é convertido em
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

que é ainda mais convertidas em
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
que, quando são apagados identificadores transparentes, é equivalente a
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
onde `x` é um identificador gerado pelo compilador que de outra forma inacessíveis e invisível.

O exemplo
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
é convertido em
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
que é ainda mais reduzida para
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
a tradução final que é
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
em que `x`, `y`, e `z` são identificadores gerado pelo compilador que estariam invisíveis e inacessíveis.

### <a name="the-query-expression-pattern"></a>O padrão de expressão de consulta

O ***padrão de expressão de consulta*** estabelece um padrão de métodos de tipos podem implementar para dar suporte a expressões de consulta. Como as expressões de consulta são traduzidas para invocações de método por meio de um mapeamento sintático, tipos têm flexibilidade considerável no modo como implementam o padrão de expressão de consulta. Por exemplo, os métodos do padrão podem ser implementados como métodos de instância ou como métodos de extensão porque os dois têm a mesma sintaxe de invocação e os métodos podem solicitar delegados ou árvores de expressão porque funções anônimas podem ser convertidas para ambos.

A forma recomendada de um tipo genérico `C<T>` que dá suporte ao padrão da expressão de consulta é mostrado abaixo. Um tipo genérico é usado para ilustrar as relações corretas entre tipos de parâmetro e resultado, mas é possível implementar o padrão para tipos não genéricos também.

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

Os métodos acima usam os tipos de delegado genérico `Func<T1,R>` e `Func<T1,T2,R>`, mas pode igualmente bem ter usados outros tipos de árvore de delegado ou expressão com as mesmas relações nos tipos de parâmetro e resultado.

Observe a relação recomendada entre `C<T>` e `O<T>` que garante que o `ThenBy` e `ThenByDescending` métodos estão disponíveis somente no resultado de um `OrderBy` ou `OrderByDescending`. Observe também a forma recomendada de resultado de `GroupBy` – uma sequência de sequências, onde cada sequência interna tem mais `Key` propriedade.

O `System.Linq` namespace fornece uma implementação do padrão de operador de consulta para qualquer tipo que implementa o `System.Collections.Generic.IEnumerable<T>` interface.

## <a name="assignment-operators"></a>Operadores de atribuição

Os operadores de atribuição atribui um novo valor para uma variável, uma propriedade, um evento ou um elemento do indexador.

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

O operando esquerdo de uma atribuição deve ser uma expressão classificada como uma variável, um acesso de propriedade, um acesso de indexador ou um acesso de evento.

O `=` é chamado de operador de ***operador de atribuição simples***. Ele atribui o valor do operando à direita para o elemento de variável, propriedade ou indexador fornecido pelo operando à esquerda. O operando esquerdo do operador de atribuição simples pode não ser um acesso de evento (exceto conforme descrito em [eventos semelhantes a campo](classes.md#field-like-events)). O operador de atribuição simples é descrito em [atribuição simples](expressions.md#simple-assignment).

Os operadores de atribuição que o `=` operador são chamados de ***operadores de atribuição composta***. Esses operadores de executam a operação indicada em dois operandos e, em seguida, atribua o valor resultante para o elemento de variável, propriedade ou indexador fornecido pelo operando à esquerda. Os operadores de atribuição composta são descritos em [atribuição composta](expressions.md#compound-assignment).

O `+=` e `-=` são chamados de operadores com uma expressão de acesso de evento como o operando esquerdo a *operadores de atribuição de evento*. Nenhum outro operador de atribuição é válido com acesso de um evento que o operando esquerdo. Os operadores de atribuição de evento são descritos em [atribuição de evento](expressions.md#event-assignment).

Os operadores de atribuição são associativos à direita, o que significa que as operações são agrupadas da direita para esquerda. Por exemplo, uma expressão do formulário `a = b = c` é avaliada como `a = (b = c)`.

### <a name="simple-assignment"></a>Atribuição simples

O `=` operador é chamado de operador de atribuição simples.

Se o operando esquerdo de uma atribuição simples tem o formato `E.P` ou `E[Ei]` onde `E` tem o tipo de tempo de compilação `dynamic`, em seguida, a atribuição é associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão de atribuição `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução com base no tipo de tempo de execução do `E`.

Em uma atribuição simples, o operando direito deve ser uma expressão que é implicitamente conversível para o tipo do operando esquerdo. A operação atribui o valor do operando à direita para o elemento de variável, propriedade ou indexador fornecido pelo operando à esquerda.

O resultado de uma expressão de atribuição simples é o valor atribuído ao operando esquerdo. O resultado tem o mesmo tipo que o operando esquerdo e sempre é classificado como um valor.

Se o operando esquerdo é um acesso de propriedade ou indexador, propriedade ou indexador deve ter um `set` acessador. Se isso não for o caso, ocorre um erro em tempo de vinculação.

O processamento de tempo de execução de uma atribuição simples do formulário `x = y` consiste as seguintes etapas:

*  Se `x` é classificado como uma variável:
   * `x` é avaliado para produzir a variável.
   * `y` é avaliado e, se necessário, convertido no tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).
   * Se a variável fornecida pelo `x` é um elemento de matriz de um *reference_type*, é realizada uma verificação de tempo de execução para garantir que o valor calculado para `y` é compatível com a instância de matriz da qual `x` é um elemento. A verificação for bem-sucedida se `y` está `null`, ou se uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe do tipo real da instância referenciada por `y` ao tipo de elemento real de que a instância de matriz que contém `x`. Caso contrário, uma `System.ArrayTypeMismatchException` será gerada.
   * O valor resultante da avaliação e a conversão de `y` é armazenado no local fornecido pela avaliação de `x`.
*  Se `x` é classificado como um acesso de propriedade ou indexador:
   * A expressão de instância (se `x` não é `static`) e a lista de argumentos (se `x` é um indexador de acesso) associadas com `x` são avaliados, e os resultados são usados em subsequente `set` invocação do acessador.
   * `y` é avaliado e, se necessário, convertido no tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).
   * O `set` acessador da `x` é invocado com o valor calculado para `y` como seu `value` argumento.

As regras de variação conjunta de matriz ([covariância de matriz](arrays.md#array-covariance)) permite um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que existe uma conversão de referência implícita de `B` para `A`. Devido a essas regras, a atribuição a um elemento de matriz de um *reference_type* requer uma verificação de tempo de execução para garantir que o valor que está sendo atribuído é compatível com a instância de matriz. No exemplo
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
faz com que a última atribuição de um `System.ArrayTypeMismatchException` ser lançada porque uma instância do `ArrayList` não podem ser armazenados em um elemento de um `string[]`.

Quando uma propriedade ou indexador declarado em uma *struct_type* é o destino de uma atribuição, a expressão de instância associado à propriedade ou o acesso ao indexador deve ser classificado como uma variável. Se a expressão de instância é classificada como um valor, ocorrerá um erro de tempo de associação. Por causa da [acesso de membro](expressions.md#member-access), a mesma regra também se aplica aos campos.

Dadas as declarações:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
No exemplo
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
as atribuições para `p.X`, `p.Y`, `r.A`, e `r.B` são permitidas porque `p` e `r` são variáveis. No entanto, no exemplo
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
as atribuições são todos inválidas, desde `r.A` e `r.B` não são variáveis.

### <a name="compound-assignment"></a>Atribuição composta

Se o operando esquerdo de uma atribuição composta tem o formato `E.P` ou `E[Ei]` onde `E` tem o tipo de tempo de compilação `dynamic`, em seguida, a atribuição é associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)). Nesse caso é o tipo de tempo de compilação da expressão de atribuição `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução com base no tipo de tempo de execução do `E`.

Uma operação do formulário `x op= y` é processado por meio da aplicação de resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação foi escrita `x op y`. Em seguida,

*  Se o tipo de retorno do operador selecionado é implicitamente conversível para o tipo de `x`, a operação é avaliada como `x = x op y`, exceto que `x` é avaliada apenas uma vez.
*  Caso contrário, se o operador selecionado é um operador predefinido, se o tipo de retorno do operador selecionado é explicitamente convertido no tipo de `x`e se `y` é implicitamente conversível para o tipo de `x` ou o operador é um operador, de deslocamento, em seguida, a operação é avaliada como `x = (T)(x op y)`, onde `T` é o tipo de `x`, exceto que `x` é avaliada apenas uma vez.
*  Caso contrário, a atribuição composta é inválida e ocorre um erro em tempo de vinculação.

O termo "avaliado apenas uma vez" significa que, na avaliação de `x op y`, os resultados de todas as expressões de constituintes `x` são temporariamente salvos e reutilizados, em seguida, ao executar a atribuição ao `x`. Por exemplo, na atribuição `A()[B()] += C()`, onde `A` é um método que retorna `int[]`, e `B` e `C` são métodos que retornam `int`, os métodos são chamados apenas uma vez, na ordem `A`, `B`, `C`.

Quando o operando esquerdo de uma atribuição composta é um acesso de propriedade ou o acesso do indexador, propriedade ou indexador deve ter uma `get` acessador e um `set` acessador. Se isso não for o caso, ocorre um erro em tempo de vinculação.

A segunda regra acima permite `x op= y` a ser avaliada como `x = (T)(x op y)` em determinados contextos. A regra existe, de modo que a operadores predefinidos podem ser usados como operadores compostos quando o operando esquerdo for do tipo `sbyte`, `byte`, `short`, `ushort`, ou `char`. Até mesmo quando ambos os argumentos forem de um desses tipos, os operadores predefinidos produzem um resultado do tipo `int`, conforme descrito em [promoções numéricas binárias](expressions.md#binary-numeric-promotions). Portanto, sem uma conversão não seria possível atribuir o resultado ao operando esquerdo.

O efeito intuitivo da regra para operadores predefinidos é simplesmente que `x op= y` é permitido se os dois dos `x op y` e `x = y` são permitidos. No exemplo
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
o motivo intuitivo para cada erro é que uma simples atribuição correspondente também seria um erro.

Isso também significa que dar suporte a operações de atribuição composta eliminada operações. No exemplo
```csharp
int? i = 0;
i += 1;             // Ok
```
o operador elevado `+(int?,int?)` é usado.

### <a name="event-assignment"></a>Atribuição de evento

Se o operando esquerdo de uma `+=` ou `-=` operador é classificado como um acesso de evento e, em seguida, a expressão é avaliada da seguinte maneira:

*  A expressão de instância, se houver, do acesso do evento é avaliada.
*  O operando direito dos `+=` ou `-=` operador é avaliado e, se necessário, convertido para o tipo do operando à esquerda por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).
*  Um acessador de evento do evento é invocado, com a lista de argumentos que consiste do operando à direita, após a avaliação e, se necessário, conversão. Se o operador foi `+=`, o `add` acessador é invocado; se o operador foi `-=`, o `remove` acessador é invocado.

Uma expressão de atribuição de evento não produz um valor. Portanto, uma expressão de atribuição de evento é válida somente no contexto de um *statement_expression* ([instruções de expressão](statements.md#expression-statements)).

## <a name="expression"></a>Expressão

Uma *expressão* é um *non_assignment_expression* ou uma *atribuição*.

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a>Expressões constantes

Um *constant_expression* é uma expressão que pode ser completamente avaliada em tempo de compilação.

```antlr
constant_expression
    : expression
    ;
```

Uma expressão constante deve ser o `null` literal ou um valor com um dos seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, ou qualquer tipo de enumeração. Somente as seguintes construções são permitidas em expressões constantes:

*  Literais (incluindo o `null` literal).
*  As referências a `const` membros de tipos de classe e struct.
*  Referências aos membros de tipos de enumeração.
*  As referências a `const` parâmetros ou variáveis locais
*  Entre parênteses subexpressões, que são as próprias expressões de constante.
*  Expressões de conversão, fornecidas o tipo de destino é um dos tipos listados acima.
*  `checked` e `unchecked` expressões
*  Expressões de valor padrão
*  Expressões Nameof
*  Predefinido `+`, `-`, `!`, e `~` operadores unários.
*  Predefinido `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, e `>=` operadores binários, desde que cada operando seja de um tipo listado acima.
*  O `?:` operador condicional.

As conversões a seguir são permitidas em expressões constantes:

*  Conversões de identidade
*  Conversões numéricas
*  Conversões de enumeração
*  Conversões de expressão de constante
*  Conversões de referência implícita e explícita, fornecidas a fonte das conversões é uma expressão constante que é avaliada como o valor nulo.

Outras conversões, incluindo a conversão boxing, conversão unboxing e referência conversões implícitas de valores não nulos não são permitidas em expressões de constante. Por exemplo:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
a inicialização de i é um erro, pois uma conversão boxing é necessária. A inicialização de str é um erro porque uma conversão de referência implícita de um valor não nulo é necessária.

Sempre que uma expressão cumpre os requisitos listados acima, a expressão é avaliada em tempo de compilação. Isso é verdadeiro mesmo se a expressão for uma subexpressão de uma expressão maior que contém construções não constante.

A avaliação do tempo de compilação de expressões de constante usa as mesmas regras como tempo de execução de avaliação de expressões não constantes, exceto que, em que a avaliação de tempo de execução teria sido gerada uma exceção, a avaliação do tempo de compilação faz com que ocorra um erro de tempo de compilação.

A menos que uma expressão de constante seja explicitamente colocada em um `unchecked` contexto, estouros que ocorrem em operações aritméticas de tipo integral e conversões durante a avaliação do tempo de compilação da expressão sempre causar erros de tempo de compilação ([Expressões de constante](expressions.md#constant-expressions)).

Expressões de constante ocorrerem nos contextos listados abaixo. Nesses contextos, ocorrerá um erro de tempo de compilação se uma expressão não pode ser completamente avaliada em tempo de compilação.

*  Declarações de constante ([constantes](classes.md#constants)).
*  Declarações de membro de enumeração ([membros de Enum](enums.md#enum-members)).
*  Argumentos de parâmetro formal listas padrão ([parâmetros de método](classes.md#method-parameters))
*  `case` rótulos de um `switch` instrução ([da instrução switch](statements.md#the-switch-statement)).
*  `goto case` instruções ([a instrução goto](statements.md#the-goto-statement)).
*  Os tamanhos em uma expressão de criação de matriz da dimensão ([expressões de criação de matriz](expressions.md#array-creation-expressions)) que inclui um inicializador.
*  Atributos ([atributos](attributes.md)).

Uma conversão implícita de expressão de constante ([conversões implícitas de expressão de constante](conversions.md#implicit-constant-expression-conversions)) permite que uma expressão constante do tipo `int` a ser convertido em `sbyte`, `byte`, `short`, `ushort`, `uint`, ou `ulong`, desde que o valor da expressão constante é dentro do intervalo de tipo de destino.

## <a name="boolean-expressions"></a>expressões Boolianas

Um *boolean_expression* é uma expressão que produz um resultado do tipo `bool`; qualquer diretamente ou por meio da aplicação de `operator true` em determinados contextos conforme especificado no exemplo a seguir.

```antlr
boolean_expression
    : expression
    ;
```

A expressão condicional de controle de um *if_statement* ([if instrução](statements.md#the-if-statement)), *while_statement* ([a instrução while](statements.md#the-while-statement)), *do_statement* ([a instrução](statements.md#the-do-statement)), ou *for_statement* ([a instrução](statements.md#the-for-statement)) é um *boolean_ expressão*. A expressão condicional de controle do `?:` operador ([operador condicional](expressions.md#conditional-operator)) segue as mesmas regras que um *boolean_expression*, mas por motivos de operador de precedência é classificada como uma *conditional_or_expression*.

Um *boolean_expression* `E` deve ser capaz de produzir um valor do tipo `bool`, da seguinte maneira:

*  Se `E` é implicitamente conversível para `bool` e em seguida, em tempo de execução que a conversão implícita é aplicada.
*  Caso contrário, o operador unário de resolução de sobrecarga ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é usado para localizar uma melhor implementação exclusiva de operador `true` em `E`, e que a implementação é aplicada em tempo de execução.
*  Se nenhum operador for encontrado, ocorrerá um erro de tempo de associação.

O `DBBool` tipo de struct no [banco de dados tipo booliano](structs.md#database-boolean-type) fornece um exemplo de um tipo que implementa `operator true` e `operator false`.
