---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704084"
---
# <a name="expressions"></a>Expressões

Uma expressão é uma sequência de operadores e operandos. Este capítulo define a sintaxe, a ordem de avaliação de operandos e operadores e o significado de expressões.

## <a name="expression-classifications"></a>Classificações de expressão

Uma expressão é classificada como uma das seguintes:

*  Um valor. Cada valor tem um tipo associado.
*  Uma variável. Cada variável tem um tipo associado, ou seja, o tipo declarado da variável.
*  Um namespace. Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)). Em qualquer outro contexto, uma expressão classificada como um namespace causa um erro em tempo de compilação.
*  Um tipo. Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)) ou como um operando para o operador de `as` ([o operador as](expressions.md#the-as-operator)), o operador de `is` ([o operador is](expressions.md#the-is-operator)) ou o operador `typeof` ([o operador typeof](expressions.md#the-typeof-operator)). Em qualquer outro contexto, uma expressão classificada como um tipo causa um erro de tempo de compilação.
*  Um grupo de métodos, que é um conjunto de métodos sobrecarregados resultante de uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)). Um grupo de métodos pode ter uma expressão de instância associada e uma lista de argumentos de tipo associado. Quando um método de instância é invocado, o resultado da avaliação da expressão de instância se torna a instância representada por `this` ([esse acesso](expressions.md#this-access)). Um grupo de métodos é permitido em *um invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)), um *delegate_creation_expression* (expressões de[criação de delegado](expressions.md#delegate-creation-expressions)) e como o lado esquerdo de um operador is e pode ser convertido implicitamente em um tipo de delegado compatível ([conversões de grupo de métodos](conversions.md#method-group-conversions)). Em qualquer outro contexto, uma expressão classificada como um grupo de métodos causa um erro em tempo de compilação.
*  Um literal nulo. Uma expressão com essa classificação pode ser convertida implicitamente em um tipo de referência ou tipo anulável.
*  Uma função anônima. Uma expressão com essa classificação pode ser convertida implicitamente em um tipo de representante ou tipo de árvore de expressão compatível.
*  Um acesso de propriedade. Cada acesso de propriedade tem um tipo associado, ou seja, o tipo da propriedade. Além disso, um acesso de propriedade pode ter uma expressão de instância associada. Quando um acessador (o `get` ou bloco de `set`) de um acesso de propriedade de instância é invocado, o resultado da avaliação da expressão de instância se torna a instância representada por `this` ([esse acesso](expressions.md#this-access)).
*  Um acesso de evento. Cada acesso de evento tem um tipo associado, ou seja, o tipo do evento. Além disso, um acesso de evento pode ter uma expressão de instância associada. Um acesso de evento pode aparecer como o operando à esquerda dos operadores de `+=` e de `-=` ([atribuição de evento](expressions.md#event-assignment)). Em qualquer outro contexto, uma expressão classificada como um acesso de evento causa um erro de tempo de compilação.
*  Um acesso ao indexador. Cada acesso ao indexador tem um tipo associado, ou seja, o tipo de elemento do indexador. Além disso, um acesso ao indexador tem uma expressão de instância associada e uma lista de argumentos associados. Quando um acessador (o `get` ou bloco de `set`) de um acesso do indexador é invocado, o resultado da avaliação da expressão de instância se torna a instância representada por `this` ([esse acesso](expressions.md#this-access)) e o resultado da avaliação da lista de argumentos se torna a lista de parâmetros da invocação.
*  Nada. Isso ocorre quando a expressão é uma invocação de um método com um tipo de retorno de `void`. Uma expressão classificada como Nothing só é válida no contexto de uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)).

O resultado final de uma expressão nunca é um namespace, tipo, grupo de métodos ou acesso de evento. Em vez disso, conforme observado acima, essas categorias de expressões são construções intermediárias que só são permitidas em determinados contextos.

Um acesso de propriedade ou de indexador é sempre reclassificado como um valor executando uma invocação do *acessador get* ou o *acessador set*. O acessador específico é determinado pelo contexto da propriedade ou do acesso do indexador: se o acesso for o destino de uma atribuição, o *acessador set* será invocado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)). Caso contrário, o *acessador get* é invocado para obter o valor atual ([valores de expressões](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Valores de expressões

A maioria das construções que envolvem uma expressão, por fim, exige que a expressão denotasse um ***valor***. Nesses casos, se a expressão real denota um namespace, um tipo, um grupo de métodos ou nada, ocorrerá um erro em tempo de compilação. No entanto, se a expressão denota um acesso de propriedade, um acesso de indexador ou uma variável, o valor da propriedade, do indexador ou da variável é substituído implicitamente:

*  O valor de uma variável é simplesmente o valor atualmente armazenado no local de armazenamento identificado pela variável. Uma variável deve ser considerada definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes que seu valor possa ser obtido ou, caso contrário, ocorrerá um erro de tempo de compilação.
*  O valor de uma expressão de acesso à propriedade é obtido invocando o *acessador get* da propriedade. Se a propriedade não tiver nenhum *acessador get*, ocorrerá um erro em tempo de compilação. Caso contrário, uma invocação de membro[de função (verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) será executada e o resultado da invocação se tornará o valor da expressão de acesso à propriedade.
*  O valor de uma expressão de acesso do indexador é obtido invocando o *acessador get* do indexador. Se o indexador não tiver nenhum *acessador get*, ocorrerá um erro em tempo de compilação. Caso contrário, uma invocação de membro[de função (verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é executada com a lista de argumentos associada à expressão de acesso do indexador e o resultado da invocação se torna o valor da expressão de acesso do indexador.

## <a name="static-and-dynamic-binding"></a>Associação estática e dinâmica

O processo de determinar o significado de uma operação com base no tipo ou valor de expressões constituintes (argumentos, operandos, receptores) é geralmente conhecido como ***Associação***. Por exemplo, o significado de uma chamada de método é determinado com base no tipo de receptor e argumentos. O significado de um operador é determinado com base no tipo de seus operandos.

No C# sentido de uma operação geralmente é determinado em tempo de compilação, com base no tipo de tempo de compilação de suas expressões constituintes. Da mesma forma, se uma expressão contiver um erro, o erro será detectado e relatado pelo compilador. Essa abordagem é conhecida como ***associação estática***.

No entanto, se uma expressão for uma expressão dinâmica (ou seja, tiver o tipo `dynamic`) isso indica que qualquer associação na qual ela participa deve ser baseada em seu tipo de tempo de execução (ou seja, o tipo real do objeto que ele denota em tempo de execução) em vez do tipo que ele tem em tempo de compilação. A associação de tal operação é, portanto, adiada até o momento em que a operação deve ser executada durante a execução do programa. Isso é conhecido como ***associação dinâmica***.

Quando uma operação é vinculada dinamicamente, pouca ou nenhuma verificação é executada pelo compilador. Em vez disso, se a associação de tempo de execução falhar, os erros serão relatados como exceções em tempo de execução.

As seguintes operações no C# estão sujeitas à associação:

*  Acesso de membro: `e.M`
*  Invocação de método: `e.M(e1, ..., eN)`
*  Invocação de delegado:`e(e1, ..., eN)`
*  Acesso ao elemento: `e[e1, ..., eN]`
*  Criação de objeto: `new C(e1, ..., eN)`
*  Operadores unários sobrecarregados: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Operadores binários sobrecarregados: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Operadores de atribuição: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Conversões implícitas e explícitas

Quando não há expressões dinâmicas envolvidas C# , o padrão é a associação estática, o que significa que os tipos de tempo de compilação de expressões constituintes são usados no processo de seleção. No entanto, quando uma das expressões constituintes nas operações listadas acima é uma expressão dinâmica, a operação é vinculada dinamicamente.

### <a name="binding-time"></a>Tempo de associação

A associação estática ocorre no tempo de compilação, enquanto a associação dinâmica ocorre em tempo de execução. Nas seções a seguir, o termo ***Associação de tempo refere-*** se ao tempo de compilação ou tempo de execução, dependendo de quando a associação ocorre.

O exemplo a seguir ilustra as noções de associação estática e dinâmica e de tempo de vinculação:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

As duas primeiras chamadas são vinculadas estaticamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de compilação de seu argumento. Portanto, o tempo de associação é de tempo de compilação.

A terceira chamada é vinculada dinamicamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de execução de seu argumento. Isso acontece porque o argumento é uma expressão dinâmica – seu tipo de tempo de compilação é `dynamic`. Portanto, o tempo de associação para a terceira chamada é tempo de execução.

### <a name="dynamic-binding"></a>Associação dinâmica

A finalidade da vinculação dinâmica é permitir C# que os programas interajam com ***objetos dinâmicos***, ou seja, objetos que não seguem as regras normais C# do sistema de tipos. Objetos dinâmicos podem ser objetos de outras linguagens de programação com sistemas de tipos diferentes, ou podem ser objetos que são configurados programaticamente para implementar sua própria semântica de ligação para operações diferentes.

O mecanismo pelo qual um objeto dinâmico implementa sua própria semântica é a implementação definida. Uma determinada interface – novamente definida--é implementada por objetos dinâmicos para sinalizar para C# o tempo de execução que eles têm semântica especial. Portanto, sempre que as operações em um objeto dinâmico forem vinculadas dinamicamente, sua própria semântica de ligação, em C# vez das especificadas neste documento, assumirá.

Embora a finalidade da vinculação dinâmica seja permitir a interoperação com objetos dinâmicos C# , o permite a vinculação dinâmica em todos os objetos, sejam eles dinâmicos ou não. Isso permite uma integração mais suave de objetos dinâmicos, pois os resultados das operações neles não podem ser objetos dinâmicos, mas ainda são de um tipo desconhecido para o programador em tempo de compilação. Além disso, a ligação dinâmica pode ajudar a eliminar códigos baseados em reflexão propensos a erros, mesmo quando nenhum objeto envolvido é um objeto dinâmico.

As seções a seguir descrevem cada construção no idioma exatamente quando a vinculação dinâmica é aplicada, qual é a verificação de tempo de compilação – se houver, se houver, e qual será o resultado de tempo de compilação e a classificação de expressão.

### <a name="types-of-constituent-expressions"></a>Tipos de expressões constituintes

Quando uma operação é vinculada estaticamente, o tipo de uma expressão constituinte (por exemplo, um receptor, um argumento, um índice ou um operando) é sempre considerado o tipo de tempo de compilação dessa expressão.

Quando uma operação é vinculada dinamicamente, o tipo de uma expressão constituinte é determinado de maneiras diferentes, dependendo do tipo de tempo de compilação da expressão constituinte:

*  Uma expressão constituinte de tipo de tempo de compilação `dynamic` é considerada como tendo o tipo do valor real que a expressão avalia em tempo de execução
*  Uma expressão constituinte cujo tipo de tempo de compilação é um parâmetro de tipo é considerado como tendo o tipo ao qual o parâmetro de tipo está associado em tempo de execução
*  Caso contrário, a expressão constituinte será considerada com seu tipo de tempo de compilação.

## <a name="operators"></a>Operadores

As expressões são construídas a partir de ***operandos*** e ***operadores***. Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos. Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`. Exemplos de operandos incluem literais, campos, variáveis locais e expressões.

Há três tipos de operadores:

*  Operadores unários. Os operadores unários usam um operando e usam a notação de prefixo (como `--x`) ou a notação de sufixo (como `x++`).
*  Operadores binários. Os operadores binários usam dois operandos e todos usam notação de infixo (como `x + y`).
*  Operador ternário. Somente um operador ternário, `?:`, existe; Ele usa três operandos e usa a notação de infixo (`c ? x : y`).

A ordem de avaliação de operadores em uma expressão é determinada pela ***precedência*** e ***Associação*** dos operadores ([precedência de operador e Associação](expressions.md#operator-precedence-and-associativity)).

Os operandos em uma expressão são avaliados da esquerda para a direita. Por exemplo, em `F(i) + G(i++) * H(i)`, o método `F` é chamado usando o valor antigo de `i`, então o método `G` é chamado com o valor antigo de `i`e, finalmente, o método `H` é chamado com o novo valor de `i`. Isso é separado de e não relacionado à precedência de operador.

Determinados operadores podem ser ***sobrecarregados***. A sobrecarga de operador permite que as implementações de operador definidas pelo usuário sejam especificadas para operações em que um ou ambos os operandos são de uma classe definida pelo usuário ou tipo struct ([sobrecarga de operador](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Precedência e associatividade do operador

Quando uma expressão contiver vários operadores, a ***precedência*** dos operadores controla a ordem na qual os operadores individuais são avaliados. Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador de `*` tem precedência maior do que o operador binário `+`. A precedência de um operador é estabelecida pela definição de sua produção de gramática associada. Por exemplo, um *additive_expression* consiste em uma sequência de *multiplicative_expression*s separados por `+` ou operadores de `-`, permitindo que os operadores `+` e `-` diminuam a precedência dos operadores `*`, `/`e `%`.

A tabela a seguir resume todos os operadores em ordem de precedência do mais alto para o mais baixo:

| __Section__                                                                                   | __Categoria__                | __Operadores__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Expressões primárias](expressions.md#primary-expressions)                                     | Primary                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
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
| [Operadores de atribuição](expressions.md#assignment-operators), [expressões de função anônimas](expressions.md#anonymous-function-expressions)  | Atribuição e expressão lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Quando um operando ocorre entre dois operadores com a mesma precedência, a Associação dos operadores controla a ordem na qual as operações são executadas:

*  Exceto para os operadores de atribuição e o operador de União nulo, todos os operadores binários são ***associativos à esquerda***, o que significa que as operações são executadas da esquerda para a direita. Por exemplo, `x + y + z` é avaliado como `(x + y) + z`.
*  Os operadores de atribuição, o operador de União nulo e o operador condicional (`?:`) são ***associativos à direita***, o que significa que as operações são executadas da direita para a esquerda. Por exemplo, `x = y = z` é avaliado como `x = (y = z)`.

Precedência e associatividade podem ser controladas usando parênteses. Por exemplo, `x + y * z` primeiro multiplica `y` por `z` e, em seguida, adiciona o resultado a `x`, mas `(x + y) * z` primeiro adiciona `x` e `y` e, em seguida, multiplica o resultado por `z`.

### <a name="operator-overloading"></a>Sobrecarga de operador

Todos os operadores unários e binários têm implementações predefinidas que estão automaticamente disponíveis em qualquer expressão. Além das implementações predefinidas, as implementações definidas pelo usuário podem ser introduzidas incluindo `operator` declarações em classes e Structs ([operadores](classes.md#operators)). Implementações de operador definidas pelo usuário sempre têm precedência sobre implementações de operador predefinidas: somente quando não houver nenhuma implementação de operador definida pelo usuário aplicável as implementações de operador predefinidas serão consideradas, conforme descrito em resolução de [sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution).

Os ***operadores unários sobrecarregados*** são:
```csharp
+   -   !   ~   ++   --   true   false
```

Embora `true` e `false` não sejam usados explicitamente em expressões (e, portanto, não estejam incluídos na tabela de precedência em [precedência de operador e Associação](expressions.md#operator-precedence-and-associativity)), eles são considerados operadores porque são invocados em vários contextos de expressão: expressões boolianas ([expressões booleanas](expressions.md#boolean-expressions)) e expressões que envolvem o condicional ([operador condicional](expressions.md#conditional-operator)) e operadores lógicos condicionais ([operadores lógicos condicional](expressions.md#conditional-logical-operators)).

Os ***operadores binários sobrecarregados*** são:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Somente os operadores listados acima podem ser sobrecarregados. Em particular, não é possível sobrecarregar o acesso de membro, a invocação de método ou os operadores `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`e `is`.

Quando um operador binário está sobrecarregado, o operador de atribuição correspondente, se houver, também estará implicitamente sobrecarregado. Por exemplo, uma sobrecarga de operador `*` também é uma sobrecarga de operador `*=`. Isso é descrito mais detalhadamente na [atribuição composta](expressions.md#compound-assignment). Observe que o próprio operador de atribuição (`=`) não pode ser sobrecarregado. Uma atribuição sempre executa uma cópia de bits simples de um valor em uma variável.

As operações de conversão, como `(T)x`, são sobrecarregadas fornecendo conversões definidas pelo usuário ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)).

O acesso ao elemento, como `a[x]`, não é considerado um operador sobrecarregado. Em vez disso, há suporte para a indexação definida pelo usuário por meio de indexadores ([indexadores](classes.md#indexers)).

Em expressões, os operadores são referenciados usando a notação de operador e, em declarações, os operadores são referenciados usando a notação funcional. A tabela a seguir mostra a relação entre as notações de operador e funcional para operadores unários e binários. Na primeira entrada, *op* denota qualquer operador de prefixo unário sobrecarregado. Na segunda entrada, *op* denota o sufixo unário `++` e os operadores de `--`. Na terceira entrada, *op* denota qualquer operador binário sobrecarregado.


| __Notação de operador__ | __Notação funcional__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

As declarações de operador definidas pelo usuário sempre exigem que pelo menos um dos parâmetros seja do tipo de classe ou struct que contém a declaração do operador. Portanto, não é possível que um operador definido pelo usuário tenha a mesma assinatura que um operador predefinido.

As declarações do operador definidas pelo usuário não podem modificar a sintaxe, a precedência ou a associação de um operador. Por exemplo, o operador de `/` é sempre um operador binário, sempre tem o nível de precedência especificado em [precedência de operador e Associação](expressions.md#operator-precedence-and-associativity), e é sempre associativo à esquerda.

Embora seja possível que um operador definido pelo usuário execute qualquer computação, as implementações que produzem resultados diferentes daqueles que são intuitivos esperados são altamente desencorajadas. Por exemplo, uma implementação de `operator ==` deve comparar os dois operandos para igualdade e retornar um resultado de `bool` apropriado.

As descrições de operadores individuais em [expressões primárias](expressions.md#primary-expressions) por meio de [operadores lógicos condicionais](expressions.md#conditional-logical-operators) especificam as implementações predefinidas dos operadores e quaisquer regras adicionais que se aplicam a cada operador. As descrições fazem uso dos termos ***resolução de sobrecarga de operador unário***, ***resolução de sobrecarga de operador binário***e ***promoção numérica***, definições das quais são encontradas nas seções a seguir.

### <a name="unary-operator-overload-resolution"></a>Resolução de sobrecarga de operador unário

Uma operação do formulário `op x` ou `x op`, em que `op` é um operador unário sobrecarregado e `x` é uma expressão do tipo `X`, é processada da seguinte maneira:

*  O conjunto de operadores candidatos definidos pelo usuário fornecido pelo `X` para a operação `operator op(x)` é determinado usando as regras de [operadores do candidato definidos pelo usuário](expressions.md#candidate-user-defined-operators).
*  Se o conjunto de operadores candidatos definidos pelo usuário não estiver vazio, isso se tornará o conjunto de operadores candidatos para a operação. Caso contrário, as implementações de `operator op` unário predefinidas, incluindo suas formas comparadas, se tornarão o conjunto de operadores candidatos para a operação. As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([expressões primárias](expressions.md#primary-expressions) e [operadores unários](expressions.md#unary-operators)).
*  As regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores candidatos para selecionar o melhor operador em relação à lista de argumentos `(x)`, e esse operador se torna o resultado do processo de resolução de sobrecarga. Se a resolução de sobrecarga falhar ao selecionar um único operador melhor, ocorrerá um erro de tempo de associação.

### <a name="binary-operator-overload-resolution"></a>Resolução de sobrecarga de operador binário

Uma operação do formulário `x op y`, em que `op` é um operador binário sobrecarregável, `x` é uma expressão do tipo `X`e `y` é uma expressão do tipo `Y`, é processada da seguinte maneira:

*  O conjunto de operadores candidatos definidos pelo usuário fornecido pelo `X` e `Y` para a operação `operator op(x,y)` é determinado. O conjunto consiste na União dos operadores candidatos fornecidos por `X` e nos operadores candidatos fornecidos pelo `Y`, cada um determinado usando as regras de [operadores definidos pelo usuário candidatos](expressions.md#candidate-user-defined-operators). Se `X` e `Y` forem do mesmo tipo, ou se `X` e `Y` forem derivados de um tipo base comum, os operadores de candidato compartilhados só ocorrerão no conjunto combinado uma vez.
*  Se o conjunto de operadores candidatos definidos pelo usuário não estiver vazio, isso se tornará o conjunto de operadores candidatos para a operação. Caso contrário, o binário predefinido `operator op` implementações, incluindo seus formulários levantados, se tornará o conjunto de operadores candidatos para a operação. As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([operadores aritméticos](expressions.md#arithmetic-operators) por meio de [operadores lógicos condicionais](expressions.md#conditional-logical-operators)). Para operadores enum e delegate predefinidos, os únicos operadores considerados são aqueles definidos por um tipo de enumeração ou de delegado que é o tipo de tempo de associação de um dos operandos.
*  As regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores candidatos para selecionar o melhor operador em relação à lista de argumentos `(x,y)`, e esse operador se torna o resultado do processo de resolução de sobrecarga. Se a resolução de sobrecarga falhar ao selecionar um único operador melhor, ocorrerá um erro de tempo de associação.

### <a name="candidate-user-defined-operators"></a>Operadores definidos pelo usuário candidatos

Dado um tipo `T` e uma operação `operator op(A)`, em que `op` é um operador que está sendo sobrecarregado e `A` é uma lista de argumentos, o conjunto de operadores candidatos definidos pelo usuário fornecido pelo `T` para `operator op(A)` é determinado da seguinte maneira:

*  Determine o tipo `T0`. Se `T` for um tipo anulável, `T0` será seu tipo subjacente, caso contrário `T0` será igual a `T`.
*  Para todas as declarações de `operator op` em `T0` e todas as formas levantadas desses operadores, se pelo menos um operador for aplicável ([membro de função aplicável](expressions.md#applicable-function-member)) em relação à lista de argumentos `A`, o conjunto de operadores candidatos consiste em todos esses operadores aplicáveis no `T0`.
*  Caso contrário, se `T0` for `object`, o conjunto de operadores candidatos estará vazio.
*  Caso contrário, o conjunto de operadores candidatos fornecidos pelo `T0` é o conjunto de operadores candidatos fornecidos pela classe base direta de `T0`ou a classe base efetiva de `T0` se `T0` for um parâmetro de tipo.

### <a name="numeric-promotions"></a>Promoções numéricas

A promoção numérica consiste em executar automaticamente determinadas conversões implícitas dos operandos dos operadores numéricos unários e binários predefinidos. A promoção numérica não é um mecanismo distinto, mas sim um efeito de aplicar a resolução de sobrecarga aos operadores predefinidos. A promoção numérica especificamente não afeta a avaliação de operadores definidos pelo usuário, embora os operadores definidos pelo usuário possam ser implementados para exibir efeitos semelhantes.

Como exemplo de promoção numérica, considere as implementações predefinidas do operador binário `*`:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Quando as regras de resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)) são aplicadas a esse conjunto de operadores, o efeito é selecionar o primeiro dos operadores para os quais conversões implícitas existem nos tipos de operando. Por exemplo, para a operação `b * s`, em que `b` é um `byte` e `s` é um `short`, a resolução de sobrecarga seleciona `operator *(int,int)` como o melhor operador. Assim, o efeito é que `b` e `s` são convertidos em `int`, e o tipo do resultado é `int`. Da mesma forma, para a operação `i * d`, em que `i` é um `int` e `d` é um `double`, a resolução de sobrecarga seleciona `operator *(double,double)` como o melhor operador.

#### <a name="unary-numeric-promotions"></a>Promoções numéricas unários

A promoção numérica unário ocorre para os operandos dos operadores predefinidos `+`, `-`e `~` unários. A promoção numérica unário simplesmente consiste na conversão de operandos do tipo `sbyte`, `byte`, `short`, `ushort`ou `char` para o tipo `int`. Além disso, para o operador de `-` unário, a promoção numérica unário converte os operandos do tipo `uint` para o tipo `long`.

#### <a name="binary-numeric-promotions"></a>Promoções numéricas binárias

A promoção numérica binária ocorre para os operandos dos operadores predefinidos `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`e `<=` binários. A promoção numérica binária converte implicitamente os dois operandos em um tipo comum que, no caso dos operadores não relacionais, também se torna o tipo de resultado da operação. A promoção numérica binária consiste na aplicação das seguintes regras, na ordem em que aparecem aqui:

*  Se um dos operandos for do tipo `decimal`, o outro operando será convertido no tipo `decimal`ou ocorrerá um erro de tempo de ligação se o outro operando for do tipo `float` ou `double`.
*  Caso contrário, se um dos operandos for do tipo `double`, o outro operando será convertido no tipo `double`.
*  Caso contrário, se um dos operandos for do tipo `float`, o outro operando será convertido no tipo `float`.
*  Caso contrário, se qualquer operando for do tipo `ulong`, o outro operando será convertido no tipo `ulong`, ou ocorrerá um erro de tempo de associação se o outro operando for do tipo `sbyte`, `short`, `int`ou `long`.
*  Caso contrário, se um dos operandos for do tipo `long`, o outro operando será convertido no tipo `long`.
*  Caso contrário, se um dos operandos for do tipo `uint` e o outro operando for do tipo `sbyte`, `short`ou `int`, ambos os operandos serão convertidos no tipo `long`.
*  Caso contrário, se um dos operandos for do tipo `uint`, o outro operando será convertido no tipo `uint`.
*  Caso contrário, ambos os operandos serão convertidos no tipo `int`.

Observe que a primeira regra não permite nenhuma operação que misture o tipo de `decimal` com os tipos `double` e `float`. A regra segue do fato de que não há conversões implícitas entre o tipo de `decimal` e os tipos de `double` e `float`.

Observe também que não é possível que um operando seja do tipo `ulong` quando o outro operando é de um tipo integral assinado. O motivo é que não existe nenhum tipo integral que possa representar o intervalo completo de `ulong`, bem como os tipos integrais assinados.

Em ambos os casos acima, uma expressão de conversão pode ser usada para converter explicitamente um operando em um tipo que seja compatível com o outro operando.

No exemplo
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
um erro de tempo de associação ocorre porque um `decimal` não pode ser multiplicado por um `double`. O erro é resolvido convertendo explicitamente o segundo operando para `decimal`, da seguinte maneira:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Operadores levantados

***Operadores levantados*** permitem operadores predefinidos e definidos pelo usuário que operam em tipos de valores não anuláveis para também serem usados com formulários anuláveis desses tipos. Os operadores levantados são construídos a partir de operadores predefinidos e definidos pelo usuário que atendem a determinados requisitos, conforme descrito no seguinte:

*   Para os operadores unários

    ```csharp
    +  ++  -  --  !  ~
    ```

    uma forma levantada de um operador existirá se o operando e os tipos de resultado forem tipos de valor não anuláveis. O formulário levantado é construído com a adição de um único modificador de `?` para os tipos de operando e de resultado. O operador levantado produz um valor nulo se o operando for nulo. Caso contrário, o operador levantado desencapsula o operando, aplica o operador subjacente e encapsula o resultado.

*   Para os operadores binários

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    Existe uma forma levantada de um operador se os tipos de operando e de resultado são todos os tipos de valor não anuláveis. O formulário levantado é construído com a adição de um único modificador de `?` a cada operando e tipo de resultado. O operador levantado produz um valor nulo se um ou ambos os operandos forem nulos (uma exceção sendo os operadores `&` e `|` do tipo `bool?`, conforme descrito em [operadores lógicos boolianos](expressions.md#boolean-logical-operators)). Caso contrário, o operador levantado desencapsula os operandos, aplica o operador subjacente e encapsula o resultado.

*   Para os operadores de igualdade

    ```csharp
    ==  !=
    ```

    Existe uma forma levantada de um operador se os tipos de operando forem tipos de valor não anuláveis e se o tipo de resultado for `bool`. O formulário levantado é construído com a adição de um único modificador de `?` a cada tipo de operando. O operador levantado considera dois valores nulos iguais e um valor nulo é diferente de qualquer valor não nulo. Se ambos os operandos forem não nulos, o operador levantado desencapsulará os operandos e aplicará o operador subjacente para produzir o resultado de `bool`.

*   Para os operadores relacionais

    ```csharp
    <  >  <=  >=
    ```

    Existe uma forma levantada de um operador se os tipos de operando forem tipos de valor não anuláveis e se o tipo de resultado for `bool`. O formulário levantado é construído com a adição de um único modificador de `?` a cada tipo de operando. O operador levantado produz o valor `false` se um ou ambos os operandos forem nulos. Caso contrário, o operador levantado desencapsula os operandos e aplica o operador subjacente para produzir o resultado de `bool`.

## <a name="member-lookup"></a>Pesquisa de membros

Uma pesquisa de membro é o processo pelo qual o significado de um nome no contexto de um tipo é determinado. Uma pesquisa de membro pode ocorrer como parte da avaliação de um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)) em uma expressão. Se o *Simple_name* ou *member_access* ocorrer como o *primary_expression* de um *invocation_expression* ([invocações de método](expressions.md#method-invocations)), o membro será chamado de chamada.

Se um membro for um método ou evento, ou se for uma constante, campo ou propriedade de um tipo delegado ([delegados](delegates.md)) ou do tipo `dynamic` ([o tipo dinâmico](types.md#the-dynamic-type)), o membro será dito como *invocável*.

A pesquisa de membros considera não apenas o nome de um membro, mas também o número de parâmetros de tipo que o membro tem e se o membro está acessível. Para fins de pesquisa de membros, os métodos genéricos e os tipos genéricos aninhados têm o número de parâmetros de tipo indicados em suas respectivas declarações e todos os outros membros têm zero parâmetros de tipo.

Uma pesquisa de membro de um nome `N` com `K`parâmetros de tipo de  em um tipo `T` é processada da seguinte maneira:

*  Primeiro, um conjunto de membros acessíveis chamado `N` é determinado:
    * Se `T` for um parâmetro de tipo, o conjunto será a União dos conjuntos de membros acessíveis chamados `N` em cada um dos tipos especificados como uma restrição primária ou restrição secundária ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) para `T`, juntamente com o conjunto de membros acessíveis chamados `N` em `object`.
    * Caso contrário, o conjunto consiste em todos os membros acessíveis ([acesso de membro](basic-concepts.md#member-access)) chamados `N` em `T`, incluindo membros herdados e os membros acessíveis chamados `N` no `object`. Se `T` for um tipo construído, o conjunto de membros será obtido substituindo argumentos de tipo conforme descrito em [membros de tipos construídos](classes.md#members-of-constructed-types). Os membros que incluem um modificador de `override` são excluídos do conjunto.
*  Em seguida, se `K` for zero, todos os tipos aninhados cujas declarações incluem parâmetros de tipo serão removidos. Se `K` não for zero, todos os membros com um número diferente de parâmetros de tipo serão removidos. Observe que quando `K` é zero, os métodos com parâmetros de tipo não são removidos, pois o processo de inferência de tipos ([inferência de tipos](expressions.md#type-inference)) pode ser capaz de inferir os argumentos de tipo.
*  Em seguida, se o membro for *invocado*, todos os membros não*invocáveis* serão removidos do conjunto.
*  Em seguida, os membros que estão ocultos por outros membros são removidos do conjunto. Para cada membro `S.M` no conjunto, em que `S` é o tipo no qual o membro `M` é declarado, as seguintes regras são aplicadas:
    * Se `M` for uma constante, campo, propriedade, evento ou membro de enumeração, todos os membros declarados em um tipo base de `S` serão removidos do conjunto.
    * Se `M` for uma declaração de tipo, todos os não tipos declarados em um tipo base de `S` serão removidos do conjunto, e todas as declarações de tipo com o mesmo número de parâmetros de tipo como `M` declarados em um tipo base de `S` serão removidas do conjunto.
    * Se `M` for um método, todos os membros que não forem do método declarados em um tipo base de `S` serão removidos do conjunto.
*  Em seguida, os membros de interface ocultos por membros de classe são removidos do conjunto. Essa etapa só terá efeito se `T` for um parâmetro de tipo e `T` tiver uma classe base em vigor diferente de `object` e um conjunto de interfaces efetivas não vazias ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Para cada membro `S.M` no conjunto, em que `S` é o tipo no qual o membro `M` é declarado, as regras a seguir serão aplicadas se `S` for uma declaração de classe diferente de `object`:
    * Se `M` for uma declaração constante, campo, propriedade, evento, membro de enumeração ou tipo, todos os membros declarados em uma declaração de interface serão removidos do conjunto.
    * Se `M` for um método, todos os membros não-método declarados em uma declaração de interface serão removidos do conjunto e todos os métodos com a mesma assinatura que `M` declarados em uma declaração de interface serão removidos do conjunto.
*  Finalmente, ter removido Membros ocultos, o resultado da pesquisa é determinado:
    * Se o conjunto consistir em um único membro que não seja um método, esse membro será o resultado da pesquisa.
    * Caso contrário, se o conjunto contiver apenas métodos, esse grupo de métodos será o resultado da pesquisa.
    * Caso contrário, a pesquisa será ambígua e ocorrerá um erro de tempo de vinculação.

Para pesquisas de membros em tipos diferentes de parâmetros de tipo e interfaces, e pesquisas de membros em interfaces que são estritamente herança simples (cada interface na cadeia de herança tem exatamente zero ou uma interface de base direta), o efeito das regras de pesquisa é simplesmente, os membros derivados ocultam os membros base com o mesmo nome ou assinatura. Essas pesquisas de herança única nunca são ambíguas. As ambiguidades que podem surgir em relação a pesquisas de membros em interfaces de várias heranças são descritas em [acesso de membro de interface](interfaces.md#interface-member-access).

### <a name="base-types"></a>Tipos base

Para fins de pesquisa de membros, um tipo `T` é considerado com os seguintes tipos base:

*  Se `T` for `object`, `T` não terá nenhum tipo base.
*  Se `T` for uma *enum_type*, os tipos de base de `T` serão os tipos de classe `System.Enum`, `System.ValueType`e `object`.
*  Se `T` for uma *struct_type*, os tipos de base de `T` serão os tipos de classe `System.ValueType` e `object`.
*  Se `T` for uma *class_type*, os tipos de base de `T` serão as classes base de `T`, incluindo o tipo de classe `object`.
*  Se `T` for uma *interface_type*, os tipos de base de `T` serão as interfaces base de `T` e o tipo de classe `object`.
*  Se `T` for um *array_type*, os tipos de base de `T` serão os tipos de classe `System.Array` e `object`.
*  Se `T` for uma *delegate_type*, os tipos de base de `T` serão os tipos de classe `System.Delegate` e `object`.

## <a name="function-members"></a>Membros da função

Membros de função são membros que contêm instruções Executáveis. Membros de função são sempre membros de tipos e não podem ser membros de namespaces. C#define as seguintes categorias de membros de função:

*  {1&gt;Métodos&lt;1}
*  {1&gt;Propriedades&lt;1}
*  Events
*  Indexadores
*  Operadores definidos pelo usuário
*  Construtores de instância
*  Construtores estáticos
*  Destruidores

Exceto para destruidores e construtores estáticos (que não podem ser invocados explicitamente), as instruções contidas nos membros da função são executadas por meio de invocações de membro de função. A sintaxe real para gravar uma invocação de membro de função depende da categoria de membro de função específica.

A lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de uma invocação de membro de função fornece valores reais ou referências de variáveis para os parâmetros do membro da função.

Invocações de métodos genéricos podem empregar inferência de tipos para determinar o conjunto de argumentos de tipo a serem passados para o método. Esse processo é descrito em [inferência de tipos](expressions.md#type-inference).

Invocações de métodos, indexadores, operadores e construtores de instância empregam resolução de sobrecarga para determinar qual de um conjunto candidato de membros de função invocar. Esse processo é descrito em [resolução de sobrecarga](expressions.md#overload-resolution).

Depois que um membro de função específico tiver sido identificado no tempo de vinculação, possivelmente por meio da resolução de sobrecarga, o processo real de tempo de execução de invocação do membro da função será descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

A tabela a seguir resume o processamento que ocorre em construções envolvendo as seis categorias de membros de função que podem ser invocadas explicitamente. Na tabela, `e`, `x`, `y`e `value` indicam expressões classificadas como variáveis ou valores, `T` indica uma expressão classificada como um tipo, `F` é o nome simples de um método e `P` é o nome simples de uma propriedade.


| __Construir__     | __Exemplo__    | __Descrição__ |
|-------------------|----------------|-----------------|
| Invocação de método | `F(x,y)`       | A resolução de sobrecarga é aplicada para selecionar o melhor método `F` na classe ou estrutura que o contém. O método é invocado com a lista de argumentos `(x,y)`. Se o método não for `static`, a expressão de instância será `this`. | 
|                   | `T.F(x,y)`     | A resolução de sobrecarga é aplicada para selecionar o melhor método `F` no `T`de classe ou struct. Ocorrerá um erro de tempo de ligação se o método não for `static`. O método é invocado com a lista de argumentos `(x,y)`. | 
|                   | `e.F(x,y)`     | A resolução de sobrecarga é aplicada para selecionar o melhor método F na classe, struct ou interface fornecida pelo tipo de `e`. Ocorrerá um erro de tempo de ligação se o método for `static`. O método é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`. | 
| Acesso à propriedade   | `P`            | O acessador de `get` da propriedade `P` na classe ou struct que a contém é invocado. Ocorrerá um erro em tempo de compilação se `P` for somente gravação. Se `P` não for `static`, a expressão de instância será `this`. | 
|                   | `P = value`    | O acessador de `set` da propriedade `P` na classe ou struct que o contém é invocado com a lista de argumentos `(value)`. Ocorrerá um erro em tempo de compilação se `P` for somente leitura. Se `P` não for `static`, a expressão de instância será `this`. | 
|                   | `T.P`          | O acessador de `get` da propriedade `P` no `T` de classe ou struct é invocado. Ocorrerá um erro de tempo de compilação se `P` não for `static` ou se `P` for somente gravação. | 
|                   | `T.P = value`  | O acessador de `set` da propriedade `P` no `T` de classe ou struct é invocado com a lista de argumentos `(value)`. Ocorrerá um erro de tempo de compilação se `P` não for `static` ou se `P` for somente leitura. | 
|                   | `e.P`          | O acessador de `get` da propriedade `P` na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e`. Ocorrerá um erro de tempo de associação se `P` for `static` ou se `P` for somente gravação. | 
|                   | `e.P = value`  | O acessador de `set` da propriedade `P` na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e` e a lista de argumentos `(value)`. Ocorrerá um erro de tempo de associação se `P` for `static` ou se `P` for somente leitura. | 
| Acesso a eventos      | `E += value`   | O acessador de `add` do `E` de eventos na classe ou struct que o contém é invocado. Se `E` não for estático, a expressão de instância será `this`. | 
|                   | `E -= value`   | O acessador de `remove` do `E` de eventos na classe ou struct que o contém é invocado. Se `E` não for estático, a expressão de instância será `this`. | 
|                   | `T.E += value` | O acessador de `add` do `E` de eventos na `T` de classe ou struct é invocado. Ocorrerá um erro de tempo de associação se `E` não for estático. | 
|                   | `T.E -= value` | O acessador de `remove` do `E` de eventos na `T` de classe ou struct é invocado. Ocorrerá um erro de tempo de associação se `E` não for estático. | 
|                   | `e.E += value` | O acessador de `add` do `E` de eventos na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e`. Ocorrerá um erro de tempo de associação se `E` for estático. | 
|                   | `e.E -= value` | O acessador de `remove` do `E` de eventos na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e`. Ocorrerá um erro de tempo de associação se `E` for estático. | 
| Acesso de indexador    | `e[x,y]`       | A resolução de sobrecarga é aplicada para selecionar o melhor indexador na classe, struct ou interface fornecida pelo tipo de e. O acessador de `get` do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`. Ocorrerá um erro de tempo de ligação se o indexador for somente gravação. | 
|                   | `e[x,y] = value` | A resolução de sobrecarga é aplicada para selecionar o melhor indexador na classe, struct ou interface fornecida pelo tipo de `e`. O acessador de `set` do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y,value)`. Ocorrerá um erro de tempo de ligação se o indexador for somente leitura. | 
| Invocação de operador | `-x`         | A resolução de sobrecarga é aplicada para selecionar o melhor operador unário na classe ou struct fornecido pelo tipo de `x`. O operador selecionado é invocado com a lista de argumentos `(x)`. | 
|                     | `x + y`      | A resolução de sobrecarga é aplicada para selecionar o melhor operador binário nas classes ou estruturas dadas pelos tipos de `x` e `y`. O operador selecionado é invocado com a lista de argumentos `(x,y)`. | 
| Invocação de construtor de instância | `new T(x,y)` | A resolução de sobrecarga é aplicada para selecionar o melhor Construtor de instância no `T`de classe ou struct. O construtor de instância é invocado com a lista de argumentos `(x,y)`. | 

### <a name="argument-lists"></a>Listas de argumentos

Cada membro de função e invocação de delegado inclui uma lista de argumentos que fornece valores reais ou referências de variáveis para os parâmetros do membro da função. A sintaxe para especificar a lista de argumentos de uma invocação de membro de função depende da categoria de membro da função:

*  Para construtores de instância, métodos, indexadores e delegados, os argumentos são especificados como um *argument_list*, conforme descrito abaixo. Para indexadores, ao invocar o acessador de `set`, a lista de argumentos também inclui a expressão especificada como o operando à direita do operador de atribuição.
*  Para propriedades, a lista de argumentos está vazia ao invocar o acessador de `get` e consiste na expressão especificada como o operando à direita do operador de atribuição ao invocar o acessador de `set`.
*  Para eventos, a lista de argumentos consiste na expressão especificada como o operando à direita do `+=` ou operador de `-=`.
*  Para operadores definidos pelo usuário, a lista de argumentos consiste no único operando do operador unário ou nos dois operandos do operador binário.

Os argumentos de Propriedades ([Propriedades](classes.md#properties)), eventos ([eventos](classes.md#events)) e operadores definidos pelo usuário ([operadores](classes.md#operators)) são sempre passados como parâmetros de valor ([parâmetros de valor](classes.md#value-parameters)). Os argumentos de indexadores ([indexadores](classes.md#indexers)) são sempre passados como parâmetros de valor ([parâmetros de valor](classes.md#value-parameters)) ou matrizes de parâmetro ([matrizes de parâmetro](classes.md#parameter-arrays)). Parâmetros de referência e saída não têm suporte para essas categorias de membros de função.

Os argumentos de um construtor de instância, método, indexador ou invocação de delegado são especificados como um *argument_list*:

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

Um *argument_list* consiste em um ou mais s de *argumento*, separados por vírgulas. Cada argumento consiste em um *argument_name* opcional seguido por um *argument_value*. Um *argumento* com um *argument_name* é conhecido como um ***argumento nomeado***, enquanto que um *argumento* sem um *argument_name* é um ***argumento posicional***. É um erro para que um argumento posicional apareça após um argumento nomeado em um *argument_list*.

O *argument_value* pode ter um dos seguintes formatos:

*  Uma *expressão*, indicando que o argumento é passado como um parâmetro de valor ([parâmetros de valor](classes.md#value-parameters)).
*  A palavra-chave `ref` seguida por uma *variable_reference* ([referências de variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de referência ([parâmetros de referência](classes.md#reference-parameters)). Uma variável deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes de poder ser passada como um parâmetro de referência. A palavra-chave `out` seguida por uma *variable_reference* ([referências de variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de saída ([parâmetros de saída](classes.md#output-parameters)). Uma variável é considerada definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) após uma invocação de membro de função na qual a variável é passada como um parâmetro de saída.

#### <a name="corresponding-parameters"></a>Parâmetros correspondentes

Para cada argumento em uma lista de argumentos, deve haver um parâmetro correspondente no membro da função ou no delegado que está sendo invocado.

A lista de parâmetros usada no seguinte é determinada como a seguir:

*  Para métodos virtuais e indexadores definidos em classes, a lista de parâmetros é escolhida da declaração mais específica ou substituição do membro da função, começando com o tipo estático do receptor e pesquisando suas classes base.
*  Para métodos de interface e indexadores, a lista de parâmetros é escolhida para formar a definição mais específica do membro, começando com o tipo de interface e pesquisando as interfaces base. Se nenhuma lista de parâmetros exclusiva for encontrada, uma lista de parâmetros com nomes inacessíveis e nenhum parâmetro opcional será construído, de modo que as invocações não poderão usar parâmetros nomeados nem omitir argumentos opcionais.
*  Para métodos parciais, a lista de parâmetros da declaração de método parcial de definição é usada.
*  Para todos os outros membros de função e delegados, há apenas uma única lista de parâmetros, que é a usada.

A posição de um argumento ou parâmetro é definida como o número de argumentos ou parâmetros que o antecedem na lista de argumentos ou na lista de parâmetros.

Os parâmetros correspondentes para argumentos de membro de função são estabelecidos da seguinte maneira:

*  Argumentos na *argument_list* de construtores de instância, métodos, indexadores e delegados:
    * Um argumento posicional em que um parâmetro fixo ocorre na mesma posição na lista de parâmetros corresponde a esse parâmetro.
    * Um argumento posicional de um membro de função com uma matriz de parâmetros invocada em seu formato normal corresponde à matriz de parâmetros, que deve ocorrer na mesma posição na lista de parâmetros.
    * Um argumento posicional de um membro de função com uma matriz de parâmetro invocada em sua forma expandida, em que nenhum parâmetro fixo ocorre na mesma posição na lista de parâmetros, corresponde a um elemento na matriz de parâmetros.
    * Um argumento nomeado corresponde ao parâmetro de mesmo nome na lista de parâmetros.
    * Para indexadores, ao invocar o acessador de `set`, a expressão especificada como o operando à direita do operador de atribuição corresponde ao parâmetro de `value` implícito da declaração de acessador de `set`.
*  Para propriedades, ao invocar o acessador de `get`, não há argumentos. Ao invocar o acessador de `set`, a expressão especificada como o operando à direita do operador de atribuição corresponde ao parâmetro de `value` implícito da declaração de acessador de `set`.
*  Para operadores unários definidos pelo usuário (incluindo conversões), o único operando corresponde ao parâmetro único da declaração do operador.
*  Para operadores binários definidos pelo usuário, o operando esquerdo corresponde ao primeiro parâmetro e o operando direito corresponde ao segundo parâmetro da declaração do operador.

#### <a name="run-time-evaluation-of-argument-lists"></a>Avaliação de tempo de execução de listas de argumentos

Durante o processamento de tempo de execução de uma invocação de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), as expressões ou referências de variáveis de uma lista de argumentos são avaliadas na ordem, da esquerda para a direita, da seguinte maneira:

*  Para um parâmetro de valor, a expressão de argumento é avaliada e uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de parâmetro correspondente é executada. O valor resultante torna-se o valor inicial do parâmetro value na invocação de membro da função.
*  Para um parâmetro de referência ou saída, a referência de variável é avaliada e o local de armazenamento resultante torna-se o local de armazenamento representado pelo parâmetro na invocação de membro da função. Se a referência de variável fornecida como um parâmetro de referência ou de saída for um elemento de matriz de um *reference_type*, uma verificação de tempo de execução será executada para garantir que o tipo de elemento da matriz seja idêntico ao tipo do parâmetro. Se essa verificação falhar, uma `System.ArrayTypeMismatchException` será lançada.

Métodos, indexadores e construtores de instância podem declarar seu parâmetro mais à direita para ser uma matriz de parâmetros ([matrizes de parâmetros](classes.md#parameter-arrays)). Esses membros de função são invocados em seu formato normal ou em sua forma expandida, dependendo do que é aplicável ([membro de função aplicável](expressions.md#applicable-function-member)):

*  Quando um membro de função com uma matriz de parâmetros é invocado em seu formato normal, o argumento fornecido para a matriz de parâmetros deve ser uma única expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetros. Nesse caso, a matriz de parâmetros funciona precisamente como um parâmetro de valor.
*  Quando um membro de função com uma matriz de parâmetros é invocado em seu formato expandido, a invocação deve especificar zero ou mais argumentos posicionais para a matriz de parâmetros, em que cada argumento é uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros. Nesse caso, a invocação cria uma instância do tipo de matriz de parâmetros com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecidos e usa a instância de matriz recém-criada como o real argumento.

As expressões de uma lista de argumentos são sempre avaliadas na ordem em que são gravadas. Portanto, o exemplo
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
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

As regras de covariância de matriz ([covariância de matriz](arrays.md#array-covariance)) permitem que um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que exista uma conversão de referência implícita de `B` para `A`. Devido a essas regras, quando um elemento de matriz de um *reference_type* é passado como um parâmetro de referência ou de saída, uma verificação de tempo de execução é necessária para garantir que o tipo de elemento real da matriz seja idêntico ao do parâmetro. No exemplo
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
a segunda invocação de `F` faz com que uma `System.ArrayTypeMismatchException` seja gerada porque o tipo de elemento real de `b` é `string` e não `object`.

Quando um membro de função com uma matriz de parâmetros é invocado em seu formato expandido, a invocação é processada exatamente como se uma expressão de criação de matriz com um inicializador de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)) foi inserida em volta dos parâmetros expandidos. Por exemplo, dada a declaração
```csharp
void F(int x, int y, params object[] args);
```
as seguintes invocações da forma expandida do método
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
corresponder exatamente a
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Em particular, observe que uma matriz vazia é criada quando há zero argumentos fornecidos para a matriz de parâmetros.

Quando os argumentos são omitidos de um membro de função com parâmetros opcionais correspondentes, os argumentos padrão da declaração de membro de função são passados implicitamente. Como elas são sempre constantes, sua avaliação não afetará a ordem de avaliação dos argumentos restantes.

### <a name="type-inference"></a>Inferência de tipos

Quando um método genérico é chamado sem especificar argumentos de tipo, um processo de ***inferência de tipos*** tenta inferir argumentos de tipo para a chamada. A presença da inferência de tipos permite que uma sintaxe mais conveniente seja usada para chamar um método genérico e permite que o programador Evite especificar informações de tipo redundante. Por exemplo, dada a declaração do método:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
é possível invocar o método `Choose` sem especificar explicitamente um argumento de tipo:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Por meio da inferência de tipos, os argumentos de tipo `int` e `string` são determinados dos argumentos para o método.

A inferência de tipos ocorre como parte do processamento do tempo de associação de uma invocação de método ([invocações de método](expressions.md#method-invocations)) e ocorre antes da etapa de resolução de sobrecarga da invocação. Quando um grupo de métodos específico é especificado em uma invocação de método e nenhum argumento de tipo é especificado como parte da invocação de método, a inferência de tipos é aplicada a cada método genérico no grupo de métodos. Se a inferência de tipos for realizada com sucesso, os argumentos de tipo deduzidos serão usados para determinar os tipos de argumentos para resolução de sobrecarga subsequente. Se a resolução de sobrecarga escolher um método genérico como o para invocar, os argumentos de tipo inferido serão usados como os argumentos de tipo real para a invocação. Se a inferência de tipos para um determinado método falhar, esse método não participará da resolução de sobrecarga. A falha da inferência de tipos, por si só, não causa um erro de tempo de ligação. No entanto, geralmente leva a um erro de tempo de ligação quando a resolução de sobrecarga não consegue encontrar nenhum método aplicável.

Se o número de argumentos fornecido for diferente do número de parâmetros no método, a inferência falhará imediatamente. Caso contrário, suponha que o método genérico tenha a seguinte assinatura:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Com uma chamada de método do formulário `M(E1...Em)` a tarefa de tipo inferência é encontrar argumentos de tipo exclusivo `S1...Sn` para cada um dos parâmetros de tipo `X1...Xn` de forma que a chamada `M<S1...Sn>(E1...Em)` se torne válida.

Durante o processo de inferência, cada parâmetro de tipo `Xi` é *corrigido* para um tipo específico `Si` ou não *fixo* com um conjunto associado de *limites*. Cada um dos limites é algum tipo `T`. Inicialmente, cada variável de tipo `Xi` é desfixada com um conjunto vazio de limites.

A inferência de tipos ocorre em fases. Cada fase tentará inferir argumentos de tipo para mais variáveis de tipo com base nas conclusões da fase anterior. A primeira fase faz algumas inferências iniciais de limites, enquanto a segunda fase corrige variáveis de tipo para tipos específicos e infere limites adicionais. A segunda fase pode ter que ser repetida várias vezes.

*Observação:* A inferência de tipos ocorre não apenas quando um método genérico é chamado. A inferência de tipos para a conversão de grupos de métodos é descrita na [inferência de tipos para a conversão de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) e a localização do melhor tipo comum de um conjunto de expressões é descrita na [localização do melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>A primeira fase

Para cada um dos argumentos do método `Ei`:

*   Se `Ei` for uma função anônima, uma *inferência de tipo de parâmetro explícita* ([induçãos de tipo de parâmetro explícita](expressions.md#explicit-parameter-type-inferences)) será feita de `Ei` para `Ti`
*   Caso contrário, se `Ei` tiver um tipo `U` e `xi` for um parâmetro de valor, uma *inferência de limite inferior* será feita *de* `U` *para* `Ti`.
*   Caso contrário, se `Ei` tiver um tipo `U` e `xi` for um parâmetro `ref` ou `out`, uma *inferência exata* será feita *de* `U` *para* `Ti`.
*   Caso contrário, nenhuma inferência é feita para esse argumento.


#### <a name="the-second-phase"></a>A segunda fase

A segunda fase continua da seguinte maneira:

*   Todas as variáveis de tipo não *fixas* `Xi` que não *dependem* ([dependência](expressions.md#dependence)) quaisquer `Xj` são fixas ([corrigindo](expressions.md#fixing)).
*   Se não existir nenhuma variável de tipo, todas as variáveis de tipo não *fixas* `Xi` serão *corrigidas* para cada uma das seguintes isenções:
    *   Há pelo menos uma variável de tipo `Xj` que depende `Xi`
    *   `Xi` tem um conjunto não vazio de limites
*   Se nenhuma das variáveis de tipo existirem e ainda houver variáveis de tipo não *fixas* , a inferência de tipos falhará.
*   Caso contrário, se nenhuma variável de tipo não *fixa* existir, a inferência de tipos terá sucesso.
*   Caso contrário, para todos os argumentos `Ei` com o tipo de parâmetro correspondente `Ti` em que os *tipos de saída* ([tipos de saída](expressions.md#output-types)) contêm variáveis de tipo não *fixos* `Xj` mas os *tipos de entrada* (tipos de[entrada](expressions.md#input-types)) não são, uma *inferência de tipo de saída* (inferências de[tipo de saída](expressions.md#output-type-inferences)) é feita *de* `Ei` *para* `Ti`. Em seguida, a segunda fase é repetida.

#### <a name="input-types"></a>Tipos de entrada

Se `E` for um grupo de métodos ou uma função anônima digitada implicitamente e `T` for um tipo delegado ou tipo de árvore de expressão, todos os tipos de parâmetro de `T` serão *tipos de entrada* de `E` *com o tipo* `T`.

####  <a name="output-types"></a>Tipos de saída

Se `E` for um grupo de métodos ou uma função anônima e `T` for um tipo delegado ou tipo de árvore de expressão, o tipo de retorno de `T` será um *tipo de saída de* `E` *com o tipo* `T`.

#### <a name="dependence"></a>Dependência

Uma variável de tipo não *fixa* `Xi` *depende diretamente de* uma variável de tipo não fixa `Xj` se for algum argumento `Ek` com o tipo `Tk` `Xj` ocorrer em um *tipo de entrada* de `Ek` com o tipo `Tk` e `Xi` ocorrer em um tipo de *saída* de `Ek` com o tipo `Tk`.

`Xj` *depende* `Xi` se `Xj` *depende diretamente de* `Xi` ou se `Xi` *depende diretamente de* `Xk` e `Xk` *depende do* `Xj`. Assim, "depende" é o fechamento transitivo, mas não reflexivo, de "depende diretamente de".

#### <a name="output-type-inferences"></a>Inferências de tipo de saída

Uma *inferência de tipo de saída* é feita *de* uma expressão `E` *para* um tipo `T` da seguinte maneira:

*  Se `E` for uma função anônima com tipo de retorno inferido `U`[(tipo de retorno inferido](expressions.md#inferred-return-type)) e `T` for um tipo delegado ou tipo de árvore de expressão com tipo de retorno `Tb`, uma *inferência de limite inferior* ([inferências de limite inferior](expressions.md#lower-bound-inferences)) será feita *de* `U` *para* `Tb`.
*  Caso contrário, se `E` for um grupo de métodos e `T` for um tipo delegado ou tipo de árvore de expressão com tipos de parâmetro `T1...Tk` e tipo de retorno `Tb`, e a resolução de sobrecarga de `E` com os tipos `T1...Tk` gerar um único método com o tipo de retorno `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `Tb`.
*  Caso contrário, se `E` for uma expressão com o tipo `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `T`.
*  Caso contrário, nenhuma inferência será feita.

#### <a name="explicit-parameter-type-inferences"></a>Inferências de tipo de parâmetro explícito

Uma *inferência de tipo de parâmetro explícita* é feita *de* uma expressão `E` *para* um tipo `T` da seguinte maneira:

*  Se `E` é uma função anônima explicitamente tipada com tipos de parâmetro `U1...Uk` e `T` é um tipo delegado ou tipo de árvore de expressão com tipos de parâmetro `V1...Vk` para cada `Ui` uma *inferência exata* ([inferências exatas](expressions.md#exact-inferences)) é feita *de* `Ui` *para* o `Vi`correspondente.

#### <a name="exact-inferences"></a>Inferências exatas

Uma *inferência exata* *de* um tipo `U` *a* um tipo `V` é feita da seguinte maneira:

*  Se `V` for um dos `Xi` não *corrigidos* , `U` será adicionado ao conjunto de limites exatos para `Xi`.

*  Caso contrário, os conjuntos `V1...Vk` e `U1...Uk` são determinados verificando se algum dos casos a seguir se aplica:

   *  `V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` da mesma classificação
   *  `V` é o tipo `V1?` e `U` é o tipo `U1?`
   *  `V` é um tipo construído `C<V1...Vk>`e `U` é um tipo construído `C<U1...Uk>`

   Se qualquer um desses casos se aplicar, uma *inferência exata* será feita *de* cada `Ui` *ao* `Vi`correspondente.

*  Caso contrário, nenhuma inferência será feita.

#### <a name="lower-bound-inferences"></a>Inferências de limite inferior

Uma *inferência de limite inferior* *de* um tipo `U` *a* um tipo `V` é feita da seguinte maneira:

*  Se `V` for um dos `Xi` não *corrigidos* , `U` será adicionado ao conjunto de limites inferiores para `Xi`.
*  Caso contrário, se `V` for o tipo `V1?`e `U` for o tipo `U1?`, uma inferência de limite inferior será feita de `U1` para `V1`.
*  Caso contrário, os conjuntos `U1...Uk` e `V1...Vk` são determinados verificando se algum dos casos a seguir se aplica:
   *  `V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` (ou um parâmetro de tipo cujo tipo base efetivo é `U1[...]`) da mesma classificação
   *  `V` é uma das `IEnumerable<V1>`, `ICollection<V1>` ou `IList<V1>` e `U` é um tipo de matriz unidimensional `U1[]`(ou um parâmetro de tipo cujo tipo base efetivo é `U1[]`)
   *  `V` é uma classe construída, struct, interface ou tipo delegado `C<V1...Vk>` e há um tipo exclusivo `C<U1...Uk>` de tal forma que `U` (ou, se `U` for um parâmetro de tipo, sua classe base efetiva ou qualquer membro de seu conjunto de interface eficaz) é idêntica a, herda de (direta ou indiretamente) ou implementa (direta ou indiretamente) `C<U1...Uk>`.

      (A restrição de "exclusividade" significa que, na interface de caso `C<T> {} class U: C<X>, C<Y> {}`, nenhuma inferência é feita ao se inferir de `U` para `C<T>` porque `U1` poderia ser `X` ou `Y`.)

   Se qualquer um desses casos se aplicar, uma inferência será feita *de* cada `Ui` *ao* `Vi` correspondente da seguinte maneira:

   *  Se `Ui` não for conhecido como um tipo de referência, uma *inferência exata* será feita
   *  Caso contrário, se `U` for um tipo de matriz, uma *inferência de limite inferior* será feita
   *  Caso contrário, se `V` for `C<V1...Vk>`, a inferência dependerá do parâmetro de tipo i-th de `C`:
      *  Se for covariant, uma *inferência de limite inferior* será feita.
      *  Se for contravariant, uma *inferência de limite superior* será feita.
      *  Se for invariável, uma *inferência exata* será feita.
*  Caso contrário, nenhuma inferência será feita.

#### <a name="upper-bound-inferences"></a>Inferências de limite superior

Uma *inferência de limite superior* *de* um tipo `U` *a* um tipo `V` é feita da seguinte maneira:

*  Se `V` for um dos `Xi` não *corrigidos* , `U` será adicionado ao conjunto de limites superiores para `Xi`.
*  Caso contrário, os conjuntos `V1...Vk` e `U1...Uk` são determinados verificando se algum dos casos a seguir se aplica:
   *  `U` é um tipo de matriz `U1[...]` e `V` é um tipo de matriz `V1[...]` da mesma classificação
   *  `U` é uma das `IEnumerable<Ue>`, `ICollection<Ue>` ou `IList<Ue>` e `V` é um tipo de matriz unidimensional `Ve[]`
   *  `U` é o tipo `U1?` e `V` é o tipo `V1?`
   *  `U` é uma classe construída, struct, interface ou tipo delegado `C<U1...Uk>` e `V` é uma classe, struct, interface ou tipo delegado, que é idêntica a, herda de (direta ou indiretamente) ou implementa (direta ou indiretamente) um tipo exclusivo `C<V1...Vk>`

      (A restrição de "exclusividade" significa que, se tivermos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, nenhuma inferência será feita ao fazer referência de `C<U1>` para `V<Q>`. Não são feitas inferências de `U1` para `X<Q>` ou `Y<Q>`.)

   Se qualquer um desses casos se aplicar, uma inferência será feita *de* cada `Ui` *ao* `Vi` correspondente da seguinte maneira:
   *  Se `Ui` não for conhecido como um tipo de referência, uma *inferência exata* será feita
   *  Caso contrário, se `V` for um tipo de matriz, uma *inferência de limite superior* será feita
   *  Caso contrário, se `U` for `C<U1...Uk>`, a inferência dependerá do parâmetro de tipo i-th de `C`:
      *  Se for covariant, uma *inferência de limite superior* será feita.
      *  Se for contravariant, uma *inferência de limite inferior* será feita.
      *  Se for invariável, uma *inferência exata* será feita.
*  Caso contrário, nenhuma inferência será feita.   

#### <a name="fixing"></a>Resolver

Uma variável de tipo não *fixa* `Xi` com um conjunto de limites é *fixa* da seguinte maneira:

*  O conjunto de *tipos candidatos* `Uj` começa como o conjunto de todos os tipos no conjunto de limites para `Xi`.
*  Em seguida, examinamos cada associado por `Xi` por sua vez: para cada `U` limite exato de `Xi` todos os tipos `Uj` que não são idênticos aos `U` são removidos do conjunto de candidatos. Para cada `U` limite inferior de `Xi` todos os tipos `Uj` para o qual *não* há uma conversão implícita de `U` são removidos do conjunto de candidatos. Para cada `U` limite superior de `Xi` todos os tipos `Uj` de onde *não* há uma conversão implícita para `U` são removidos do conjunto de candidatos.
*  Se entre os tipos candidatos restantes `Uj` houver um tipo exclusivo `V` do qual há uma conversão implícita para todos os outros tipos candidatos, `Xi` será corrigido para `V`.
*  Caso contrário, a inferência de tipos falhará.

#### <a name="inferred-return-type"></a>Tipo de retorno inferido

O tipo de retorno inferido de uma função anônima `F` é usado durante a inferência de tipos e resolução de sobrecarga. O tipo de retorno inferido só pode ser determinado para uma função anônima em que todos os tipos de parâmetro são conhecidos, seja porque eles são explicitamente fornecidos, fornecidos por meio de uma conversão de função anônima ou inferido durante a inferência de tipos em um genérico delimitador invocação de método.

O ***tipo de resultado deduzido*** é determinado da seguinte maneira:

*  Se o corpo de `F` for uma *expressão* que tem um tipo, o tipo de resultado inferido de `F` será o tipo dessa expressão.
*  Se o corpo de `F` for um *bloco* e o conjunto de expressões nas instruções `return` do bloco tiver um tipo mais comum `T` ([encontrando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), o tipo de resultado inferido de `F` será `T`.
*  Caso contrário, um tipo de resultado não pode ser inferido para `F`.

O ***tipo de retorno inferido*** é determinado da seguinte maneira:

*  Se `F` for Async e o corpo de `F` for uma expressão classificada como Nothing ([classificações de expressão](expressions.md#expression-classifications)) ou um bloco de instruções em que nenhuma instrução de retorno tem expressões, o tipo de retorno inferido será `System.Threading.Tasks.Task`
*  Se `F` for Async e tiver um tipo de resultado inferido `T`, o tipo de retorno inferido será `System.Threading.Tasks.Task<T>`.
*  Se `F` for não Async e tiver um tipo de resultado inferido `T`, o tipo de retorno inferido será `T`.
*  Caso contrário, não será possível inferir um tipo de retorno para `F`.

Como exemplo de inferência de tipos envolvendo funções anônimas, considere o método de extensão `Select` declarado na classe `System.Linq.Enumerable`:
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

Supondo que o namespace de `System.Linq` foi importado com uma cláusula `using` e dado uma classe `Customer` com uma propriedade `Name` do tipo `string`, o método `Select` pode ser usado para selecionar os nomes de uma lista de clientes:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

A invocação do método de extensão ([invocações do método de extensão](expressions.md#extension-method-invocations)) de `Select` é processada regravando a invocação para uma invocação de método estático:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Como os argumentos de tipo não foram especificados explicitamente, a inferência de tipos é usada para inferir os argumentos de tipo. Primeiro, o argumento `customers` está relacionado ao parâmetro `source`, inferindo `T` a ser `Customer`. Em seguida, usando o processo de inferência de tipo de função anônima descrito acima, `c` recebe o tipo `Customer`e a expressão `c.Name` está relacionada ao tipo de retorno do parâmetro `selector`, inferindo `S` a ser `string`. Portanto, a invocação é equivalente a
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
e o resultado é do tipo `IEnumerable<string>`.

O exemplo a seguir demonstra como a inferência de tipo de função anônima permite que informações de tipo "Flow" Entrem argumentos em uma invocação de método genérico. Dado o método:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Inferência de tipos para a invocação:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
continua da seguinte maneira: primeiro, o argumento `"1:15:30"` está relacionado ao parâmetro `value`, informando `X` a ser `string`. Em seguida, o parâmetro da primeira função anônima, `s`, recebe o tipo inferido `string`e a expressão `TimeSpan.Parse(s)` está relacionada ao tipo de retorno de `f1`, inferindo `Y` a ser `System.TimeSpan`. Por fim, o parâmetro da segunda função anônima, `t`, recebe o tipo inferido `System.TimeSpan`e a expressão `t.TotalSeconds` está relacionada ao tipo de retorno de `f2`, inferindo `Z` a ser `double`. Assim, o resultado da invocação é do tipo `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Inferência de tipos para conversão de grupos de métodos

Semelhante às chamadas de métodos genéricos, a inferência de tipos também deve ser aplicada quando um grupo de método `M` contendo um método genérico é convertido em um determinado tipo de delegado `D` ([conversões de grupo de métodos](conversions.md#method-group-conversions)). Dado um método
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
e o grupo de métodos `M` sendo atribuído ao tipo delegado `D` a tarefa da inferência de tipos é encontrar argumentos de tipo `S1...Sn` para que a expressão:
```csharp
M<S1...Sn>
```
torna-se compatível ([delegar declarações](delegates.md#delegate-declarations)) com `D`.

Ao contrário do algoritmo de inferência de tipos para chamadas de método genérico, nesse caso, há apenas *tipos*de argumento, sem *expressões*de argumento. Em particular, não existem funções anônimas e, portanto, não há necessidade de várias fases de inferência.

Em vez disso, todos os `Xi` são considerados não *corrigidos*e uma *inferência de limite inferior* é feita *de* cada tipo de argumento `Uj` de `D` *para* o tipo de parâmetro correspondente `Tj` de `M`. Se para qualquer um dos `Xi` nenhum limite for encontrado, a inferência de tipos falhará. Caso contrário, todos os `Xi` são *corrigidos* para `Si`correspondentes, que são o resultado da inferência de tipos.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Encontrando o melhor tipo comum de um conjunto de expressões

Em alguns casos, um tipo comum precisa ser inferido para um conjunto de expressões. Em particular, os tipos de elemento de matrizes tipadas implicitamente e os tipos de retorno de funções anônimas com corpos de *bloco* são encontrados dessa maneira.

Intuitivamente, dado um conjunto de expressões `E1...Em` essa inferência deve ser equivalente à chamada de um método
```csharp
Tr M<X>(X x1 ... X xm)
```
com o `Ei` como argumentos.

Mais precisamente, a inferência começa com uma variável de tipo não *fixa* `X`. As *inferências de tipo de saída* são feitas *de* cada `Ei` *para* `X`. Por fim, `X` é *fixo* e, se for bem-sucedido, o tipo resultante `S` é o melhor tipo comum resultante para as expressões. Se não existir tal `S`, as expressões não terão o melhor tipo comum.

### <a name="overload-resolution"></a>Resolução de sobrecarga

A resolução de sobrecarga é um mecanismo de tempo de ligação para selecionar o melhor membro de função para invocar uma lista de argumentos e um conjunto de membros de funções candidatas. Resolução de sobrecarga seleciona o membro da função a ser invocado nos seguintes contextos distintos em C#:

*  Invocação de um método chamado em um *invocation_expression* ([invocações de método](expressions.md#method-invocations)).
*  Invocação de um construtor de instância chamado em um *object_creation_expression* ([expressões de criação de objeto](expressions.md#object-creation-expressions)).
*  Invocação de um acessador de indexador por meio de um *element_access* ([acesso de elemento](expressions.md#element-access)).
*  Invocação de um operador predefinido ou definido pelo usuário referenciado em uma expressão (resolução de[sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)).

Cada um desses contextos define o conjunto de membros da função candidata e a lista de argumentos de forma exclusiva, conforme descrito em detalhes nas seções listadas acima. Por exemplo, o conjunto de candidatos para uma invocação de método não inclui métodos marcados `override` ([pesquisa de membro](expressions.md#member-lookup)) e os métodos em uma classe base não serão candidatos se qualquer método em uma classe derivada for aplicável ([invocações de método](expressions.md#method-invocations)).

Depois que os membros da função candidata e a lista de argumentos tiverem sido identificados, a seleção do melhor membro de função será a mesma em todos os casos:

*  Dado o conjunto de membros da função candidata aplicável, o melhor membro da função nesse conjunto está localizado. Se o conjunto contiver apenas um membro de função, esse membro de função será o melhor membro de função. Caso contrário, o melhor membro da função é o membro de uma função que é melhor do que todos os outros membros da função em relação à lista de argumentos fornecida, desde que cada membro da função seja comparado com todos os outros membros da função usando as regras no [melhor membro da função](expressions.md#better-function-member). Se não houver exatamente um membro da função que seja melhor do que todos os outros membros da função, a invocação do membro da função será ambígua e ocorrerá um erro de tempo de vinculação.

As seções a seguir definem os significados exatos do ***membro da função aplicável*** de termos e ***um melhor membro de função***.

#### <a name="applicable-function-member"></a>Membro da função aplicável

Um membro de função é considerado um ***membro de função aplicável*** em relação a uma lista de argumentos `A` quando todas as seguintes são verdadeiras:

*  Cada argumento em `A` corresponde a um parâmetro na declaração de membro de função, conforme descrito em [parâmetros correspondentes](expressions.md#corresponding-parameters), e qualquer parâmetro ao qual nenhum argumento corresponde é um parâmetro opcional.
*  Para cada argumento em `A`, o modo de passagem de parâmetro do argumento (ou seja, value, `ref`ou `out`) é idêntico ao modo de passagem de parâmetro do parâmetro correspondente e
   *  para um parâmetro de valor ou uma matriz de parâmetros, existe uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) do argumento para o tipo do parâmetro correspondente ou
   *  para um parâmetro `ref` ou `out`, o tipo do argumento é idêntico ao tipo do parâmetro correspondente. Afinal, um parâmetro `ref` ou `out` é um alias para o argumento passado.

Para um membro de função que inclui uma matriz de parâmetros, se o membro da função for aplicável pelas regras acima, ele será considerado como aplicável em seu ***formato normal***. Se um membro de função que inclui uma matriz de parâmetros não for aplicável em seu formato normal, o membro da função poderá ser aplicável em seu ***formato expandido***:

*  O formulário expandido é construído pela substituição da matriz de parâmetros na declaração de membro de função por zero ou mais parâmetros de valor do tipo de elemento da matriz de parâmetros, de modo que o número de argumentos na lista de argumentos `A` corresponda ao número total de parâmetros. Se `A` tiver menos argumentos do que o número de parâmetros fixos na declaração de membro da função, a forma expandida do membro da função não poderá ser construída e, portanto, não será aplicável.
*  Caso contrário, o formulário expandido será aplicável se para cada argumento em `A` o modo de passagem de parâmetro do argumento for idêntico ao modo de passagem de parâmetro do parâmetro correspondente e
   *  para um parâmetro de valor fixo ou um parâmetro de valor criado pela expansão, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe do tipo do argumento para o tipo do parâmetro correspondente ou
   *  para um parâmetro `ref` ou `out`, o tipo do argumento é idêntico ao tipo do parâmetro correspondente.

#### <a name="better-function-member"></a>Melhor membro da função

Para fins de determinação do melhor membro de função, uma lista de argumentos desativados A é construída contendo apenas as expressões de argumento em si na ordem em que aparecem na lista de argumentos originais.

As listas de parâmetros para cada um dos membros da função candidata são construídos da seguinte maneira:

*  O formulário expandido será usado se o membro da função for aplicável somente no formulário expandido.
*  Parâmetros opcionais sem argumentos correspondentes são removidos da lista de parâmetros
*  Os parâmetros são reordenados para que eles ocorram na mesma posição que o argumento correspondente na lista de argumentos.

Dado uma lista de argumentos `A` com um conjunto de expressões de argumento `{E1, E2, ..., En}` e dois membros de função aplicáveis `Mp` e `Mq` com tipos de parâmetro `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}`, `Mp` é definido como um ***membro de função melhor*** do que `Mq` se

*  para cada argumento, a conversão implícita de `Ex` para `Qx` não é melhor do que a conversão implícita de `Ex` para `Px`e
*  para pelo menos um argumento, a conversão de `Ex` para `Px` é melhor do que a conversão de `Ex` para `Qx`.

Ao executar essa avaliação, se `Mp` ou `Mq` for aplicável em sua forma expandida, `Px` ou `Qx` se referirá a um parâmetro na forma expandida da lista de parâmetros.

Caso as sequências de tipo de parâmetro `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}` sejam equivalentes (ou seja, cada `Pi` tem uma conversão de identidade para o `Qi`correspondente), as seguintes regras de quebra de empate são aplicadas, em ordem, para determinar o melhor membro da função.

*  Se `Mp` for um método não genérico e `Mq` for um método genérico, `Mp` será melhor do que `Mq`.
*  Caso contrário, se `Mp` for aplicável em seu formato normal e `Mq` tiver uma matriz de `params` e for aplicável apenas em sua forma expandida, `Mp` será melhor do que `Mq`.
*  Caso contrário, se `Mp` tiver mais parâmetros declarados que `Mq`, `Mp` será melhor do que `Mq`. Isso pode ocorrer se ambos os métodos tiverem matrizes `params` e forem aplicáveis somente em suas formas expandidas.
*  Caso contrário, se todos os parâmetros de `Mp` tiverem um argumento correspondente, enquanto os argumentos padrão precisarão ser substituídos por pelo menos um parâmetro opcional em `Mq`, `Mp` será melhor do que `Mq`.
*  Caso contrário, se `Mp` tiver tipos de parâmetro mais específicos do que `Mq`, `Mp` será melhor do que `Mq`. Permita que `{R1, R2, ..., Rn}` e `{S1, S2, ..., Sn}` representem os tipos de parâmetros não instanciados e não expandidos de `Mp` e `Mq`. os tipos de parâmetro de `Mp`são mais específicos do que `Mq`se, para cada parâmetro, `Rx` não for menos específico que `Sx`e, para pelo menos um parâmetro, `Rx` for mais específico que `Sx`:
   *  Um parâmetro de tipo é menos específico que um parâmetro sem tipo.
   *  Recursivamente, um tipo construído é mais específico do que outro tipo construído (com o mesmo número de argumentos de tipo) se pelo menos um argumento de tipo for mais específico e nenhum argumento de tipo for menos específico do que o argumento de tipo correspondente no outro.
   *  Um tipo de matriz é mais específico do que outro tipo de matriz (com o mesmo número de dimensões) se o tipo de elemento do primeiro for mais específico do que o tipo de elemento do segundo.
*  Caso contrário, se um membro for um operador não-comparado e o outro for um operador de aumento, o que não foi levantado será melhor.
*  Caso contrário, nenhum membro da função é melhor.

#### <a name="better-conversion-from-expression"></a>Melhor conversão de expressão

Dado um `C1` de conversão implícita que converte de uma expressão `E` para um tipo `T1`e um `C2` de conversão implícita que converte de uma expressão `E` em um tipo `T2`, `C1` é uma ***conversão melhor*** do que `C2` se `E` não corresponde exatamente a `T2` e pelo menos uma das seguintes isenções:

* `E` corresponde exatamente `T1` ([expressão exatamente correspondente](expressions.md#exactly-matching-expression))
* `T1` é um destino de conversão melhor do que `T2` ([melhor destino de conversão](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Expressão exatamente correspondente

Dado um `E` de expressão e um tipo `T`, `E` exatamente corresponde `T` se uma das seguintes isenções:

*  `E` tem um tipo `S`, e existe uma conversão de identidade de `S` para `T`
*  `E` é uma função anônima, `T` é um tipo delegado `D` ou um tipo de árvore de expressão `Expression<D>` e uma das seguintes isenções:
   *  Um tipo de retorno inferido `X` existe para `E` no contexto da lista de parâmetros de `D` ([tipo de retorno inferido](expressions.md#inferred-return-type)) e uma conversão de identidade existe de `X` para o tipo de retorno de `D`
   *  O `E` é não Async e `D` tem um tipo de retorno `Y` ou `E` é Async e `D` tem um tipo de retorno `Task<Y>`e uma das seguintes isenções:
      * O corpo de `E` é uma expressão que corresponde exatamente `Y`
      * O corpo de `E` é um bloco de instruções em que cada instrução de retorno retorna uma expressão que corresponde exatamente a `Y`

#### <a name="better-conversion-target"></a>Melhor destino de conversão

Considerando dois tipos diferentes `T1` e `T2`, `T1` é um destino de conversão melhor do que `T2` se nenhuma conversão implícita de `T2` para `T1` existir e pelo menos uma das seguintes isenções:

*  Uma conversão implícita de `T1` para `T2` existe
*  `T1` é um tipo delegado `D1` ou um tipo de árvore de expressão `Expression<D1>`, `T2` é um tipo delegado `D2` ou um tipo de árvore de expressão `Expression<D2>`, `D1` tem um tipo de retorno `S1` e uma das seguintes isenções:
   * a `D2` está sendo anulada retornando
   * `D2` tem um tipo de retorno `S2`e `S1` é um destino de conversão melhor do que o `S2`
*  `T1` é `Task<S1>`, `T2` é `Task<S2>`e `S1` é um destino de conversão melhor do que o `S2`
*  `T1` é `S1` ou `S1?` onde `S1` é um tipo integral assinado e `T2` é `S2` ou `S2?` em que `S2` é um tipo integral não assinado. Especificamente:
   * `S1` é `sbyte` e `S2` é `byte`, `ushort`, `uint`ou `ulong`
   * `S1` é `short` e `S2` é `ushort`, `uint`ou `ulong`
   * `S1` é `int` e `S2` é `uint`ou `ulong`
   * `S1` é `long` e `S2` é `ulong`

#### <a name="overloading-in-generic-classes"></a>Sobrecarregando em classes genéricas

Embora as assinaturas como declaradas devam ser exclusivas, é possível que a substituição dos argumentos de tipo resulte em assinaturas idênticas. Nesses casos, as regras de quebra de sobrecarga da resolução de sobrecarga acima escolherão o membro mais específico.

Os exemplos a seguir mostram sobrecargas que são válidas e inválidas de acordo com esta regra:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Verificação de tempo de compilação da resolução dinâmica de sobrecarga

Para operações associadas mais dinamicamente, o conjunto de possíveis candidatos para resolução é desconhecido em tempo de compilação. Em determinados casos, no entanto, o conjunto candidato é conhecido em tempo de compilação:

*  Chamadas de método estático com argumentos dinâmicos
*  O método de instância chama onde o receptor não é uma expressão dinâmica
*  Chamadas do indexador onde o receptor não é uma expressão dinâmica
*  Chamadas de construtor com argumentos dinâmicos

Nesses casos, uma verificação limitada de tempo de compilação é executada para cada candidato para ver se qualquer uma delas poderia ser aplicada em tempo de execução. Essa verificação consiste nas seguintes etapas:

*  Inferência de tipo parcial: qualquer argumento de tipo que não dependa direta ou indiretamente em um argumento do tipo `dynamic` é inferido usando as regras de [inferência de tipo](expressions.md#type-inference). Os argumentos de tipo restantes são desconhecidos.
*  Verificação de aplicabilidade parcial: a aplicabilidade é verificada de acordo com o [membro de função aplicável](expressions.md#applicable-function-member), mas ignorando parâmetros cujos tipos são desconhecidos.
*  Se nenhum candidato passar nesse teste, ocorrerá um erro em tempo de compilação.

### <a name="function-member-invocation"></a>Invocação de membro de função

Esta seção descreve o processo que ocorre em tempo de execução para invocar um membro de função específico. Supõe-se que um processo de tempo de ligação já determinou o membro específico para invocar, possivelmente aplicando a resolução de sobrecarga a um conjunto de membros de funções candidatas.

Para fins de descrição do processo de invocação, os membros da função são divididos em duas categorias:

*  Membros da função estática. Esses são construtores de instância, métodos estáticos, acessadores de propriedade estática e operadores definidos pelo usuário. Membros de função estática são sempre não virtuais.
*  Membros da função de instância. Esses são métodos de instância, acessadores de propriedade de instância e acessadores do indexador. Os membros da função de instância são não virtuais ou virtuais e são sempre invocados em uma instância específica. A instância é computada por uma expressão de instância e torna-se acessível dentro do membro da função como `this` ([esse acesso](expressions.md#this-access)).

O processamento em tempo de execução de uma invocação de membro de função consiste nas etapas a seguir, em que `M` é o membro da função e, se `M` for um membro de instância, `E` será a expressão de instância:

*  Se `M` for um membro de função estática:
   * A lista de argumentos é avaliada conforme descrito em [listas de argumentos](expressions.md#argument-lists).
   * `M` é invocado.

*  Se `M` for um membro de função de instância declarado em uma *value_type*:
   * `E` é avaliado. Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.
   * Se `E` não for classificada como uma variável, uma variável local temporária do tipo de `E`será criada e o valor de `E` será atribuído a essa variável. `E`, em seguida, é reclassificado como uma referência a essa variável local temporária. A variável temporária é acessível como `this` em `M`, mas não de nenhuma outra maneira. Assim, somente quando `E` é uma variável true é possível que o chamador Observe as alterações que `M` faz para `this`.
   * A lista de argumentos é avaliada conforme descrito em [listas de argumentos](expressions.md#argument-lists).
   * `M` é invocado. A variável referenciada por `E` se torna a variável referenciada por `this`.

*  Se `M` for um membro de função de instância declarado em uma *reference_type*:
   * `E` é avaliado. Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.
   * A lista de argumentos é avaliada conforme descrito em [listas de argumentos](expressions.md#argument-lists).
   * Se o tipo de `E` for uma *value_type*, uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) será executada para converter `E` para o tipo `object`e `E` será considerado como sendo do tipo `object` nas etapas a seguir. Nesse caso, `M` poderia ser apenas um membro de `System.Object`.
   * O valor de `E` é verificado para ser válido. Se o valor de `E` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.
   * A implementação do membro da função a ser invocada é determinada:
     * Se o tipo de tempo de associação de `E` for uma interface, o membro da função a ser invocado será a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`. Esse membro de função é determinado pela aplicação das regras de mapeamento de interface ([mapeamento de interface](interfaces.md#interface-mapping)) para determinar a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.
     * Caso contrário, se `M` for um membro de função virtual, o membro da função a ser invocado será a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`. Esse membro de função é determinado pela aplicação das regras para determinar a implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) de `M` em relação ao tipo de tempo de execução da instância referenciada por `E`.
     * Caso contrário, `M` é um membro de função não virtual e o membro da função a ser invocado é `M` ele mesmo.
   * A implementação do membro da função determinada na etapa acima é invocada. O objeto referenciado por `E` se torna o objeto referenciado por `this`.

#### <a name="invocations-on-boxed-instances"></a>Invocações em instâncias em caixas

Um membro de função implementado em um *value_type* pode ser invocado por meio de uma instância em caixa do que *value_type* nas seguintes situações:

*  Quando o membro da função é uma `override` de um método herdado do tipo `object` e é invocado por meio de uma expressão de instância do tipo `object`.
*  Quando o membro da função é uma implementação de um membro da função de interface e é invocado por meio de uma expressão de instância de um *interface_type*.
*  Quando o membro da função é invocado por meio de um delegado.

Nessas situações, a instância em caixa é considerada para conter uma variável do *value_type*, e essa variável se torna a variável referenciada por `this` na invocação de membro da função. Em particular, isso significa que, quando um membro de função é invocado em uma instância em caixa, é possível que o membro da função modifique o valor contido na instância em caixa.

## <a name="primary-expressions"></a>Expressões primárias

As expressões primárias incluem as formas mais simples de expressões.

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

As expressões primárias são divididas entre *array_creation_expression*s e *primary_no_array_creation_expression*s. Tratar a expressão de criação de matriz dessa maneira, em vez de listá-la junto com outros formulários de expressão simples, permite que a gramática inpermita códigos potencialmente confusos, como
```csharp
object o = new int[3][1];
```
que, de outra forma, seriam interpretados como
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>{1&gt;Literais&lt;1}

Um *primary_expression* que consiste em um *literal* ([literais](lexical-structure.md#literals)) é classificado como um valor.


### <a name="interpolated-strings"></a>Cadeias de caracteres interpoladas

Um *interpolated_string_expression* consiste em um sinal de `$` seguido por um literal de cadeia de caracteres regular ou textual, no qual os buracos, delimitados por `{` e `}`, incluem expressões e especificações de formatação. Uma expressão de cadeia de caracteres interpolada é o resultado de um *interpolated_string_literal* que foi dividido em tokens individuais, conforme descrito em [literais de cadeia de caracteres interpolados](lexical-structure.md#interpolated-string-literals).

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

O *constant_expression* em uma interpolação deve ter uma conversão implícita para `int`.

Um *interpolated_string_expression* é classificado como um valor. Se ele for imediatamente convertido em `System.IFormattable` ou `System.FormattableString` com uma conversão de cadeia de caracteres interpolada implícita ([conversões de cadeia de caracteres interpoladas implícitas](conversions.md#implicit-interpolated-string-conversions)), a expressão de cadeia de caracteres interpolada terá esse tipo. Caso contrário, ele tem o tipo `string`.

Se o tipo de uma cadeia de caracteres interpolada for `System.IFormattable` ou `System.FormattableString`, o significado será uma chamada para `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Se o tipo for `string`, o significado da expressão será uma chamada para `string.Format`. Em ambos os casos, a lista de argumentos da chamada consiste em um literal de cadeia de caracteres de formato com espaços reservados para cada interpolação e um argumento para cada expressão correspondente aos detentores de lugar.

A cadeia de caracteres de formato literal é construída da seguinte maneira, em que `N` é o número de interpolações no *interpolated_string_expression*:

*  Se um *interpolated_regular_string_whole* ou um *interpolated_verbatim_string_whole* seguir o sinal de `$`, a cadeia de caracteres de formato literal será esse token.
*  Caso contrário, o literal de cadeia de caracteres de formato consiste em: 
   *  Primeiro *interpolated_regular_string_start* ou *interpolated_verbatim_string_start*
   *  Em seguida, para cada número `I` de `0` para `N-1`: 
      * A representação decimal de `I`
      * Em seguida, se a *interpolação* correspondente tiver um *constant_expression*, uma `,` (vírgula) seguida da representação decimal do valor do *constant_expression*
      * Em seguida, o *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* ou *interpolated_verbatim_string_end* imediatamente após a interpolação correspondente.

Os argumentos subsequentes são simplesmente as *expressões* das *interpolações* (se houver), em ordem.

TODO: exemplos.


### <a name="simple-names"></a>Nomes simples

Um *Simple_name* consiste em um identificador, opcionalmente seguido por uma lista de argumentos de tipo:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

Uma *Simple_name* está no formato `I` ou no formato `I<A1,...,Ak>`, em que `I` é um único identificador e `<A1,...,Ak>` é um *type_argument_list*opcional. Quando nenhum *type_argument_list* for especificado, considere `K` como zero. A *Simple_name* é avaliada e classificada da seguinte maneira:

*  Se `K` for zero e o *Simple_name* aparecer em um *bloco* e se o espaço da declaração local ([declarações](basic-concepts.md#declarations)) do *bloco*(ou um *bloco*delimitador) contiver uma variável local, um parâmetro ou uma constante com o nome `I`, o *Simple_name* se refere a essa variável local, parâmetro ou constante e é classificado como uma variável ou valor.
*  Se `K` for zero e o *Simple_name* aparecer dentro do corpo de uma declaração de método genérico e se essa declaração incluir um parâmetro de tipo com o nome `I`, o *Simple_name* se referirá a esse parâmetro de tipo.
*  Caso contrário, para cada tipo de instância `T` ([o tipo de instância](classes.md#the-instance-type)), começando com o tipo de instância da declaração de tipo delimitadora imediatamente e continuando com o tipo de instância de cada declaração de classe ou struct de circunscrição (se houver):
   *  Se `K` for zero e a declaração de `T` incluir um parâmetro de tipo com o nome `I`, o *Simple_name* se referirá a esse parâmetro de tipo.
   *  Caso contrário, se uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` em `T` com argumentos de tipo de  `K`produzir uma correspondência:
      * Se `T` for o tipo de instância do tipo de classe ou struct de circunscrição imediatamente e a pesquisa identificar um ou mais métodos, o resultado será um grupo de métodos com uma expressão de instância associada de `this`. Se uma lista de argumentos de tipo tiver sido especificada, ela será usada na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).
      * Caso contrário, se `T` for o tipo de instância do tipo de classe ou struct imediatamente delimitadora, se a pesquisa identificar um membro de instância e se a referência ocorrer dentro do corpo de um construtor de instância, um método de instância ou um acessador de instância, o resultado será o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `this.I`. Isso só pode acontecer quando `K` é zero.
      * Caso contrário, o resultado será o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `T.I` ou `T.I<A1,...,Ak>`. Nesse caso, é um erro de tempo de associação para o *Simple_name* fazer referência a um membro de instância.

*  Caso contrário, para cada namespace `N`, começando com o namespace no qual o *Simple_name* ocorre, continuando com cada namespace delimitador (se houver) e terminando com o namespace global, as etapas a seguir são avaliadas até que uma entidade seja localizada:
   *  Se `K` for zero e `I` for o nome de um namespace no `N`, então:
      * Se o local em que a *Simple_name* ocorre estiver entre uma declaração de namespace para `N` e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` a um namespace ou tipo, a *Simple_name* será ambígua e ocorrerá um erro em tempo de compilação.
      * Caso contrário, a *Simple_name* se refere ao namespace chamado `I` no `N`.
   *  Caso contrário, se `N` contiver um tipo acessível com nome `I` e `K`parâmetros de tipo  , então:
      * Se `K` for zero e o local onde o *Simple_name* ocorrer for incluído por uma declaração de namespace para `N` e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` a um namespace ou tipo, a *Simple_name* será ambígua e ocorrerá um erro de tempo de compilação.
      * Caso contrário, o *namespace_or_type_name* se refere ao tipo construído com os argumentos de tipo fornecidos.
   *  Caso contrário, se o local em que o *Simple_name* ocorrer for incluído por uma declaração de namespace para `N`:
      * Se `K` for zero e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` a um namespace ou tipo importado, a *Simple_name* se referirá a esse namespace ou tipo.
      * Caso contrário, se os namespaces e as declarações de tipo importados pelo *using_namespace_directive*s e *using_static_directive*s da declaração de namespace contiverem exatamente um tipo acessível ou membro estático sem extensão com nome `I` e `K`parâmetros de tipo de  , o *Simple_name* se referirá a esse tipo ou membro construído com os argumentos de tipo fornecidos.
      * Caso contrário, se os namespaces e os tipos importados pelo *using_namespace_directive*s da declaração de namespace contiverem mais de um tipo acessível ou membro estático de método não-extensão com nome `I` e `K`parâmetros de tipo de  , o *Simple_name* será ambíguo e ocorrerá um erro.

   Observe que essa etapa inteira é exatamente paralela à etapa correspondente no processamento de um *namespace_or_type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)).

*  Caso contrário, o *Simple_name* será indefinido e ocorrerá um erro em tempo de compilação.


### <a name="parenthesized-expressions"></a>Expressões entre parênteses

Uma *parenthesized_expression* consiste em uma *expressão* entre parênteses.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Uma *parenthesized_expression* é avaliada avaliando a *expressão* dentro dos parênteses. Se a *expressão* dentro dos parênteses denota um namespace ou tipo, ocorre um erro de tempo de compilação. Caso contrário, o resultado da *parenthesized_expression* é o resultado da avaliação da *expressão*contida.

### <a name="member-access"></a>Acesso de membros

Um *member_access* consiste em um *primary_expression*, um *predefined_type*ou um *qualified_alias_member*, seguido por um token "`.`", seguido por um *identificador*, opcionalmente seguido por um *type_argument_list*.

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

A produção *qualified_alias_member* é definida em [qualificadores de alias de namespace](namespaces.md#namespace-alias-qualifiers).

Uma *member_access* está no formato `E.I` ou no formato `E.I<A1, ..., Ak>`, em que `E` é uma expressão primária, `I` é um único identificador e `<A1, ..., Ak>` é um *type_argument_list*opcional. Quando nenhum *type_argument_list* for especificado, considere `K` como zero.

Uma *member_access* com uma *primary_expression* do tipo `dynamic` é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o compilador classifica o acesso de membro como um acesso de Propriedade do tipo `dynamic`. As regras abaixo para determinar o significado da *member_access* são então aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação do *primary_expression*. Se essa classificação de tempo de execução leva a um grupo de métodos, o acesso de membro deve ser o *primary_expression* de um *invocation_expression*.

A *member_access* é avaliada e classificada da seguinte maneira:

*  Se `K` for zero e `E` for um namespace e `E` contiver um namespace aninhado com o nome `I`, o resultado será esse namespace.
*  Caso contrário, se `E` for um namespace e `E` contiver um tipo acessível com nome `I` e `K`parâmetros de tipo de  , o resultado será aquele tipo construído com os argumentos de tipo fornecidos.
*  Se `E` for uma *predefined_type* ou uma *primary_expression* classificada como um tipo, se `E` não for um parâmetro de tipo e se uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` em `E` com parâmetros de tipo `K` produzir uma correspondência, `E.I` será avaliado e classificado da seguinte maneira:
   *  Se `I` identificar um tipo, o resultado será aquele tipo construído com os argumentos de tipo fornecidos.
   *  Se `I` identificar um ou mais métodos, o resultado será um grupo de métodos sem nenhuma expressão de instância associada. Se uma lista de argumentos de tipo tiver sido especificada, ela será usada na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).
   *  Se `I` identificar uma propriedade `static`, o resultado será um acesso de propriedade sem nenhuma expressão de instância associada.
   *  Se `I` identificar um campo de `static`:
      * Se o campo for `readonly` e a referência ocorrer fora do construtor estático da classe ou struct no qual o campo é declarado, o resultado será um valor, ou seja, o valor do campo estático `I` em `E`.
      * Caso contrário, o resultado será uma variável, ou seja, o campo estático `I` em `E`.
   *  Se `I` identificar um evento de `static`:
      * Se a referência ocorrer dentro da classe ou struct na qual o evento é declarado e o evento tiver sido declarado sem *event_accessor_declarations* ([eventos](classes.md#events)), `E.I` será processado exatamente como se `I` fosse um campo estático.
      * Caso contrário, o resultado será um acesso de evento sem nenhuma expressão de instância associada.
   *  Se `I` identificar uma constante, o resultado será um valor, ou seja, o valor dessa constante.
    * Se `I` identificar um membro de enumeração, o resultado será um valor, ou seja, o valor desse membro de enumeração.
    * Caso contrário, `E.I` é uma referência de membro inválida e ocorre um erro em tempo de compilação.
*  Se `E` for um acesso de propriedade, acesso ao indexador, variável ou valor, o tipo de que é `T`e uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` em `T` com argumentos de tipo `K` produz uma correspondência, então `E.I` é avaliado e classificado da seguinte maneira:
   *  Primeiro, se `E` for um acesso de propriedade ou indexador, o valor da propriedade ou do acesso do indexador será obtido ([valores de expressões](expressions.md#values-of-expressions)) e `E` será reclassificado como um valor.
   *  Se `I` identificar um ou mais métodos, o resultado será um grupo de métodos com uma expressão de instância associada de `E`. Se uma lista de argumentos de tipo tiver sido especificada, ela será usada na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).
   *  Se `I` identificar uma propriedade de instância,
      * Se `E` for `this`, `I` identificará uma propriedade implementada automaticamente ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)) sem um setter, e a referência ocorrerá dentro de um construtor de instância para um tipo de classe ou struct `T`, o resultado será uma variável, ou seja, o campo de apoio oculto para a propriedade automática fornecida pelo `I` na instância de `T` fornecida pelo `this`.
      * Caso contrário, o resultado é um acesso de propriedade com uma expressão de instância associada de `E`.
   *  Se `T` for um *class_type* e `I` identificar um campo de instância desse *class_type*:
      * Se o valor de `E` for `null`, um `System.NullReferenceException` será gerado.
      * Caso contrário, se o campo for `readonly` e a referência ocorrer fora de um construtor de instância da classe na qual o campo é declarado, o resultado será um valor, ou seja, o valor do campo `I` no objeto referenciado por `E`.
      * Caso contrário, o resultado será uma variável, ou seja, o campo `I` no objeto referenciado por `E`.
   *  Se `T` for um *struct_type* e `I` identificar um campo de instância desse *struct_type*:
      * Se `E` for um valor ou se o campo for `readonly` e a referência ocorrer fora de um construtor de instância do struct no qual o campo é declarado, o resultado será um valor, ou seja, o valor do campo `I` na instância de struct fornecida pelo `E`.
      * Caso contrário, o resultado será uma variável, ou seja, o campo `I` na instância de struct fornecida por `E`.
   *  Se `I` identificar um evento de instância:
      * Se a referência ocorrer dentro da classe ou estrutura na qual o evento é declarado e o evento tiver sido declarado sem *event_accessor_declarations* ([eventos](classes.md#events)) e a referência não ocorrer como o lado esquerdo de um operador `+=` ou `-=`, `E.I` será processada exatamente como se `I` fosse um campo de instância.
      * Caso contrário, o resultado é um acesso de evento com uma expressão de instância associada de `E`.
*  Caso contrário, será feita uma tentativa de processar `E.I` como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)). Se isso falhar, `E.I` será uma referência de membro inválida e ocorrerá um erro de tempo de vinculação.

#### <a name="identical-simple-names-and-type-names"></a>Nomes de tipos e nomes simples idênticos

Em um acesso de membro do formulário `E.I`, se `E` for um único identificador, e se o significado de `E` como um *Simple_name* ([nomes simples](expressions.md#simple-names)) for uma constante, campo, propriedade, variável local ou parâmetro com o mesmo tipo que o significado de `E` como um *type_name* ([nomes de namespace e tipo](basic-concepts.md#namespace-and-type-names)), então ambos os significados possíveis de `E` são permitidos. Os dois significados possíveis de `E.I` nunca são ambíguos, pois `I` deve necessariamente ser um membro do tipo `E` em ambos os casos. Em outras palavras, a regra simplesmente permite o acesso aos membros estáticos e tipos aninhados de `E` em que, caso contrário, ocorreria um erro de tempo de compilação. Por exemplo:
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

#### <a name="grammar-ambiguities"></a>Ambiguidades gramaticais

As produções para *Simple_name* ([nomes simples](expressions.md#simple-names)) e *member_access* ([acesso de membro](expressions.md#member-access)) podem dar ao aumento das ambiguidades na gramática para expressões. Por exemplo, a instrução:
```csharp
F(G<A,B>(7));
```
pode ser interpretado como uma chamada para `F` com dois argumentos, `G < A` e `B > (7)`. Como alternativa, ele pode ser interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.

Se uma sequência de tokens puder ser analisada (no contexto) como *um Simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro](expressions.md#member-access)) ou *pointer_member_access* (acesso de[membro de ponteiro](unsafe-code.md#pointer-member-access)) terminando com um *type_argument_list* ([argumentos de tipo](types.md#type-arguments)), o token imediatamente após o `>` token de fechamento será examinado. Se for um de
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
em seguida, a *type_argument_list* é mantida como parte da *Simple_name*, *member_access* ou *pointer_member_access* e qualquer outra análise possível da sequência de tokens é descartada. Caso contrário, o *type_argument_list* não será considerado como parte do *Simple_name*, *member_access* ou *pointer_member_access*, mesmo que não haja outra análise possível da sequência de tokens. Observe que essas regras não são aplicadas ao analisar um *type_argument_list* em um *namespace_or_type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)). A instrução
```csharp
F(G<A,B>(7));
```
de acordo com essa regra, será interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular. As instruções
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
cada um será interpretado como uma chamada para `F` com dois argumentos. A instrução
```csharp
x = F < A > +y;
```
será interpretado como um operador menor que, maior que operador, e operador de adição unário, como se a instrução tivesse sido gravada `x = (F < A) > (+y)`, em vez de como um *Simple_name* com um *type_argument_list* seguido por um operador Binary Plus. Na instrução
```csharp
x = y is C<T> + z;
```
os tokens `C<T>` são interpretados como um *namespace_or_type_name* com um *type_argument_list*.

### <a name="invocation-expressions"></a>Expressões de invocação

Um *invocation_expression* é usado para invocar um método.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Uma *invocation_expression* é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) se pelo menos uma das seguintes isenções:

* O *primary_expression* tem `dynamic`de tipo de tempo de compilação.
* Pelo menos um argumento da *argument_list* opcional tem o tipo de tempo de compilação `dynamic` e o *primary_expression* não tem um tipo delegado.

Nesse caso, o compilador classifica o *invocation_expression* como um valor do tipo `dynamic`. As regras abaixo para determinar o significado do *invocation_expression* são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação dos argumentos *primary_expression* e que têm o tipo de tempo de compilação `dynamic`. Se o *primary_expression* não tiver o tipo de tempo de compilação `dynamic`, a invocação de método passará por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

O *primary_expression* de um *invocation_expression* deve ser um grupo de métodos ou um valor de um *delegate_type*. Se o *primary_expression* for um grupo de métodos, a *invocation_expression* será uma invocação de método ([invocações de método](expressions.md#method-invocations)). Se o *primary_expression* for um valor de um *delegate_type*, o *invocation_expression* será uma invocação de delegado ([invocações de delegado](expressions.md#delegate-invocations)). Se o *primary_expression* não for um grupo de métodos nem um valor de um *delegate_type*, ocorrerá um erro de tempo de associação.

O *argument_list* opcional ([listas de argumentos](expressions.md#argument-lists)) fornece valores ou referências de variáveis para os parâmetros do método.

O resultado da avaliação de um *invocation_expression* é classificado da seguinte maneira:

*  Se o *invocation_expression* invocar um método ou delegado que retorna `void`, o resultado será Nothing. Uma expressão que é classificada como nada é permitida apenas no contexto de uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)) ou como o corpo de uma *lambda_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)). Caso contrário, ocorrerá um erro de tempo de vinculação.
*  Caso contrário, o resultado será um valor do tipo retornado pelo método ou delegado.

#### <a name="method-invocations"></a>Invocações de método

Para uma invocação de método, a *primary_expression* do *invocation_expression* deve ser um grupo de métodos. O grupo de métodos identifica o método a ser invocado ou o conjunto de métodos sobrecarregados do qual escolher um método específico a ser invocado. No último caso, a determinação do método específico a ser invocado é baseada no contexto fornecido pelos tipos dos argumentos na *argument_list*.

O processamento de tempo de associação de uma invocação de método do formulário `M(A)`, em que `M` é um grupo de métodos (possivelmente incluindo um *type_argument_list*) e `A` é um *argument_list*opcional, consiste nas seguintes etapas:

*  O conjunto de métodos candidatos para a invocação do método é construído. Para cada método `F` associado ao grupo de métodos `M`:
   *  Se `F` não for genérica, `F` será um candidato quando:
      * `M` não tem nenhuma lista de argumentos de tipo e
      * `F` é aplicável em relação a `A` ([membro da função aplicável](expressions.md#applicable-function-member)).
   *  Se `F` for genérica e `M` não tiver nenhuma lista de argumentos de tipo, `F` será um candidato quando:
      * A inferência de tipos ([inferência de tipos](expressions.md#type-inference)) é bem sucedido, inferindo-se a uma lista de argumentos de tipo para a chamada e
      * Depois que os argumentos de tipo inferido são substituídos pelos parâmetros de tipo de método correspondentes, todos os tipos construídos na lista de parâmetros de F atendem às suas restrições ([atendendo às restrições](types.md#satisfying-constraints)) e a lista de parâmetros de `F` é aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)).
   *  Se `F` for genérica e `M` incluir uma lista de argumentos de tipo, `F` será um candidato quando:
      * `F` tem o mesmo número de parâmetros de tipo de método que foram fornecidos na lista de argumentos de tipo e
      * Depois que os argumentos de tipo são substituídos pelos parâmetros de tipo de método correspondentes, todos os tipos construídos na lista de parâmetros de F atendem às suas restrições ([atendendo às restrições](types.md#satisfying-constraints)) e a lista de parâmetros de `F` é aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)).
*  O conjunto de métodos candidatos é reduzido para conter apenas métodos dos tipos mais derivados: para cada método `C.F` no conjunto, em que `C` é o tipo no qual o método `F` é declarado, todos os métodos declarados em um tipo base de `C` são removidos do conjunto. Além disso, se `C` for um tipo de classe diferente de `object`, todos os métodos declarados em um tipo de interface serão removidos do conjunto. (Essa última regra só afeta quando o grupo de métodos era o resultado de uma pesquisa de membro em um parâmetro de tipo com uma classe base efetiva diferente de Object e uma interface efetiva não vazia definida.)
*  Se o conjunto resultante de métodos candidatos estiver vazio, o processamento adicional nas etapas a seguir será abandonado e, em vez disso, será feita uma tentativa de processar a invocação como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)). Se isso falhar, não existirá nenhum método aplicável e ocorrerá um erro de tempo de vinculação.
*  O melhor método do conjunto de métodos candidatos é identificado usando as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution). Se um único método melhor não puder ser identificado, a invocação do método será ambígua e ocorrerá um erro de tempo de ligação. Ao executar a resolução de sobrecarga, os parâmetros de um método genérico são considerados após a substituição dos argumentos de tipo (fornecidos ou inferidos) para os parâmetros de tipo de método correspondentes.
*  A validação final do melhor método escolhido é executada:
   * O método é validado no contexto do grupo de métodos: se o melhor método é um método estático, o grupo de métodos deve ter resultado de um *Simple_name* ou de um *member_access* por meio de um tipo. Se o melhor método for um método de instância, o grupo de métodos deverá ter resultado de uma *Simple_name*, uma *member_access* por meio de uma variável ou valor ou uma *base_access*. Se nenhum desses requisitos for verdadeiro, ocorrerá um erro de tempo de ligação.
   * Se o melhor método for um método genérico, os argumentos de tipo (fornecidos ou inferidos) serão verificados em relação às restrições ([atendendo às restrições](types.md#satisfying-constraints)) declaradas no método genérico. Se qualquer argumento de tipo não atender às restrições correspondentes no parâmetro de tipo, ocorrerá um erro de tempo de associação.

Depois que um método tiver sido selecionado e validado em tempo de vinculação pelas etapas acima, a invocação de tempo de execução real será processada de acordo com as regras de invocação de membro de função descrita na [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

O efeito intuitivo das regras de resolução descritas acima é o seguinte: para localizar o método específico invocado por uma invocação de método, comece com o tipo indicado pela invocação do método e continue a cadeia de herança até pelo menos uma aplicável, declaração de método acessível e não substituída encontrada. Em seguida, execute a inferência de tipos e a resolução de sobrecarga no conjunto de métodos aplicáveis, acessíveis e não substituição declarados nesse tipo e invoque o método, portanto, selecionado. Se nenhum método foi encontrado, tente em vez disso processar a invocação como uma invocação de método de extensão.

#### <a name="extension-method-invocations"></a>Invocações de método de extensão

Em uma invocação de método ([invocações em instâncias em caixas](expressions.md#invocations-on-boxed-instances)) de um dos formulários
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Se o processamento normal da invocação não encontrar nenhum método aplicável, será feita uma tentativa de processar a construção como uma invocação de método de extensão. Se *expr* ou qualquer um dos *args* tiver o tipo de tempo de compilação `dynamic`, os métodos de extensão não serão aplicados.

O objetivo é encontrar a melhor *type_name* `C`, para que a invocação de método estático correspondente possa ocorrer:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Um método de extensão `Ci.Mj` será ***elegível*** se:

*  `Ci` é uma classe não genérica e não aninhada
*  O nome do `Mj` é *identificador*
*  `Mj` é acessível e aplicável quando aplicado aos argumentos como um método estático, conforme mostrado acima
*  Existe uma identidade implícita, uma referência ou uma conversão boxing de *expr* para o tipo do primeiro parâmetro de `Mj`.

A pesquisa por `C` prossegue da seguinte maneira:

*  Começando com a declaração de namespace delimitadora mais próxima, continuando com cada declaração de namespace delimitadora e terminando com a unidade de compilação que a contém, as tentativas sucessivas são feitas para encontrar um conjunto candidato de métodos de extensão:
   * Se o namespace ou a unidade de compilação fornecida diretamente contiver declarações de tipo não genéricas `Ci` com métodos de extensão qualificados `Mj`, o conjunto desses métodos de extensão será o conjunto de candidatos.
   * Se tipos `Ci` importados por *using_static_declarations* e declarados diretamente em namespaces importados por *using_namespace_directive*s no namespace ou na unidade de compilação fornecida diretamente contêm métodos de extensão qualificados `Mj`, o conjunto desses métodos de extensão é o conjunto candidato.
*  Se nenhum conjunto candidato for encontrado em qualquer declaração de namespace ou unidade de compilação delimitadora ocorrer um erro em tempo de compilação.
*  Caso contrário, a resolução de sobrecarga será aplicada ao conjunto de candidatos, conforme descrito em ([resolução de sobrecarga](expressions.md#overload-resolution)). Se nenhum único método melhor for encontrado, ocorrerá um erro em tempo de compilação.
*  `C` é o tipo no qual o melhor método é declarado como um método de extensão.

Usando `C` como um destino, a chamada de método é processada como uma invocação de método estático ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

As regras anteriores significam que os métodos de instância têm precedência sobre os métodos de extensão, que os métodos de extensão disponíveis nas declarações de namespace interno têm precedência sobre os métodos de extensão disponíveis nas declarações de namespace externo e essa extensão os métodos declarados diretamente em um namespace têm precedência sobre os métodos de extensão importados para o mesmo namespace com uma diretiva de namespace using. Por exemplo:
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

No exemplo, o método de `B`tem precedência sobre o primeiro método de extensão e o método de `C`tem precedência sobre os dois métodos de extensão.

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
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` tem precedência sobre `C.G`e `E.F` tem precedência sobre `D.F` e `C.F`.

#### <a name="delegate-invocations"></a>Delegar invocações

Para uma invocação de delegado, a *primary_expression* do *invocation_expression* deve ser um valor de um *delegate_type*. Além disso, Considerando que a *delegate_type* seja um membro de função com a mesma lista de parâmetros que a *delegate_type*, a *delegate_type* deve ser aplicável ([membro de função aplicável](expressions.md#applicable-function-member)) em relação ao *argument_list* do *invocation_expression*.

O processamento em tempo de execução de uma invocação de delegado do formulário `D(A)`, em que `D` é uma *primary_expression* de um *delegate_type* e `A` é um *argument_list*opcional, consiste nas seguintes etapas:

*  `D` é avaliado. Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.
*  O valor de `D` é verificado para ser válido. Se o valor de `D` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.
*  Caso contrário, `D` é uma referência a uma instância de delegate. Invocações de membro de função ([verificação de tempo de compilação de resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) são executadas em cada uma das entidades que podem ser chamadas na lista de invocação do delegado. Para entidades que podem ser chamadas que consistem em um método de instância e instância, a instância para a invocação é a instância contida na entidade que pôde ser chamada.

### <a name="element-access"></a>Acesso ao elemento

Um *element_access* consiste em um *primary_no_array_creation_expression*, seguido por um token "`[`", seguido por um *argument_list*, seguido por um token "`]`". O *argument_list* consiste em um ou mais s de *argumento*, separados por vírgulas.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

O *argument_list* de um *element_access* não pode conter argumentos `ref` ou `out`.

Uma *element_access* é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) se pelo menos uma das seguintes isenções:

* O *primary_no_array_creation_expression* tem `dynamic`de tipo de tempo de compilação.
* Pelo menos uma expressão da *argument_list* tem o tipo de tempo de compilação `dynamic` e a *primary_no_array_creation_expression* não tem um tipo de matriz.

Nesse caso, o compilador classifica o *element_access* como um valor do tipo `dynamic`. As regras abaixo para determinar o significado da *element_access* são então aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação das expressões *primary_no_array_creation_expression* e *argument_list* que têm o tipo de tempo de compilação `dynamic`. Se o *primary_no_array_creation_expression* não tiver o tipo de tempo de compilação `dynamic`, o acesso ao elemento passará por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Se a *primary_no_array_creation_expression* de um *element_access* for um valor de um *array_type*, o *element_access* será um acesso à matriz ([acesso à matriz](expressions.md#array-access)). Caso contrário, o *primary_no_array_creation_expression* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface que tenha um ou mais membros do indexador; nesse caso, a *element_access* é um acesso do indexador ([acesso ao indexador](expressions.md#indexer-access)).

#### <a name="array-access"></a>Acesso de matriz

Para um acesso de matriz, o *primary_no_array_creation_expression* do *element_access* deve ser um valor de um *array_type*. Além disso, o *argument_list* de um acesso à matriz não tem permissão para conter argumentos nomeados. O número de expressões na *argument_list* deve ser igual à classificação da *array_type*, e cada expressão deve ser do tipo `int`, `uint`, `long`, `ulong`ou deve ser conversível implicitamente em um ou mais desses tipos.

O resultado da avaliação de um acesso à matriz é uma variável do tipo de elemento da matriz, ou seja, o elemento de matriz selecionado pelo (s) valor (es) das expressões no *argument_list*.

O processamento em tempo de execução de um acesso de matriz do formulário `P[A]`, em que `P` é uma *primary_no_array_creation_expression* de um *array_type* e `A` é um *argument_list*, consiste nas seguintes etapas:

*  `P` é avaliado. Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.
*  As expressões de índice da *argument_list* são avaliadas na ordem, da esquerda para a direita. Após a avaliação de cada expressão de índice, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para um dos seguintes tipos é executada: `int`, `uint`, `long``ulong`. O primeiro tipo nesta lista para o qual existe uma conversão implícita é escolhido. Por exemplo, se a expressão de índice for do tipo `short`, uma conversão implícita em `int` será executada, já que conversões implícitas de `short` para `int` e de `short` para `long` são possíveis. Se a avaliação de uma expressão de índice ou a conversão implícita subsequente causar uma exceção, não serão avaliadas outras expressões de índice e nenhuma etapa adicional será executada.
*  O valor de `P` é verificado para ser válido. Se o valor de `P` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.
*  O valor de cada expressão na *argument_list* é verificado em relação aos limites reais de cada dimensão da instância de matriz referenciada por `P`. Se um ou mais valores estiverem fora do intervalo, um `System.IndexOutOfRangeException` será lançado e nenhuma etapa adicional será executada.
*  O local do elemento da matriz fornecido pelas expressões de índice é computado e esse local se torna o resultado do acesso à matriz.

#### <a name="indexer-access"></a>Acesso de indexador

Para um acesso de indexador, o *primary_no_array_creation_expression* do *element_access* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface, e esse tipo deve implementar um ou mais indexadores que são aplicáveis em relação ao *argument_list* do *element_access*.

O processamento de tempo de vinculação de um acesso de indexador do formulário `P[A]`, em que `P` é uma *primary_no_array_creation_expression* de um tipo de classe, estrutura ou interface `T`e `A` é um *argument_list*, consiste nas seguintes etapas:

*  O conjunto de indexadores fornecido pelo `T` é construído. O conjunto consiste em todos os indexadores declarados em `T` ou em um tipo base de `T` que não são `override` declarações e estão acessíveis no contexto atual ([acesso de membro](basic-concepts.md#member-access)).
*  O conjunto é reduzido para os indexadores aplicáveis e não ocultos por outros indexadores. As regras a seguir são aplicadas a cada `S.I` do indexador no conjunto, em que `S` é o tipo no qual o indexador `I` é declarado:
   * Se `I` não for aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)), `I` será removido do conjunto.
   * Se `I` for aplicável em relação ao `A` ([membro da função aplicável](expressions.md#applicable-function-member)), todos os indexadores declarados em um tipo base de `S` serão removidos do conjunto.
   * Se `I` for aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)) e `S` for um tipo de classe diferente de `object`, todos os indexadores declarados em uma interface serão removidos do conjunto.
*  Se o conjunto resultante de indexadores candidatos estiver vazio, não existirá nenhum indexador aplicável e ocorrerá um erro de tempo de ligação.
*  O melhor indexador do conjunto de indexadores candidatos é identificado usando as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution). Se um único melhor indexador não puder ser identificado, o acesso ao indexador será ambíguo e ocorrerá um erro de tempo de ligação.
*  As expressões de índice da *argument_list* são avaliadas na ordem, da esquerda para a direita. O resultado do processamento do acesso ao indexador é uma expressão classificada como um acesso de indexador. A expressão de acesso do indexador faz referência ao indexador determinado na etapa acima e tem uma expressão de instância associada de `P` e uma lista de argumentos associados de `A`.

Dependendo do contexto no qual ele é usado, um acesso ao indexador causa a invocação do *acessador get* ou do *acessador set* do indexador. Se o acesso do indexador for o destino de uma atribuição, o *acessador set* será invocado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)). Em todos os outros casos, o *acessador get* é invocado para obter o valor atual ([valores de expressões](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Este acesso

Uma *this_access* consiste na palavra reservada `this`.

```antlr
this_access
    : 'this'
    ;
```

Um *this_access* é permitido apenas no *bloco* de um construtor de instância, um método de instância ou um acessador de instância. Ele tem um dos seguintes significados:

*  Quando `this` é usado em um *primary_expression* dentro de um construtor de instância de uma classe, ele é classificado como um valor. O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe dentro da qual o uso ocorre e o valor é uma referência ao objeto que está sendo construído.
*  Quando `this` é usado em um *primary_expression* dentro de um método de instância ou acessador de instância de uma classe, ele é classificado como um valor. O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe dentro da qual o uso ocorre e o valor é uma referência ao objeto para o qual o método ou acessador foi invocado.
*  Quando `this` é usado em um *primary_expression* dentro de um construtor de instância de uma struct, ele é classificado como uma variável. O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da estrutura dentro da qual o uso ocorre e a variável representa a estrutura que está sendo construída. A variável `this` de um construtor de instância de um struct se comporta exatamente como um parâmetro `out` do tipo struct — em particular, isso significa que a variável deve ser definitivamente atribuída em cada caminho de execução do construtor da instância.
*  Quando `this` é usado em um *primary_expression* dentro de um método de instância ou acessador de instância de uma struct, ele é classificado como uma variável. O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da estrutura na qual o uso ocorre.
   * Se o método ou acessador não for um iterador ([iteradores](classes.md#iterators)), a variável `this` representará a estrutura para a qual o método ou o acessador foi invocado e se comlatará exatamente como um parâmetro `ref` do tipo struct.
   * Se o método ou o acessador for um iterador, a variável `this` representa uma cópia da estrutura para a qual o método ou acessador foi invocado e se comporta exatamente como um parâmetro de valor do tipo struct.

O uso de `this` em um *primary_expression* em um contexto diferente daqueles listados acima é um erro de tempo de compilação. Em particular, não é possível fazer referência a `this` em um método estático, um acessador de propriedade estática ou em uma *variable_initializer* de uma declaração de campo.

### <a name="base-access"></a>Acesso de base

Um *base_access* consiste na palavra reservada `base` seguida por um token "`.`" e um identificador ou um *argument_list* entre colchetes:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Uma *base_access* é usada para acessar membros da classe base que são ocultados por membros nomeados de forma semelhante na classe ou struct atual. Um *base_access* é permitido apenas no *bloco* de um construtor de instância, um método de instância ou um acessador de instância. Quando `base.I` ocorre em uma classe ou struct, `I` deve indicar um membro da classe base dessa classe ou struct. Da mesma forma, quando `base[E]` ocorre em uma classe, um indexador aplicável deve existir na classe base.

No momento da associação, *base_access* expressões do formulário `base.I` e `base[E]` são avaliadas exatamente como se fossem escritas `((B)this).I` e `((B)this)[E]`, em que `B` é a classe base da classe ou struct na qual a construção ocorre. Assim, `base.I` e `base[E]` correspondem a `this.I` e `this[E]`, exceto `this` é exibido como uma instância da classe base.

Quando um *base_access* referencia um membro de função virtual (um método, uma propriedade ou um indexador), a determinação de qual membro de função invocar em tempo de execução ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é alterada. O membro da função que é invocado é determinado pela localização da implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) do membro da função em relação a `B` (em vez de em relação ao tipo de tempo de execução de `this`, como seria usual em um acesso não-base). Assim, dentro de um `override` de um membro de função `virtual`, um *base_access* pode ser usado para invocar a implementação herdada do membro da função. Se o membro da função referenciado por uma *base_access* for abstract, ocorrerá um erro de tempo de associação.

### <a name="postfix-increment-and-decrement-operators"></a>Operadores de incremento e decremento pós-fixados

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

O operando de um incremento de sufixo ou uma operação de decréscimo deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso de indexador. O resultado da operação é um valor do mesmo tipo que o operando.

Se o *primary_expression* tiver o tipo de tempo de compilação `dynamic`, o operador será vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)), o *post_increment_expression* ou *post_decrement_expression* tem o tipo de tempo de compilação `dynamic` e as regras a seguir são aplicadas em tempo de execução usando o tipo de tempo de execução do *primary_expression*.

Se o operando de um incremento de sufixo ou uma operação de decréscimo for um acesso de propriedade ou indexador, a propriedade ou o indexador deverá ter um `get` e um acessador `set`. Se esse não for o caso, ocorrerá um erro de tempo de associação.

Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Existem `++` predefinidos e operadores de `--` para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`e qualquer tipo de enumeração. Os operadores de `++` predefinidos retornam o valor produzido pela adição de 1 ao operando e os operadores de `--` predefinidos retornam o valor produzido pela subtração de 1 do operando. Em um contexto `checked`, se o resultado dessa adição ou subtração estiver fora do intervalo do tipo de resultado e o tipo de resultado for um tipo integral ou tipo de enumeração, um `System.OverflowException` será gerado.

O processamento em tempo de execução de um incremento de sufixo ou uma operação de diminuição do formulário `x++` ou `x--` consiste nas seguintes etapas:

*   Se `x` for classificado como uma variável:
    * `x` é avaliado para produzir a variável.
    * O valor de `x` é salvo.
    * O operador selecionado é invocado com o valor salvo de `x` como seu argumento.
    * O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.
    * O valor salvo de `x` se torna o resultado da operação.
*   Se `x` for classificado como um acesso de propriedade ou indexador:
    * A expressão de instância (se `x` não for `static`) e a lista de argumentos (se `x` for um acesso de indexador) associada a `x` são avaliadas e os resultados são usados nas invocações subsequentes de `get` e `set`.
    * O acessador de `get` de `x` é invocado e o valor retornado é salvo.
    * O operador selecionado é invocado com o valor salvo de `x` como seu argumento.
    * O acessador de `set` de `x` é invocado com o valor retornado pelo operador como seu argumento `value`.
    * O valor salvo de `x` se torna o resultado da operação.

Os operadores `++` e `--` também dão suporte à notação de prefixo ([operadores de incremento e diminuição de prefixo](expressions.md#prefix-increment-and-decrement-operators)). Normalmente, o resultado de `x++` ou `x--` é o valor de `x` antes da operação, enquanto o resultado de `++x` ou `--x` é o valor de `x` após a operação. Em ambos os casos, `x` ele mesmo tem o mesmo valor após a operação.

Uma implementação `operator ++` ou `operator --` pode ser chamada usando o sufixo ou a notação de prefixo. Não é possível ter implementações de operador separadas para as duas notações.

### <a name="the-new-operator"></a>O novo operador

O operador `new` é usado para criar novas instâncias de tipos.

Há três formas de expressões de `new`:

*  As expressões de criação de objeto são usadas para criar novas instâncias de tipos de classe e tipos de valor.
*  As expressões de criação de matriz são usadas para criar novas instâncias de tipos de matriz.
*  As expressões de criação de representante são usadas para criar novas instâncias de tipos delegados.

O operador `new` implica a criação de uma instância de um tipo, mas não implica necessariamente a alocação dinâmica da memória. Em particular, as instâncias de tipos de valor não exigem memória adicional além das variáveis em que residem, e nenhuma alocação dinâmica ocorre quando `new` é usada para criar instâncias de tipos de valor.

#### <a name="object-creation-expressions"></a>Expressões de criação de objeto

Um *object_creation_expression* é usado para criar uma nova instância de um *class_type* ou um *value_type*.

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

O *tipo* de um *object_creation_expression* deve ser um *class_type*, um *value_type* ou um *type_parameter*. O *tipo* não pode ser um `abstract` *class_type*.

O *argument_list* opcional ([listas de argumentos](expressions.md#argument-lists)) só será permitido se o *tipo* for um *class_type* ou um *struct_type*.

Uma expressão de criação de objeto pode omitir a lista de argumentos do construtor e os parênteses delimitadores fornecidos incluem um inicializador de objeto ou inicializador de coleção. Omitir a lista de argumentos do construtor e parênteses delimitadores é equivalente a especificar uma lista de argumentos vazia.

O processamento de uma expressão de criação de objeto que inclui um inicializador de objeto ou inicializador de coleção consiste no primeiro processamento do construtor de instância e no processamento das inicializações de membro ou elemento especificadas pelo inicializador de objeto ([inicializadores de objeto](expressions.md#object-initializers)) ou pelo inicializador de coleção ([inicializadores de coleção](expressions.md#collection-initializers)).

Se qualquer um dos argumentos na *argument_list* opcional tiver o tipo de tempo de compilação `dynamic`, a *object_creation_expression* será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) e as regras a seguir serão aplicadas em tempo de execução usando o tipo de tempo de execução desses argumentos da *argument_list* que têm o tipo de tempo de compilação `dynamic`. No entanto, a criação do objeto passa por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

O processamento de tempo de associação de um *object_creation_expression* do formulário `new T(A)`, em que `T` é um *class_type* ou um *value_type* e `A` é um *argument_list*opcional, consiste nas seguintes etapas:

*   Se `T` for um *value_type* e `A` não estiver presente:
    * O *object_creation_expression* é uma invocação de construtor padrão. O resultado da *object_creation_expression* é um valor do tipo `T`, ou seja, o valor padrão para `T` conforme definido no [tipo System. ValueType](types.md#the-systemvaluetype-type).
*   Caso contrário, se `T` for um *type_parameter* e `A` não estiver presente:
    * Se nenhuma restrição de tipo de valor ou restrição de Construtor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) tiver sido especificada para `T`, ocorrerá um erro de tempo de associação.
    * O resultado da *object_creation_expression* é um valor do tipo de tempo de execução ao qual o parâmetro de tipo foi associado, ou seja, o resultado da invocação do construtor padrão desse tipo. O tipo de tempo de execução pode ser um tipo de referência ou um tipo de valor.
*   Caso contrário, se `T` for uma *class_type* ou uma *struct_type*:
    * Se `T` for um `abstract` *class_type*, ocorrerá um erro em tempo de compilação.
    * O construtor de instância a ser invocado é determinado usando as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution). O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessíveis declarados em `T` que são aplicáveis em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)). Se o conjunto de construtores de instância de candidato estiver vazio, ou se um único Construtor de instância recomendada não puder ser identificado, ocorrerá um erro de tempo de associação.
    * O resultado da *object_creation_expression* é um valor do tipo `T`, ou seja, o valor produzido por invocar o construtor da instância determinado na etapa acima.
*  Caso contrário, o *object_creation_expression* é inválido e ocorre um erro de tempo de ligação.

Mesmo que a *object_creation_expression* seja vinculada dinamicamente, o tipo de tempo de compilação ainda será `T`.

O processamento em tempo de execução de um *object_creation_expression* do formulário `new T(A)`, em que `T` é *class_type* ou uma *struct_type* e `A` é uma *argument_list*opcional, consiste nas seguintes etapas:

*   Se `T` for uma *class_type*:
    * Uma nova instância da classe `T` é alocada. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.
    * Todos os campos da nova instância são inicializados para seus valores padrão ([valores padrão](variables.md#default-values)).
    * O construtor de instância é invocado de acordo com as regras de invocação de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Uma referência à instância alocada recentemente é passada automaticamente para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.
*   Se `T` for uma *struct_type*:
    * Uma instância do tipo `T` é criada alocando-se uma variável local temporária. Como um construtor de instância de um *struct_type* é necessário para atribuir definitivamente um valor a cada campo da instância que está sendo criada, nenhuma inicialização da variável temporária é necessária.
    * O construtor de instância é invocado de acordo com as regras de invocação de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Uma referência à instância alocada recentemente é passada automaticamente para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.

#### <a name="object-initializers"></a>Inicializadores de objeto

Um ***inicializador de objeto*** especifica valores para zero ou mais campos, propriedades ou elementos indexados de um objeto.

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

Um inicializador de objeto consiste em uma sequência de inicializadores de membro, delimitados por `{` e `}` tokens e separados por vírgulas. Cada *member_initializer* designa um destino para a inicialização. Um *identificador* deve nomear um campo ou Propriedade acessível do objeto que está sendo inicializado, enquanto um *argument_list* entre colchetes deve especificar argumentos para um indexador acessível no objeto que está sendo inicializado. É um erro para um inicializador de objeto incluir mais de um inicializador de membro para o mesmo campo ou propriedade.

Cada *initializer_target* é seguida por um sinal de igual e uma expressão, um inicializador de objeto ou um inicializador de coleção. Não é possível que as expressões no inicializador de objeto façam referência ao objeto recém-criado que está sendo inicializado.

Um inicializador de membro que especifica uma expressão após o sinal de igual é processado da mesma maneira que uma atribuição ([atribuição simples](expressions.md#simple-assignment)) para o destino.

Um inicializador de membro que especifica um inicializador de objeto após o sinal de igual é um ***inicializador de objeto aninhado***, ou seja, uma inicialização de um objeto inserido. Em vez de atribuir um novo valor ao campo ou à propriedade, as atribuições no inicializador de objeto aninhado são tratadas como atribuições para membros do campo ou da propriedade. Inicializadores de objeto aninhados não podem ser aplicados a propriedades com um tipo de valor, ou a campos somente leitura com um tipo de valor.

Um inicializador de membro que especifica um inicializador de coleção após o sinal de igual é uma inicialização de uma coleção inserida. Em vez de atribuir uma nova coleção ao campo de destino, à propriedade ou ao indexador, os elementos fornecidos no inicializador são adicionados à coleção referenciada pelo destino. O destino deve ser de um tipo de coleção que satisfaça os requisitos especificados em [inicializadores de coleção](expressions.md#collection-initializers).

Os argumentos para um inicializador de índice sempre serão avaliados exatamente uma vez. Portanto, mesmo se os argumentos acabarem nunca sendo usados (por exemplo, devido a um inicializador aninhado vazio), eles serão avaliados para seus efeitos colaterais.

A classe a seguir representa um ponto com duas coordenadas:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Uma instância do `Point` pode ser criada e inicializada da seguinte maneira:
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
em que `__a` é uma variável temporária invisível e inacessível. A classe a seguir representa um retângulo criado a partir de dois pontos:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Uma instância do `Rectangle` pode ser criada e inicializada da seguinte maneira:
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
onde `__r`, `__p1` e `__p2` são variáveis temporárias que, de outra forma, são invisíveis e inacessíveis.

Se o construtor de `Rectangle`alocar as duas instâncias de `Point` inseridas
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
a construção a seguir pode ser usada para inicializar as instâncias de `Point` inseridas em vez de atribuir novas instâncias:
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

Dada uma definição apropriada de C, o exemplo a seguir:
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
é equivalente a esta série de atribuições:
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
onde `__c`, etc., são variáveis geradas que são invisíveis e inacessíveis para o código-fonte. Observe que os argumentos para `[0,0]` são avaliados apenas uma vez e os argumentos para `[1,2]` são avaliados uma vez, mesmo que nunca sejam usados.

#### <a name="collection-initializers"></a>Inicializadores de coleção

Um inicializador de coleção especifica os elementos de uma coleção.

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

Um inicializador de coleção consiste em uma sequência de inicializadores de elemento, delimitada por `{` e `}` tokens e separados por vírgulas. Cada inicializador de elemento especifica um elemento a ser adicionado ao objeto de coleção que está sendo inicializado e consiste em uma lista de expressões delimitadas por `{` e `}` tokens e separados por vírgulas.  Um inicializador de elemento de uma única expressão pode ser gravado sem chaves, mas não pode ser uma expressão de atribuição para evitar ambigüidade com inicializadores de membro. A produção *non_assignment_expression* é definida na [expressão](expressions.md#expression).

Veja a seguir um exemplo de uma expressão de criação de objeto que inclui um inicializador de coleção:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

O objeto de coleção ao qual um inicializador de coleção é aplicado deve ser de um tipo que implementa `System.Collections.IEnumerable` ou um erro em tempo de compilação ocorre. Para cada elemento especificado na ordem, o inicializador de coleção invoca um método `Add` no objeto de destino com a lista de expressões do inicializador de elemento como lista de argumentos, aplicando a resolução de membro normal e a solução de sobrecarga para cada invocação. Assim, o objeto de coleção deve ter uma instância aplicável ou um método de extensão com o nome `Add` para cada inicializador de elemento.

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
onde `__clist`, `__c1` e `__c2` são variáveis temporárias que, de outra forma, são invisíveis e inacessíveis.

#### <a name="array-creation-expressions"></a>Expressões de criação de matriz

Um *array_creation_expression* é usado para criar uma nova instância de um *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Uma expressão de criação de matriz do primeiro formulário aloca uma instância de matriz do tipo que resulta da exclusão de cada uma das expressões individuais da lista de expressões. Por exemplo, a expressão de criação de matriz `new int[10,20]` produz uma instância de matriz do tipo `int[,]`e a expressão de criação de matriz `new int[10][,]` produz uma matriz do tipo `int[][,]`. Cada expressão na lista de expressões deve ser do tipo `int`, `uint`, `long`ou `ulong`ou conversível implicitamente em um ou mais desses tipos. O valor de cada expressão determina o comprimento da dimensão correspondente na instância de matriz alocada recentemente. Como o comprimento de uma dimensão de matriz deve ser não negativo, é um erro de tempo de compilação ter um *constant_expression* com um valor negativo na lista de expressões.

Exceto em um contexto não seguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)), o layout das matrizes não é especificado.

Se uma expressão de criação de matriz do primeiro formulário incluir um inicializador de matriz, cada expressão na lista de expressões deverá ser uma constante e os comprimentos de classificação e de dimensão especificados pela lista de expressões devem corresponder àqueles do inicializador de matriz.

Em uma expressão de criação de matriz do segundo ou terceiro formulário, a classificação do tipo de matriz ou especificador de classificação especificado deve corresponder à do inicializador de matriz. Os comprimentos de dimensão individuais são inferidos do número de elementos em cada um dos níveis de aninhamento correspondentes do inicializador de matriz. Portanto, a expressão
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
corresponde exatamente a
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Uma expressão de criação de matriz do terceiro formulário é referida como uma ***expressão de criação de matriz digitada implicitamente***. Ele é semelhante ao segundo formulário, exceto pelo fato de que o tipo de elemento da matriz não é fornecido explicitamente, mas determinado como o melhor tipo comum ([encontrando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) do conjunto de expressões no inicializador de matriz. Para uma matriz multidimensional, ou seja, uma em que a *rank_specifier* contém pelo menos uma vírgula, esse conjunto inclui todas as *expressões*s encontradas em *array_initializer*s aninhadas.

Inicializadores de matriz são descritos mais detalhadamente em [inicializadores de matriz](arrays.md#array-initializers).

O resultado da avaliação de uma expressão de criação de matriz é classificado como um valor, ou seja, uma referência à instância de matriz alocada recentemente. O processamento em tempo de execução de uma expressão de criação de matriz consiste nas seguintes etapas:

*  As expressões de comprimento da dimensão da *expression_list* são avaliadas na ordem, da esquerda para a direita. Após a avaliação de cada expressão, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para um dos seguintes tipos é executada: `int`, `uint`, `long``ulong`. O primeiro tipo nesta lista para o qual existe uma conversão implícita é escolhido. Se a avaliação de uma expressão ou a conversão implícita subsequente causar uma exceção, nenhuma expressão adicional será avaliada e nenhuma outra etapa será executada.
*  Os valores computados para os comprimentos das dimensões são validados da seguinte maneira. Se um ou mais dos valores forem menores que zero, um `System.OverflowException` será lançado e nenhuma etapa adicional será executada.
*  Uma instância de matriz com os tamanhos de dimensão fornecidos é alocada. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.
*  Todos os elementos da nova instância de matriz são inicializados para seus valores padrão ([valores padrão](variables.md#default-values)).
*  Se a expressão de criação de matriz contiver um inicializador de matriz, cada expressão no inicializador de matriz será avaliada e atribuída ao elemento de matriz correspondente. As avaliações e as atribuições são executadas na ordem em que as expressões são gravadas no inicializador de matriz – em outras palavras, os elementos são inicializados em ordem de índice crescente, com a dimensão mais à direita aumentando primeiro. Se a avaliação de uma determinada expressão ou a atribuição subsequente ao elemento de matriz correspondente causar uma exceção, nenhum elemento adicional será inicializado (e os elementos restantes terão, portanto, seus valores padrão).

Uma expressão de criação de matriz permite instanciação de uma matriz com elementos de um tipo de matriz, mas os elementos de tal matriz devem ser inicializados manualmente. Por exemplo, a instrução:
```csharp
int[][] a = new int[100][];
```
Cria uma matriz unidimensional com 100 elementos do tipo `int[]`. O valor inicial de cada elemento é `null`. Não é possível que a mesma expressão de criação de matriz também instancie as submatrizes e a instrução
```csharp
int[][] a = new int[100][5];        // Error
```
resulta em um erro de tempo de compilação. Em vez disso, a instanciação das submatrizes deve ser executada manualmente, como em
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Quando uma matriz de matrizes tem uma forma "retangular", ou seja, quando as submatrizes têm o mesmo comprimento, é mais eficiente usar uma matriz multidimensional. No exemplo acima, a instanciação da matriz de matrizes cria 101 objetos — uma matriz externa e submatrizes 100. Por outro lado,
```csharp
int[,] = new int[100, 5];
```
cria apenas um único objeto, uma matriz bidimensional e realiza a alocação em uma única instrução.

Veja a seguir exemplos de expressões de criação de matriz digitadas implicitamente:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

A última expressão causa um erro de tempo de compilação porque nem `int` nem `string` é implicitamente conversível para a outra e, portanto, não há um melhor tipo comum. Uma expressão de criação de matriz tipada explicitamente deve ser usada neste caso, por exemplo, especificando o tipo a ser `object[]`. Como alternativa, um dos elementos pode ser convertido em um tipo base comum, que se tornaria o tipo de elemento inferido.

As expressões de criação de matriz com tipo implícito podem ser combinadas com inicializadores de objeto anônimo ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) para criar estruturas de dados com tipo anônimo. Por exemplo:
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

#### <a name="delegate-creation-expressions"></a>Delegar expressões de criação

Uma *delegate_creation_expression* é usada para criar uma nova instância de um *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

O argumento de uma expressão de criação de delegado deve ser um grupo de métodos, uma função anônima ou um valor do tipo de tempo de compilação `dynamic` ou um *delegate_type*. Se o argumento for um grupo de métodos, ele identificará o método e, para um método de instância, o objeto para o qual criar um delegado. Se o argumento for uma função anônima, ele definirá diretamente os parâmetros e o corpo do método do destino delegado. Se o argumento for um valor, ele identificará uma instância de delegado da qual criar uma cópia.

Se a *expressão* tiver o tipo de tempo de compilação `dynamic`, a *delegate_creation_expression* será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) e as regras abaixo serão aplicadas em tempo de execução usando o tipo de tempo de execução da *expressão*. Caso contrário, as regras são aplicadas em tempo de compilação.

O processamento de tempo de associação de um *delegate_creation_expression* do formulário `new D(E)`, em que `D` é uma *delegate_type* e `E` é uma *expressão*, consiste nas seguintes etapas:

*  Se `E` for um grupo de métodos, a expressão de criação de representante será processada da mesma maneira que uma conversão de grupo de métodos ([conversões de grupos de métodos](conversions.md#method-group-conversions)) de `E` para `D`.
*  Se `E` for uma função anônima, a expressão de criação de representante será processada da mesma maneira que uma conversão de função anônima ([conversões de função anônimas](conversions.md#anonymous-function-conversions)) de `E` para `D`.
*  Se `E` for um valor, `E` deverá ser compatível ([delegar declarações](delegates.md#delegate-declarations)) com `D`e o resultado será uma referência a um delegado recém-criado do tipo `D` que se refere à mesma lista de invocação que o `E`. Se `E` não for compatível com `D`, ocorrerá um erro em tempo de compilação.

O processamento em tempo de execução de um *delegate_creation_expression* do formulário `new D(E)`, em que `D` é uma *delegate_type* e `E` é uma *expressão*, consiste nas seguintes etapas:

*   Se `E` for um grupo de métodos, a expressão de criação de representante será avaliada como uma conversão de grupo de métodos ([conversões de grupos de métodos](conversions.md#method-group-conversions)) de `E` para `D`.
*   Se `E` for uma função anônima, a criação de delegado será avaliada como uma conversão de função anônima de `E` para `D` ([conversões de função anônimas](conversions.md#anonymous-function-conversions)).
*   Se `E` for um valor de uma *delegate_type*:
    * `E` é avaliado. Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.
    * Se o valor de `E` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.
    * Uma nova instância do tipo delegado `D` é alocada. Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.
    * A nova instância de delegado é inicializada com a mesma lista de invocação que a instância de delegado fornecida por `E`.

A lista de invocação de um delegado é determinada quando o delegado é instanciado e, em seguida, permanece constante para todo o tempo de vida do delegado. Em outras palavras, não é possível alterar as entidades de destino que podem ser chamadas de um delegado depois que ele tiver sido criado. Quando dois delegados são combinados ou um é removido de outro ([delegar declarações](delegates.md#delegate-declarations)), um novo delegado resulta; nenhum delegado existente tem seu conteúdo alterado.

Não é possível criar um delegado que se refere a uma propriedade, indexador, operador definido pelo usuário, Construtor de instância, destruidor ou construtor estático.

Conforme descrito acima, quando um delegado é criado a partir de um grupo de métodos, a lista de parâmetros formais e o tipo de retorno do delegado determinam qual dos métodos sobrecarregados selecionar. No exemplo
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
o campo `A.f` é inicializado com um delegado que se refere ao segundo método `Square` porque esse método corresponde exatamente à lista de parâmetros formais e ao tipo de retorno de `DoubleFunc`. Se o segundo método de `Square` não estivesse presente, ocorreria um erro em tempo de compilação.

#### <a name="anonymous-object-creation-expressions"></a>Expressões de criação de objeto anônimo

Um *anonymous_object_creation_expression* é usado para criar um objeto de um tipo anônimo.

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

Um inicializador de objeto anônimo declara um tipo anônimo e retorna uma instância desse tipo. Um tipo anônimo é um tipo de classe sem nome que herda diretamente de `object`. Os membros de um tipo anônimo são uma sequência de propriedades somente leitura inferidas do inicializador de objeto anônimo usado para criar uma instância do tipo. Especificamente, um inicializador de objeto anônimo do formulário
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
onde cada `Tx` é o tipo da expressão correspondente `ex`. A expressão usada em um *member_declarator* deve ter um tipo. Portanto, é um erro de tempo de compilação para uma expressão em um *member_declarator* ser nulo ou uma função anônima. Também é um erro de tempo de compilação para que a expressão tenha um tipo não seguro.

Os nomes de um tipo anônimo e do parâmetro para seu método `Equals` são gerados automaticamente pelo compilador e não podem ser referenciados no texto do programa.

Dentro do mesmo programa, dois inicializadores de objeto anônimos que especificam uma sequência de propriedades dos mesmos nomes e tipos de tempo de compilação na mesma ordem produzirão instâncias do mesmo tipo anônimo.

No exemplo
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
a atribuição na última linha é permitida porque `p1` e `p2` são do mesmo tipo anônimo.

Os métodos `Equals` e `GetHashcode` em tipos anônimos substituem os métodos herdados de `object`e são definidos em termos de `Equals` e `GetHashcode` das propriedades, de forma que duas instâncias do mesmo tipo anônimo sejam iguais se e somente se todas as suas propriedades forem iguais.

Um Declarador de membro pode ser abreviado como um nome simples ([inferência de tipo](expressions.md#type-inference)), um acesso de membro ([verificação de tempo de compilação de resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), um acesso base ([acesso de base](expressions.md#base-access)) ou um membro condicional nulo de acesso ([expressões condicionais nulas como inicializadores de projeção](expressions.md#null-conditional-expressions-as-projection-initializers)). Isso é chamado de ***inicializador de projeção*** e é abreviado para uma declaração de e atribuição a uma propriedade com o mesmo nome. Especificamente, os declaradores de membros dos formulários
```csharp
identifier
expr.identifier
```
são precisamente equivalentes aos seguintes, respectivamente:
```csharp
identifier = identifier
identifier = expr.identifier
```

Portanto, em um inicializador de projeção, o *identificador* seleciona o valor e o campo ou propriedade ao qual o valor é atribuído. Intuitivamente, um inicializador de projeção projeta não apenas um valor, mas também o nome do valor.

### <a name="the-typeof-operator"></a>O operador typeof

O operador de `typeof` é usado para obter o objeto de `System.Type` para um tipo.

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

A primeira forma de *typeof_expression* consiste em uma palavra-chave `typeof` seguida por um *tipo*entre parênteses. O resultado de uma expressão desse formulário é o objeto `System.Type` para o tipo indicado. Há apenas um objeto `System.Type` para qualquer tipo específico. Isso significa que, para um tipo `T`, `typeof(T) == typeof(T)` é sempre verdadeiro. O *tipo* não pode ser `dynamic`.

A segunda forma de *typeof_expression* consiste em uma palavra-chave `typeof` seguida por uma *unbound_type_name*entre parênteses. Um *unbound_type_name* é muito semelhante a um *type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)), exceto pelo fato de que um *unbound_type_name* contém *generic_dimension_specifier*s onde um *type_name* contém *type_argument_list*s. Quando o operando de uma *typeof_expression* é uma sequência de tokens que satisfaz as gramáticas de *unbound_type_name* e *type_name*, ou seja, quando ele não contém uma *generic_dimension_specifier* nem uma *type_argument_list*, a sequência de tokens é considerada uma *type_name*. O significado de um *unbound_type_name* é determinado da seguinte maneira:

*  Converta a sequência de tokens em uma *type_name* substituindo cada *generic_dimension_specifier* por uma *type_argument_list* com o mesmo número de vírgulas e a palavra-chave `object` como cada *type_argument*.
*  Avalie as *type_name*resultantes, ignorando todas as restrições de parâmetro de tipo.
*  O *unbound_type_name* é resolvido para o tipo genérico não associado associado ao tipo construído resultante ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)).

O resultado da *typeof_expression* é o objeto `System.Type` para o tipo genérico não associado resultante.

A terceira forma de *typeof_expression* consiste em uma palavra-chave `typeof` seguida por uma palavra-chave `void` entre parênteses. O resultado de uma expressão desse formulário é o `System.Type` objeto que representa a ausência de um tipo. O objeto de tipo retornado por `typeof(void)` é diferente do objeto de tipo retornado para qualquer tipo. Esse objeto de tipo especial é útil em bibliotecas de classes que permitem a reflexão em métodos no idioma, em que esses métodos desejam ter uma maneira de representar o tipo de retorno de qualquer método, incluindo métodos void, com uma instância de `System.Type`.

O operador `typeof` pode ser usado em um parâmetro de tipo. O resultado é o objeto `System.Type` para o tipo de tempo de execução que foi associado ao parâmetro de tipo. O operador de `typeof` também pode ser usado em um tipo construído ou um tipo genérico não associado ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)). O objeto `System.Type` para um tipo genérico não associado não é o mesmo que o objeto `System.Type` do tipo de instância. O tipo de instância é sempre um tipo construído fechado em tempo de execução, de modo que seu objeto de `System.Type` depende dos argumentos de tipo de tempo de execução em uso, enquanto o tipo genérico não associado não tem argumentos de tipo.

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
```console
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

Observe também que o resultado de `typeof(X<>)` não depende do argumento de tipo, mas o resultado de `typeof(X<T>)`.

### <a name="the-checked-and-unchecked-operators"></a>Os operadores verificados e não verificados

Os operadores de `checked` e `unchecked` são usados para controlar o ***contexto de verificação de estouro*** para operações e conversões aritméticas de tipo integral.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

O operador `checked` avalia a expressão contida em um contexto selecionado e o operador `unchecked` avalia a expressão contida em um contexto desmarcado. Um *checked_expression* ou *unchecked_expression* corresponde exatamente a um *parenthesized_expression* ([expressões entre parênteses](expressions.md#parenthesized-expressions)), exceto que a expressão contida é avaliada no contexto de verificação de estouro especificado.

O contexto de verificação de estouro também pode ser controlado por meio das instruções `checked` e `unchecked` ([as instruções marcadas e não verificadas](statements.md#the-checked-and-unchecked-statements)).

As operações a seguir são afetadas pelo contexto de verificação de estouro estabelecido pelo `checked` e `unchecked` de operadores e instruções:

*  Os operadores predefinidos `++` e `--` unários ([incremento de sufixo e diminuição de operadores](expressions.md#postfix-increment-and-decrement-operators) e [incrementos de prefixo e diminuição](expressions.md#prefix-increment-and-decrement-operators)) quando o operando é de um tipo integral.
*  O operador unário predefinido `-` ([operador menos unário](expressions.md#unary-minus-operator)), quando o operando é de um tipo integral.
*  O `+`predefinido, `-`, `*`e `/` operadores binários ([operadores aritméticos](expressions.md#arithmetic-operators)), quando ambos os operandos são de tipos integrais.
*  Conversões numéricas explícitas ([conversões numéricas explícitas](conversions.md#explicit-numeric-conversions)) de um tipo integral para outro tipo integral, ou de `float` ou `double` a um tipo integral.

Quando uma das operações acima produz um resultado muito grande para representar no tipo de destino, o contexto no qual a operação é executada controla o comportamento resultante:

*  Em um contexto de `checked`, se a operação for uma expressão constante ([expressões constantes](expressions.md#constant-expressions)), ocorrerá um erro em tempo de compilação. Caso contrário, quando a operação for executada em tempo de execução, uma `System.OverflowException` será lançada.
*  Em um contexto de `unchecked`, o resultado é truncado descartando quaisquer bits de ordem superior que não caibam no tipo de destino.

Para expressões não constantes (expressões que são avaliadas em tempo de execução) que não são delimitadas por nenhuma `checked` ou `unchecked` operadores ou instruções, o contexto de verificação de estouro padrão é `unchecked`, a menos que fatores externos (como switches de compilador e configuração do ambiente de execução) chamem `checked` avaliação.

Para expressões constantes (expressões que podem ser totalmente avaliadas em tempo de compilação), o contexto de verificação de estouro padrão é sempre `checked`. A menos que uma expressão constante seja inserida explicitamente em um contexto de `unchecked`, os estouros que ocorrerem durante a avaliação do tempo de compilação da expressão sempre causarão erros de tempo de compilação.

O corpo de uma função anônima não é afetado por `checked` ou `unchecked` contextos nos quais a função anônima ocorre.

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
Não são relatados erros de tempo de compilação, pois nenhuma das expressões pode ser avaliada em tempo de compilação. Em tempo de execução, o método `F` gera uma `System.OverflowException`e o método `G` retorna-727379968 (os bits inferiores 32 do resultado fora do intervalo). O comportamento do método `H` depende do contexto de verificação de estouro padrão para a compilação, mas é o mesmo que `F` ou o mesmo que `G`.

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
os estouros que ocorrem ao avaliar as expressões constantes em `F` e `H` causam erros de tempo de compilação a serem relatados porque as expressões são avaliadas em um contexto de `checked`. Um estouro também ocorre ao avaliar a expressão constante em `G`, mas como a avaliação ocorre em um contexto de `unchecked`, o estouro não é relatado.

Os operadores `checked` e `unchecked` afetam apenas o contexto de verificação de estouro para as operações que estão contidas nos tokens "`(`" e "`)`". Os operadores não têm nenhum efeito nos membros da função que são invocados como resultado da avaliação da expressão contida. No exemplo
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
o uso de `checked` no `F` não afeta a avaliação de `x * y` no `Multiply`, portanto, `x * y` é avaliado no contexto de verificação de estouro padrão.

O operador de `unchecked` é conveniente ao escrever constantes dos tipos integrais assinados em notação hexadecimal. Por exemplo:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Ambas as constantes hexadecimais acima são do tipo `uint`. Como as constantes estão fora do intervalo de `int`, sem o operador `unchecked`, as conversões para `int` produzirão erros em tempo de compilação.

Os operadores e as instruções `checked` e `unchecked` permitem que os programadores controlem determinados aspectos de alguns cálculos numéricos. No entanto, o comportamento de alguns operadores numéricos depende dos tipos de dados dos operandos. Por exemplo, multiplicar dois decimais sempre resulta em uma exceção no estouro mesmo dentro de uma construção de `unchecked` explicitamente. Da mesma forma, multiplicar dois floats nunca resulta em uma exceção no estouro mesmo dentro de uma construção de `checked` explicitamente. Além disso, outros operadores nunca são afetados pelo modo de verificação, sejam eles padrão ou explícitos.

### <a name="default-value-expressions"></a>Expressões de valor padrão

Uma expressão de valor padrão é usada para obter o valor padrão ([valores padrão](variables.md#default-values)) de um tipo. Normalmente, uma expressão de valor padrão é usada para parâmetros de tipo, pois ela poderá não ser conhecida se o parâmetro de tipo for um tipo de valor ou de referência. (Não existe nenhuma conversão do `null` literal para um parâmetro de tipo, a menos que o parâmetro de tipo seja conhecido como um tipo de referência.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Se o *tipo* em um *default_value_expression* for avaliado em tempo de execução para um tipo de referência, o resultado será `null` convertido nesse tipo. Se o *tipo* em um *default_value_expression* for avaliado em tempo de execução para um tipo de valor, o resultado será o valor padrão do *value_type*([construtores padrão](types.md#default-constructors)).

Uma *default_value_expression* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) se o tipo é um tipo de referência ou um parâmetro de tipo que é conhecido como um tipo de referência ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)). Além disso, um *default_value_expression* é uma expressão constante se o tipo é um dos seguintes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`ou qualquer tipo de enumeração.


### <a name="nameof-expressions"></a>Expressões nameof

Um *nameof_expression* é usado para obter o nome de uma entidade de programa como uma cadeia de caracteres constante.

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

Em termos de gramática, o operando *named_entity* sempre é uma expressão. Como `nameof` não é uma palavra-chave reservada, uma expressão nameof é sempre sintaticamente ambígua com uma invocação do nome simples `nameof`. Por motivos de compatibilidade, se uma pesquisa de nome ([nomes simples](expressions.md#simple-names)) do nome `nameof` tiver sucesso, a expressão será tratada como uma *invocation_expression* , independentemente de a invocação ser legal ou não. Caso contrário, é um *nameof_expression*.

O significado da *named_entity* de uma *nameof_expression* é o significado dela como uma expressão; ou seja, como um *Simple_name*, um *base_access* ou um *member_access*. No entanto, onde a pesquisa descrita em [nomes simples](expressions.md#simple-names) e [acesso de membro](expressions.md#member-access) resulta em um erro porque um membro de instância foi encontrado em um contexto estático, um *nameof_expression* não produz esse erro.

É um erro de tempo de compilação para um *named_entity* designar um grupo de métodos para ter um *type_argument_list*. É um erro de tempo de compilação para um *named_entity_target* ter o tipo `dynamic`.

Uma *nameof_expression* é uma expressão constante do tipo `string`e não tem nenhum efeito no tempo de execução. Especificamente, sua *named_entity* não é avaliada e é ignorada para fins de análise de atribuição definitiva ([regras gerais para expressões simples](variables.md#general-rules-for-simple-expressions)). Seu valor é o último identificador da *named_entity* antes da *type_argument_list*final opcional, transformada da seguinte maneira:

* O prefixo "`@`", se usado, será removido.
* Cada *unicode_escape_sequence* é transformada em seu caractere Unicode correspondente.
* Todas as *formatting_characters* são removidas.

Essas são as mesmas transformações aplicadas em [identificadores](lexical-structure.md#identifiers) ao testar a igualdade entre identificadores.

TODO: exemplos

### <a name="anonymous-method-expressions"></a>Expressões de método anônimo

Uma *anonymous_method_expression* é uma das duas maneiras de definir uma função anônima. Eles são descritos mais detalhadamente em [expressões de função anônimas](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Operadores unários

Os operadores `?`, `+`, `-`, `!`, `~`, `++`, `--`, CAST e `await` são chamados de operadores unários.

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

Se o operando de um *unary_expression* tiver o tipo de tempo de compilação `dynamic`, ele será vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação do *unary_expression* é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução do operando.

### <a name="null-conditional-operator"></a>Operador NULL-condicional

O operador NULL-Conditional aplica uma lista de operações ao operando somente se esse operando não for nulo. Caso contrário, o resultado da aplicação do operador será `null`.

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

A lista de operações pode incluir acesso a membros e operações de acesso de elemento (que podem ser nulas-condicionais), bem como invocação.

Por exemplo, a expressão `a.b?[0]?.c()` é uma *null_conditional_expression* com uma *primary_expression* `a.b` e *null_conditional_operations* `?[0]` (acesso a elemento condicional nulo), `?.c` (acesso de membro condicional nulo) e `()` (invocação).

Para um *null_conditional_expression* `E` com um `P`de *primary_expression* , permita que `E0` seja a expressão obtida com a remoção textual da `?` à esquerda de cada *null_conditional_operations* de `E` que tenha um. Conceitualmente, `E0` é a expressão que será avaliada se nenhuma das verificações nulas representadas pelo `?`s encontrar uma `null`.

Além disso, permita que `E1` seja a expressão obtida com a remoção textual da `?` à esquerda apenas da primeira das *null_conditional_operations* no `E`. Isso pode levar a uma *expressão primária* (se houver apenas uma `?`) ou a outra *null_conditional_expression*.

Por exemplo, se `E` for a expressão `a.b?[0]?.c()`, `E0` será a expressão `a.b[0].c()` e `E1` será a `a.b[0]?.c()`de expressão.

Se `E0` for classificada como Nothing, `E` será classificada como Nothing. Caso contrário, E será classificado como um valor.

`E0` e `E1` são usados para determinar o significado de `E`:

*  Se `E` ocorrer como um *statement_expression* o significado de `E` será o mesmo que a instrução

   ```csharp
   if ((object)P != null) E1;
   ```

   Exceto que P é avaliada apenas uma vez.

*  Caso contrário, se `E0` for classificado como nada ocorrerá um erro de tempo de compilação.

*  Caso contrário, permita que `T0` seja o tipo de `E0`.

   *  Se `T0` for um parâmetro de tipo que não é conhecido como um tipo de referência ou um tipo de valor não anulável, ocorrerá um erro em tempo de compilação.

   *  Se `T0` for um tipo de valor não anulável, o tipo de `E` será `T0?`e o significado de `E` será o mesmo que

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      Exceto que `P` é avaliada apenas uma vez.

   *  Caso contrário, o tipo de E é T0, e o significado de E é o mesmo que

      ```csharp
      ((object)P == null) ? null : E1
      ```

      Exceto que `P` é avaliada apenas uma vez.

Se `E1` for um *null_conditional_expression*, essas regras serão aplicadas novamente, aninhando os testes para `null` até que não haja mais `?`, e a expressão foi reduzida até a `E0`de expressão primária...

Por exemplo, se a expressão `a.b?[0]?.c()` ocorrer como uma expressão de instrução, como na instrução:
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

Se ocorrer em um contexto em que seu valor é usado, como em:
```csharp
var x = a.b?[0]?.c();
```
e supondo que o tipo da invocação final não seja um tipo de valor não anulável, seu significado é equivalente a:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Expressões condicionais nulas como inicializadores de projeção

Uma expressão condicional nula só é permitida como uma *member_declarator* em uma *anonymous_object_creation_expression* (expressões de[criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) se terminar com um acesso de membro (opcionalmente nulo). Na gramática, esse requisito pode ser expresso como:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Este é um caso especial da gramática para *null_conditional_expression* acima. A produção para *member_declarator* em [expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions) inclui apenas *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Expressões condicionais nulas como expressões de instrução

Uma expressão condicional nula só é permitida como uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)) se terminar com uma invocação. Na gramática, esse requisito pode ser expresso como:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Este é um caso especial da gramática para *null_conditional_expression* acima. A produção para *statement_expression* em [instruções de expressão](statements.md#expression-statements) , em seguida, inclui apenas *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Operador unário de adição

Para uma operação do formulário `+x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador. Os operadores de adição de unários predefinidos são:

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

### <a name="unary-minus-operator"></a>Operador unário de subtração

Para uma operação do formulário `-x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador. Os operadores de negação predefinidos são:

*  Negação de inteiro:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   O resultado é calculado com a subtração de `x` de zero. Se o valor de `x` for o menor valor representável do tipo de operando (-2 ^ 31 para `int` ou-2 ^ 63 para `long`), a negação matemática de `x` não será representável dentro do tipo de operando. Se isso ocorrer em um contexto de `checked`, um `System.OverflowException` será gerado; Se ocorrer dentro de um contexto de `unchecked`, o resultado será o valor do operando e o estouro não será relatado.

   Se o operando do operador de negação for do tipo `uint`, ele será convertido no tipo `long`e o tipo do resultado será `long`. Uma exceção é a regra que permite que o valor de `int`-2147483648 (-2 ^ 31) seja gravado como um literal inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).

   Se o operando do operador de negação for do tipo `ulong`, ocorrerá um erro em tempo de compilação. Uma exceção é a regra que permite que o `long` valor-9.223.372.036.854.775.808 (-2 ^ 63) seja gravado como um literal inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).

*  Negação de ponto flutuante:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   O resultado é o valor de `x` com seu sinal invertido. Se `x` for NaN, o resultado também será NaN.

*  Negação decimal:

   ```csharp
   decimal operator -(decimal x);
   ```

   O resultado é calculado com a subtração de `x` de zero. A negação decimal é equivalente a usar o operador de subtração unário do tipo `System.Decimal`.

### <a name="logical-negation-operator"></a>Operador de negação lógico

Para uma operação do formulário `!x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador. Existe apenas um operador de negação lógica predefinido:
```csharp
bool operator !(bool x);
```

Esse operador computa a negação lógica do operando: se o operando for `true`, o resultado será `false`. Se o operando for `false`, o resultado será `true`.

### <a name="bitwise-complement-operator"></a>Operador de complemento de bits

Para uma operação do formulário `~x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador. Os operadores predefinidos de Complementos de bits são:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Para cada um desses operadores, o resultado da operação é o complemento bit a bit de `x`.

Todos os tipos de enumeração `E` fornecem implicitamente o seguinte operador de complemento bit a bit:

```csharp
E operator ~(E x);
```

O resultado da avaliação de `~x`, em que `x` é uma expressão de um tipo de enumeração `E` com um tipo subjacente `U`, é exatamente o mesmo que avaliar `(E)(~(U)x)`, exceto que a conversão em `E` é sempre executada como se fosse em um contexto de `unchecked` ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Operadores de incremento e decremento pré-fixados

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

O operando de uma operação de incremento ou diminuição de prefixo deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso de indexador. O resultado da operação é um valor do mesmo tipo que o operando.

Se o operando de uma operação de incremento ou diminuição de prefixo for um acesso de propriedade ou indexador, a propriedade ou o indexador deverá ter um `get` e um acessador `set`. Se esse não for o caso, ocorrerá um erro de tempo de associação.

Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Existem `++` predefinidos e operadores de `--` para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`e qualquer tipo de enumeração. Os operadores de `++` predefinidos retornam o valor produzido pela adição de 1 ao operando e os operadores de `--` predefinidos retornam o valor produzido pela subtração de 1 do operando. Em um contexto `checked`, se o resultado dessa adição ou subtração estiver fora do intervalo do tipo de resultado e o tipo de resultado for um tipo integral ou tipo de enumeração, um `System.OverflowException` será gerado.

O processamento em tempo de execução de uma operação de incremento de prefixo ou diminuição do formulário `++x` ou `--x` consiste nas seguintes etapas:

*   Se `x` for classificado como uma variável:
    * `x` é avaliado para produzir a variável.
    * O operador selecionado é invocado com o valor de `x` como seu argumento.
    * O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.
    * O valor retornado pelo operador torna-se o resultado da operação.
*   Se `x` for classificado como um acesso de propriedade ou indexador:
    * A expressão de instância (se `x` não for `static`) e a lista de argumentos (se `x` for um acesso de indexador) associada a `x` são avaliadas e os resultados são usados nas invocações subsequentes de `get` e `set`.
    * O acessador de `get` de `x` é invocado.
    * O operador selecionado é invocado com o valor retornado pelo acessador `get` como seu argumento.
    * O acessador de `set` de `x` é invocado com o valor retornado pelo operador como seu argumento `value`.
    * O valor retornado pelo operador torna-se o resultado da operação.

Os operadores `++` e `--` também dão suporte à notação de sufixo ([operadores de aumento de sufixo e diminuição](expressions.md#postfix-increment-and-decrement-operators)). Normalmente, o resultado de `x++` ou `x--` é o valor de `x` antes da operação, enquanto o resultado de `++x` ou `--x` é o valor de `x` após a operação. Em ambos os casos, `x` ele mesmo tem o mesmo valor após a operação.

Uma implementação `operator++` ou `operator--` pode ser chamada usando o sufixo ou a notação de prefixo. Não é possível ter implementações de operador separadas para as duas notações.

### <a name="cast-expressions"></a>Expressões de conversão

Uma *cast_expression* é usada para converter explicitamente uma expressão em um determinado tipo.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

Uma *cast_expression* do formulário `(T)E`, em que `T` é um *tipo* e `E` é um *unary_expression*, executa uma conversão explícita ([conversões explícitas](conversions.md#explicit-conversions)) do valor de `E` para o tipo `T`. Se não existir nenhuma conversão explícita de `E` para `T`, ocorrerá um erro de tempo de associação. Caso contrário, o resultado será o valor produzido pela conversão explícita. O resultado é sempre classificado como um valor, mesmo se `E` denota uma variável.

A gramática de um *cast_expression* leva a determinadas ambiguidades sintáticas. Por exemplo, a expressão `(x)-y` poderia ser interpretada como uma *cast_expression* (uma conversão de `-y` para o tipo `x`) ou como um *additive_expression* combinado com um *parenthesized_expression* (que computa o valor `x - y)`.

Para resolver as ambiguidades *cast_expression* , a seguinte regra existe: uma sequência de um ou mais *token*s ([espaço em branco](lexical-structure.md#white-space)) entre parênteses é considerada o início de uma *cast_expression* somente se pelo menos uma das seguintes opções for verdadeira:

*  A sequência de tokens é a gramática correta para um *tipo*, mas não para uma *expressão*.
*  A sequência de tokens é a gramática correta para um *tipo*, e o token imediatamente após os parênteses de fechamento é o token "`~`", o token "`!`", o token "`(`", um *identificador* ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)), um *literal* ([literais](lexical-structure.md#literals)) ou qualquer *palavra-chave* ([palavras-chave](lexical-structure.md#keywords)), exceto `as` e `is`.

O termo "correção gramatical" acima significa apenas que a sequência de tokens deve estar de acordo com a produção gramatical específica. Ele não considera especificamente o significado real de quaisquer identificadores constituintes. Por exemplo, se `x` e `y` forem identificadores, `x.y` será a gramática correta para um tipo, mesmo se `x.y` não denotar realmente um tipo.

A partir da regra de desambiguidade, ela é a seguinte, se `x` e `y` são identificadores, `(x)y`, `(x)(y)`e `(x)(-y)` são *cast_expression*s, mas `(x)-y` não é, mesmo que `x` identifique um tipo. No entanto, se `x` for uma palavra-chave que identifica um tipo predefinido (como `int`), todas as quatro formas serão *cast_expression*s (porque essa palavra-chave não poderia ser, por si só, uma expressão).

### <a name="await-expressions"></a>Expressões Await

O operador Await é usado para suspender a avaliação da função Async delimitadora até que a operação assíncrona representada pelo operando tenha sido concluída.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Um *await_expression* só é permitido no corpo de uma função Async ([iteradores](classes.md#iterators)). Na função assíncrona delimitadora mais próxima, um *await_expression* pode não ocorrer nesses locais:

*  Dentro de uma função anônima aninhada (não assíncrona)
*  Dentro do bloco de um *lock_statement*
*  Em um contexto sem segurança

Observe que um *await_expression* não pode ocorrer na maioria dos lugares em um *query_expression*, pois eles são sintaticamente transformados para usar expressões lambda não assíncronas.

Dentro de uma função Async, `await` não pode ser usada como um identificador. Portanto, não há nenhuma ambiguidade sintática entre Await-Expressions e várias expressões que envolvem identificadores. Fora das funções assíncronas, `await` atua como um identificador normal.

O operando de um *await_expression* é chamado de ***tarefa***. Ele representa uma operação assíncrona que pode ou não ser concluída no momento em que a *await_expression* for avaliada. A finalidade do operador Await é suspender a execução da função Async delimitadora até que a tarefa esperada seja concluída e, em seguida, obter seu resultado.

#### <a name="awaitable-expressions"></a>Expressões awaitable

A tarefa de uma expressão Await deve ser ***awaitable***. Uma expressão `t` será awaitable se uma das seguintes isenções:

*  `t` é do tipo de tempo de compilação `dynamic`
*  `t` tem uma instância acessível ou método de extensão chamado `GetAwaiter` sem parâmetros e nenhum parâmetro de tipo, e um tipo de retorno `A` para o qual todos os seguintes itens contêm:
   * `A` implementa a interface `System.Runtime.CompilerServices.INotifyCompletion` (daqui em diante, conhecida como `INotifyCompletion` para fins de brevidade)
   * `A` tem uma propriedade de instância acessível e legível `IsCompleted` do tipo `bool`
   * `A` tem um método de instância acessível `GetResult` sem parâmetros e nenhum parâmetro de tipo

A finalidade do método de `GetAwaiter` é obter um ***aguardador*** para a tarefa. O tipo `A` é chamado de ***tipo de aguardador*** para a expressão Await.

A finalidade da propriedade `IsCompleted` é determinar se a tarefa já foi concluída. Nesse caso, não há necessidade de suspender a avaliação.

A finalidade do método de `INotifyCompletion.OnCompleted` é inscrever uma "continuação" para a tarefa; ou seja, um delegado (do tipo `System.Action`) que será invocado quando a tarefa for concluída.

A finalidade do método de `GetResult` é obter o resultado da tarefa quando ela for concluída. Esse resultado pode ser uma conclusão bem-sucedida, possivelmente com um valor de resultado, ou pode ser uma exceção que é gerada pelo método de `GetResult`.

#### <a name="classification-of-await-expressions"></a>Classificação de expressões Await

A expressão `await t` é classificada da mesma maneira que a expressão `(t).GetAwaiter().GetResult()`. Portanto, se o tipo de retorno de `GetResult` for `void`, o *await_expression* será classificado como Nothing. Se ele tiver um tipo de retorno não void `T`, o *await_expression* será classificado como um valor do tipo `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Avaliação de tempo de execução de expressões Await

Em tempo de execução, a expressão `await t` é avaliada da seguinte maneira:

*  Uma `a` de aguardador é obtida avaliando a expressão `(t).GetAwaiter()`.
*  Um `b` `bool` é obtido avaliando a expressão `(a).IsCompleted`.
*  Se `b` for `false`, a avaliação dependerá se `a` implementa a interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (daqui em diante, conhecida como `ICriticalNotifyCompletion` para fins de brevidade). Essa verificação é feita no momento da Associação; ou seja, em tempo de execução, se `a` tiver o tipo de tempo de compilação `dynamic`e, em caso contrário, o tempo de compilação. Permitir que `r` denotar o delegado de continuação ([iteradores](classes.md#iterators)):
    * Se `a` não implementar `ICriticalNotifyCompletion`, a expressão `(a as (INotifyCompletion)).OnCompleted(r)` será avaliada.
    * Se `a` implementa `ICriticalNotifyCompletion`, a expressão `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` é avaliada.
    * A avaliação é suspensa e o controle é retornado para o chamador atual da função Async.
*  Imediatamente após (se `b` foi `true`) ou após a invocação posterior do delegado de continuação (se `b` era `false`), a expressão `(a).GetResult()` é avaliada. Se ele retornar um valor, esse valor será o resultado da *await_expression*. Caso contrário, o resultado será Nothing.

A implementação de um esperador dos métodos de interface `INotifyCompletion.OnCompleted` e `ICriticalNotifyCompletion.UnsafeOnCompleted` deve fazer com que o delegado `r` seja invocado no máximo uma vez. Caso contrário, o comportamento da função assíncrona delimitadora será indefinido.

## <a name="arithmetic-operators"></a>Operadores aritméticos

Os operadores `*`, `/`, `%`, `+`e `-` são chamados de operadores aritméticos.

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

Se um operando de um operador aritmético tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.

### <a name="multiplication-operator"></a>Operador de multiplicação

Para uma operação do formulário `x * y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores de multiplicação predefinidos estão listados abaixo. Todos os operadores computam o produto de `x` e `y`.

*  Multiplicação de inteiro:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   Em um contexto de `checked`, se o produto estiver fora do intervalo do tipo de resultado, um `System.OverflowException` será gerado. Em um contexto de `unchecked`, os estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.


*  Multiplicação de ponto flutuante:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   O produto é calculado de acordo com as regras de aritmética de IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN. Na tabela, `x` e `y` são valores finitos positivos. `z` é o resultado de `x * y`. Se o resultado for muito grande para o tipo de destino, `z` será infinito. Se o resultado for muito pequeno para o tipo de destino, `z` será zero.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | +y   | -y   | +0  | -0  | +inf | -inf | {1&gt;NaN&lt;1} | 
   | {1&gt;+&lt;1}x   | +z   | -z   | +0  | -0  | +inf | -inf | {1&gt;NaN&lt;1} | 
   | -x   | -z   | +z   | -0  | +0  | -inf | +inf | {1&gt;NaN&lt;1} | 
   | +0   | +0   | -0   | +0  | -0  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | -0   | -0   | +0   | -0  | +0  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | +inf | +inf | -inf | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | +inf | -inf | {1&gt;NaN&lt;1} | 
   | -inf | -inf | +inf | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | -inf | +inf | {1&gt;NaN&lt;1} | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 

*  Multiplicação decimal:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado. Se o valor do resultado for muito pequeno para representar no formato de `decimal`, o resultado será zero. A escala do resultado, antes de qualquer arredondamento, é a soma das escalas dos dois operandos.

   A multiplicação decimal é equivalente a usar o operador de multiplicação do tipo `System.Decimal`.


### <a name="division-operator"></a>Operador de divisão

Para uma operação do formulário `x / y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores de divisão predefinidos estão listados abaixo. Todos os operadores computam o quociente de `x` e `y`.

*  Divisão de inteiro:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Se o valor do operando direito for zero, um `System.DivideByZeroException` será lançado.

   A divisão arredonda o resultado em direção a zero. Portanto, o valor absoluto do resultado é o maior inteiro possível que é menor ou igual ao valor absoluto do quociente dos dois operandos. O resultado é zero ou positivo quando os dois operandos têm o mesmo sinal e zero ou negativos quando os dois operandos têm sinais opostos.

   Se o operando esquerdo for o menor `int` representável ou o valor de `long` e o operando direito for `-1`, ocorrerá um estouro. Em um contexto de `checked`, isso faz com que um `System.ArithmeticException` (ou uma subclasse dele) seja gerado. Em um contexto de `unchecked`, ele é definido pela implementação para determinar se um `System.ArithmeticException` (ou uma subclasse dele) é gerado ou se o estouro não é reportado com o valor resultante sendo o do operando esquerdo.

*  Divisão de ponto flutuante:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   O quociente é calculado de acordo com as regras de aritmética de IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN. Na tabela, `x` e `y` são valores finitos positivos. `z` é o resultado de `x / y`. Se o resultado for muito grande para o tipo de destino, `z` será infinito. Se o resultado for muito pequeno para o tipo de destino, `z` será zero.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | {1&gt;NaN&lt;1}  | 
   | {1&gt;+&lt;1}x   | +z   | -z   | +inf | -inf | +0   | -0   | {1&gt;NaN&lt;1}  | 
   | -x   | -z   | +z   | -inf | +inf | -0   | +0   | {1&gt;NaN&lt;1}  | 
   | +0   | +0   | -0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | +0   | -0   | {1&gt;NaN&lt;1}  | 
   | -0   | -0   | +0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -0   | +0   | {1&gt;NaN&lt;1}  | 
   | +inf | +inf | -inf | +inf | -inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -inf | -inf | +inf | -inf | +inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Divisão decimal:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Se o valor do operando direito for zero, um `System.DivideByZeroException` será lançado. Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado. Se o valor do resultado for muito pequeno para representar no formato de `decimal`, o resultado será zero. A escala do resultado é a menor escala que preservará um resultado igual ao valor decimal representável mais próximo para o resultado matemático verdadeiro.

   A divisão decimal é equivalente a usar o operador de divisão do tipo `System.Decimal`.


### <a name="remainder-operator"></a>Operador de resto

Para uma operação do formulário `x % y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores restantes predefinidos estão listados abaixo. Todos os operadores calculam o restante da divisão entre `x` e `y`.

*  Restante do inteiro:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   O resultado de `x % y` é o valor produzido por `x - (x / y) * y`. Se `y` for zero, um `System.DivideByZeroException` será gerado.

   Se o operando esquerdo for o menor `int` ou valor de `long` e o operando direito for `-1`, um `System.OverflowException` será gerado. Em nenhuma hipótese `x % y` gerar uma exceção em que `x / y` não geraria uma exceção.

*  Restante do ponto flutuante:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN. Na tabela, `x` e `y` são valores finitos positivos. `z` é o resultado de `x % y` e é computado como `x - n * y`, em que `n` é o maior inteiro possível que é menor ou igual a `x / y`. Esse método de computação do restante é análogo ao usado para operandos inteiros, mas difere da definição do IEEE 754 (em que `n` é o número inteiro mais próximo de `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | {1&gt;NaN&lt;1}  | 
   | {1&gt;+&lt;1}x   | +z   | +z   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | x    | x    | {1&gt;NaN&lt;1}  | 
   | -x   | -z   | -z   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -x   | -x   | {1&gt;NaN&lt;1}  | 
   | +0   | +0   | +0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | +0   | +0   | {1&gt;NaN&lt;1}  | 
   | -0   | -0   | -0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -0   | -0   | {1&gt;NaN&lt;1}  | 
   | +inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Restante decimal:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Se o valor do operando direito for zero, um `System.DivideByZeroException` será lançado. A escala do resultado, antes de qualquer arredondamento, é maior das escalas dos dois operandos, e o sinal do resultado, se for diferente de zero, é o mesmo que o de `x`.

   O restante decimal é equivalente a usar o operador resto do tipo `System.Decimal`.


### <a name="addition-operator"></a>Operador de adição

Para uma operação do formulário `x + y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores de adição predefinidos são listados abaixo. Para tipos numéricos e de enumeração, os operadores de adição predefinidos computam a soma dos dois operandos. Quando um ou ambos os operandos são do tipo cadeia de caracteres, os operadores de adição predefinidos concatenam a representação de cadeia de caracteres dos operandos.

*  Adição de inteiro:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   Em um contexto de `checked`, se a soma estiver fora do intervalo do tipo de resultado, uma `System.OverflowException` será lançada. Em um contexto de `unchecked`, os estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.

*  Adição de ponto flutuante:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   A soma é calculada de acordo com as regras de aritmética de IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN. Na tabela, `x` e `y` são valores finitos diferentes de zero e `z` é o resultado de `x + y`. Se `x` e `y` tiverem a mesma magnitude, mas sinais opostos, `z` será zero positivo. Se `x + y` for muito grande para representar no tipo de destino, `z` será um infinito com o mesmo sinal que `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | {1&gt;y&lt;1}    | +0   | -0   | +inf | -inf | {1&gt;NaN&lt;1}  | 
   | x    | {1&gt;de&lt;1}    | x    | x    | +inf | -inf | {1&gt;NaN&lt;1}  | 
   | +0   | {1&gt;y&lt;1}    | +0   | +0   | +inf | -inf | {1&gt;NaN&lt;1}  | 
   | -0   | {1&gt;y&lt;1}    | +0   | -0   | +inf | -inf | {1&gt;NaN&lt;1}  | 
   | +inf | +inf | +inf | +inf | +inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -inf | -inf | -inf | -inf | {1&gt;NaN&lt;1}  | -inf | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Adição de decimal:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado. A escala do resultado, antes de qualquer arredondamento, é maior das escalas dos dois operandos.

   A adição de decimal é equivalente a usar o operador de adição do tipo `System.Decimal`.

*  Adição de enumeração. Cada tipo de enumeração fornece implicitamente os seguintes operadores predefinidos, em que `E` é o tipo de enumeração e `U` é o tipo subjacente de `E`:

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

   Essas sobrecargas do operador de `+` binário executam a concatenação de cadeia de caracteres. Se um operando de concatenação de cadeia de caracteres for `null`, uma cadeia de caracteres vazia será substituída. Caso contrário, qualquer argumento que não seja de cadeia de caracteres é convertido em sua representação de cadeia de caracteres invocando o método de `ToString` virtual herdado do tipo `object`. Se `ToString` retornar `null`, uma cadeia de caracteres vazia será substituída.

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

   O resultado do operador de concatenação de cadeia de caracteres é uma cadeia de caracteres que consiste nos caracteres do operando esquerdo seguidos dos caracteres do operando à direita. O operador de concatenação de cadeia de caracteres nunca retorna um valor `null`. Um `System.OutOfMemoryException` poderá ser gerado se não houver memória suficiente disponível para alocar a cadeia de caracteres resultante.

*  Combinação de delegado. Cada tipo delegate fornece implicitamente o seguinte operador predefinido, em que `D` é o tipo delegado:

   ```csharp
   D operator +(D x, D y);
   ```

   O operador Binary `+` executa a combinação de delegado quando ambos os operandos são de algum tipo delegado `D`. (Se os operandos tiverem tipos delegados diferentes, ocorrerá um erro de tempo de associação.) Se o primeiro operando for `null`, o resultado da operação será o valor do segundo operando (mesmo que também seja `null`). Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando. Caso contrário, o resultado da operação é uma nova instância de delegado que, quando invocada, invoca o primeiro operando e, em seguida, invoca o segundo operando. Para obter exemplos de combinação de delegado, consulte [operador de subtração](expressions.md#subtraction-operator) e [invocação de delegado](delegates.md#delegate-invocation). Como `System.Delegate` não é um tipo delegado, `operator` `+` não está definido para ele.

### <a name="subtraction-operator"></a>Operador de subtração

Para uma operação do formulário `x - y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores de subtração predefinidos estão listados abaixo. Os operadores todos subtraiem `y` de `x`.

*  Subtração de inteiro:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   Em um contexto de `checked`, se a diferença estiver fora do intervalo do tipo de resultado, um `System.OverflowException` será gerado. Em um contexto de `unchecked`, os estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.

*  Subtração de ponto flutuante:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   A diferença é calculada de acordo com as regras de aritmética de IEEE 754. A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaNs. Na tabela, `x` e `y` são valores finitos diferentes de zero e `z` é o resultado de `x - y`. Se `x` e `y` forem iguais, `z` será zero positivo. Se `x - y` for muito grande para representar no tipo de destino, `z` será um infinito com o mesmo sinal que `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | {1&gt;y&lt;1}    | +0   | -0   | +inf | -inf | {1&gt;NaN&lt;1} | 
   | x    | {1&gt;de&lt;1}    | x    | x    | -inf | +inf | {1&gt;NaN&lt;1} | 
   | +0   | -y   | +0   | +0   | -inf | +inf | {1&gt;NaN&lt;1} | 
   | -0   | -y   | -0   | +0   | -inf | +inf | {1&gt;NaN&lt;1} | 
   | +inf | +inf | +inf | +inf | {1&gt;NaN&lt;1}  | +inf | {1&gt;NaN&lt;1} | 
   | -inf | -inf | -inf | -inf | -inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 

*  Subtração decimal:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado. A escala do resultado, antes de qualquer arredondamento, é maior das escalas dos dois operandos.

   A subtração decimal é equivalente a usar o operador de subtração do tipo `System.Decimal`.

*  Subtração de enumeração. Cada tipo de enumeração fornece implicitamente o seguinte operador predefinido, em que `E` é o tipo de enumeração e `U` é o tipo subjacente de `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Esse operador é avaliado exatamente como `(U)((U)x - (U)y)`. Em outras palavras, o operador computa a diferença entre os valores ordinais de `x` e `y`, e o tipo de resultado é o tipo subjacente da enumeração.

   ```csharp
   E operator -(E x, U y);
   ```

   Esse operador é avaliado exatamente como `(E)((U)x - y)`. Em outras palavras, o operador subtrai um valor do tipo subjacente da enumeração, produzindo um valor da enumeração.

*  Remoção de delegado. Cada tipo delegate fornece implicitamente o seguinte operador predefinido, em que `D` é o tipo delegado:

   ```csharp
   D operator -(D x, D y);
   ```

   O operador Binary `-` executa a remoção de delegado quando ambos os operandos são de algum tipo delegado `D`. Se os operandos tiverem tipos delegados diferentes, ocorrerá um erro de tempo de associação. Se o primeiro operando for `null`, o resultado da operação será `null`. Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando. Caso contrário, os dois operandos representam listas de invocação ([declarações de delegado](delegates.md#delegate-declarations)) que têm uma ou mais entradas e o resultado é uma nova lista de invocação que consiste na lista do primeiro operando com as entradas do segundo operando removidas, desde que a lista do segundo operando seja uma sublista contígua adequada do primeiro.     (Para determinar a igualdade da sublista, as entradas correspondentes são comparadas com o operador de igualdade de delegado ([operadores de igualdade de delegado](expressions.md#delegate-equality-operators)).) Caso contrário, o resultado será o valor do operando esquerdo. Nenhuma das listas de operandos é alterada no processo. Se a lista do segundo operando corresponder a várias sublistas de entradas contíguas na lista do primeiro operando, a sublista correspondente à direita de entradas contíguas será removida. Se a remoção resultar em uma lista vazia, o resultado será `null`. Por exemplo:

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

Os operadores de `<<` e `>>` são usados para executar operações de mudança de bits.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Se um operando de um *shift_expression* tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.

Para uma operação do formulário `x << count` ou `x >> count`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Ao declarar um operador de deslocamento sobrecarregado, o tipo do primeiro operando sempre deve ser a classe ou struct que contém a declaração do operador e o tipo do segundo operando sempre deve ser `int`.

Os operadores de alternância predefinidos estão listados abaixo.

*  Deslocar para a esquerda:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   O operador `<<` muda `x` à esquerda por um número de bits calculados, conforme descrito abaixo.

   Os bits de ordem superior fora do intervalo do tipo de resultado de `x` são descartados, os bits restantes são deslocados para a esquerda e as posições de bit vazia de ordem inferior são definidas como zero.

*  Deslocar para a direita:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   O operador `>>` muda `x` para a direita por um número de bits calculados, conforme descrito abaixo.

   Quando `x` é do tipo `int` ou `long`, os bits de ordem inferior de `x` são descartados, os bits restantes são deslocados para a direita e as posições de bit vazio de ordem superior são definidas como zero se `x` não for negativo e definido como um se `x` for negativo.

   Quando `x` é do tipo `uint` ou `ulong`, os bits de ordem inferior de `x` são descartados, os bits restantes são deslocados para a direita e as posições de bit vazio de ordem superior são definidas como zero.

Para os operadores predefinidos, o número de bits a serem deslocados é calculado da seguinte maneira:

*  Quando o tipo de `x` é `int` ou `uint`, a contagem de turnos é fornecida pelos cinco bits de ordem inferior de `count`. Em outras palavras, a contagem de deslocamento é calculada a partir de `count & 0x1F`.
*  Quando o tipo de `x` é `long` ou `ulong`, a contagem de turnos é fornecida pelos seis bits de ordem inferior de `count`. Em outras palavras, a contagem de deslocamento é calculada a partir de `count & 0x3F`.

Se a contagem de deslocamento resultante for zero, os operadores de alternância simplesmente retornarão o valor de `x`.

As operações de Shift nunca causam estouros e produzem os mesmos resultados em contextos `checked` e `unchecked`.

Quando o operando esquerdo do operador de `>>` for de um tipo integral assinado, o operador executará um deslocamento aritmético à direita, no qual o valor do bit mais significativo (o bit de sinal) do operando será propagado para as posições de bit vazio de ordem superior. Quando o operando esquerdo do operador de `>>` é de um tipo integral não assinado, o operador executa um direito de deslocamento lógico onde as posições de bits vazias de ordem superior são sempre definidas como zero. Para executar a operação oposta da inferida do tipo de operando, as conversões explícitas podem ser usadas. Por exemplo, se `x` for uma variável do tipo `int`, a operação `unchecked((int)((uint)x >> y))` executará um deslocamento lógico à direita de `x`.

## <a name="relational-and-type-testing-operators"></a>Operadores de teste de tipo e relacional

Os operadores `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` e `as` são chamados de operadores relacionais e de teste de tipo.

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

O operador de `is` é descrito no [operador is](expressions.md#the-is-operator) e o operador `as` é descrito no [operador as](expressions.md#the-as-operator).

Os operadores `==`, `!=`, `<`, `>`, `<=` e `>=` são ***operadores de comparação***.

Se um operando de um operador de comparação tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.

Para uma operação do formulário `x` *op* `y`, em que *op* é um operador de comparação, a resolução de sobrecarga ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores de comparação predefinidos são descritos nas seções a seguir. Todos os operadores de comparação predefinidos retornam um resultado do tipo `bool`, conforme descrito na tabela a seguir.


| __Operação__ | __Result__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` se `x` for igual a `y`, `false` de outra forma                 | 
| `x != y`      | `true` se `x` não for igual a `y`, `false` de outra forma             | 
| `x < y`       | `true` se `x` for menor que `y`, `false` de outra forma                | 
| `x > y`       | `true` se `x` for maior que `y`, `false` de outra forma             | 
| `x <= y`      | `true` se `x` for menor ou igual a `y`, `false` caso contrário    | 
| `x >= y`      | `true` se `x` for maior ou igual a `y`, `false` de outra forma | 

### <a name="integer-comparison-operators"></a>Operadores de comparação de inteiros

Os operadores de comparação de inteiros predefinidos são:
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

Cada um desses operadores compara os valores numéricos dos dois operando inteiros e retorna um valor `bool` que indica se a relação específica é `true` ou `false`.

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

*  Se qualquer operando for NaN, o resultado será `false` para todos os operadores, exceto `!=`, para o qual o resultado é `true`. Para quaisquer dois operandos, `x != y` sempre produz o mesmo resultado que `!(x == y)`. No entanto, quando um ou ambos os operandos forem NaN, os operadores `<`, `>`, `<=`e `>=` não produzirão os mesmos resultados que a negação lógica do operador oposto. Por exemplo, se um dos `x` e `y` for NaN, `x < y` será `false`, mas `!(x >= y)` será `true`.
*  Quando nenhum operando é NaN, os operadores comparam os valores dos dois operandos de ponto flutuante em relação à ordenação

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   onde `min` e `max` são os menores e maiores valores finitos positivos que podem ser representados no formato de ponto flutuante fornecido. Os efeitos notáveis dessa ordem são:
   * Zeros negativos e positivos são considerados iguais.
   * Um infinito negativo é considerado menor que todos os outros valores, mas é igual a outro infinito negativo.
   * Um infinito positivo é considerado maior que todos os outros valores, mas é igual a outro infinito positivo.

### <a name="decimal-comparison-operators"></a>Operadores de comparação decimal

Os operadores de comparação decimal predefinidos são:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Cada um desses operadores compara os valores numéricos dos dois operando decimais e retorna um valor `bool` que indica se a relação específica é `true` ou `false`. Cada comparação decimal é equivalente a usar o operador relacional ou de igualdade correspondente do tipo `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operadores de igualdade boolianos

Os operadores de igualdade boolianos predefinidos são:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

O resultado de `==` é `true` se `x` e `y` são `true` ou se `x` e `y` são `false`. Caso contrário, o resultado será `false`.

O resultado de `!=` é `false` se `x` e `y` são `true` ou se `x` e `y` são `false`. Caso contrário, o resultado será `true`. Quando os operandos são do tipo `bool`, o operador `!=` produz o mesmo resultado que o operador de `^`.

### <a name="enumeration-comparison-operators"></a>Operadores de comparação de enumeração

Cada tipo de enumeração fornece implicitamente os seguintes operadores de comparação predefinidos:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

O resultado da avaliação de `x op y`, em que `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`, e `op` é um dos operadores de comparação, é exatamente o mesmo que avaliar `((U)x) op ((U)y)`. Em outras palavras, os operadores de comparação de tipo de enumeração apenas comparam os valores integrais subjacentes dos dois operandos.

### <a name="reference-type-equality-operators"></a>Operadores de igualdade de tipo de referência

Os operadores de igualdade do tipo de referência predefinido são:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Os operadores retornam o resultado da comparação das duas referências para igualdade ou não igualdade.

Como os operadores de igualdade do tipo de referência predefinido aceitam operandos do tipo `object`, eles se aplicam a todos os tipos que não declaram os membros `operator ==` e `operator !=` aplicáveis. Por outro lado, qualquer operador de igualdade definido pelo usuário aplicável efetivamente oculta os operadores de igualdade do tipo de referência predefinido.

Os operadores de igualdade do tipo de referência predefinidos exigem um dos seguintes:

*  Ambos os operandos são um valor de um tipo conhecido como um *reference_type* ou o `null`literal. Além disso, uma conversão de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) existe no tipo de um dos operandos para o tipo do outro operando.
*  Um operando é um valor do tipo `T` em que `T` é uma *type_parameter* e o outro operando é o `null`literal. Além disso `T` não tem a restrição de tipo de valor.

A menos que uma dessas condições seja verdadeira, ocorre um erro de tempo de associação. As principais implicações dessas regras são:

*  É um erro de tempo de ligação para usar os operadores de igualdade do tipo de referência predefinido para comparar duas referências que são conhecidas para serem diferentes no tempo de associação. Por exemplo, se os tipos de tempo de Associação dos operandos forem dois tipos de classe `A` e `B`, e se nem `A` nem `B` derivar de outro, seria impossível para os dois operandos referenciarem o mesmo objeto. Assim, a operação é considerada um erro de tempo de associação.
*  Os operadores de igualdade do tipo de referência predefinido não permitem que os operandos de tipo de valor sejam comparados. Portanto, a menos que um tipo de struct declare seus próprios operadores de igualdade, não é possível comparar valores desse tipo struct.
*  Os operadores de igualdade do tipo de referência predefinido nunca causam a ocorrência de operações Boxing para seus operandos. Seria sem sentido executar essas operações boxing, já que as referências às instâncias emolduradas recentemente alocadas seriam necessariamente diferentes de todas as outras referências.
*  Se um operando de um tipo de parâmetro de tipo `T` for comparado com `null`e o tipo de tempo de execução de `T` for um tipo de valor, o resultado da comparação será `false`.

O exemplo a seguir verifica se um argumento de um tipo de parâmetro de tipo irrestrito é `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

A construção `x == null` é permitida embora `T` possa representar um tipo de valor, e o resultado é simplesmente definido como `false` quando `T` é um tipo de valor.

Para uma operação do formulário `x == y` ou `x != y`, se qualquer `operator ==` aplicável ou `operator !=` existir, as regras de resolução de sobrecarga de operador ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) selecionarão esse operador em vez do operador de igualdade do tipo de referência predefinido. No entanto, sempre é possível selecionar o operador de igualdade do tipo de referência predefinido, convertendo explicitamente um ou ambos os operandos para o tipo `object`. O exemplo
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
```console
True
False
False
False
```

As variáveis `s` e `t` referem-se a duas instâncias de `string` distintas contendo os mesmos caracteres. A primeira comparação gera `True` porque o operador de igualdade de cadeia de caracteres predefinido ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)) é selecionado quando ambos os operandos são do tipo `string`. As comparações restantes são todas `False` de saída porque o tipo de referência predefinido operador de igualdade é selecionado quando um ou ambos os operandos são do tipo `object`.

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
gera `False` porque as conversões criam referências a duas instâncias separadas de valores de `int` boxed.

### <a name="string-equality-operators"></a>Operadores de igualdade de cadeia de caracteres

Os operadores de igualdade de cadeia de caracteres predefinidos são:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dois valores de `string` são considerados iguais quando um dos seguintes é verdadeiro:

*  Ambos os valores são `null`.
*  Ambos os valores são referências não nulas a instâncias de cadeia de caracteres que têm comprimentos idênticos e caracteres idênticos em cada posição de caractere.

Os operadores de igualdade de cadeia de caracteres comparam valores de cadeia de caracteres em vez de referências Quando duas instâncias de cadeias de caracteres separadas contêm exatamente a mesma sequência de caracteres, os valores das cadeias são iguais, mas as referências são diferentes. Conforme descrito em [operadores de igualdade de tipo de referência](expressions.md#reference-type-equality-operators), os operadores de igualdade de tipo de referência podem ser usados para comparar referências de cadeia de caracteres em vez de valores de cadeia de caracteres

### <a name="delegate-equality-operators"></a>Delegar operadores de igualdade

Cada tipo de delegado fornece implicitamente os seguintes operadores de comparação predefinidos:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Duas instâncias de delegado são consideradas iguais da seguinte maneira:

*  Se uma das instâncias de delegado for `null`, elas serão iguais se e somente se ambas forem `null`.
*  Se os delegados tiverem um tipo de tempo de execução diferente, nunca serão iguais.
*  Se ambas as instâncias de delegado tiverem uma lista de invocação ([declarações de delegado](delegates.md#delegate-declarations)), essas instâncias serão iguais se e somente se suas listas de invocação tiverem o mesmo comprimento, e cada entrada na lista de invocação de uma for igual (conforme definido abaixo) à entrada correspondente, na ordem, na lista de invocação do outro.

As regras a seguir regem a igualdade de entradas da lista de invocação:

*  Se duas entradas da lista de invocação fizerem referência ao mesmo método estático, as entradas serão iguais.
*  Se duas entradas da lista de invocação fizerem referência ao mesmo método não-estático no mesmo objeto de destino (conforme definido pelos operadores de igualdade de referência), as entradas serão iguais.
*  As entradas da lista de invocação produzidas da avaliação de *anonymous_method_expression*s ou *lambda_expressions*semanticamente idênticas com o mesmo conjunto (possivelmente vazio) de instâncias de variáveis externas capturadas são permitidas (mas não obrigatórias) para serem iguais.

### <a name="equality-operators-and-null"></a>Operadores de igualdade e nulo

Os operadores `==` e `!=` permitem que um operando seja um valor de um tipo anulável e o outro para ser o `null` literal, mesmo que nenhum operador predefinido ou definido pelo usuário (em forma não elevada ou levantada) exista para a operação.

Para uma operação de um dos formulários
```csharp
x == null
null == x
x != null
null != x
```
em que `x` é uma expressão de um tipo anulável, se a resolução de sobrecarga do operador ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) falhar ao localizar um operador aplicável, o resultado será calculado da propriedade `HasValue` de `x`. Especificamente, as duas primeiras formas são convertidas em `!x.HasValue`, e os últimos dois formulários são traduzidos em `x.HasValue`.

### <a name="the-is-operator"></a>O operador is

O operador `is` é usado para verificar dinamicamente se o tipo de tempo de execução de um objeto é compatível com um determinado tipo. O resultado da operação `E is T`, em que `E` é uma expressão e `T` é um tipo, é um valor booliano que indica se `E` pode ser convertido com êxito no tipo `T` por uma conversão de referência, uma conversão boxing ou uma conversão unboxing. A operação é avaliada da seguinte maneira, após os argumentos de tipo terem sido substituídos por todos os parâmetros de tipo:

*  Se `E` for uma função anônima, ocorrerá um erro em tempo de compilação
*  Se `E` for um grupo de métodos ou o literal de `null`, de se o tipo de `E` for um tipo de referência ou um tipo anulável e o valor de `E` for NULL, o resultado será false.
*  Caso contrário, permita que `D` represente o tipo dinâmico de `E` da seguinte maneira:
   * Se o tipo de `E` for um tipo de referência, `D` será o tipo de tempo de execução da referência de instância por `E`.
   * Se o tipo de `E` for um tipo anulável, `D` será o tipo subjacente desse tipo anulável.
   * Se o tipo de `E` for um tipo de valor não anulável, `D` será o tipo de `E`.
*  O resultado da operação depende `D` e `T` da seguinte maneira:
   * Se `T` for um tipo de referência, o resultado será true se `D` e `T` forem do mesmo tipo, se `D` for um tipo de referência e uma conversão de referência implícita de `D` para `T` existir ou se `D` for um tipo de valor e uma conversão de Boxing de `D` para `T` existir.
   * Se `T` for um tipo anulável, o resultado será true se `D` for o tipo subjacente de `T`.
   * Se `T` for um tipo de valor não anulável, o resultado será true se `D` e `T` forem do mesmo tipo.
   * Caso contrário, o resultado será false.

Observe que as conversões definidas pelo usuário não são consideradas pelo operador de `is`.

### <a name="the-as-operator"></a>O operador as

O operador `as` é usado para converter explicitamente um valor em um determinado tipo de referência ou tipo anulável. Ao contrário de uma expressão de conversão ([expressões de conversão](expressions.md#cast-expressions)), o operador de `as` nunca gera uma exceção. Em vez disso, se a conversão indicada não for possível, o valor resultante será `null`.

Em uma operação do formulário `E as T`, `E` deve ser uma expressão e `T` deve ser um tipo de referência, um parâmetro de tipo conhecido como um tipo de referência ou um tipo anulável. Além disso, pelo menos um dos seguintes deve ser verdadeiro ou um erro de tempo de compilação ocorre:

*  Uma identidade ([conversão de identidade](conversions.md#identity-conversion)), anulável implícita[(conversões anuláveis implícitas](conversions.md#implicit-nullable-conversions)), referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)), conversão de Boxing ([conversões de conversão](conversions.md#boxing-conversions)de Boxing), nulo explícito ([conversões anuláveis explícitas](conversions.md#explicit-nullable-conversions)), a referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) ou não há conversões unboxing ([unboxing](conversions.md#unboxing-conversions)) de `E` para `T`.
*  O tipo de `E` ou `T` é um tipo aberto.
*  `E` é o literal de `null`.

Se o tipo de tempo de compilação de `E` não for `dynamic`, a operação `E as T` produzirá o mesmo resultado que
```csharp
E is T ? (T)(E) : (T)null
```
exceto que `E` é avaliado apenas uma vez. O compilador pode ser esperado para otimizar `E as T` executar no máximo uma verificação de tipo dinâmico em oposição às duas verificações de tipo dinâmico implícitas pela expansão acima.

Se o tipo de tempo de compilação de `E` for `dynamic`, diferentemente do operador de conversão, o operador de `as` não será vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Portanto, a expansão, nesse caso, é:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Observe que algumas conversões, como conversões definidas pelo usuário, não são possíveis com o operador de `as` e, em vez disso, devem ser executadas usando expressões de conversão.

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
o parâmetro de tipo `T` de `G` é conhecido como um tipo de referência, porque ele tem a restrição de classe. No entanto, o parâmetro de tipo `U` de `H`; Portanto, o uso do operador de `as` no `H` não é permitido.

## <a name="logical-operators"></a>Operadores lógicos

Os operadores `&`, `^`e `|` são chamados de operadores lógicos.

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

Se um operando de um operador lógico tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.

Para uma operação do formulário `x op y`, em que `op` é um dos operadores lógicos, a resolução de sobrecarga ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica. Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.

Os operadores lógicos predefinidos são descritos nas seções a seguir.

### <a name="integer-logical-operators"></a>Operadores lógicos de inteiros

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

O operador de `&` computa o `AND` lógico de bits de dois operandos, o operador de `|` computa os `OR` lógicos de bit-de-bit dos dois operandos, e o operador `^` computa a `OR` exclusiva de bits lógicas dos dois operandos. Não são possíveis estouros dessas operações.

### <a name="enumeration-logical-operators"></a>Operadores lógicos de enumeração

Cada tipo de enumeração `E` fornece implicitamente os seguintes operadores lógicos predefinidos:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

O resultado da avaliação de `x op y`, em que `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`e `op` é um dos operadores lógicos, é exatamente o mesmo que avaliar `(E)((U)x op (U)y)`. Em outras palavras, os operadores lógicos do tipo de enumeração simplesmente executam a operação lógica no tipo subjacente dos dois operandos.

### <a name="boolean-logical-operators"></a>Operadores lógicos boolianos

Os operadores lógicos boolianos predefinidos são:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

O resultado de `x & y` será `true` se ambos `x` e `y` forem `true`. Caso contrário, o resultado será `false`.

O resultado de `x | y` será `true` se `x` ou `y` for `true`. Caso contrário, o resultado será `false`.

O resultado de `x ^ y` será `true` se `x` for `true` e `y` for `false`ou `x` for `false` e `y` for `true`. Caso contrário, o resultado será `false`. Quando os operandos são do tipo `bool`, o operador `^` computa o mesmo resultado que o operador de `!=`.

### <a name="nullable-boolean-logical-operators"></a>Operadores lógicos boolianos anuláveis

O tipo booliano anulável `bool?` pode representar três valores, `true`, `false`e `null`e é conceitualmente semelhante ao tipo de três valores usado para expressões booleanas no SQL. Para garantir que os resultados produzidos pelos operadores de `&` e `|` para os operandos `bool?` sejam consistentes com a lógica de três valores do SQL, os seguintes operadores predefinidos são fornecidos:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

A tabela a seguir lista os resultados produzidos por esses operadores para todas as combinações dos valores `true`, `false`e `null`.

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

Os operadores de `&&` e `||` são chamados de operadores lógicos condicionais. Eles também são chamados de operadores lógicos de "curto-circuito".

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

Os operadores `&&` e `||` são versões condicionais dos operadores `&` e `|`:

*  A operação `x && y` corresponde à operação `x & y`, exceto que `y` será avaliada somente se `x` não for `false`.
*  A operação `x || y` corresponde à operação `x | y`, exceto que `y` será avaliada somente se `x` não for `true`.

Se um operando de um operador lógico condicional tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.

Uma operação do formulário `x && y` ou `x || y` é processada pela aplicação da resolução de sobrecarga ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação fosse gravada `x & y` ou `x | y`. E, em seguida,

*  Se a resolução de sobrecarga não encontrar um único operador melhor, ou se a resolução de sobrecarga selecionar um dos operadores lógicos inteiros predefinidos, ocorrerá um erro de tempo de associação.
*  Caso contrário, se o operador selecionado for um dos operadores lógicos boolianos predefinidos ([operadores lógicos boolianos](expressions.md#boolean-logical-operators)) ou operadores lógicos boolianos anuláveis ([operadores lógicos boolianos anuláveis](expressions.md#nullable-boolean-logical-operators)), a operação será processada conforme descrito em [operadores lógicos condicionais boolianos](expressions.md#boolean-conditional-logical-operators).
*  Caso contrário, o operador selecionado é um operador definido pelo usuário e a operação é processada conforme descrito em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).

Não é possível sobrecarregar diretamente os operadores lógicos condicionais. No entanto, como os operadores lógicos condicionais são avaliados em termos de operadores lógicos regulares, as sobrecargas dos operadores lógicos regulares são, com certas restrições, também consideradas sobrecargas dos operadores lógicos condicionais. Isso é descrito mais detalhadamente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Operadores lógicos condicionais boolianos

Quando os operandos de `&&` ou `||` são do tipo `bool`, ou quando os operandos são de tipos que não definem um `operator &` ou `operator |`aplicável, mas definem conversões implícitas para `bool`, a operação é processada da seguinte maneira:

*  A operação `x && y` é avaliada como `x ? y : false`. Em outras palavras, `x` é avaliado pela primeira vez e convertido no tipo `bool`. Em seguida, se `x` for `true`, `y` será avaliado e convertido no tipo `bool`, e isso se tornará o resultado da operação. Caso contrário, o resultado da operação será `false`.
*  A operação `x || y` é avaliada como `x ? true : y`. Em outras palavras, `x` é avaliado pela primeira vez e convertido no tipo `bool`. Em seguida, se `x` for `true`, o resultado da operação será `true`. Caso contrário, `y` será avaliada e convertida no tipo `bool`, e isso se tornará o resultado da operação.

### <a name="user-defined-conditional-logical-operators"></a>Operadores lógicos condicionais definidos pelo usuário

Quando os operandos de `&&` ou `||` são de tipos que declaram um `operator &` ou `operator |`definido pelo usuário, os dois itens a seguir devem ser verdadeiros, em que `T` é o tipo no qual o operador selecionado é declarado:

*  O tipo de retorno e o tipo de cada parâmetro do operador selecionado devem ser `T`. Em outras palavras, o operador deve calcular o `AND` lógico ou o `OR` lógico de dois operandos do tipo `T`e deve retornar um resultado do tipo `T`.
*  `T` deve conter declarações de `operator true` e `operator false`.

Um erro de tempo de associação ocorre se um desses requisitos não for atendido. Caso contrário, a operação de `&&` ou `||` é avaliada pela combinação do `operator true` definido pelo usuário ou `operator false` com o operador selecionado definido pelo usuário:

*  A operação `x && y` é avaliada como `T.false(x) ? x : T.&(x, y)`, em que `T.false(x)` é uma invocação da `operator false` declarada em `T`e `T.&(x, y)` é uma invocação da `operator &`selecionada. Em outras palavras, `x` é avaliado pela primeira vez e `operator false` é invocado no resultado para determinar se `x` é definitivamente falso. Em seguida, se `x` for definitivamente false, o resultado da operação será o valor calculado anteriormente para `x`. Caso contrário, `y` é avaliado e o `operator &` selecionado é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.
*  A operação `x || y` é avaliada como `T.true(x) ? x : T.|(x, y)`, em que `T.true(x)` é uma invocação da `operator true` declarada em `T`e `T.|(x,y)` é uma invocação da `operator|`selecionada. Em outras palavras, `x` é avaliado pela primeira vez e `operator true` é invocado no resultado para determinar se `x` é definitivamente verdadeiro. Em seguida, se `x` for definitivamente true, o resultado da operação será o valor calculado anteriormente para `x`. Caso contrário, `y` é avaliado e o `operator |` selecionado é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.

Em qualquer uma dessas operações, a expressão fornecida pelo `x` é avaliada apenas uma vez e a expressão fornecida pelo `y` não é avaliada ou avaliada exatamente uma vez.

Para obter um exemplo de um tipo que implementa `operator true` e `operator false`, consulte [tipo booliano do banco de dados](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>O operador de União nulo

O operador `??` é chamado de operador de União nulo.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Uma expressão de União nula do formulário `a ?? b` requer que `a` seja de um tipo anulável ou tipo de referência. Se `a` não for NULL, o resultado de `a ?? b` será `a`; caso contrário, o resultado será `b`. A operação só avaliará `b` se `a` for nula.

O operador de União nulo é associativo à direita, o que significa que as operações são agrupadas da direita para a esquerda. Por exemplo, uma expressão do formulário `a ?? b ?? c` é avaliada como `a ?? (b ?? c)`. Em termos gerais, uma expressão do formulário `E1 ?? E2 ?? ... ?? En` retornará o primeiro dos operandos que não for nulo ou NULL se todos os operandos forem nulos.

O tipo da expressão `a ?? b` depende de quais conversões implícitas estão disponíveis nos operandos. Em ordem de preferência, o tipo de `a ?? b` é `A0`, `A`ou `B`, em que `A` é o tipo de `a` (desde que `a` tenha um tipo), `B` é o tipo de `b` (desde que `b` tenha um tipo) e `A0` seja o tipo subjacente de `A` se `A` for um tipo anulável ou `A` contrário. Especificamente, `a ?? b` é processada da seguinte maneira:

*  Se `A` existir e não for um tipo anulável ou um tipo de referência, ocorrerá um erro em tempo de compilação.
*  Se `b` for uma expressão dinâmica, o tipo de resultado será `dynamic`. Em tempo de execução, `a` é avaliada pela primeira vez. Se `a` não for NULL, `a` será convertido em dinâmico, e isso se tornará o resultado. Caso contrário, `b` será avaliado e isso se tornará o resultado.
*  Caso contrário, se `A` existir e for um tipo anulável e existir uma conversão implícita de `b` para `A0`, o tipo de resultado será `A0`. Em tempo de execução, `a` é avaliada pela primeira vez. Se `a` não for NULL, `a` será desencapsulado para o tipo `A0`, e isso se tornará o resultado. Caso contrário, `b` será avaliada e convertida no tipo `A0`, e isso se tornará o resultado.
*  Caso contrário, se `A` existir e uma conversão implícita existir de `b` para `A`, o tipo de resultado será `A`. Em tempo de execução, `a` é avaliada pela primeira vez. Se `a` não for NULL, `a` se tornará o resultado. Caso contrário, `b` será avaliada e convertida no tipo `A`, e isso se tornará o resultado.
*  Caso contrário, se `b` tiver um tipo `B` e uma conversão implícita existir de `a` para `B`, o tipo de resultado será `B`. Em tempo de execução, `a` é avaliada pela primeira vez. Se `a` não for NULL, `a` será desencapsulado para o tipo `A0` (se `A` existir e for anulável) e convertido para o tipo `B`, e isso se tornará o resultado. Caso contrário, `b` será avaliado e se tornará o resultado.
*  Caso contrário, `a` e `b` são incompatíveis e ocorre um erro em tempo de compilação.

## <a name="conditional-operator"></a>Operador condicional

O operador `?:` é chamado de operador condicional. Ele ocorre às vezes também chamado de operador ternário.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Uma expressão condicional do formulário `b ? x : y` primeiro avalia a condição `b`. Em seguida, se `b` for `true`, `x` será avaliado e se tornará o resultado da operação. Caso contrário, `y` será avaliado e se tornará o resultado da operação. Uma expressão condicional nunca avalia `x` e `y`.

O operador condicional é associativo à direita, o que significa que as operações são agrupadas da direita para a esquerda. Por exemplo, uma expressão do formulário `a ? b : c ? d : e` é avaliada como `a ? b : (c ? d : e)`.

O primeiro operando do operador de `?:` deve ser uma expressão que possa ser convertida implicitamente em `bool`ou uma expressão de um tipo que implemente `operator true`. Se nenhum desses requisitos for atendido, ocorrerá um erro em tempo de compilação.

O segundo e o terceiro operandos, `x` e `y`, do operador de `?:` controlam o tipo da expressão condicional.

*  Se `x` tiver tipo `X` e `y` tiver tipo `Y`
   * Se houver uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) de `X` para `Y`, mas não de `Y` para `X`, `Y` será o tipo da expressão condicional.
   * Se houver uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) de `Y` para `X`, mas não de `X` para `Y`, `X` será o tipo da expressão condicional.
   * Caso contrário, nenhum tipo de expressão pode ser determinado e ocorre um erro em tempo de compilação.
*  Se apenas um dos `x` e `y` tiver um tipo, e `x` e `y`forem conversíveis implicitamente para esse tipo, esse será o tipo da expressão condicional.
*  Caso contrário, nenhum tipo de expressão pode ser determinado e ocorre um erro em tempo de compilação.

O processamento em tempo de execução de uma expressão condicional do formulário `b ? x : y` consiste nas seguintes etapas:

*  Primeiro, `b` é avaliado e o valor de `bool` de `b` é determinado:
   * Se uma conversão implícita do tipo de `b` para `bool` existir, essa conversão implícita será executada para produzir um valor de `bool`.
   * Caso contrário, o `operator true` definido pelo tipo de `b` será invocado para produzir um valor de `bool`.
*  Se o valor de `bool` produzido pela etapa acima for `true`, `x` será avaliado e convertido para o tipo da expressão condicional, e isso se tornará o resultado da expressão condicional.
*  Caso contrário, `y` é avaliado e convertido para o tipo da expressão condicional, e isso se torna o resultado da expressão condicional.

## <a name="anonymous-function-expressions"></a>Expressões de função anônimas

Uma ***função anônima*** é uma expressão que representa uma definição de método "embutida". Uma função anônima não tem um valor ou tipo próprio, mas é conversível em um delegate compatível ou tipo de árvore de expressão. A avaliação de uma conversão de função anônima depende do tipo de destino da conversão: se for um tipo delegado, a conversão será avaliada como um valor delegado que referencia o método que a função anônima define. Se for um tipo de árvore de expressão, a conversão será avaliada como uma árvore de expressão que representa a estrutura do método como uma estrutura de objeto.

Por motivos históricos, há dois tipos sintáticos de funções anônimas, ou seja, *lambda_expression*s e *anonymous_method_expression*s. Para quase todas as finalidades, *lambda_expression*s são mais concisas e expressivas do que *anonymous_method_expression*s, que permanecem na linguagem para compatibilidade com versões anteriores.

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

O operador de `=>` tem a mesma precedência de atribuição (`=`) e é associativo à direita.

Uma função anônima com o modificador de `async` é uma função Async e segue as regras descritas em [iteradores](classes.md#iterators).

Os parâmetros de uma função anônima na forma de um *lambda_expression* podem ser digitados explícita ou implicitamente. Em uma lista de parâmetros explicitamente tipados, o tipo de cada parâmetro é explicitamente declarado. Em uma lista de parâmetros Tipados implicitamente, os tipos dos parâmetros são inferidos do contexto no qual a função anônima ocorre — especificamente, quando a função anônima é convertida em um tipo de delegado compatível ou tipo de árvore de expressão, esse tipo fornece os tipos de parâmetro ([conversões de função anônima](conversions.md#anonymous-function-conversions)).

Em uma função anônima com um único parâmetro tipado implicitamente, os parênteses podem ser omitidos da lista de parâmetros. Em outras palavras, uma função anônima do formulário
```csharp
( param ) => expr
```
pode ser abreviado como
```csharp
param => expr
```

A lista de parâmetros de uma função anônima na forma de uma *anonymous_method_expression* é opcional. Se for fornecido, os parâmetros deverão ser tipados explicitamente. Caso contrário, a função anônima é conversível em um delegado com qualquer lista de parâmetros que não contém `out` parâmetros.

Um corpo de *bloco* de uma função anônima pode ser acessado ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)), a menos que a função anônima ocorra dentro de uma instrução inacessível.

Veja a seguir alguns exemplos de funções anônimas:

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

O comportamento de *lambda_expression*s e *anonymous_method_expression*s é o mesmo, exceto os seguintes pontos:

*  *anonymous_method_expression*s permitem que a lista de parâmetros seja omitida inteiramente, produzindo convertibilidade para delegar tipos de qualquer lista de parâmetros de valor.
*  *lambda_expression*s permitem que os tipos de parâmetro sejam omitidos e inferidos, enquanto *anonymous_method_expression*s exigem que os tipos de parâmetros sejam declarados explicitamente.
*  O corpo de uma *lambda_expression* pode ser uma expressão ou um bloco de instrução, enquanto o corpo de uma *anonymous_method_expression* deve ser um bloco de instruções.
*  Somente *lambda_expression*s têm conversões para tipos de árvore de expressão compatíveis ([tipos de árvore de expressão](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Assinaturas de funções anônimas

O *anonymous_function_signature* opcional de uma função anônima define os nomes e, opcionalmente, os tipos dos parâmetros formais para a função anônima. O escopo dos parâmetros da função anônima é o *anonymous_function_body*. ([Escopos](basic-concepts.md#scopes)) Junto com a lista de parâmetros (se fornecido), o corpo do método anônimo constitui um espaço de declaração ([declarações](basic-concepts.md#declarations)). Portanto, é um erro de tempo de compilação para o nome de um parâmetro da função anônima corresponder ao nome de uma variável local, constante local ou parâmetro cujo escopo inclui a *anonymous_method_expression* ou *lambda_expression*.

Se uma função anônima tiver um *explicit_anonymous_function_signature*, o conjunto de tipos de delegados compatíveis e tipos de árvore de expressão será restrito àqueles que têm os mesmos tipos de parâmetro e modificadores na mesma ordem. Em contraste com conversões de grupos de métodos ([conversões de grupos de métodos](conversions.md#method-group-conversions)), não há suporte para a variância de contrato de tipos de parâmetro de função anônima. Se uma função anônima não tiver um *anonymous_function_signature*, o conjunto de tipos de delegados compatíveis e tipos de árvore de expressão será restrito àqueles que não têm parâmetros de `out`.

Observe que um *anonymous_function_signature* não pode incluir atributos ou uma matriz de parâmetros. No entanto, um *anonymous_function_signature* pode ser compatível com um tipo delegado cuja lista de parâmetros contenha uma matriz de parâmetros.

Observe também que a conversão para um tipo de árvore de expressão, mesmo se compatível, ainda pode falhar em tempo de compilação ([tipos de árvore de expressão](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Corpos de funções anônimas

O corpo (*expressão* ou *bloco*) de uma função anônima está sujeito às seguintes regras:

*  Se a função anônima incluir uma assinatura, os parâmetros especificados na assinatura estarão disponíveis no corpo. Se a função anônima não tiver nenhuma assinatura, ela poderá ser convertida em um tipo de representante ou tipo de expressão com parâmetros ([conversões de função anônimas](conversions.md#anonymous-function-conversions)), mas os parâmetros não poderão ser acessados no corpo.
*  Exceto para `ref` ou `out` parâmetros especificados na assinatura (se houver) da função anônima delimitadora mais próxima, trata-se de um erro de tempo de compilação para o corpo acessar um parâmetro `ref` ou `out`.
*  Quando o tipo de `this` é um tipo struct, é um erro de tempo de compilação para o corpo acessar `this`. Isso é verdadeiro se o acesso for explícito (como no `this.x`) ou implícito (como em `x` em que `x` é um membro de instância da struct). Essa regra simplesmente proíbe esse acesso e não afeta se a pesquisa de membros resulta em um membro da estrutura.
*  O corpo tem acesso às variáveis externas ([variáveis externas](expressions.md#outer-variables)) da função anônima. O acesso de uma variável externa fará referência à instância da variável que está ativa no momento em que o *lambda_expression* ou *anonymous_method_expression* é avaliado ([avaliação de expressões de função anônimas](expressions.md#evaluation-of-anonymous-function-expressions)).
*  É um erro de tempo de compilação para o corpo conter uma instrução `goto`, `break` instrução ou `continue` instrução cujo destino está fora do corpo ou dentro do corpo de uma função anônima contida.
*  Uma instrução `return` no corpo retorna o controle de uma invocação da função anônima delimitadora mais próxima, não do membro da função delimitadora. Uma expressão especificada em uma instrução `return` deve ser conversível implicitamente para o tipo de retorno do tipo delegado ou tipo de árvore de expressão para o qual o *lambda_expression* ou *anonymous_method_expression* de fechamento mais próximo é convertido ([conversões de função anônimas](conversions.md#anonymous-function-conversions)).

Ele é explicitamente não especificado se há qualquer maneira de executar o bloco de uma função anônima além da avaliação e invocação do *lambda_expression* ou *anonymous_method_expression*. Em particular, o compilador pode optar por implementar uma função anônima sintetizando um ou mais métodos ou tipos nomeados. Os nomes de quaisquer elementos sintetizados devem ser de um formulário reservado para uso do compilador.

### <a name="overload-resolution-and-anonymous-functions"></a>Resolução de sobrecarga e funções anônimas

As funções anônimas em uma lista de argumentos participam da inferência de tipos e da resolução de sobrecarga. Veja a [inferência de tipos](expressions.md#type-inference) e a [resolução de sobrecarga](expressions.md#overload-resolution) para as regras exatas.

O exemplo a seguir ilustra o efeito de funções anônimas na resolução de sobrecarga.

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

A classe `ItemList<T>` tem dois métodos `Sum`. Cada um recebe um argumento `selector`, que extrai o valor para somar de um item de lista. O valor extraído pode ser um `int` ou um `double` e a soma resultante é, da mesma forma, uma `int` ou uma `double`.

Os métodos de `Sum` poderiam, por exemplo, ser usados para computar somas de uma lista de linhas de detalhes em uma ordem.

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

Na primeira invocação de `orderDetails.Sum`, ambos os métodos de `Sum` são aplicáveis porque a função anônima `d => d. UnitCount` é compatível com `Func<Detail,int>` e `Func<Detail,double>`. No entanto, a resolução de sobrecarga escolhe o primeiro método `Sum` porque a conversão em `Func<Detail,int>` é melhor do que a conversão para `Func<Detail,double>`.

Na segunda invocação de `orderDetails.Sum`, somente o segundo método de `Sum` é aplicável porque a função anônima `d => d.UnitPrice * d.UnitCount` produz um valor do tipo `double`. Portanto, a resolução de sobrecarga escolhe o segundo método de `Sum` para essa invocação.

### <a name="anonymous-functions-and-dynamic-binding"></a>Funções anônimas e associação dinâmica

Uma função anônima não pode ser um receptor, argumento ou operando de uma operação vinculada dinamicamente.

### <a name="outer-variables"></a>Variáveis externas

Qualquer variável local, parâmetro de valor ou matriz de parâmetros cujo escopo inclui o *lambda_expression* ou *anonymous_method_expression* é chamado de uma ***variável externa*** da função anônima. Em um membro de função de instância de uma classe, o valor de `this` é considerado um parâmetro de valor e é uma variável externa de qualquer função anônima contida no membro da função.

#### <a name="captured-outer-variables"></a>Variáveis externas capturadas

Quando uma variável externa é referenciada por uma função anônima, diz-se que a variável externa foi ***capturada*** pela função anônima. Normalmente, o tempo de vida de uma variável local é limitado à execução do bloco ou da instrução com a qual está associado ([variáveis locais](variables.md#local-variables)). No entanto, o tempo de vida de uma variável externa capturada é estendido pelo menos até que a árvore de expressão ou delegado criada na função anônima se torne elegível para a coleta de lixo.

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
a variável local `x` é capturada pela função anônima, e o tempo de vida de `x` é estendido pelo menos até que o delegado retornado de `F` se torne qualificado para coleta de lixo (o que não ocorre até o final do programa). Como cada invocação da função anônima opera na mesma instância do `x`, a saída do exemplo é:
```console
1
2
3
```

Quando uma variável local ou um parâmetro de valor é capturado por uma função anônima, a variável local ou o parâmetro não é mais considerado uma variável fixa ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)), mas, em vez disso, é considerado uma variável móvel. Portanto, qualquer código `unsafe` que pega o endereço de uma variável externa capturada deve primeiro usar a instrução `fixed` para corrigir a variável.

Observe que, diferentemente de uma variável não capturada, uma variável local capturada pode ser exposta simultaneamente a vários threads de execução.

#### <a name="instantiation-of-local-variables"></a>Instanciação de variáveis locais

Uma variável local é considerada como ***instanciada*** quando a execução entra no escopo da variável. Por exemplo, quando o método a seguir é invocado, a variável local `x` é instanciada e inicializada três vezes — uma vez para cada iteração do loop.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

No entanto, mover a declaração de `x` fora do loop resulta em uma única instanciação de `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Quando não é capturado, não há como observar exatamente com que frequência uma variável local é instanciada — como os tempos de vida das instâncias são não contíguos, é possível que cada instanciação simplesmente use o mesmo local de armazenamento. No entanto, quando uma função anônima captura uma variável local, os efeitos da instanciação se tornam aparentes.

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
```console
1
3
5
```

No entanto, quando a declaração de `x` é movida para fora do loop:
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
a saída será:
```console
5
5
5
```

Se um loop for declarar uma variável de iteração, essa variável será considerada como sendo declarada fora do loop. Portanto, se o exemplo for alterado para capturar a própria variável de iteração:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
somente uma instância da variável de iteração é capturada, o que produz a saída:
```console
3
3
3
```

É possível que os delegados de função anônima compartilhem algumas variáveis capturadas, embora tenham instâncias separadas de outros. Por exemplo, se `F` for alterado para
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
os três delegados capturam a mesma instância de `x`, mas instâncias separadas de `y`, e a saída é:
```console
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
as duas funções anônimas capturam a mesma instância da variável local `x`e, portanto, podem "se comunicar" por meio dessa variável. A saída do exemplo é:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Avaliação de expressões de função anônimas

Uma função anônima `F` sempre deve ser convertida em um tipo delegado `D` ou um tipo de árvore de expressão `E`, diretamente ou por meio da execução de uma expressão de criação de delegado `new D(F)`. Essa conversão determina o resultado da função anônima, conforme descrito em [conversões de função anônimas](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Expressões de consulta

As ***expressões de consulta*** fornecem uma sintaxe de linguagem integrada para consultas que são semelhantes a linguagens de consulta relacionais e hierárquicas, como SQL e XQuery.

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

Uma expressão de consulta começa com uma cláusula `from` e termina com uma cláusula `select` ou `group`. A cláusula de `from` inicial pode ser seguida por zero ou mais `from`, `let`, `where`, `join` ou `orderby` cláusulas. Cada cláusula de `from` é um gerador que introduz uma ***variável de intervalo*** que varia sobre os elementos de uma ***sequência***. Cada cláusula de `let` introduz uma variável de intervalo que representa um valor calculado por meio de variáveis de intervalo anteriores. Cada cláusula de `where` é um filtro que exclui itens do resultado. Cada cláusula `join` compara as chaves especificadas da sequência de origem com chaves de outra sequência, gerando pares correspondentes. Cada cláusula de `orderby` reordena os itens de acordo com os critérios especificados. A cláusula final `select` ou `group` especifica a forma do resultado em termos das variáveis de intervalo. Por fim, uma cláusula `into` pode ser usada para "unir" consultas tratando os resultados de uma consulta como um gerador em uma consulta subsequente.

### <a name="ambiguities-in-query-expressions"></a>Ambiguidades em expressões de consulta

As expressões de consulta contêm um número de "palavras-chave contextuais", ou seja, identificadores que têm significado especial em um determinado contexto. Especificamente, esses são `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` e `by`. Para evitar ambigüidades em expressões de consulta causadas pelo uso misto desses identificadores como palavras-chave ou nomes simples, esses identificadores são considerados palavras-chave quando ocorrem em qualquer lugar dentro de uma expressão de consulta.

Para essa finalidade, uma expressão de consulta é qualquer expressão que comece com "`from identifier`" seguida por qualquer token, exceto "`;`", "`=`" ou "`,`".

Para usar essas palavras como identificadores dentro de uma expressão de consulta, elas podem ser prefixadas com "`@`" ([identificadores](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Conversão de expressão de consulta

O C# idioma não especifica a semântica de execução das expressões de consulta. Em vez disso, as expressões de consulta são convertidas em invocações de métodos que aderem ao *padrão de expressão de consulta* ([o padrão de expressão de consulta](expressions.md#the-query-expression-pattern)). Especificamente, as expressões de consulta são convertidas em invocações de métodos chamados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`e `Cast`. Esses métodos devem ter assinaturas e tipos de resultados específicos, conforme descrito no [padrão de expressão de consulta](expressions.md#the-query-expression-pattern). Esses métodos podem ser métodos de instância do objeto que está sendo consultado ou métodos de extensão que são externos ao objeto e implementam a execução real da consulta.

A conversão de expressões de consulta para invocações de método é um mapeamento sintático que ocorre antes que qualquer associação de tipo ou resolução de sobrecarga tenha sido executada. A conversão tem a garantia de estar sintaticamente correta, mas não há garantia de que ele produza código C# semanticamente correto. Após a tradução das expressões de consulta, as invocações de método resultantes são processadas como invocações de método regular, e isso pode, por sua vez, revelar erros, por exemplo, se os métodos não existirem, se os argumentos tiverem tipos incorretos ou se os métodos forem genéricos e falha na inferência de tipos.

Uma expressão de consulta é processada pela aplicação repetida das seguintes traduções até que nenhuma redução adicional seja possível. As traduções são listadas em ordem de aplicativo: cada seção pressupõe que as traduções nas seções anteriores foram executadas exaustivamente e, uma vez esgotadas, uma seção não será revisitada posteriormente no processamento da mesma expressão de consulta.

A atribuição de variáveis de intervalo não é permitida em expressões de consulta. No entanto, uma C# implementação é permitida nem sempre impor essa restrição, uma vez que isso pode, às vezes, não ser possível com o esquema de tradução sintático apresentado aqui.

Determinadas traduções injetam variáveis de intervalo com identificadores transparentes denotados por `*`. As propriedades especiais de identificadores transparentes são discutidas mais detalhadamente em [identificadores transparentes](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Cláusulas Select e GroupBy com continuaçãos

Uma expressão de consulta com uma continuação
```csharp
from ... into x ...
```
é convertido em
```csharp
from x in ( from ... ) ...
```

As traduções nas seções a seguir pressupõem que as consultas não têm `into` de continuação.

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
a tradução final do que é
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Tipos de variável de intervalo explícito

Uma cláusula `from` que especifica explicitamente um tipo de variável de intervalo
```csharp
from T x in e
```
é convertido em
```csharp
from x in ( e ) . Cast < T > ( )
```

Uma cláusula `join` que especifica explicitamente um tipo de variável de intervalo
```csharp
join T x in e on k1 equals k2
```
é convertido em
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

As traduções nas seções a seguir pressupõem que as consultas não têm nenhum tipo de variável de intervalo explícito.

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
a tradução final do que é
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Os tipos de variável de intervalo explícitos são úteis para consultar coleções que implementam a interface de `IEnumerable` não genérica, mas não a interface genérica `IEnumerable<T>`. No exemplo acima, esse seria o caso se `customers` fosse do tipo `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Degerar expressões de consulta

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

Uma expressão de consulta de degeneração é aquela que seleciona de trivial os elementos da origem. Uma fase posterior da tradução remove as consultas de degeneração introduzidas por outras etapas de conversão, substituindo-as por sua fonte. No entanto, é importante garantir que o resultado de uma expressão de consulta nunca seja o próprio objeto de origem, pois isso revelaria o tipo e a identidade da origem para o cliente da consulta. Portanto, essa etapa protege as consultas de degeneração escritas diretamente no código-fonte chamando explicitamente `Select` na fonte. Em seguida, é até os implementadores de `Select` e outros operadores de consulta para garantir que esses métodos nunca retornem o próprio objeto de origem.

#### <a name="from-let-where-join-and-orderby-clauses"></a>De, Let, Where, cláusulas Join e OrderBy

Uma expressão de consulta com uma segunda cláusula `from` seguida por uma cláusula `select`
```csharp
from x1 in e1
from x2 in e2
select v
```
é convertido em
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Uma expressão de consulta com uma segunda cláusula `from` seguida por algo diferente de uma cláusula `select`:

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

Uma expressão de consulta com uma cláusula `let`
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

Uma expressão de consulta com uma cláusula `where`
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

Uma expressão de consulta com uma cláusula `join` sem uma `into` seguida por uma cláusula `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
é convertido em
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Uma expressão de consulta com uma cláusula `join` sem uma `into` seguida por algo diferente de uma cláusula `select`
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

Uma expressão de consulta com uma cláusula `join` com uma `into` seguida por uma cláusula `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
é convertido em
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Uma expressão de consulta com uma cláusula `join` com uma `into` seguida por algo diferente de uma cláusula `select`
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

Uma expressão de consulta com uma cláusula `orderby`
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

Se uma cláusula de ordenação especificar um indicador de direção de `descending`, uma invocação de `OrderByDescending` ou `ThenByDescending` será produzida em vez disso.

As traduções a seguir pressupõem que não há cláusulas `let`, `where`, `join` ou `orderby` e não mais do que a cláusula de `from` inicial em cada expressão de consulta.

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
a tradução final do que é
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
onde `x` é um identificador gerado pelo compilador que, de outra forma, é invisível e inacessível.

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
a tradução final do que é
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
onde `x` é um identificador gerado pelo compilador que, de outra forma, é invisível e inacessível.

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
a tradução final do que é
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
onde `x` e `y` são identificadores gerados pelo compilador que, de outra forma, são invisíveis e inacessíveis.

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

#### <a name="select-clauses"></a>Cláusulas de seleção

Uma expressão de consulta do formulário
```csharp
from x in e select v
```
é convertido em
```csharp
( e ) . Select ( x => v )
```
Exceto quando v é o identificador x, a tradução é simplesmente
```csharp
( e )
```

Por exemplo
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
é simplesmente traduzido em
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Cláusulas GroupBy

Uma expressão de consulta do formulário
```csharp
from x in e group v by k
```
é convertido em
```csharp
( e ) . GroupBy ( x => k , x => v )
```
Exceto quando v é o identificador x, a conversão é
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

Determinadas traduções injetam variáveis de intervalo com ***identificadores transparentes*** denotados por `*`. Os identificadores transparentes não são um recurso de idioma adequado; elas existem somente como uma etapa intermediária no processo de conversão de expressão de consulta.

Quando uma conversão de consulta injeta um identificador transparente, as etapas de tradução adicionais propagam o identificador transparente para funções anônimas e inicializadores de objeto anônimos. Nesses contextos, os identificadores transparentes têm o seguinte comportamento:

*  Quando um identificador transparente ocorre como um parâmetro em uma função anônima, os membros do tipo anônimo associado são automaticamente no escopo no corpo da função anônima.
*  Quando um membro com um identificador transparente está no escopo, os membros desse membro também estão no escopo.
*  Quando um identificador transparente ocorre como um Declarador de membro em um inicializador de objeto anônimo, ele introduz um membro com um identificador transparente.
*  Nas etapas de conversão descritas acima, os identificadores transparentes são sempre introduzidos junto com tipos anônimos, com a intenção de capturar várias variáveis de intervalo como membros de um único objeto. Uma implementação do C# tem permissão para usar um mecanismo diferente de tipos anônimos para agrupar várias variáveis de intervalo. Os exemplos de conversão a seguir pressupõem que os tipos anônimos são usados e mostram como os identificadores transparentes podem ser traduzidos fora.

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

que é ainda mais convertida em
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
que, quando os identificadores transparentes são apagados, é equivalente a
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
onde `x` é um identificador gerado pelo compilador que, de outra forma, é invisível e inacessível.

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
que é ainda mais reduzido para
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
a tradução final do que é
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
onde `x`, `y`e `z` são identificadores gerados pelo compilador que, de outra forma, são invisíveis e inacessíveis.

### <a name="the-query-expression-pattern"></a>O padrão de expressão de consulta

O ***padrão de expressão de consulta*** estabelece um padrão de métodos que os tipos podem implementar para dar suporte a expressões de consulta. Como as expressões de consulta são convertidas em invocações de método por meio de um mapeamento sintático, os tipos têm flexibilidade considerável em como implementam o padrão de expressão de consulta. Por exemplo, os métodos do padrão podem ser implementados como métodos de instância ou como métodos de extensão porque os dois têm a mesma sintaxe de invocação, e os métodos podem solicitar delegados ou árvores de expressão porque as funções anônimas são conversíveis para ambos.

A forma recomendada de um `C<T>` de tipo genérico que dá suporte ao padrão de expressão de consulta é mostrada abaixo. Um tipo genérico é usado para ilustrar as relações apropriadas entre os tipos de parâmetro e de resultado, mas é possível implementar o padrão para tipos não genéricos também.

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

Os métodos acima usam os tipos de delegado genéricos `Func<T1,R>` e `Func<T1,T2,R>`, mas eles poderiam ter usado igualmente outros tipos de árvore de expressão ou de representante com as mesmas relações nos tipos de parâmetro e resultado.

Observe a relação recomendada entre `C<T>` e `O<T>` que garante que os métodos `ThenBy` e `ThenByDescending` estejam disponíveis somente no resultado de um `OrderBy` ou `OrderByDescending`. Observe também a forma recomendada do resultado de `GroupBy` – uma sequência de sequências, em que cada sequência interna tem uma propriedade `Key` adicional.

O namespace `System.Linq` fornece uma implementação do padrão de operador de consulta para qualquer tipo que implemente a interface `System.Collections.Generic.IEnumerable<T>`.

## <a name="assignment-operators"></a>Operadores de atribuição

Os operadores de atribuição atribuem um novo valor a uma variável, uma propriedade, um evento ou um elemento do indexador.

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

O operador `=` é chamado de ***operador de atribuição simples***. Ele atribui o valor do operando direito à variável, à propriedade ou ao elemento do indexador fornecido pelo operando esquerdo. O operando esquerdo do operador de atribuição simples não pode ser um acesso de evento (exceto conforme descrito em [eventos de campo](classes.md#field-like-events)). O operador de atribuição simples é descrito em [atribuição simples](expressions.md#simple-assignment).

Os operadores de atribuição diferentes do operador de `=` são chamados de ***operadores de atribuição compostos***. Esses operadores executam a operação indicada nos dois operandos e, em seguida, atribuim o valor resultante à variável, à propriedade ou ao elemento do indexador fornecido pelo operando esquerdo. Os operadores de atribuição compostos são descritos em [atribuição composta](expressions.md#compound-assignment).

Os operadores de `+=` e `-=` com uma expressão de acesso de evento como o operando à esquerda são chamados de *operadores de atribuição de eventos*. Nenhum outro operador de atribuição é válido com um acesso de evento como o operando esquerdo. Os operadores de atribuição de eventos são descritos em [atribuição de eventos](expressions.md#event-assignment).

Os operadores de atribuição são associativos à direita, o que significa que as operações são agrupadas da direita para a esquerda. Por exemplo, uma expressão do formulário `a = b = c` é avaliada como `a = (b = c)`.

### <a name="simple-assignment"></a>Atribuição simples

O operador `=` é chamado de operador de atribuição simples.

Se o operando esquerdo de uma atribuição simples estiver no formato `E.P` ou `E[Ei]` em que `E` tem o tipo de tempo de compilação `dynamic`, a atribuição será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão de atribuição é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução com base no tipo de tempo de execução de `E`.

Em uma atribuição simples, o operando à direita deve ser uma expressão que seja conversível implicitamente no tipo do operando esquerdo. A operação atribui o valor do operando direito à variável, à propriedade ou ao elemento do indexador fornecido pelo operando esquerdo.

O resultado de uma expressão de atribuição simples é o valor atribuído ao operando à esquerda. O resultado tem o mesmo tipo que o operando esquerdo e sempre é classificado como um valor.

Se o operando esquerdo for um acesso de propriedade ou indexador, a propriedade ou o indexador deverá ter um acessador `set`. Se esse não for o caso, ocorrerá um erro de tempo de associação.

O processamento em tempo de execução de uma atribuição simples do formulário `x = y` consiste nas seguintes etapas:

*  Se `x` for classificado como uma variável:
   * `x` é avaliado para produzir a variável.
   * `y` é avaliado e, se necessário, convertido para o tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).
   * Se a variável fornecida por `x` for um elemento de matriz de um *reference_type*, uma verificação de tempo de execução será executada para garantir que o valor computado para `y` seja compatível com a instância de matriz da qual `x` é um elemento. A verificação terá sucesso se `y` estiver `null`, ou se uma conversão de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existir do tipo real da instância referenciada por `y` para o tipo de elemento real da instância de matriz que contém `x`. Caso contrário, uma `System.ArrayTypeMismatchException` será gerada.
   * O valor resultante da avaliação e da conversão de `y` é armazenado no local fornecido pela avaliação de `x`.
*  Se `x` for classificado como um acesso de propriedade ou indexador:
   * A expressão de instância (se `x` não for `static`) e a lista de argumentos (se `x` for um acesso de indexador) associada a `x` são avaliadas e os resultados são usados na invocação de acessador de `set` subsequente.
   * `y` é avaliado e, se necessário, convertido para o tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).
   * O acessador de `set` de `x` é invocado com o valor calculado para `y` como seu argumento de `value`.

As regras de covariância de matriz ([covariância de matriz](arrays.md#array-covariance)) permitem que um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que exista uma conversão de referência implícita de `B` para `A`. Devido a essas regras, a atribuição a um elemento de matriz de um *reference_type* requer uma verificação de tempo de execução para garantir que o valor que está sendo atribuído seja compatível com a instância de matriz. No exemplo
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
a última atribuição faz com que uma `System.ArrayTypeMismatchException` seja gerada porque uma instância de `ArrayList` não pode ser armazenada em um elemento de uma `string[]`.

Quando uma propriedade ou um indexador declarado em um *struct_type* é o destino de uma atribuição, a expressão de instância associada à propriedade ou ao acesso do indexador deve ser classificada como uma variável. Se a expressão de instância for classificada como um valor, ocorrerá um erro de tempo de associação. Devido ao [acesso de membro](expressions.md#member-access), a mesma regra também se aplica a campos.

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
no exemplo
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
as atribuições para `p.X`, `p.Y`, `r.A`e `r.B` são permitidas porque `p` e `r` são variáveis. No entanto, no exemplo
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
as atribuições são todas inválidas, pois `r.A` e `r.B` não são variáveis.

### <a name="compound-assignment"></a>Atribuição composta

Se o operando esquerdo de uma atribuição composta estiver no formato `E.P` ou `E[Ei]` em que `E` tem o tipo de tempo de compilação `dynamic`, a atribuição será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)). Nesse caso, o tipo de tempo de compilação da expressão de atribuição é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução com base no tipo de tempo de execução de `E`.

Uma operação do formulário `x op= y` é processada pela aplicação da resolução de sobrecarga do operador binário ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação fosse gravada `x op y`. E, em seguida,

*  Se o tipo de retorno do operador selecionado for implicitamente conversível para o tipo de `x`, a operação será avaliada como `x = x op y`, exceto que `x` é avaliada apenas uma vez.
*  Caso contrário, se o operador selecionado for um operador predefinido, se o tipo de retorno do operador selecionado for explicitamente conversível para o tipo de `x`, e se `y` for implicitamente conversível no tipo de `x` ou se o operador for um operador de deslocamento, a operação será avaliada como `x = (T)(x op y)`, em que `T` é o tipo de `x`, exceto que `x` é avaliado apenas uma vez.
*  Caso contrário, a atribuição composta é inválida e ocorre um erro de tempo de ligação.

O termo "avaliado apenas uma vez" significa que, na avaliação de `x op y`, os resultados de quaisquer expressões constituintes de `x` são salvos temporariamente e, em seguida, reutilizados ao executar a atribuição para `x`. Por exemplo, no `A()[B()] += C()`de atribuição, em que `A` é um método que retorna `int[]`e `B` e `C` são métodos que retornam `int`, os métodos são invocados apenas uma vez, na ordem `A`, `B`, `C`.

Quando o operando esquerdo de uma atribuição composta é um acesso de propriedade ou indexador, a propriedade ou o indexador deve ter um acessador `get` e um acessador `set`. Se esse não for o caso, ocorrerá um erro de tempo de associação.

A segunda regra acima permite que `x op= y` seja avaliada como `x = (T)(x op y)` em determinados contextos. A regra existe de modo que os operadores predefinidos possam ser usados como operadores compostos quando o operando esquerdo for do tipo `sbyte`, `byte`, `short`, `ushort`ou `char`. Mesmo quando ambos os argumentos são de um desses tipos, os operadores predefinidos produzem um resultado do tipo `int`, conforme descrito em [promoções numéricas binárias](expressions.md#binary-numeric-promotions). Portanto, sem uma conversão, não seria possível atribuir o resultado ao operando esquerdo.

O efeito intuitivo da regra para operadores predefinidos é simplesmente que `x op= y` é permitido se ambos os `x op y` e `x = y` são permitidos. No exemplo
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
o motivo intuitivo de cada erro é que uma atribuição simples correspondente também teria sido um erro.

Isso também significa que as operações de atribuição compostas dão suporte a operações levantadas. No exemplo
```csharp
int? i = 0;
i += 1;             // Ok
```
o operador levantado `+(int?,int?)` é usado.

### <a name="event-assignment"></a>Atribuição de evento

Se o operando esquerdo de um operador de `+=` ou `-=` for classificado como um acesso de evento, a expressão será avaliada da seguinte maneira:

*  A expressão de instância, se houver, do acesso ao evento é avaliada.
*  O operando à direita do operador `+=` ou `-=` é avaliado e, se necessário, é convertido para o tipo do operando esquerdo por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).
*  Um acessador de eventos do evento é invocado, com a lista de argumentos que consiste no operando direito, após a avaliação e, se necessário, a conversão. Se o operador foi `+=`, o acessador de `add` é invocado; Se o operador tiver sido `-=`, o acessador de `remove` será invocado.

Uma expressão de atribuição de evento não produz um valor. Assim, uma expressão de atribuição de evento é válida somente no contexto de uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)).

## <a name="expression"></a>Expressão

Uma *expressão* é uma *non_assignment_expression* ou uma *atribuição*.

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

Uma *constant_expression* é uma expressão que pode ser totalmente avaliada em tempo de compilação.

```antlr
constant_expression
    : expression
    ;
```

Uma expressão constante deve ser o `null` literal ou um valor com um dos seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`ou qualquer tipo de enumeração. Somente as seguintes construções são permitidas em expressões constantes:

*  Literais (incluindo o `null` literal).
*  Referências a `const` membros de tipos de classe e struct.
*  Referências a membros de tipos de enumeração.
*  Referências a parâmetros de `const` ou variáveis locais
*  Subexpressão entre parênteses, que são expressões constantes.
*  Expressões de conversão, desde que o tipo de destino seja um dos tipos listados acima.
*  expressões de `checked` e `unchecked`
*  Expressões de valor padrão
*  Expressões nameof
*  Os operadores `+`, `-`, `!`e `~` unários predefinidos.
*  Os operadores predefinidos `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`e `>=` binários, desde que cada operando seja de um tipo listado acima.
*  O operador condicional `?:`.

As conversões a seguir são permitidas em expressões constantes:

*  Conversões de identidade
*  Conversões numéricas
*  Conversões de enumeração
*  Conversões de expressão de constante
*  Conversões de referência implícitas e explícitas, desde que a origem das conversões seja uma expressão constante que é avaliada como o valor NULL.

Outras conversões incluindo boxing, unboxing e conversões de referência implícita de valores não nulos não são permitidas em expressões constantes. Por exemplo:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
a inicialização de i é um erro porque uma conversão boxing é necessária. A inicialização de str é um erro porque uma conversão de referência implícita de um valor não nulo é necessária.

Sempre que uma expressão atende aos requisitos listados acima, a expressão é avaliada em tempo de compilação. Isso é verdadeiro mesmo que a expressão seja uma subexpressão de uma expressão maior que contenha construções não constantes.

A avaliação de tempo de compilação de expressões constantes usa as mesmas regras que a avaliação de tempo de execução de expressões não constantes, exceto que, quando a avaliação de tempo de execução tiver gerado uma exceção, a avaliação de tempo de compilação causará a ocorrência de um erro de tempo de compilação.

A menos que uma expressão constante seja colocada explicitamente em um contexto de `unchecked`, os estouros que ocorrem em operações aritméticas de tipo integral e conversões durante a avaliação de tempo de compilação da expressão sempre causam erros de tempo de compilação ([expressões constantes](expressions.md#constant-expressions)).

Expressões constantes ocorrem nos contextos listados abaixo. Nesses contextos, ocorrerá um erro de tempo de compilação se uma expressão não puder ser totalmente avaliada em tempo de compilação.

*  Declarações de constantes ([constantes](classes.md#constants)).
*  Declarações de membro de enumeração ([membros enum](enums.md#enum-members)).
*  Argumentos padrão de listas de parâmetros formais ([parâmetros de método](classes.md#method-parameters))
*  `case` rótulos de uma instrução `switch` ([a instrução switch](statements.md#the-switch-statement)).
*  instruções de `goto case` ([a instrução goto](statements.md#the-goto-statement)).
*  Comprimentos de dimensão em uma expressão de criação de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)) que inclui um inicializador.
*  Atributos ([atributos](attributes.md)).

Uma conversão de expressão constante implícita ([conversões de expressão de constante implícita](conversions.md#implicit-constant-expression-conversions)) permite que uma expressão constante do tipo `int` seja convertida em `sbyte`, `byte`, `short`, `ushort`, `uint`ou `ulong`, desde que o valor da expressão constante esteja dentro do intervalo do tipo de destino.

## <a name="boolean-expressions"></a>expressões boolianas

Uma *Boolean_expression* é uma expressão que produz um resultado do tipo `bool`; diretamente ou por meio da aplicação de `operator true` em determinados contextos, conforme especificado no seguinte.

```antlr
boolean_expression
    : expression
    ;
```

A expressão condicional de controle de *um if_statement* ([a instrução if](statements.md#the-if-statement)), *while_statement* ([a instrução while](statements.md#the-while-statement)) *, do_statement* ([a instrução](statements.md#the-do-statement)do) ou *for_statement* ([a instrução for](statements.md#the-for-statement)) é uma *Boolean_expression*. A expressão condicional de controle do operador de `?:` ([operador condicional](expressions.md#conditional-operator)) segue as mesmas regras que uma *Boolean_expression*, mas, por motivos de precedência de operador, é classificada como uma *conditional_or_expression*.

Um `E` de *Boolean_expression* é necessário para poder produzir um valor do tipo `bool`, da seguinte maneira:

*  Se `E` for implicitamente conversível para `bool`, em tempo de execução, a conversão implícita será aplicada.
*  Caso contrário, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é usada para encontrar uma melhor implementação exclusiva do operador `true` em `E`, e essa implementação é aplicada em tempo de execução.
*  Se esse operador não for encontrado, ocorrerá um erro de tempo de associação.

O tipo de struct `DBBool` no [tipo de banco de dados booliano](structs.md#database-boolean-type) fornece um exemplo de um tipo que implementa `operator true` e `operator false`.
