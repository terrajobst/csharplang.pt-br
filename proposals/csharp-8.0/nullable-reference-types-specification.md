---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485206"
---
# <a name="nullable-reference-types-specification"></a>Especificação de tipos de referência anulável

Este é um trabalho em andamento-várias partes estão ausentes ou incompletas. ***

## <a name="syntax"></a>Sintaxe

### <a name="nullable-reference-types"></a>Tipos de referência anuláveis

Os tipos de referência anuláveis têm a mesma sintaxe `T?` que a forma abreviada de tipos de valores anuláveis, mas não têm um formato longo correspondente.

Para os fins da especificação, a produção de `nullable_type` atual é renomeada para `nullable_value_type`e uma `nullable_reference_type` produção é adicionada:

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

O `non_nullable_reference_type` em uma `nullable_reference_type` deve ser um tipo de referência não anulável (classe, interface, delegado ou matriz) ou um parâmetro de tipo restrito a ser um tipo de referência não anulável (por meio da restrição de `class` ou uma classe diferente de `object`).

Tipos de referência anuláveis não podem ocorrer nas seguintes posições:

- como uma classe base ou interface
- como receptor de um `member_access`
- como o `type` em um `object_creation_expression`
- como o `delegate_type` em um `delegate_creation_expression`
- como o `type` em um `is_expression`, um `catch_clause` ou um `type_pattern`
- como o `interface` em um nome de membro de interface totalmente qualificado

Um aviso é fornecido em um `nullable_reference_type` em que o contexto de anotação anulável está desabilitado.

### <a name="nullable-class-constraint"></a>Restrição de classe anulável

A restrição `class` tem um equivalente anulável `class?`:

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>O operador NULL-tolerante

O operador post-Fix `!` é chamado de operador NULL-tolerante.

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

O `primary_expression` deve ser de um tipo de referência.  

O operador de `!` de sufixo não tem efeito de tempo de execução-ele é avaliado como o resultado da expressão subjacente. Sua única função é alterar o estado nulo da expressão e limitar os avisos fornecidos em seu uso.

### <a name="nullable-implicitly-typed-local-variables"></a>variáveis de local digitadas implicitamente de tipo nulo

`var` infere um tipo anotado para tipos de referência.
Por exemplo, em `var s = "";` a `var` é inferida como `string?`.

### <a name="nullable-compiler-directives"></a>Diretivas de compilador anuláveis

`#nullable` diretivas controlam os contextos de anotação e de aviso que permitem valor nulo.

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

`#pragma warning` diretivas são expandidas para permitir a alteração do contexto de aviso anulável e para permitir que avisos individuais sejam habilitados em mesmo quando estão desabilitados por padrão:

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

Observe que a nova forma de `pragma_warning_body` usa `nullable_action`, não `warning_action`.

## <a name="nullable-contexts"></a>Contextos que permitem valor nulo

Cada linha de código-fonte tem um *contexto de anotação anulável* e um *contexto de aviso anulável*. Eles controlam se as anotações anuláveis têm efeito e se são fornecidos avisos de nulidade. O contexto de anotação de uma determinada linha está *desabilitado* ou *habilitado*. O contexto de aviso de uma determinada linha está *desabilitado* ou *habilitado*.

Ambos os contextos podem ser especificados no nível do projeto (fora C# do código-fonte) ou em qualquer lugar dentro de um arquivo de origem por meio de diretivas de pré-processador `#nullable`. Se nenhuma configuração de nível de projeto for fornecida, o padrão será para os dois contextos a serem *desabilitados*.

A diretiva `#nullable` controla os contextos de anotação e de aviso dentro do texto de origem e tem precedência sobre as configurações de nível de projeto.

Uma diretiva define os contextos que ele controla para linhas de código subsequentes, até que outra diretiva a substitua ou até o final do arquivo de origem.

O efeito das diretivas é o seguinte:

- `#nullable disable`: define a anotação anulável e contextos de aviso como *desabilitado*
- `#nullable enable`: define a anotação anulável e contextos de aviso como *habilitados*
- `#nullable restore`: restaura os contextos de anotação e de aviso anuláveis para as configurações do projeto
- `#nullable disable annotations`: define o contexto de anotação anulável como *desabilitado*
- `#nullable enable annotations`: define o contexto de anotação anulável como *habilitado*
- `#nullable restore annotations`: restaura o contexto de anotação anulável para as configurações do projeto
- `#nullable disable warnings`: define o contexto de aviso anulável como *desabilitado*
- `#nullable enable warnings`: define o contexto de aviso anulável como *habilitado*
- `#nullable restore warnings`: restaura o contexto de aviso anulável para as configurações do projeto

## <a name="nullability-of-types"></a>Possibilidade de nulidade de tipos

Um determinado tipo pode ter um dos quatro nullabilities: *alheios*, *nonnullble*, *Nullable* e *Unknown*. 

Tipos não *nulos* e *desconhecidos* podem causar avisos se um valor `null` potencial for atribuído a eles. Os tipos *alheios* e *Nullable* , no entanto, são "*nulo-atribuíveis*" e podem ter `null` valores atribuídos a eles sem avisos. 

Tipos *alheios* e *nonnull* podem ser desreferenciados ou atribuídos sem avisos. Os valores de tipos *anuláveis* e *desconhecidos* , no entanto, são "*nulos*" e podem causar avisos quando desreferenciados ou atribuídos sem a verificação nula adequada. 

O *estado nulo padrão* de um tipo de rendimento nulo é "Talvez nulo". O estado nulo padrão de um tipo não nulo de rendimento é "NOT NULL".

O tipo e o contexto de anotação anulável que ele ocorre em determinar sua nulidade:

- Um tipo de valor não nulo `S` é sempre não *nulo*
- Um tipo de valor anulável `S?` é sempre *anulável*
- Um tipo de referência não anotado `C` em um contexto de anotação *desabilitado* é *alheios*
- Um tipo de referência não anotado `C` em um contexto de anotação *habilitado* não é *nulo*
- Um tipo de referência anulável `C?` é *anulável* (mas um aviso pode ser produzido em um contexto de anotação *desabilitado* )

Além disso, os parâmetros de tipo levam suas restrições em conta:

- Um parâmetro de tipo `T` em que todas as restrições (se houver) são tipos de rendimento nulo (*anuláveis* e *desconhecidos*) ou a restrição de `class?` é *desconhecida*
- Um parâmetro de tipo `T` em que pelo menos uma restrição é *alheios* ou não *nulo* ou uma das restrições `struct` ou `class` é
    - *alheios* em um contexto de anotação *desabilitado*
    - Não *nulo* em um contexto de anotação *habilitado*
- Um parâmetro de tipo anulável `T?` em que pelo menos uma das restrições de `T`é *alheios* ou não *nulo* , ou uma das restrições `struct` ou `class`, é
    - *permite valor nulo* em um contexto de anotação *desabilitado* (mas um aviso é produzido)
    - *permite valor nulo* em um contexto de anotação *habilitado*

Para um parâmetro de tipo `T`, `T?` só será permitido se `T` for conhecido como um tipo de valor ou se for conhecido como um tipo de referência.

### <a name="oblivious-vs-nonnullable"></a>Alheios vs não nulo

Um `type` é considerado em um determinado contexto de anotação quando o último token do tipo está dentro desse contexto.

Se um determinado tipo de referência `C` no código-fonte é interpretado como alheios ou não nulo depende do contexto de anotação desse código-fonte. Mas, uma vez estabelecida, ele é considerado parte desse tipo e "viaja com ele", por exemplo, durante a substituição de argumentos de tipo genérico. É como se houver uma anotação como `?` no tipo, mas invisível.

## <a name="constraints"></a>Restrições

Tipos de referência anuláveis podem ser usados como restrições genéricas. Além disso, `object` agora é válido como uma restrição explícita. A ausência de uma restrição agora é equivalente a uma restrição de `object?` (em vez de `object`), mas (ao contrário de `object` antes) `object?` não é proibida como uma restrição explícita.

`class?` é uma nova restrição que indica "tipo de referência possivelmente anulável", enquanto `class` denota "tipo de referência não nula".

A nulidade de um argumento de tipo ou de uma restrição não afeta se o tipo satisfaz a restrição, exceto onde esse já é o caso hoje (os tipos de valores anuláveis não satisfazem a restrição de `struct`). No entanto, se o argumento de tipo não atender aos requisitos de nulidade da restrição, um aviso poderá ser dado.

## <a name="null-state-and-null-tracking"></a>Estado nulo e acompanhamento nulo

Cada expressão em um local de origem específico tem um *estado nulo*, que indica se acredita potencialmente ser possível avaliar como NULL. O estado nulo é "NOT NULL" ou "Talvez NULL". O estado NULL é usado para determinar se um aviso deve ser fornecido sobre conversões e desreferências sem segurança nula.

### <a name="null-tracking-for-variables"></a>Acompanhamento nulo para variáveis

Para determinadas expressões que denotam variáveis ou propriedades, o estado NULL é rastreado entre ocorrências, com base nas atribuições para elas, nos testes executados neles e no fluxo de controle entre elas. Isso é semelhante a como a atribuição definitiva é controlada para variáveis. As expressões rastreadas são aquelas do seguinte formato:

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

Onde os identificadores denotam campos ou propriedades.

***Descrever as transições de estado nulos semelhantes à atribuição definitiva***

### <a name="null-state-for-expressions"></a>Estado nulo para expressões

O estado nulo de uma expressão é derivado de seu formulário e tipo e do estado nulo das variáveis envolvidas.

### <a name="literals"></a>{1&gt;Literais&lt;1}

O estado nulo de um literal de `null` é "Talvez nulo". O estado nulo de um literal `default` que está sendo convertido em um tipo que é conhecido não como um tipo de valor não nulo é "Talvez nulo". O estado nulo de qualquer outro literal é "NOT NULL".

### <a name="simple-names"></a>Nomes simples

Se uma `simple_name` não for classificada como um valor, seu estado nulo será "NOT NULL". Caso contrário, é uma expressão rastreada e seu estado nulo é seu estado nulo rastreado neste local de origem.

### <a name="member-access"></a>Acesso de membros

Se uma `member_access` não for classificada como um valor, seu estado nulo será "NOT NULL". Caso contrário, se for uma expressão rastreada, seu estado nulo será seu estado NULL rastreado nesse local de origem. Caso contrário, seu estado nulo será o estado nulo padrão de seu tipo.

### <a name="invocation-expressions"></a>Expressões de invocação

Se um `invocation_expression` invocar um membro declarado com um ou mais atributos para um comportamento nulo especial, o estado NULL será determinado por esses atributos. Caso contrário, o estado nulo da expressão será o estado nulo padrão de seu tipo.

### <a name="element-access"></a>Acesso ao elemento

Se uma `element_access` invocar um indexador declarado com um ou mais atributos para um comportamento nulo especial, o estado NULL será determinado por esses atributos. Caso contrário, o estado nulo da expressão será o estado nulo padrão de seu tipo.

### <a name="base-access"></a>Acesso de base

Se `B` denota o tipo base do tipo delimitador, `base.I` tem o mesmo estado nulo que `((B)this).I` e `base[E]` tem o mesmo estado nulo que `((B)this)[E]`.

### <a name="default-expressions"></a>Expressões padrão

`default(T)` tem o estado nulo "não nulo" se `T` é conhecido como um tipo de valor não nulo. Caso contrário, ele tem o estado nulo "Talvez nulo".

### <a name="null-conditional-expressions"></a>Expressões condicionais nulas

Um `null_conditional_expression` tem o estado nulo "Talvez nulo".

### <a name="cast-expressions"></a>Expressões de conversão

Se uma expressão de conversão `(T)E` invocar uma conversão definida pelo usuário, o estado nulo da expressão será o estado nulo padrão para seu tipo. Caso contrário, se `T` for nula, produzindo (*anulável* ou *desconhecido*), o estado NULL será "Talvez NULL". Caso contrário, o estado nulo será o mesmo que o estado nulo de `E`.

### <a name="await-expressions"></a>Expressões Await

O estado nulo de `await E` é o estado nulo padrão de seu tipo.

### <a name="the-as-operator"></a>O operador de `as`

Uma expressão de `as` tem o estado nulo "Talvez NULL".

### <a name="the-null-coalescing-operator"></a>O operador de União nula

`E1 ?? E2` tem o mesmo estado nulo que `E2`

### <a name="the-conditional-operator"></a>O operador condicional

O estado nulo de `E1 ? E2 : E3` será "NOT NULL" se o estado nulo de `E2` e `E3` forem "NOT NULL". Caso contrário, será "Talvez NULL".

### <a name="query-expressions"></a>Expressões de consulta

O estado nulo de uma expressão de consulta é o estado nulo padrão de seu tipo.

### <a name="assignment-operators"></a>Operadores de atribuição

`E1 = E2` e `E1 op= E2` têm o mesmo estado nulo que `E2` depois que qualquer conversões implícita tiver sido aplicada.

### <a name="unary-and-binary-operators"></a>Operadores unários e binários

Se um operador unário ou binário invocar um operador definido pelo usuário que é declarado com um ou mais atributos para um comportamento nulo especial, o estado NULL será determinado por esses atributos. Caso contrário, o estado nulo da expressão será o estado nulo padrão de seu tipo.

***Algo especial a fazer para `+` binários sobre cadeias de caracteres e delegados?***

### <a name="expressions-that-propagate-null-state"></a>Expressões que propagam o estado nulo

`(E)`, `checked(E)` e `unchecked(E)` têm o mesmo estado nulo que `E`.

### <a name="expressions-that-are-never-null"></a>Expressões que nunca são nulas

O estado nulo dos formulários de expressão a seguir é sempre "NOT NULL":

- `this` acesso
- cadeias de caracteres interpoladas
- expressões de `new` (objeto, delegado, objeto anônimo e expressões de criação de matriz)
- Expressões `typeof`
- Expressões `nameof`
- funções anônimas (métodos anônimos e expressões lambda)
- expressões tolerante nulas
- Expressões `is`

## <a name="type-inference"></a>Inferência de tipos

### <a name="type-inference-for-var"></a>Inferência de tipos para `var`

O tipo inferido para variáveis locais declaradas com `var` é informado pelo Estado nulo da expressão de inicialização.

```csharp
var x = E;
```

Se o tipo de `E` for um tipo de referência anulável `C?` e o estado nulo de `E` for "NOT NULL", o tipo inferido para `x` será `C`. Caso contrário, o tipo inferido é o tipo de `E`.

A nulidade do tipo inferido para `x` é determinada conforme descrito acima, com base no contexto de anotação do `var`, assim como se o tipo tivesse sido fornecido explicitamente nessa posição.

### <a name="type-inference-for-var"></a>Inferência de tipos para `var?`

O tipo inferido para variáveis locais declaradas com `var?` é independente do estado nulo da expressão de inicialização.

```csharp
var? x = E;
```

Se o tipo `T` de `E` for um tipo de valor anulável ou um tipo de referência anulável, o tipo inferido para `x` será `T`. Caso contrário, se `T` for um tipo de valor não nulo `S` o tipo inferido será `S?`. Caso contrário, se `T` for um tipo de referência não nulo `C` o tipo inferido será `C?`. Caso contrário, a declaração é inválida.

A nulidade do tipo inferido para `x` é sempre *anulável*.

### <a name="generic-type-inference"></a>Inferência de tipo genérico

A inferência de tipo genérico é aprimorada para ajudar a decidir se os tipos de referência inferidos devem ser anuláveis ou não. Esse é um melhor esforço e não em si gera avisos, mas pode levar a avisos anuláveis quando os tipos inferidos da sobrecarga selecionada são aplicados aos argumentos.

A inferência de tipos não depende do contexto de anotação dos tipos de entrada. Em vez disso, um `type` é inferido que adquire seu próprio contexto de anotação de onde "teria sido" se ele tivesse sido expresso explicitamente. Isso sublinha a função da inferência de tipos como uma conveniência para o que você poderia ter escrito por conta própria.

Mais precisamente, o contexto de anotação para um argumento de tipo inferido é o contexto do token que teria sido seguido pela lista de parâmetros de tipo `<...>`, tinha um; ou seja, o nome do método genérico que está sendo chamado. Para expressões de consulta que são traduzidas para essas chamadas, o contexto é obtido da palavra-chave contextual inicial da cláusula de consulta da qual a chamada é gerada.

### <a name="the-first-phase"></a>A primeira fase

Os tipos de referência anuláveis fluem para os limites das expressões iniciais, conforme descrito abaixo. Além disso, dois novos tipos de limites, ou seja, `null` e `default` são introduzidos. Sua finalidade é realizar ocorrências de `null` ou `default` nas expressões de entrada, o que pode fazer com que um tipo inferido seja anulável, mesmo quando não fosse. Isso funciona mesmo para tipos de *valores* anuláveis, que são aprimorados para pegar a "nulidade" no processo de inferência.

A determinação dos limites a serem adicionados na primeira fase é aprimorada da seguinte maneira:

Se um argumento `Ei` tiver um tipo de referência, o tipo `U` usado para a inferência dependerá do estado nulo de `Ei`, bem como seu tipo declarado:
- Se o tipo declarado for um tipo de referência não nulo `U0` ou um tipo de referência anulável `U0?`
    - Se o estado nulo de `Ei` for "NOT NULL", `U` será `U0`
    - Se o estado nulo de `Ei` for "Talvez nulo", `U` será `U0?`
- Caso contrário, se `Ei` tiver um tipo declarado, `U` será esse tipo
- Caso contrário, se `Ei` for `null`, `U` será o limite especial `null`
- Caso contrário, se `Ei` for `default`, `U` será o limite especial `default`
- Caso contrário, nenhuma inferência será feita.

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>Inferências exatas, de limite superior e de limite inferior

Em inferências *do* tipo `U` *ao* tipo `V`, se `V` for um tipo de referência anulável `V0?`, `V0` será usado em vez de `V` nas cláusulas a seguir.
- Se `V` for uma das variáveis de tipo não fixas, `U` será adicionado como um limite exato, superior ou inferior como antes
- Caso contrário, se `U` for `null` ou `default`, nenhuma inferência será feita
- Caso contrário, se `U` for um tipo de referência anulável `U0?`, `U0` será usado em vez de `U` nas cláusulas subsequentes.

A essência é que a nulidade que se refere diretamente a uma das variáveis de tipo não fixos é preservada em seus limites. Por outro lado, para as inferências que recursivamente os tipos de origem e destino, a nulidade é ignorada. Pode ou não corresponder, mas se não for, um aviso será emitido mais tarde se a sobrecarga for escolhida e aplicada.

### <a name="fixing"></a>Resolver

Atualmente, a especificação não faz um bom trabalho descrevendo o que acontece quando vários limites são conversíveis de identidade entre si, mas são diferentes. Isso pode ocorrer entre `object` e `dynamic`, entre os tipos de tupla que diferem apenas em nomes de elementos, entre os tipos construídos e agora também entre `C` e `C?` para tipos de referência.

Além disso, precisamos propagar "nulidade" das expressões de entrada para o tipo de resultado. 

Para lidar com isso, adicionamos mais fases para corrigir, que agora é:

1. Reunir todos os tipos em todos os limites como candidatos, removendo `?` de todos os que são tipos de referência anuláveis
2. Elimine os candidatos com base nos requisitos de limites exatos, inferiores e superiores (mantendo os limites de `null` e `default`)
3. Elimine os candidatos que não têm uma conversão implícita para todos os outros candidatos
4. Se todos os candidatos restantes não tiverem conversões de identidade entre si, a inferência de tipos falhará
5. *Mescle* os candidatos restantes conforme descrito abaixo
6. Se o candidato resultante for um tipo de referência ou um tipo de valor não nulo e *todos* os limites exatos ou *qualquer* um dos limites inferiores forem tipos de valores anuláveis, tipos de referência anuláveis, `null` ou `default`, `?` será adicionado ao candidato resultante, tornando-o um tipo de valor anulável ou tipo de referência.

A *mesclagem* é descrita entre dois tipos candidatos. Ele é transitivo e comutador, portanto, os candidatos podem ser mesclados em qualquer ordem com o mesmo resultado final. Ele será indefinido se os dois tipos candidatos não forem conversíveis de identidade entre si.

A função *Merge* usa dois tipos candidatos e uma direção ( *+* ou *-* ):

- *Merge*(`T`, `T`, *d*) = t
- *Merge*(`S`, `T?`, *+* ) = *mesclagem*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`
- *Merge*(`S`, `T?`, *-* ) = *mesclagem*(`S?`, `T`, *-* ) = *Merge*(`S`, `T`, *-* )
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*mesclagem*(`S1`, `T1`, *D1*)`,...,`*mesclagem*(`Sn`, `Tn`, *DN*)`>`, *em que*
    - `di` =  *+* se o parâmetro do `i`' th type de `C<...>` for Covariance
    - `di` =  *-* se o parâmetro do `i`' th type de `C<...>` for Compensate ou inconstante
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*mesclagem*(`S1`, `T1`, *D1*)`,...,`*mesclagem*(`Sn`, `Tn`, *DN*)`>`, *em que*
    - `di` =  *-* se o parâmetro do `i`' th type de `C<...>` for Covariance
    - `di` =  *+* se o parâmetro do `i`' th type de `C<...>` for Compensate ou inconstante
- *Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*mesclar*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *em que*
    - `ni` estiver ausente se `si` e `ti` forem diferentes ou se ambos estiverem ausentes
    - `ni` `si` se `si` e `ti` forem iguais
- *Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>Warnings

### <a name="potential-null-assignment"></a>Possível atribuição nula

### <a name="potential-null-dereference"></a>Desreferência de nulo potencial

### <a name="constraint-nullability-mismatch"></a>Incompatibilidade de nulidade de restrição

### <a name="nullable-types-in-disabled-annotation-context"></a>Tipos anuláveis no contexto de anotação desabilitado

## <a name="attributes-for-special-null-behavior"></a>Atributos para comportamento nulo especial


