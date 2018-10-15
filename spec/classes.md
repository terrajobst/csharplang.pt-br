# <a name="classes"></a>Classes

Uma classe é uma estrutura de dados que pode conter dados membros (campos e constantes), membros da função (métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e tipos aninhados. Tipos de classe dá suporte à herança, um mecanismo pelo qual uma classe derivada pode estender e especializar uma classe base.

## <a name="class-declarations"></a>Declarações de classe

Um *class_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara uma nova classe.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Um *class_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *class_modifier*s ([modificadores de classe](classes.md#class-modifiers)), seguido por um recurso opcional `partial` modificador, seguido da palavra-chave `class` e uma *identificador* que nomeia a classe, seguida por um opcional *type_parameter_list* ([parâmetros de tipo](classes.md#type-parameters)), seguido por um recurso opcional *class_base* especificação ([da Base de dados de classe especificação](classes.md#class-base-specification)), seguido por um conjunto opcional de *type_parameter_constraints_clause*s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), seguido por um *class_body*  ([Classe corpo](classes.md#class-body)), opcionalmente seguido por um ponto e vírgula.

Uma declaração de classe não pode fornecer *type_parameter_constraints_clause*s, a menos que ele também fornece um *type_parameter_list*.

Uma declaração de classe que fornece um *type_parameter_list* é um ***declaração de classe genérica***. Além disso, qualquer classe aninhada dentro de uma declaração de classe genérica ou uma declaração de struct genérico é em si uma declaração de classe genérica, já que os parâmetros de tipo para o tipo de conteúdo devem ser fornecidos para criar um tipo construído.

### <a name="class-modifiers"></a>Modificadores de classe

Um *class_declaration* opcionalmente pode incluir uma sequência de modificadores de classe:

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de classe.

O `new` modificador é permitido em classes aninhadas. Especifica que a classe oculta um membro herdado com o mesmo nome, conforme descrito em [o novo modificador](classes.md#the-new-modifier). É um erro de tempo de compilação para o `new` modificador para aparecer em uma declaração de classe não é uma declaração de classe aninhada.

O `public`, `protected`, `internal`, e `private` modificadores de acessibilidade da classe de controle. Dependendo do contexto no qual ocorre a declaração de classe, alguns desses modificadores não podem ter permissão ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).

O `abstract`, `sealed` e `static` modificadores são discutidos nas seções a seguir.

#### <a name="abstract-classes"></a>Classes abstratas

O `abstract` modificador é usado para indicar que uma classe é incompleta e que ele é destinado a ser usado apenas como uma classe base. Uma classe abstrata é diferente de uma classe não abstrata das seguintes maneiras:

*  Uma classe abstrata não pode ser instanciada diretamente, e é um erro de tempo de compilação para usar o `new` operador em uma classe abstrata. Embora seja possível ter variáveis e valores cujos tipos de tempo de compilação são abstratos, tais variáveis e valores serão necessariamente ser `null` ou contém referências às instâncias de classes não abstratas derivadas dos tipos abstratos.
*  Uma classe abstrata é permitida (mas não obrigatório) para conter membros abstratos.
*  Uma classe abstrata não pode ser sealed.

Quando uma classe não abstrata é derivada de uma classe abstrata, a classe não abstrata deve incluir implementações reais de todos os membros abstratos herdados, substituindo, portanto, esses membros abstratos. No exemplo
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
a classe abstrata `A` apresenta um método abstrato `F`. Classe `B` apresenta um método adicional `G`, mas já que ele não fornece uma implementação de `F`, `B` também deve ser declarada como abstrata. Classe `C` substituições `F` e fornece uma implementação real. Como não há nenhum membro abstrato na `C`, `C` é permitido (mas não obrigatório) a ser não abstrata.

#### <a name="sealed-classes"></a>Classes lacradas

O `sealed` modificador é usado para evitar a derivação de uma classe. Ocorrerá um erro de tempo de compilação se uma classe selada for especificada como a classe base de outra classe.

Uma classe selada não pode ser também uma classe abstrata.

O `sealed` modificador é usado principalmente para evitar derivação não intencional, mas ele também permite que determinadas otimizações de tempo de execução. Em particular, como uma classe selada é conhecida nunca ter quaisquer classes derivadas, é possível transformar as invocações de membro de função virtual em instâncias de classe sealed em chamadas não virtuais.

#### <a name="static-classes"></a>Classes estáticas

O `static` modificador é usado para marcar a classe que está sendo declarada como um ***classe estática***. Uma classe estática não pode ser instanciada, não pode ser usada como um tipo e pode conter apenas membros estáticos. Apenas uma classe estática pode conter declarações de métodos de extensão ([métodos de extensão](classes.md#extension-methods)).

Uma declaração de classe estática está sujeito às seguintes restrições:

*  Uma classe estática não pode incluir um `sealed` ou `abstract` modificador. No entanto, observe que uma vez que uma classe estática não pode ser instanciada ou derivada, ele se comporta como se ele foi bloqueado e abstrato.
*  Uma classe estática não pode incluir um *class_base* especificação ([classe base especificação](classes.md#class-base-specification)) e não é possível especificar explicitamente uma classe base ou uma lista de interfaces implementadas. Uma classe estática herda implicitamente de tipo `object`.
*  Uma classe estática pode conter apenas membros estáticos ([membros estáticos e de instância](classes.md#static-and-instance-members)). Observe que as constantes e tipos aninhados são classificados como membros estáticos.
*  Uma classe estática não pode ter membros com `protected` ou `protected internal` declarado de acessibilidade.

Ele é um erro de tempo de compilação para violar qualquer uma dessas restrições.

Uma classe estática não tem nenhum construtor de instância. Não é possível declarar um construtor de instância em uma classe estática e nenhum construtor de instância padrão ([1&gt;construtores padrão](classes.md#default-constructors)) é fornecido para uma classe estática.

Os membros de uma classe estática não são automaticamente estáticos, e as declarações de membro devem incluir explicitamente um `static` modificador (exceto constantes e tipos aninhados). Quando uma classe é aninhada dentro de uma classe estática de externa, a classe aninhada não é uma classe estática, a menos que ele inclua explicitamente um `static` modificador.

__Referenciar tipos de classe estática__

Um *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) tem permissão para fazer referência a uma classe estática se

*  O *namespace_or_type_name* é o `T` em uma *namespace_or_type_name* do formulário `T.I`, ou
*  O *namespace_or_type_name* é o `T` em uma *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) do formulário `typeof(T)`.

Um *primary_expression* ([membros de função](expressions.md#function-members)) tem permissão para fazer referência a uma classe estática se

*  O *primary_expression* é o `E` em uma *member_access* ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) do formulário `E.I`.

Em qualquer outro contexto é um erro de tempo de compilação para fazer referência a uma classe estática. Por exemplo, é um erro para uma classe estática para ser usado como uma classe base, um tipo constituinte ([tipos aninhados](classes.md#nested-types)) de um membro, um argumento de tipo genérico ou uma restrição de parâmetro de tipo. Da mesma forma, uma classe estática não pode ser usada em um tipo de matriz, um tipo de ponteiro, uma `new` expressão, uma expressão de conversão, uma `is` expressão, um `as` expressão, um `sizeof` expressão ou uma expressão de valor padrão.

### <a name="partial-modifier"></a>Modificador parcial

O `partial` modificador é usado para indicar que este *class_declaration* é uma declaração de tipo parcial. Combinam várias declarações de tipo parcial com o mesmo nome dentro de uma declaração de namespace ou tipo delimitador à declaração de um tipo de formulário, seguindo as regras especificadas na [tipos parciais](classes.md#partial-types).

Tendo a declaração de uma classe distribuída em segmentos separados do texto do programa pode ser útil se esses segmentos são produzidos ou mantidos em diferentes contextos. Por exemplo, uma parte de uma declaração de classe pode ser gerada, de máquina, enquanto o outro é criado manualmente. Separação textual dos dois impede que atualizações por um entrem em conflito com as atualizações por outros.

### <a name="type-parameters"></a>Parâmetros de tipo

Um parâmetro de tipo é um identificador simples que denota um espaço reservado para um tipo de argumento fornecido para criar um tipo construído. Um parâmetro de tipo é um espaço reservado formal para um tipo que será fornecido posteriormente. Por outro lado, um argumento de tipo ([argumentos de tipo](types.md#type-arguments)) é o tipo real que é substituído para o parâmetro de tipo quando um tipo construído é criado.

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

Cada parâmetro de tipo em uma declaração de classe define um nome no espaço de declaração ([declarações](basic-concepts.md#declarations)) dessa classe. Portanto, ele não pode ter o mesmo nome que outro parâmetro de tipo ou um membro declarado na classe. Um parâmetro de tipo não pode ter o mesmo nome que o próprio tipo.

### <a name="class-base-specification"></a>Especificação de classe base

Uma declaração de classe pode incluir um *class_base* especificação, que define a classe base direta da classe e as interfaces ([Interfaces](interfaces.md)) diretamente implementado pela classe.

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

A classe base especificada em uma declaração de classe pode ser um tipo de classe construído ([construído tipos](types.md#constructed-types)). Uma classe base não pode ser um parâmetro de tipo por conta própria, embora isso pode envolver os parâmetros de tipo que estão no escopo.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Classes base

Quando um *class_type* está incluído na *class_base*, ele especifica a classe base direta da classe que está sendo declarado. Se uma declaração de classe não tiver nenhuma *class_base*, ou se o *class_base* listas apenas tipos de interface, a classe base direta é assumida ser `object`. Uma classe herda os membros de sua classe base direta, conforme descrito em [herança](classes.md#inheritance).

No exemplo
```csharp
class A {}

class B: A {}
```
classe `A` deve ser a classe base direta do `B`, e `B` deve ser derivado de `A`. Uma vez que `A` faz não explicitamente especificar uma classe base direta, sua classe base direta é implicitamente `object`.

Para um tipo de classe construída, se uma classe base for especificada na declaração de classe genérica, a classe base do tipo construído é obtida pela substituição, para cada *type_parameter* na declaração de classe base, correspondente *type_argument* do tipo construído. Dadas as declarações de classe genérica
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
a classe base do tipo construído `G<int>` seria `B<string,int[]>`.

A classe base direta de um tipo de classe deve ser pelo menos tão acessíveis quanto o próprio tipo de classe ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)). Por exemplo, é um erro de tempo de compilação para um `public` classe para derivar de uma `private` ou `internal` classe.

A classe base direta de um tipo de classe não deve ser qualquer um dos seguintes tipos: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, ou `System.ValueType`. Além disso, uma declaração de classe genérica não é possível usar `System.Attribute` como uma classe base direta ou indireta.

Ao determinar o significado da especificação de classe base direta `A` de uma classe `B`, a classe base direta de `B` temporariamente será considerado como `object`. Intuitivamente, isso garante que o significado de uma especificação de classe base não pode recursivamente depender de si mesma. O Exemplo:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
é um erro desde na especificação de classe base `A<C.B>` a classe base direta do `C` é considerado `object`e, portanto, (pelas regras da [nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) `C` não é considerado como ter um membro `B`.

As classes de base de um tipo de classe são a classe base direta e suas classes base. Em outras palavras, o conjunto de classes base é o fechamento transitivo da relação de classe base direta. Referindo-se ao exemplo anterior, as classes base `B` estão `A` e `object`. No exemplo
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
as classes base `D<int>` estão `C<int[]>`, `B<IComparable<int[]>>`, `A`, e `object`.

Com exceção de classe `object`, cada tipo de classe tem exatamente uma classe base direta. O `object` classe não tem nenhuma classe base direta e é a classe base definitiva de todas as outras classes.

Quando uma classe `B` deriva de uma classe `A`, ele é um erro de tempo de compilação para `A` depender `B`. Uma classe ***depende diretamente*** sua classe base direta (se houver) e ***depende diretamente*** a classe dentro do qual ele está aninhado imediatamente (se houver). Dada esta definição, o conjunto completo de classes dos quais depende de uma classe é o fechamento reflexivo e transitivo do ***depende diretamente*** relação.

O exemplo
```csharp
class A: A {}
```
está incorreto porque a classe depende em si. Da mesma forma, o exemplo
```csharp
class A: B {}
class B: C {}
class C: A {}
```
é um erro porque as classes circularmente dependem em si. Por fim, o exemplo
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
resulta em um erro de tempo de compilação porque `A` depende `B.C` (sua classe base direta), que depende `B` (sua classe delimitadora), que depende circularmente `A`.

Observe que uma classe não depende das classes que estão aninhadas dentro dele. No exemplo
```csharp
class A
{
    class B: A {}
}
```
`B` depende `A` (porque `A` é sua classe base direta e sua classe delimitadora), mas `A` não depende `B` (como `B` não é uma classe base nem uma classe delimitadora de `A` ). Portanto, o exemplo é válido.

Não é possível derivar de um `sealed` classe. No exemplo
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
classe `B` é um erro porque ele tenta derivar a `sealed` classe `A`.

#### <a name="interface-implementations"></a>Implementações de interfaces

Um *class_base* especificação pode incluir uma lista dos tipos de interface, na qual caso a classe deve implementar diretamente os tipos de interface fornecido. Implementações de interface são discutidas mais detalhadamente em [implementações de Interface](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Restrições de parâmetro de tipo

Declarações de tipo e método genéricas, opcionalmente, podem especificar restrições de parâmetro de tipo, incluindo *type_parameter_constraints_clause*s.

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

Cada *type_parameter_constraints_clause* consiste o token `where`, seguido do nome de um parâmetro de tipo, seguido por dois-pontos e a lista de restrições para parâmetro de tipo. Pode haver no máximo um `where` cláusula para cada parâmetro de tipo e o `where` cláusulas podem ser listadas em qualquer ordem. Como o `get` e `set` tokens em um acessador de propriedade, o `where` token não é uma palavra-chave.

A lista de restrições fornecido em uma `where` cláusula pode incluir qualquer um dos componentes a seguir, nesta ordem: uma única restrição primária, uma ou mais restrições secundárias e a restrição de construtor, `new()`.

Uma restrição primária pode ser um tipo de classe ou o ***restrição de tipo de referência*** `class` ou o ***restrição de tipo de valor*** `struct`. Uma restrição secundária pode ser um *type_parameter* ou *interface_type*.

A restrição de tipo de referência Especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de referência. Essa restrição atendem a todos os tipos de classe, tipos de interface, tipos de delegados, tipos de matriz e os parâmetros de tipo conhecidos por ser um tipo de referência (conforme definido abaixo).

A restrição de tipo de valor Especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de valor não anulável. Todos os tipos de struct não anuláveis, tipos enum e parâmetros de tipo com a restrição de tipo de valor satisfazem essa restrição. Observe que, embora classificado como um tipo de valor, um tipo anulável ([tipos anuláveis](types.md#nullable-types)) não satisfaz a restrição de tipo de valor. Um parâmetro de tipo com a restrição de tipo de valor não pode ter também o *constructor_constraint*.

Tipos de ponteiro nunca são permitidos para alguns argumentos de tipo e não são considerados para satisfazer qualquer referência tipo ou valor restrições de tipo.

Se uma restrição é um tipo de classe, um tipo de interface ou um parâmetro de tipo, esse tipo Especifica um mínimo "tipo base" que deve oferecer suporte a cada argumento de tipo usado para esse parâmetro de tipo. Sempre que um tipo construído ou método genérico for usado, o argumento de tipo é verificado em relação às restrições no parâmetro de tipo em tempo de compilação. O argumento de tipo fornecido deve satisfazer as condições descritas [atendendo às restrições de](types.md#satisfying-constraints).

Um *class_type* restrição deve satisfazer as regras a seguir:

*  O tipo deve ser um tipo de classe.
*  O tipo não deve ser `sealed`.
*  O tipo não deve ser um dos seguintes tipos: `System.Array`, `System.Delegate`, `System.Enum`, ou `System.ValueType`.
*  O tipo não deve ser `object`. Como todos os tipos derivam `object`, tal restrição não terá efeito algum se ele eram permitido.
*  No máximo uma restrição para um parâmetro de tipo determinado pode ser um tipo de classe.

Um tipo especificado como uma *interface_type* restrição deve satisfazer as regras a seguir:

*  O tipo deve ser um tipo de interface.
*  Um tipo não deve ser especificado mais de uma vez em um determinado `where` cláusula.

Em ambos os casos, a restrição pode envolver qualquer um dos parâmetros de tipo da declaração de método como parte de um tipo construído ou tipo associado e pode envolver o tipo que está sendo declarado.

Qualquer tipo de classe ou interface especificado como uma restrição de parâmetro de tipo deve ser pelo menos tão acessível ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)) como o tipo genérico ou método que está sendo declarado.

Um tipo especificado como uma *type_parameter* restrição deve satisfazer as regras a seguir:

*  O tipo deve ser um parâmetro de tipo.
*  Um tipo não deve ser especificado mais de uma vez em um determinado `where` cláusula.

Além disso não deve haver nenhum ciclo no gráfico de dependência de parâmetros de tipo, em que a dependência é uma relação transitiva definida por:

*  Se um parâmetro de tipo `T` é usado como uma restrição de parâmetro de tipo `S` , em seguida, `S` ***depende*** `T`.
*  Se um parâmetro de tipo `S` depende de um parâmetro de tipo `T` e `T` depende de um parâmetro de tipo `U` , em seguida, `S` ***depende*** `U`.

Dada essa relação, é um erro de tempo de compilação para um parâmetro de tipo depender de si mesma (direta ou indiretamente).

Todas as restrições devem ser consistentes entre os parâmetros de tipo dependente. Se o parâmetro de tipo `S` depende do parâmetro de tipo `T` então:

*  `T` não deve ter a restrição de tipo de valor. Caso contrário, `T` efetivamente o está lacrado `S` seria forçado a ter o mesmo tipo que `T`, eliminando a necessidade de dois parâmetros de tipo.
*  Se `S` tem a restrição de tipo de valor, em seguida, `T` não deve ter uma *class_type* restrição.
*  Se `S` tem uma *class_type* restrição `A` e `T` tem uma *class_type* restrição `B` e em seguida, deve haver uma conversão de identidade ou implícita fazer referência a conversão de `A` à `B` ou uma conversão de referência implícita da `B` para `A`.
*  Se `S` também depende do parâmetro de tipo `U` e `U` tem um *class_type* restrição `A` e `T` tem um *class_type* restrição `B` e em seguida, deve haver uma conversão de identidade ou uma conversão de referência implícita da `A` ao `B` ou uma conversão de referência implícita da `B` para `A`.

Ele é válido para `S` tenha a restrição de tipo de valor e `T` ter restrição de tipo de referência. Efetivamente, isso limita `T` para os tipos `System.Object`, `System.ValueType`, `System.Enum`e qualquer tipo de interface.

Se o `where` cláusula para um parâmetro de tipo inclui uma restrição de construtor (que tem o formato `new()`), é possível usar o `new` operador para criar instâncias do tipo ([deexpressõesdecriaçãodoobjeto](expressions.md#object-creation-expressions)). Qualquer tipo de argumento usado para um parâmetro de tipo com uma restrição de construtor deve ter um construtor sem parâmetros público (Este construtor implicitamente existe para qualquer tipo de valor) ou ser um parâmetro de tipo com a restrição de tipo de valor ou a restrição de construtor (consulte [Restrições de parâmetro de tipo](classes.md#type-parameter-constraints) para obter detalhes).

A seguir estão exemplos de restrições:
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

O exemplo a seguir está em erro, porque ele faz com que uma circularidade no grafo de dependência dos parâmetros de tipo:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Os exemplos a seguir ilustram situações inválidas adicionais:
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

O ***efetivo classe base*** de um parâmetro de tipo `T` é definido da seguinte maneira:

*  Se `T` não tem nenhuma restrição primária ou restrições de parâmetro de tipo, sua classe base eficaz é `object`.
*  Se `T` tem a restrição de tipo de valor, sua classe base eficaz é `System.ValueType`.
*  Se `T` tem uma *class_type* restrição `C` , mas nenhum *type_parameter* restrições, sua classe base eficaz é `C`.
*  Se `T` não tem nenhum *class_type* restrição, mas tem um ou mais *type_parameter* restrições, sua classe base efetiva é o tipo mais contido ([eliminada conversão operadores](conversions.md#lifted-conversion-operators)) no conjunto de classes base efetivos de seu *type_parameter* restrições. As regras de consistência se existe um tipo mais contido.
*  Se `T` tem um *class_type* restrição e um ou mais *type_parameter* restrições, sua classe base efetiva é o tipo mais contido ([eliminada conversão operadores](conversions.md#lifted-conversion-operators)) no conjunto que consiste o *class_type* restrição de `T` e as classes base efetivas de seu *type_parameter* restrições. As regras de consistência se existe um tipo mais contido.
*  Se `T` tem a restrição de tipo de referência, mas não *class_type* restrições, sua classe base eficaz é `object`.

Com a finalidade dessas regras, se T tem uma restrição `V` que é um *value_type*, em vez disso, use o tipo de base mais específico `V` que é um *class_type*. Jamais pode acontecer em uma restrição explicitamente especificada, mas pode ocorrer quando as restrições de um método genérico são implicitamente herdadas por uma declaração de método de substituição ou uma implementação explícita de um método de interface.

Essas regras garantem que a classe base eficaz é sempre uma *class_type*.

O ***conjunto da interface efetivo*** de um parâmetro de tipo `T` é definido da seguinte maneira:

*  Se `T` não tem nenhum *secondary_constraints*, seu conjunto de interface efetiva está vazio.
*  Se `T` tem *interface_type* restrições, mas não *type_parameter* restrições, seu conjunto de interface eficaz é seu conjunto de *interface_type* restrições.
*  Se `T` não tem nenhum *interface_type* restrições, mas tem *type_parameter* restrições, seu conjunto de interface eficaz é a união dos conjuntos interface efetivo de seu *type_ parâmetro* restrições.
*  Se `T` tem ambos *interface_type* restrições e *type_parameter* restrições, seu conjunto de interface eficaz é a união de seu conjunto de *interface_type* restrições e os conjuntos de interface efetivo de seu *type_parameter* restrições.

É um parâmetro de tipo ***conhecido por ser um tipo de referência*** se ele tem a restrição de tipo de referência ou não é de sua classe base efetivo `object` ou `System.ValueType`.

Valores de um tipo de parâmetro de tipo restrita podem ser usados para acessar os membros de instância implicados pelas restrições. No exemplo
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
os métodos de `IPrintable` pode ser chamado diretamente nas `x` porque `T` é restrito para implementar sempre `IPrintable`.

### <a name="class-body"></a>Corpo da classe

O *class_body* de uma classe define os membros dessa classe.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Tipos parciais

Uma declaração de tipo pode ser dividida entre vários ***declarações de tipo parcial***. A declaração de tipo é construída de suas partes, seguindo as regras nesta seção, momento em que ele será tratado como uma única declaração durante o restante do processamento de tempo de compilação e tempo de execução do programa.

Um *class_declaration*, *struct_declaration* ou *interface_declaration* representa uma declaração de tipo parcial se ela incluir um `partial` modificador. `partial` não é uma palavra-chave e atua apenas como um modificador se ela aparecer imediatamente antes de uma das palavras-chave `class`, `struct` ou `interface` em uma declaração de tipo, ou antes do tipo `void` em uma declaração de método. Em outros contextos, ele pode ser usado como um identificador normal.

Cada parte de uma declaração de tipo parcial deve incluir um `partial` modificador. Ele deve ter o mesmo nome e ser declarado no mesmo namespace ou tipo de declaração como as outras partes. O `partial` modificador indica que as partes adicionais da declaração de tipo podem existir em outro lugar, mas a existência de tais partes adicionais não é um requisito; ele é válido para um tipo com uma única declaração incluir o `partial` modificador.

Todas as partes de um tipo parcial devem ser compiladas juntos, de modo que as partes podem ser mescladas em tempo de compilação em uma única declaração de tipo. Especificamente, tipos parciais não permitem tipos já compilados seja estendida.

Tipos aninhados podem ser declarados em várias partes, usando o `partial` modificador. Normalmente, o tipo recipiente é declarado usando `partial` como bem e cada parte do tipo aninhado é declarado em uma parte diferente do tipo recipiente.

O `partial` modificador não é permitido em declarações de enum ou delegado.

### <a name="attributes"></a>Atributos

Os atributos de um tipo parcial são determinados pela combinação, em uma ordem não especificada, os atributos de cada uma das partes. Se um atributo é colocado em várias partes, é equivalente a especificar o atributo várias vezes no tipo. Por exemplo, as duas partes:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
são equivalentes a uma declaração, como:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Combinam atributos em parâmetros de tipo de maneira semelhante.

### <a name="modifiers"></a>Modificadores

Quando uma declaração de tipo parcial inclui uma especificação de acessibilidade (o `public`, `protected`, `internal`, e `private` modificadores) ele deve concordar com todas as outras partes que incluem uma especificação de acessibilidade. Se nenhuma parte de um tipo parcial inclui uma especificação de acessibilidade, o tipo é determinado a acessibilidade padrão apropriado ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).

Se um ou mais declarações parciais de um tipo aninhado incluem um `new` modificador, nenhum aviso é relatado se o tipo aninhado oculta um membro herdado ([ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance)).

Se um ou mais declarações parciais de uma classe incluem uma `abstract` modificador, a classe será considerada abstrata ([classes abstratas](classes.md#abstract-classes)). Caso contrário, a classe é considerada não-abstrata.

Se um ou mais declarações parciais de uma classe incluem uma `sealed` modificador, a classe será considerada lacrado ([lacrados classes](classes.md#sealed-classes)). Caso contrário, a classe é considerada sem lacre.

Observe que uma classe não pode ser abstract e sealed.

Quando o `unsafe` modificador é usado em uma declaração de tipo parcial, somente essa parte é considerada um contexto inseguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parâmetros de tipo e restrições

Se um tipo genérico é declarado em várias partes, cada parte deve declarar os parâmetros de tipo. Cada parte deve ter o mesmo número de parâmetros de tipo e o mesmo nome para cada parâmetro de tipo, em ordem.

Quando uma declaração de tipo genérico parcial inclui restrições (`where` cláusulas), as restrições devem concordar com todas as outras partes que incluem restrições. Especificamente, cada parte que inclui as restrições deve ter restrições para o mesmo conjunto de parâmetros de tipo, e para cada parâmetro de tipo dos conjuntos de primário, secundário e restrições de construtor devem ser equivalente. Dois conjuntos de restrições são equivalentes se eles contiverem os mesmos membros. Se nenhuma parte de um tipo genérico parcial especifica restrições de parâmetro de tipo, o tipo de parâmetros são considerados sem restrição.

O exemplo
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
é correto porque as partes que incluem restrições (os dois primeiros) efetivamente especificam o mesmo conjunto de primário, secundário e restrições de construtor para o mesmo conjunto de parâmetros de tipo, respectivamente.

### <a name="base-class"></a>Classe base

Quando uma declaração de classe parcial inclui uma especificação de classe base, ele deve concordar com todas as outras partes que incluem uma especificação de classe base. Se nenhuma parte de uma classe parcial inclui uma especificação de classe base, torna-se a classe base `System.Object` ([classes Base](classes.md#base-classes)).

### <a name="base-interfaces"></a>Interfaces base

O conjunto de interfaces base para um tipo declarado em várias partes é a união das interfaces base especificada em cada parte. Uma interface de base específica somente pode ser nomeada de uma vez em cada parte, mas é permitido para várias partes nomear as mesmas interfaces base. Deve haver apenas uma implementação dos membros de uma determinada interface base.

No exemplo
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
o conjunto de interfaces base para a classe `C` está `IA`, `IB`, e `IC`.

Normalmente, cada parte fornece uma implementação de interface (s) declarado na parte em questão; No entanto, isso não é um requisito. Uma parte pode fornecer a implementação de uma interface declarada em uma parte diferente:
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Membros

Com exceção dos métodos parciais ([métodos parciais](classes.md#partial-methods)), o conjunto de membros de um tipo declarado em várias partes é simplesmente a união de conjunto de membros declarados em cada parte. Os corpos de todas as partes da declaração do tipo compartilham o mesmo espaço de declaração ([declarações](basic-concepts.md#declarations)) e o escopo de cada membro ([escopos](basic-concepts.md#scopes)) estende a órgãos de desenvolvimento de todas as partes. O domínio de acessibilidade de qualquer membro sempre inclui todas as partes do tipo delimitador; um `private` membro declarado em uma parte está livremente acessível de outra parte. Ele é um erro de tempo de compilação para declarar o mesmo membro em mais de uma parte do tipo, a menos que esse membro é um tipo com o `partial` modificador.

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

A ordenação dos membros dentro de um tipo é raramente significativo ao código c#, mas pode ser significativo ao fazer interface com outras linguagens e ambientes. Nesses casos, a ordenação dos membros dentro de um tipo declarado em várias partes é indefinido.

### <a name="partial-methods"></a>Métodos parciais

Métodos parciais podem ser definidos em uma parte de uma declaração de tipo e implementados em outro. A implementação é opcional. Se nenhuma parte implementa o método parcial, a declaração de método parcial e todas as chamadas para ele são removidas da declaração de tipo resultante da combinação das partes.

Métodos parciais não é possível definir os modificadores de acesso, mas são implicitamente `private`. Tipo de retorno deve ser `void`, e seus parâmetros não podem ter o `out` modificador. O identificador `partial` é reconhecido como uma palavra-chave especial em uma declaração de método somente se ela aparecer imediatamente antes o `void` tipo; caso contrário, ele pode ser usado como um identificador normal. Um método parcial não pode implementar métodos de interface explicitamente.

Há dois tipos de declarações de método parcial: se o corpo da declaração do método é um ponto e vírgula, a declaração é considerada um ***definindo a declaração de método parcial***. Se o corpo é fornecido como uma *bloco*, a declaração deve ser um ***implementar a declaração de método parcial***. Entre as partes de uma declaração de tipo pode haver apenas uma definição de declaração de método parcial com uma determinada assinatura e pode haver apenas uma implementação de declaração de método parcial com uma determinada assinatura. Se uma declaração de método parcial de implementação é fornecida, uma definição de declaração de método parcial correspondente deve existir e as declarações devem corresponder como especificado no seguinte:

* As declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem), nome do método, o número de parâmetros de tipo e o número de parâmetros.
* Parâmetros correspondentes nas declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem) e os mesmos tipos (módulo diferenças nos nomes de parâmetro de tipo).
* Parâmetros de tipo correspondente nas declarações devem ter as mesmas restrições (módulo diferenças nos nomes de parâmetro de tipo).

Uma declaração de método parcial de implementação pode aparecer na mesma parte da declaração de método parcial de definição correspondente.

Um método definição parcial participa da resolução de sobrecarga. Portanto, se uma declaração de implementação é fornecida, expressões de invocação podem resolver para invocações de método parcial. Como um método parcial sempre retorna `void`, tais expressões de invocação sempre será instruções de expressão. Além disso, como um método parcial é implicitamente `private`, tais instruções sempre ocorrerá dentro de uma das partes da declaração de tipo dentro do qual o método parcial é declarado.

Se nenhuma parte de uma declaração de tipo parcial contém uma declaração de implementação para um determinado método parcial, qualquer instrução de expressão invocá-lo é simplesmente removida da declaração de tipo combinado. Portanto, a expressão de invocação, incluindo todas as expressões constituintes, não tem nenhum efeito em tempo de execução. O método parcial em si também é removido e não será um membro da declaração de tipo combinado.

Se existir uma declaração de implementação para um determinado método parcial, as invocações de métodos parciais são mantidas. O método parcial dá origem a uma declaração de método, semelhante à declaração de método parcial de implementação, exceto os seguintes:

* O `partial` modificador não está incluído
* Os atributos na declaração de método resultantes são os atributos combinados da definição e a declaração de método parcial de implementação em ordem não especificada. Duplicatas não são removidas.
* Os atributos nos parâmetros de declaração de método resultantes são os atributos combinados dos parâmetros correspondentes da definição e a declaração de método parcial de implementação em ordem não especificada. Duplicatas não são removidas.

Se uma declaração de definição, mas não uma declaração de implementação é fornecido para um método parcial M, as seguintes restrições se aplicam:

* É um erro de tempo de compilação para criar um delegado de método ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)).
* É um erro de tempo de compilação para fazer referência a `M` dentro de uma função anônima que é convertida em um tipo de árvore de expressão ([avaliação das conversões de função anônima para tipos de árvore de expressão](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Expressões que ocorrem como parte de uma invocação de `M` não afetam o estado de atribuição definitiva ([atribuição definitiva](variables.md#definite-assignment)), que potencialmente podem levar a erros de tempo de compilação.
* `M` não pode ser o ponto de entrada para um aplicativo ([inicialização do aplicativo](basic-concepts.md#application-startup)).

Métodos parciais são úteis para permitir que uma parte de uma declaração de tipo para personalizar o comportamento da outra parte, por exemplo, aquele que é gerado por uma ferramenta. Considere a seguinte declaração de classe parcial:
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

Se essa classe será compilada sem outras partes, as declarações de método parcial a definição e suas invocações serão removidas e a declaração de classe combinado resultante será equivalente ao seguinte:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

Suponha que outra parte for fornecido, no entanto, que fornece declarações de implementação dos métodos parciais:
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

Em seguida, a declaração de classe combinado resultante será equivalente ao seguinte:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>Associação de nome

Embora cada parte de um tipo extensível deve ser declarado no mesmo namespace, as partes normalmente são gravadas dentro de declarações de namespace diferente. Assim, diferentes `using` diretivas ([diretivas Using](namespaces.md#using-directives)) podem estar presentes para cada parte. Ao interpretar os nomes simples ([inferência](expressions.md#type-inference)) dentro de uma parte, apenas o `using` diretivas das ções do namespace delimitador essa parte são consideradas. Isso pode resultar no mesmo identificador de ter diferentes significados em partes diferentes:
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>Membros de classe

Os membros de uma classe consistem em membros introduzidos por seus *class_member_declaration*s e os membros herdados da classe base direta.

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

Os membros de um tipo de classe são divididos nas seguintes categorias:

*  Constantes, que representam valores constantes associados com a classe ([constantes](classes.md#constants)).
*  Campos, que são as variáveis da classe ([campos](classes.md#fields)).
*  Métodos que implementam o cálculos e ações que podem ser executadas pela classe ([métodos](classes.md#methods)).
*  Propriedades que definem as características de chamada e as ações associadas à leitura e gravação a essas características ([propriedades](classes.md#properties)).
*  Eventos, que definem as notificações que podem ser geradas pela classe ([eventos](classes.md#events)).
*  Indexadores, que permitem que instâncias da classe a serem indexados da mesma forma (sintaticamente) como matrizes ([indexadores](classes.md#indexers)).
*  Operadores, que definem os operadores de expressão que podem ser aplicados a instâncias da classe ([operadores](classes.md#operators)).
*  Construtores, que implementam as ações necessárias para inicializar instâncias da classe de instância ([construtores de instância](classes.md#instance-constructors))
*  Destruidores, que implementam as ações a serem executadas antes de instâncias da classe serem descartadas permanentemente ([destruidores](classes.md#destructors)).
*  Construtores estáticos, que implementam as ações necessárias para inicializar a classe em si ([construtores estáticos](classes.md#static-constructors)).
*  Tipos que representam os tipos que são locais para a classe ([tipos aninhados](classes.md#nested-types)).

Os membros que podem conter código executável são conhecidos coletivamente como o *membros de função* do tipo de classe. Os membros da função de um tipo de classe são os métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos desse tipo de classe.

Um *class_declaration* cria um novo espaço de declaração ([declarações](basic-concepts.md#declarations)) e o *class_member_declaration*s imediatamente contido pelo *classe _declaration* introduzir novos membros nesse espaço de declaração. As seguintes regras se aplicam a *class_member_declaration*s:

*  Construtores de instância, destruidores e construtores estáticos devem ter o mesmo nome que a classe delimitadora. Todos os outros membros devem ter nomes que diferem do nome da classe delimitadora imediatamente.
*  O nome de uma constante, campo, propriedade, evento ou tipo deve ser diferente dos nomes de todos os outros membros declarados na mesma classe.
*  O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe. Além disso, a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe não podem ter assinaturas que diferem apenas por `ref` e `out`.
*  A assinatura de um construtor de instância deve ser diferente das assinaturas de todos os outros construtores de instância declarados na mesma classe e dois construtores declarados na mesma classe não podem ter assinaturas que diferem somente por `ref` e `out`.
*  A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.
*  A assinatura de um operador deve ser diferente das assinaturas de todos os outros operadores declarados na mesma classe.

Os membros herdados de um tipo de classe ([herança](classes.md#inheritance)) não fazem parte do espaço da declaração de uma classe. Portanto, uma classe derivada tem permissão para declarar um membro com o mesmo nome ou assinatura como um membro herdado (que na verdade oculta o membro herdado).

### <a name="the-instance-type"></a>O tipo de instância

Cada declaração de classe tem um tipo de limite associado ([associado e não associados a tipos](types.md#bound-and-unbound-types)), o ***tipo de instância***. Para uma declaração de classe genérica, o tipo de instância é formado pela criação de um tipo construído ([construído tipos](types.md#constructed-types)) da declaração de tipo, com cada um do tipo fornecido argumentos que são correspondentes parâmetro de tipo. Como o tipo de instância usa os parâmetros de tipo, ele só pode ser usado em que os parâmetros de tipo estão no escopo; ou seja, dentro da declaração de classe. O tipo de instância é o tipo de `this` para código escrito na declaração de classe. Para classes não genéricas, o tipo de instância é simplesmente a classe declarada. O exemplo a seguir mostra várias declarações de classe, juntamente com seus tipos de instância: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Membros de tipos construídos

Os membros não herdado de um tipo construído são obtidos pela substituição, para cada *type_parameter* na declaração de membro, correspondente *type_argument* do tipo construído. O processo de substituição se baseia o significado semântico de declarações de tipo e não é substituição simplesmente textual.

Por exemplo, dada a declaração de classe genérica
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
o tipo construído `Gen<int[],IComparable<string>>` tem os seguintes membros:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

O tipo do membro `a` na declaração de classe genérica `Gen` é "matriz bidimensional de `T`", de modo que o tipo do membro `a` no tipo construído acima é "matriz bidimensional de matriz unidimensional de `int`", ou `int[,][]`.

Dentro de membros da função de instância, o tipo de `this` é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da declaração do recipiente.

Todos os membros de uma classe genérica podem usar parâmetros de tipo de qualquer classe delimitadora, diretamente ou como parte de um tipo construído. Quando um determinado fechada tipo construído ([aberto e fechado tipos](types.md#open-and-closed-types)) é usado em tempo de execução, cada uso de um parâmetro de tipo é substituído com o argumento de tipo real fornecido para o tipo construído. Por exemplo:
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>Herança

Uma classe ***herda*** os membros de seu tipo de classe base direta. Herança significa que uma classe contém implicitamente todos os membros de seu tipo de classe base direta, exceto para os construtores de instância, destruidores e construtores estáticos da classe base. Alguns aspectos importantes de herança são:

*  A herança é transitiva. Se `C` deriva `B`, e `B` é derivado de `A`, em seguida, `C` herda os membros declarados na `B` , bem como os membros declarados na `A`.
*  Uma classe derivada estende sua classe base direta. Uma classe derivada pode adicionar novos membros aos que ela herda, mas ela não pode remover a definição de um membro herdado.
*  Construtores de instância, destruidores e construtores estáticos não são herdados, mas são todos os outros membros, independentemente de sua acessibilidade declarada ([acesso de membro](basic-concepts.md#member-access)). No entanto, dependendo de sua acessibilidade declarada, os membros herdados podem não estar acessíveis em uma classe derivada.
*  Uma classe derivada pode ***ocultar*** ([ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance)) membros herdados, declarando novos membros com o mesmo nome ou assinatura. Observe entretanto que ocultar um membro herdado não remove esse membro — ele apenas tornará esse membro inacessível diretamente por meio da classe derivada.
*  Uma instância de uma classe contém um conjunto de todos os campos de instância declarada na classe e suas classes base e uma conversão implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe a partir de um tipo de classe derivada para qualquer um dos seus tipos de classe base. Portanto, uma referência a uma instância de uma classe derivada pode ser tratada como uma referência a uma instância de qualquer uma das suas classes base.
*  Uma classe pode declarar indexadores, propriedades e métodos virtuais, e as classes derivadas podem substituir a implementação desses membros de função. Isso permite que classes apresentam comportamento polimórfico, no qual as ações executadas por uma invocação de membro de função varia de acordo com o tipo de tempo de execução da instância por meio do qual esse membro da função é invocado.

O membro herdado de um tipo de classe construída são os membros do tipo de classe base imediata ([classes Base](classes.md#base-classes)), que é encontrado, substituindo os argumentos de tipo do tipo construído para cada ocorrência do tipo correspondente parâmetros de *class_base* especificação. Esses membros, por sua vez, são transformados pelo substituindo, para cada *type_parameter* na declaração de membro, correspondente *type_argument* dos *class_base* especificação.

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

No exemplo acima, o tipo construído `D<int>` tem um membro não herdado `public int G(string s)` obtidas substituindo o argumento de tipo `int` para o parâmetro de tipo `T`. `D<int>` também tem um membro herdado da declaração de classe `B`. Esse membro herdado é determinado pelo primeiro determinando o tipo de classe base `B<int[]>` dos `D<int>` substituindo `int` para `T` na especificação de classe base `B<T[]>`. Em seguida, como um argumento de tipo para `B`, `int[]` substituirá `U` na `public U F(long index)`, produzindo o membro herdado `public int[] F(long index)`.

### <a name="the-new-modifier"></a>O modificador new

Um *class_member_declaration* tem permissão para declarar um membro com o mesmo nome ou assinatura como um membro herdado. Quando isso ocorrer, o membro da classe derivada é considerado ***ocultar*** membro da classe base. Ocultar um membro herdado não é considerado um erro, mas ele faz com que o compilador emita um aviso. Para suprimir o aviso, a declaração do membro da classe derivada pode incluir um `new` modificador para indicar que o membro derivado destina-se para ocultar o membro base. Este tópico é discutido mais detalhadamente em [ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance).

Se um `new` modificador é incluído em uma declaração que não oculta um membro herdado, um aviso para esse efeito é emitido. Esse aviso é suprimido, removendo o `new` modificador.

### <a name="access-modifiers"></a>Modificadores de acesso

Um *class_member_declaration* pode ter qualquer um dos cinco tipos possíveis de acessibilidade declarada ([declarado acessibilidade](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , ou `private`. Exceto para o `protected internal` combinação, é um erro de tempo de compilação para especificar mais de um modificador de acesso. Quando um *class_member_declaration* não inclui nenhum modificador de acesso, `private` será assumido.

### <a name="constituent-types"></a>Tipos constituintes

Tipos que são usados na declaração de membro são chamados os tipos de membros daquele membro. Os tipos possíveis constituintes são o tipo de uma constante, campo, propriedade, indexador ou evento, o tipo de retorno de um método ou operador e os tipos de parâmetro de um método, indexador, operador ou Construtor de instância. Os tipos constituintes de um membro devem ser pelo menos tão acessíveis quanto o próprio membro ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Membros estáticos e de instância

Membros de uma classe são ***membros estáticos*** ou ***membros de instância***. Em termos gerais, é útil pensar em membros estáticos como pertencente a tipos de classes e membros de instância como pertencentes a objetos (instâncias de tipos de classe).

Quando uma declaração de campo, método, propriedade, evento, operador ou construtor inclui um `static` modificador, ele declara um membro estático. Além disso, uma declaração de constante ou um tipo declara implicitamente um membro estático. Membros estáticos têm as seguintes características:

*  Quando um membro estático `M` é referenciada em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, `E` deve indicar um tipo que contém `M`. É um erro de tempo de compilação para `E` para denotar uma instância.
*  Um campo estático identifica exatamente um local de armazenamento para serem compartilhados por todas as instâncias de um tipo de classe fechado. Não importa quantas instâncias de um tipo de classe fechados são criadas, há sempre apenas uma cópia de um campo estático.
*  Um membro de função estática (método, propriedade, evento, operador ou construtor) não funciona em uma instância específica e é um erro de tempo de compilação para se referir a `this` em tal um membro da função.

Quando uma declaração de campo, método, propriedade, evento, indexador, construtor ou destruidor não incluir um `static` modificador, ele declara um membro de instância. (Um membro de instância é chamado, às vezes, um membro não estático.) Membros de instância têm as seguintes características:

*  Quando um membro de instância `M` é referenciada em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, `E` preciso marcar uma instância de um tipo que contém `M`. É um erro de tempo de associação para `E` para denotar um tipo.
*  Cada instância de uma classe contém um conjunto separado de todos os campos de instância da classe.
*  Um membro de função de instância (método, propriedade, indexador, construtor de instância ou destruidor) opera em uma determinada instância da classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).

O exemplo a seguir ilustra as regras para acessar estáticas e membros de instância:
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

O `F` método mostra que em um membro da função de instância, uma *simple_name* ([nomes simples](expressions.md#simple-names)) pode ser usado para acessar membros de instância e membros estáticos. O `G` método mostra que em um membro da função estática, é um erro de tempo de compilação para acessar um membro de instância por meio de um *simple_name*. O `Main` método mostra que em um *member_access* ([acesso de membro](expressions.md#member-access)), membros de instância devem ser acessados por meio de instâncias e os membros estáticos devem ser acessados por meio de tipos.

### <a name="nested-types"></a>Tipos aninhados

Um tipo declarado dentro de uma declaração de classe ou struct é chamado de um ***tipo aninhado***. Um tipo que é declarado dentro de uma unidade de compilação ou namespace é chamado de um ***tipo não aninhado***.

No exemplo
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
classe `B` é um tipo aninhado porque é declarado na classe `A`e a classe `A` é um tipo não aninhado porque é declarado dentro de uma unidade de compilação.

#### <a name="fully-qualified-name"></a>Nome totalmente qualificado

O nome totalmente qualificado ([nomes totalmente qualificados](basic-concepts.md#fully-qualified-names)) para um tipo aninhado é `S.N` onde `S` é o nome totalmente qualificado do tipo no qual tipo `N` é declarada.

#### <a name="declared-accessibility"></a>Acessibilidade declarada

Tipos aninhados não podem ter `public` ou `internal` declarado acessibilidade e ter `internal` declarado acessibilidade por padrão. Tipos aninhados podem ter essas formas de acessibilidade declarada também, além de uma ou mais formas adicionais de acessibilidade declarada, dependendo se o tipo recipiente é uma classe ou struct:

*  Um tipo aninhado que é declarado em uma classe pode ter qualquer um dos cinco formas de acessibilidade declarada (`public`, `protected internal`, `protected`, `internal`, ou `private`) e, como outros membros de classe, o padrão é `private` declarado acessibilidade.
*  Um tipo aninhado que é declarado em um struct pode ter qualquer um dos três formas de acessibilidade declarada (`public`, `internal`, ou `private`) e, como outros membros de struct, o padrão é `private` declarado de acessibilidade.

O exemplo
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
declara uma classe particular aninhada `Node`.

#### <a name="hiding"></a>Ocultando

Um tipo aninhado pode ocultar ([ocultar nomes](basic-concepts.md#name-hiding)) um membro base. O `new` modificador é permitido em declarações de tipo aninhado, de modo que a ocultação de pode ser expressos explicitamente. O exemplo
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
mostra uma classe aninhada `M` que oculta o método `M` definidos no `Base`.

#### <a name="this-access"></a>Esse acesso

Um tipo aninhado e seu tipo recipiente não têm uma relação especial em relação ao *this_access* ([esse acesso](expressions.md#this-access)). Especificamente, `this` dentro de um tipo aninhado não pode ser usado para fazer referência a membros de instância do tipo recipiente. Em casos onde um tipo aninhado precisa acessar os membros de instância de seu tipo recipiente, o acesso pode ser fornecido, fornecendo o `this` para a instância do tipo recipiente, como um argumento de construtor para o tipo aninhado. O exemplo a seguir
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
mostra detalhes desta técnica. Uma instância do `C` cria uma instância do `Nested` e passa seu próprio `this` ao `Nested`do construtor para fornecer acesso subsequente ao `C`do membros da instância.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Acesso a membros particulares e protegidos do tipo recipiente

Um tipo aninhado tem acesso a todos os membros que são acessíveis ao seu tipo recipiente, incluindo os membros do tipo recipiente que tenham `private` e `protected` declarado de acessibilidade. O exemplo
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
mostra uma classe `C` que contém uma classe aninhada `Nested`. Dentro de `Nested`, o método `G` chama o método estático `F` definidos no `C`, e `F` privada declarou acessibilidade.

Um tipo aninhado também pode acessar membros protegidos definidos em um tipo base do seu tipo recipiente. No exemplo
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
a classe aninhada `Derived.Nested` acessa o método protegido `F` definidos no `Derived`da classe base `Base`, por chamada por meio de uma instância de `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Tipos aninhados nas classes genéricas

Uma declaração de classe genérica pode conter declarações de tipo aninhado. Os parâmetros de tipo da classe delimitadora podem ser usados dentro dos tipos aninhados. Uma declaração de tipo aninhado pode conter parâmetros de tipo adicionais que se aplicam somente ao tipo aninhado.

Cada declaração de tipo contida dentro de uma declaração de classe genérica é implicitamente uma declaração de tipo genérico. Quando escrever uma referência a um tipo aninhado dentro de um tipo genérico, tipo construído recipiente, incluindo seus argumentos de tipo deve ser nomeado. No entanto, de dentro da classe externa, o tipo aninhado pode ser usado sem qualificação; o tipo de instância da classe externa pode ser usado implicitamente ao construir o tipo aninhado. O exemplo a seguir mostra três maneiras diferentes de corretas para se referir a um tipo construído criado a partir de `Inner`; as duas primeiras são equivalentes:
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

Embora seja ruim programação estilo, um parâmetro de tipo em um tipo aninhado pode ocultar um membro ou parâmetro declarado no tipo externo do tipo:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Nomes de membro reservado

Para facilitar a c# tempo de execução implementação subjacente, para cada declaração de membro de origem é uma propriedade, evento ou indexador, a implementação deverá reservar duas assinaturas de método com base no tipo de declaração do membro, seu nome e seu tipo. Ele é um erro de tempo de compilação para um programa declarar um membro cuja assinatura coincide com uma destas opções reservadas assinaturas, mesmo se a implementação de tempo de execução subjacente não faz uso dessas reservas.

Os nomes reservados não introduzem declarações, portanto, eles não participam de pesquisa de membro. No entanto, uma declaração associada ao método reservado assinaturas participem da herança ([herança](classes.md#inheritance)) e pode ser ocultada com o `new` modificador ([o novo modificador](classes.md#the-new-modifier)).

A reserva desses nomes atende a três propósitos:

*  Para permitir que a implementação subjacente usar um identificador comum como um nome de método para obter ou definir o acesso para o recurso de linguagem c#.
*  Para permitir que outras linguagens interoperar usando um identificador comum como um nome de método para obter ou definir o acesso para o recurso de linguagem c#.
*  Para ajudar a garantir que o código-fonte aceitos por um compilador de conformidade é aceito por outro, fazendo as especificidades do membro reservado nomes consistentes em todas as implementações do c#.

A declaração de um destruidor ([destruidores](classes.md#destructors)) também faz com que uma assinatura a ser reservado ([nomes de membro reservados para os destruidores](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Nomes de membro para propriedades reservados

Para uma propriedade `P` ([Properties](classes.md#properties)) do tipo `T`, as assinaturas a seguir são reservadas:

```csharp
T get_P();
void set_P(T value);
```

Ambas as assinaturas são reservadas, mesmo se a propriedade é somente leitura ou somente gravação.

No exemplo
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
uma classe `A` define uma propriedade somente leitura `P`, assim, reservando assinaturas para `get_P` e `set_P` métodos. Uma classe `B` deriva `A` e oculta a ambas as assinaturas reservadas. O exemplo produz a saída:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Nomes de membro reservados para eventos

Para um evento `E` ([eventos](classes.md#events)) de tipo de delegado `T`, as assinaturas a seguir são reservadas:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Nomes de membro reservados para indexadores

Para um indexador ([indexadores](classes.md#indexers)) do tipo `T` com a lista de parâmetros `L`, as assinaturas a seguir são reservadas:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Ambas as assinaturas são reservadas, mesmo que o indexador é somente leitura ou somente gravação.

Além do nome do membro `Item` é reservado.

#### <a name="member-names-reserved-for-destructors"></a>Nomes de membro reservados para os destruidores

Para uma classe que contém um destruidor ([destruidores](classes.md#destructors)), a assinatura a seguir é reservada:
```csharp
void Finalize();
```

## <a name="constants"></a>Constantes

Um ***constante*** é um membro da classe que representa um valor constante: um valor que pode ser calculado em tempo de compilação. Um *constant_declaration* introduz uma ou mais constantes de um determinado tipo.

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

Um *constant_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um `new` modificador ([o novo modificador](classes.md#the-new-modifier)), e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)). Os atributos e modificadores que se aplicam a todos os membros declarados pela *constant_declaration*. Mesmo que as constantes são consideradas membros estáticos, um *constant_declaration* não exige nem permite que um `static` modificador. É um erro para o mesmo modificador aparecer várias vezes em uma declaração de constante.

O *tipo* de uma *constant_declaration* Especifica o tipo dos membros apresentados pela declaração. O tipo é seguido por uma lista de *constant_declarator*s, cada uma delas apresenta um novo membro. Um *constant_declarator* consiste em um *identificador* que nomeia o membro, seguido por um "`=`" token, seguido por um *constant_expression* ([ Expressões de constante](expressions.md#constant-expressions)) que fornece o valor do membro.

O *tipo* especificado em uma constante de declaração deve ser `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, uma *enum_type*, ou um *reference_type*. Cada *constant_expression* deve produzir um valor do tipo de destino ou de um tipo que pode ser convertido para o tipo de destino por uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).

O *tipo* de uma constante deve ser pelo menos tão acessível quanto a própria constante ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O valor de uma constante é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)).

Uma constante em si pode participar em uma *constant_expression*. Portanto, uma constante pode ser usada em qualquer construção que requer uma *constant_expression*. Exemplos de tais construções `case` rótulos `goto case` instruções, `enum` declarações de membros, atributos e outras declarações de constante.

Conforme descrito em [expressões constantes](expressions.md#constant-expressions), um *constant_expression* é uma expressão que pode ser completamente avaliada em tempo de compilação. Como o único modo para criar um valor não nulo de uma *reference_type* diferente de `string` é aplicar o `new` operador e desde o `new` operador não é permitido em um *constant_ expressão*, o único valor possível para constantes de *reference_type*s diferente `string` é `null`.

Quando um nome simbólico para um valor constante for desejado, mas quando o tipo de valor não é permitido em uma declaração de constante, ou quando o valor não pode ser calculado em tempo de compilação por um *constant_expression*, um `readonly` (de campo [Campos somente leitura](classes.md#readonly-fields)) pode ser usado em vez disso.

Uma declaração de constante que declara várias constantes é equivalente a várias declarações de constantes únicos com o mesmo atributos, modificadores e tipo. Por exemplo
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
equivale a
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

Constantes são permitidas para dependem outras constantes dentro do mesmo programa, desde que as dependências não são de natureza circular. O compilador organiza automaticamente para avaliar as declarações de constante na ordem apropriada. No exemplo
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
o compilador primeiro avalia `A.Y`, em seguida, avalia `B.Z`e finalmente avalia `A.X`, produzindo os valores `10`, `11`, e `12`. Declarações de constante podem depender de constantes de outros programas, mas essas dependências só são possíveis em uma única direção. Referindo-se ao exemplo acima, se `A` e `B` foram declarados em programas separados, seria possível `A.X` depender `B.Z`, mas `B.Z` poderia, em seguida, não simultaneamente dependem `A.Y`.

## <a name="fields"></a>Campos

Um ***campo*** é um membro que representa uma variável associada a um objeto ou classe. Um *field_declaration* apresenta um ou mais campos de um determinado tipo.

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Um *field_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um `new` modificador ([o novo modificador](classes.md#the-new-modifier)), um uma combinação válida de quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) e uma `static` modificador ([campos estáticos e de instância](classes.md#static-and-instance-fields)). Além disso, uma *field_declaration* pode incluir uma `readonly` modificador ([campos somente leitura](classes.md#readonly-fields)) ou um `volatile` modificador ([campos voláteis](classes.md#volatile-fields)), mas não ambos. Os atributos e modificadores que se aplicam a todos os membros declarados pela *field_declaration*. É um erro para o mesmo modificador aparecer várias vezes em uma declaração de campo.

O *tipo* de uma *field_declaration* Especifica o tipo dos membros apresentados pela declaração. O tipo é seguido por uma lista de *variable_declarator*s, cada uma delas apresenta um novo membro. Um *variable_declarator* consiste em um *identificador* que nomeia esse membro, opcionalmente seguido por um "`=`" token e uma *variable_initializer* ([ Inicializadores de variável](classes.md#variable-initializers)) que fornece o valor inicial desse membro.

O *tipo* de um campo deve ser pelo menos tão acessíveis quanto o próprio campo ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O valor de um campo é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)). O valor de um campo não readonly é modificado usando um *atribuição* ([operadores de atribuição](expressions.md#assignment-operators)). O valor de um campo não readonly pode ser obtida e modificados usando incremento de sufixo e operadores de decremento ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators)) e o incremento de prefixo e operadores de decremento ([prefixo operadores de incremento e decremento](expressions.md#prefix-increment-and-decrement-operators)).

Uma declaração de campo que declara vários campos é equivalente a várias declarações de campos únicos com o mesmo atributos, modificadores e tipo. Por exemplo
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
equivale a
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>Campos estáticos e de instância

Quando uma declaração de campo inclui um `static` modificador, os campos apresentados pela declaração estão ***campos estáticos***. Quando nenhum `static` modificador estiver presente, os campos apresentados pela declaração estão ***campos de instância***. Campos estáticos e campos de instância são dois dos vários tipos de variáveis ([variáveis](variables.md)) comportados pelo c# e às vezes eles são denominados ***variáveis estáticas*** e ***variáveis de instância*** , respectivamente.

Um campo estático não é parte de uma instância específica; em vez disso, ele é compartilhado entre todas as instâncias de um tipo fechado ([aberto e fechado tipos](types.md#open-and-closed-types)). Não importa quantas instâncias de um tipo de classe fechados são criadas, há sempre apenas uma cópia de um campo estático para o domínio de aplicativo associado.

Por exemplo:
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

Um campo de instância pertence a uma instância. Especificamente, cada instância de uma classe contém um conjunto separado de todos os campos de instância dessa classe.

Quando um campo é referenciado em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é um campo estático, `E` deve indicar um tipo que contém `M` e se `M` é um campo de instância, E preciso marcar uma instância de um tipo que contém `M`.

As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Campos somente leitura

Quando um *field_declaration* inclui um `readonly` modificador, os campos apresentados pela declaração são ***campos somente leitura***. As atribuições diretas aos campos somente leitura só podem ocorrer como parte da declaração ou em um construtor de instância ou o construtor estático na mesma classe. (Um campo somente leitura pode ser atribuído a várias vezes nesses contextos.) Especificamente, as atribuições diretas para um `readonly` campo são permitidos apenas nos seguintes contextos:

*  No *variable_declarator* que apresenta o campo (incluindo um *variable_initializer* na declaração).
*  Para um campo de instância, nos construtores de instância da classe que contém a declaração de campo; para um campo estático, no construtor estático da classe que contém a declaração do campo. Eles também são os únicos contextos em que ele é válido para passar uma `readonly` campo como um `out` ou `ref` parâmetro.

A tentativa de atribuir a um `readonly` campo ou passá-lo como um `out` ou `ref` parâmetro em qualquer outro contexto é um erro de tempo de compilação.

#### <a name="using-static-readonly-fields-for-constants"></a>Usando campos somente leitura estático para constantes

Um `static readonly` campo é útil quando um nome simbólico para um valor constante é desejado, mas quando o tipo do valor não é permitido em um `const` declaração, ou quando o valor não pode ser calculado em tempo de compilação. No exemplo
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
o `Black`, `White`, `Red`, `Green`, e `Blue` membros não podem ser declarados como `const` membros porque seus valores não podem ser computados em tempo de compilação. No entanto, declará-las `static readonly` em vez disso, tem muito o mesmo efeito.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Controle de versão de campos somente leitura estático e constantes

Constantes e campos somente leitura têm semântica de controle de versão de binários diferentes. Quando uma expressão fizer referência a uma constante, o valor da constante é obtido em tempo de compilação, mas quando uma expressão fizer referência a um campo somente leitura, o valor do campo não é obtido até o tempo de execução. Considere um aplicativo que consiste em dois programas separados:
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

O `Program1` e `Program2` namespaces denotam dois programas que são compilados separadamente. Porque `Program1.Utils.X` é declarado como um campo somente leitura estático, a saída de valor a `Console.WriteLine` instrução não é conhecida no tempo de compilação, mas em vez disso, é obtida em tempo de execução. Portanto, se o valor de `X` é alterada e `Program1` é recompilado, o `Console.WriteLine` instrução produzirá o novo valor, mesmo se `Program2` não é recompilado. No entanto, tinha `X` foi uma constante, o valor da `X` seria obtido no momento `Program2` foi compilado e não seriam afetadas pelas alterações nos `Program1` até que `Program2` é recompilado.

### <a name="volatile-fields"></a>Campos voláteis

Quando um *field_declaration* inclui um `volatile` são do modificador, os campos apresentados pela declaração ***campos voláteis***.

Para campos de não-volátil, técnicas de otimização que reorganizar as instruções podem levar a resultados inesperados e imprevisíveis em programas multithread que acessam campos sem sincronização como a fornecida pelo *lock_statement*  ([a instrução lock](statements.md#the-lock-statement)). Essas otimizações podem ser executadas pelo compilador, o sistema de tempo de execução ou hardware. Para os campos voláteis, essas otimizações de reordenação são restritas:

*  Uma leitura de um campo volátil é chamada um ***volátil de leitura***. Uma leitura volátil tem "semântica de aquisição"; ou seja, é garantido que ele ocorrer antes de todas as referências a memória que ocorrem depois na sequência da instrução.
*  Uma gravação de um campo volátil é chamada de um ***escrita volátil***. Uma escrita volátil tem "versão semântica"; ou seja, é garantido que ele ocorrer após todas as referências de memória antes da instrução de gravação na sequência da instrução.

Essas restrições garantem que todos os threads observem as gravações voláteis executadas por qualquer outro thread na ordem em que elas foram realizadas. Uma implementação em conformidade não é necessário para fornecer uma ordenação total único de gravações voláteis como visto no todos os threads de execução. O tipo de um campo volátil deve ser um destes procedimentos:

*  Um *reference_type*.
*  O tipo `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, ou` System.UIntPtr`.
*  Uma *enum_type* tem um tipo de base de enumeração de `byte`, `sbyte`, `short`, `ushort`, `int`, ou `uint`.

O exemplo
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
produz a saída:
```
result = 143
```

Neste exemplo, o método `Main` inicia um novo thread que executa o método `Thread2`. Este método armazena um valor em um campo não volátil denominado `result`, em seguida, armazena `true` no campo volátil `finished`. O thread principal aguarda o campo `finished` seja definida como `true`, em seguida, lê o campo `result`. Uma vez que `finished` tiver sido declarado `volatile`, o thread principal deve ler o valor `143` do campo `result`. Se o campo `finished` não tivessem sido declarados `volatile`, será permitido para o repositório para `result` fique visível para o thread principal após o armazenamento para `finished`e, portanto, para o thread principal ler o valor `0` do campo `result`. Declarando `finished` como um `volatile` campo impede que qualquer inconsistência tal.

### <a name="field-initialization"></a>Inicialização do campo

O valor inicial de um campo, seja em um campo estático ou um campo de instância, é o valor padrão ([valores padrão](variables.md#default-values)) do tipo do campo. Não é possível observar o valor de um campo antes dessa inicialização padrão tenha ocorrido e um campo, portanto, nunca é "não inicializado". O exemplo
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
produz a saída
```
b = False, i = 0
```
porque `b` e `i` são ambos inicializadas automaticamente para valores padrão.

### <a name="variable-initializers"></a>Inicializadores de variável

Declarações de campo podem incluir *variable_initializer*s. Para campos estáticos, inicializadores de variável correspondem às instruções de atribuição que são executadas durante a inicialização de classe. Por exemplo, campos de inicializadores de variável correspondem às instruções de atribuição que são executadas quando uma instância da classe é criada.

O exemplo
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
produz a saída
```
x = 1.4142135623731, i = 100, s = Hello
```
porque uma atribuição para `x` ocorre quando executam os inicializadores de campo estático e as atribuições a `i` e `s` ocorrer ao executar os inicializadores de campo de instância.

A inicialização de valor padrão descrita em [inicialização do campo](classes.md#field-initialization) ocorre para todos os campos, incluindo os campos que têm inicializadores de variável. Assim, quando uma classe é inicializada, todos os campos estáticos nessa classe são inicializados pela primeira vez para seus valores padrão e, em seguida, os inicializadores de campo estático são executados na ordem textual. Da mesma forma, quando uma instância de uma classe é criada, todos os campos de instância nessa instância são inicializados pela primeira vez para seus valores padrão e, em seguida, os inicializadores de campo de instância são executados na ordem textual.

É possível para campos estáticos com inicializadores de variável a ser observado em seu estado de valor padrão. No entanto, isso é altamente desaconselhável como uma questão de estilo. O exemplo
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
exibir esse comportamento. Apesar das definições circulares de um e b, o programa é válido. Isso resulta na saída
```
a = 1, b = 2
```
porque os campos estáticos `a` e `b` são inicializadas como `0` (o valor padrão para `int`) antes de seus inicializadores são executados. Quando o inicializador para `a` é executado, o valor da `b` for zero e, portanto `a` é inicializado como `1`. Quando o inicializador para `b` é executado, o valor da `a` já está `1`e, portanto `b` é inicializado como `2`.

#### <a name="static-field-initialization"></a>Inicialização do campo estático

Os inicializadores de variável de campo estático de uma classe correspondem a uma sequência de atribuições que são executados na ordem textual em que aparecem na declaração da classe. Se um construtor estático ([construtores estáticos](classes.md#static-constructors)) existe na classe, a execução de inicializadores de campo estático ocorre imediatamente antes de executar esse construtor estático. Caso contrário, os inicializadores de campo estático são executados em um momento dependente da implementação antes do primeiro uso de um campo estático da classe. O exemplo
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
pode produzir a saída:
```
Init A
Init B
1 1
```
ou a saída:
```
Init B
Init A
1 1
```
porque a execução de `X`do inicializador e `Y`do inicializador pode ocorrer em qualquer ordem; eles apenas são restritos a ocorrer antes das referências a esses campos. No entanto, no exemplo:
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
a saída deve ser:
```
Init B
Init A
1 1
```
porque as regras para quando executam os construtores estáticos (conforme definido em [construtores estáticos](classes.md#static-constructors)) fornecê-lo `B`do construtor estático (e, portanto, `B`do inicializadores de campo estático) deve ser executado antes `A`do construtor estático e inicializadores de campo.

#### <a name="instance-field-initialization"></a>Inicialização do campo de instância

Os inicializadores de variável de campo de instância de uma classe correspondem a uma sequência de atribuições que são executados imediatamente após a entrada para qualquer um dos construtores de instância ([inicializadores de construtor](classes.md#constructor-initializers)) dessa classe. Os inicializadores de variável são executados na ordem textual em que aparecem na declaração da classe. O processo de criação e a inicialização da instância classe é descrito posteriormente em [construtores de instância](classes.md#instance-constructors).

Um inicializador de variável para um campo de instância não pode referenciar a instância que está sendo criada. Portanto, é um erro de tempo de compilação para fazer referência a `this` em um inicializador de variável, pois ele é um erro de tempo de compilação para um inicializador de variável fazer referência a qualquer membro de instância por meio de um *simple_name*. No exemplo
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
o inicializador de variável de `y` resulta em um erro de tempo de compilação, pois ela faz referência a um membro da instância que está sendo criado.

## <a name="methods"></a>Métodos

Um ***método*** é um membro que implementa um cálculo ou uma ação que pode ser executada por um objeto ou classe. Os métodos são declarados usando *method_declaration*s:

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Um *method_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos e de instância](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([Substituir métodos](classes.md#override-methods)), `sealed` ([lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([Métodos externos](classes.md#external-methods)) modificadores.

Uma declaração tem uma combinação válida dos modificadores se todos os itens a seguir forem verdadeiras:

*  A declaração inclui uma combinação válida de modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)).
*  A declaração não inclui o mesmo modificador várias vezes.
*  A declaração inclui no máximo um dos modificadores a seguir: `static`, `virtual`, e `override`.
*  A declaração inclui no máximo um dos modificadores a seguir: `new` e `override`.
*  Se a declaração inclui o `abstract` modificador e, em seguida, a declaração não inclui nenhum dos modificadores a seguir: `static`, `virtual`, `sealed` ou `extern`.
*  Se a declaração inclui o `private` modificador e, em seguida, a declaração não inclui nenhum dos modificadores a seguir: `virtual`, `override`, ou `abstract`.
*  Se a declaração inclui o `sealed` modificador e, em seguida, a declaração também inclui o `override` modificador.
*  Se a declaração inclui o `partial` modificador, em seguida, ele não inclui nenhum dos modificadores a seguir: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, ou `extern`.

Um método que tem o `async` modificador é uma função assíncrona e segue as regras descritas em [funções assíncronas](classes.md#async-functions).

O *return_type* de um método de declaração especifica o tipo de valor calculado e retornado pelo método. O *return_type* é `void` se o método não retorna um valor. Se a declaração inclui o `partial` modificador e, em seguida, o tipo de retorno deve ser `void`.

O *member_name* Especifica o nome do método. A menos que o método é uma implementação de membro de interface explícita ([implementações de membros de interface explícita](interfaces.md#explicit-interface-member-implementations)), o *member_name* é simplesmente uma *identificador*. Para uma implementação de membro de interface explícita, o *member_name* consiste em um *interface_type* seguido por um "`.`" e uma *identificador*.

Opcional *type_parameter_list* Especifica os parâmetros de tipo do método ([parâmetros de tipo](classes.md#type-parameters)). Se um *type_parameter_list* for especificado o método é um ***método genérico***. Se o método tem um `extern` modificador, uma *type_parameter_list* não pode ser especificado.

Opcional *formal_parameter_list* Especifica os parâmetros do método ([parâmetros de método](classes.md#method-parameters)).

Opcional *type_parameter_constraints_clause*s especificar restrições em parâmetros de tipo individuais ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) e só pode ser especificada se uma *type_parameter_ lista* também for fornecido, e o método não tem um `override` modificador.

O *return_type* e cada um dos tipos referenciados nas *formal_parameter_list* de um método deve ser pelo menos tão acessíveis quanto o próprio método ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O *method_body* é um vírgula, um ***corpo da instrução*** ou uma ***corpo da expressão***. Um corpo de declaração consiste em uma *bloco*, que especifica as instruções para executar quando o método é invocado. Consiste em um corpo de expressão `=>` seguido por um *expressão* e um ponto e vírgula e denota uma única expressão para executar quando o método é invocado. 

Para `abstract` e `extern` métodos, o *method_body* consiste simplesmente em um ponto e vírgula. Para `partial` métodos de *method_body* pode consistir em um ponto e vírgula, um corpo do bloco ou um corpo de expressão. Para todos os outros métodos, o *method_body* é um corpo do bloco ou um corpo de expressão.

Se o *method_body* consiste em um ponto e vírgula, em seguida, a declaração não pode incluir o `async` modificador.

O nome, a lista de parâmetros de tipo e a lista de parâmetros formais de um método de definem a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do método. Especificamente, a assinatura de um método consiste em seu nome, o número de parâmetros de tipo e o número, modificadores e tipos de seus parâmetros formais. Para esses fins, qualquer parâmetro de tipo do método que ocorre no tipo de um parâmetro formal é identificado, não por seu nome, mas, por sua posição ordinal na lista de argumentos de tipo do método. O tipo de retorno não é parte da assinatura do método, nem é os nomes dos parâmetros de tipo ou os parâmetros formais.

O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe. Além disso, a assinatura de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe não podem ter assinaturas que diferem somente por `ref` e `out`.

O método *type_parameter*s estão no escopo em todo o *method_declaration*e pode ser usado para tipos de formulário em todo o escopo no *return_type*, *method_body*, e *type_parameter_constraints_clause*s, mas não no *atributos*.

Todos os parâmetros de tipo e parâmetros formais devem ter nomes diferentes.

### <a name="method-parameters"></a>Parâmetros de método

Os parâmetros de um método, se houver, são declarados pelo método *formal_parameter_list*.

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

A lista de parâmetros formal consiste em um ou mais parâmetros separados por vírgulas dos quais apenas a última pode ser um *parameter_array*.

Um *fixed_parameter* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um recurso opcional `ref`, `out` ou `this` modificador, um *tipo*, um *identificador* e um opcional *default_argument*. Cada *fixed_parameter* declara um parâmetro do tipo especificado com o nome fornecido. O `this` modificador designa o método como um método de extensão e só é permitido no primeiro parâmetro de um método estático. Métodos de extensão são descritos na [métodos de extensão](classes.md#extension-methods).

Um *fixed_parameter* com um *default_argument* é conhecido como um ***parâmetro opcional***, enquanto uma *fixed_parameter* sem um *default_argument* é um ***parâmetro necessário***. Um parâmetro obrigatório não pode aparecer após um parâmetro opcional em uma *formal_parameter_list*.

Um `ref` ou `out` parâmetro não pode ter um *default_argument*. O *expressão* em um *default_argument* deve ser um dos seguintes:

*  um *constant_expression*
*  uma expressão do formulário `new S()` onde `S` é um tipo de valor
*  uma expressão do formulário `default(S)` onde `S` é um tipo de valor

O *expressão* deve ser implicitamente conversível por uma identidade ou uma conversão que permitem valor nulo para o tipo do parâmetro.

Se os parâmetros opcionais ocorrem em uma declaração de método parcial de implementação ([métodos parciais](classes.md#partial-methods)), uma implementação de membro de interface explícita ([implementações de membros de interface explícita](interfaces.md#explicit-interface-member-implementations)) ou em um declaração de indexador único parâmetro ([indexadores](classes.md#indexers)) o compilador deve fornecer um aviso, pois esses membros nunca podem ser invocados de forma que permite que os argumentos a serem omitidos.

Um *parameter_array* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um `params` modificador, um *array_type*, e um *identificador*. Uma matriz de parâmetros declara um único parâmetro do tipo matriz fornecida com o nome fornecido. O *array_type* de um parâmetro de matriz deve ser um tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)). Em uma invocação de método, uma matriz de parâmetro permite que um único argumento do tipo de matriz especificada a ser especificado ou permite a zero ou mais argumentos do tipo de elemento de matriz sejam especificados. Matrizes de parâmetros são descritos mais detalhadamente em [matrizes de parâmetro](classes.md#parameter-arrays).

Um *parameter_array* pode ocorrer após um parâmetro opcional, mas não pode ter um valor padrão – a omissão de argumentos para um *parameter_array* resultaria em vez disso, a criação de uma matriz vazia.

O exemplo a seguir ilustra os diferentes tipos de parâmetros:
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

No *formal_parameter_list* para `M`, `i` é um parâmetro ref necessária, `d` é um parâmetro de valor necessário `b`, `s`, `o` e `t` parâmetros de valor opcional e `a` é uma matriz de parâmetros.

Uma declaração de método cria um espaço de declaração separada para parâmetros, parâmetros de tipo e variáveis locais. Nomes são apresentados nesse espaço de declaração, a lista de parâmetros de tipo e a lista de parâmetros formais do método e por declarações de variável local na *bloco* do método. É um erro para dois membros de um espaço de declaração de método ter o mesmo nome. É um erro para o espaço de declaração de método e o espaço de declaração de variável local de um espaço de declaração aninhadas para conter elementos com o mesmo nome.

Uma invocação de método ([invocações de método](expressions.md#method-invocations)) cria uma cópia específica para essa invocação, de parâmetros formais e variáveis locais do método e lista de argumentos de invocação atribui valores ou referências de variável para o recém-criados parâmetros formais. Dentro de *bloco* de um método, os parâmetros formais podem ser referenciados por seus identificadores na *simple_name* expressões ([nomes simples](expressions.md#simple-names)).

Há quatro tipos de parâmetros formais:

*  Parâmetros de valor, que são declarados sem qualquer modificador.
*  Fazer referência a parâmetros, que são declarados com o `ref` modificador.
*  Parâmetros de saída, que são declarados com o `out` modificador.
*  Matrizes de parâmetros que são declarados com o `params` modificador.

Conforme descrito em [assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading), o `ref` e `out` modificadores fazem parte da assinatura do método, mas o `params` modificador não é.

#### <a name="value-parameters"></a>Parâmetros de valor

Um parâmetro declarado com nenhum modificador é um parâmetro de valor. Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento correspondente fornecido na invocação de método.

Quando um parâmetro formal é um parâmetro de valor, o argumento correspondente em uma invocação de método deve ser uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de parâmetro formal.

Um método tem permissão para atribuir novos valores para um parâmetro de valor. Essas atribuições afetam apenas o local de armazenamento local representado pelo parâmetro de valor — eles não têm efeito sobre o argumento real fornecido na invocação do método.

#### <a name="reference-parameters"></a>Parâmetros de referência

Um parâmetro declarado com um `ref` modificador é um parâmetro de referência. Ao contrário de um parâmetro de valor, um parâmetro de referência não cria um novo local de armazenamento. Em vez disso, um parâmetro de referência representa o mesmo local de armazenamento como a variável fornecido como o argumento na invocação de método.

Quando um parâmetro formal é um parâmetro de referência, o argumento correspondente em uma invocação de método deve conter a palavra-chave `ref` seguido por um *variable_reference* ([precisas regras para determinar atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo tipo que o parâmetro formal. Uma variável deve ser definitivamente atribuída antes que ele pode ser passado como um parâmetro de referência.

Dentro de um método, um parâmetro de referência é sempre considerado atribuído definitivamente.

Um método declarado como um iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de referência.

O exemplo
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
produz a saída
```
i = 2, j = 1
```

Para a invocação dos `Swap` na `Main`, `x` representa `i` e `y` representa `j`. Portanto, a invocação tem o efeito de troca os valores das `i` e `j`.

Em um método que usa parâmetros de referência, é possível que vários nomes representar o mesmo local de armazenamento. No exemplo
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
a invocação de `F` na `G` passa uma referência a `s` para ambos `a` e `b`. Assim, para essa invocação, os nomes `s`, `a`, e `b` todas se referem ao mesmo local de armazenamento e as todas as atribuições de três modificar o campo de instância `s`.

#### <a name="output-parameters"></a>Parâmetros de saída

Um parâmetro declarado com um `out` modificador é um parâmetro de saída. Semelhante a um parâmetro de referência, um parâmetro de saída não cria um novo local de armazenamento. Em vez disso, um parâmetro de saída representa o mesmo local de armazenamento como a variável fornecido como o argumento na invocação de método.

Quando um parâmetro formal é um parâmetro de saída, o argumento correspondente em uma invocação de método deve conter a palavra-chave `out` seguido por um *variable_reference* ([precisas regras para determinar atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo tipo que o parâmetro formal. Uma variável não precisa ser definitivamente atribuída antes que ela pode ser passada como um parâmetro output, mas após uma invocação em que uma variável foi passada como um parâmetro de saída, a variável é considerada atribuída definitivamente.

Dentro de um método, assim como um local variável, um parâmetro de saída é inicialmente considerado não atribuídos e deve ser definitivamente atribuído antes que seu valor é usado.

Cada parâmetro de saída de um método deve ser definitivamente atribuído antes que o método retorna.

Um método declarado como um método parcial ([métodos parciais](classes.md#partial-methods)) ou um iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de saída.

Parâmetros de saída normalmente são usados em métodos que produzem vários valores de retorno. Por exemplo:
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

O exemplo produz a saída:
```
c:\Windows\System\
hello.txt
```

Observe que o `dir` e `name` variáveis podem ser atribuídas antes de serem passados para `SplitPath`, e eles são considerados definitivamente atribuídos após a chamada.

#### <a name="parameter-arrays"></a>Matrizes de parâmetros

Um parâmetro declarado com um `params` modificador é uma matriz de parâmetros. Se uma lista de parâmetros formal inclui uma matriz de parâmetros, ele deve ser o último parâmetro na lista e ele deve ser de um tipo de matriz unidimensional. Por exemplo, os tipos `string[]` e `string[][]` pode ser usado como o tipo de uma matriz de parâmetros, mas o tipo `string[,]` não pode. Não é possível combinar as `params` modificador com os modificadores `ref` e `out`.

Uma matriz de parâmetro permite que os argumentos a ser especificada em uma das duas maneiras de uma invocação de método:

*  O argumento fornecido para uma matriz de parâmetros pode ser uma única expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetro. Nesse caso, a matriz de parâmetro Age exatamente como um parâmetro de valor.
*  Como alternativa, a invocação pode especificar zero ou mais argumentos para a matriz de parâmetros, onde cada argumento é uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros. Nesse caso, a invocação cria uma instância do tipo de parâmetro de matriz com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecido e usa a instância de matriz recém-criado como o real argumento.

Exceto para permitir que um número variável de argumentos em uma invocação, uma matriz de parâmetros é precisamente equivalente a um parâmetro de valor ([parâmetros de valores](classes.md#value-parameters)) do mesmo tipo.

O exemplo
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
produz a saída
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

A primeira invocação da `F` simplesmente passa a matriz `a` como um parâmetro de valor. A segunda chamada de `F` cria automaticamente os quatro elementos `int[]` com os valores do elemento fornecido e passa esse instância como um parâmetro de valor de matriz. Da mesma forma, a terceira invocação de `F` cria um elemento de zero `int[]` e passa essa instância como um parâmetro de valor. As invocações de segunda e terceira são precisamente equivalentes a gravação:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Ao executar a resolução de sobrecarga, um método com uma matriz de parâmetros pode ser aplicável em sua forma normal ou em sua forma expandida ([membro da função aplicável](expressions.md#applicable-function-member)). A forma expandida de um método está disponível somente se o formulário normal do método não é aplicável e somente se um método aplicável com a mesma assinatura que a forma expandida já não está declarado no mesmo tipo.

O exemplo
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
produz a saída
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

No exemplo, duas das formas possíveis expandidas do método com uma matriz de parâmetros já estão incluídas na classe como métodos regulares. Esses formulários expandidos, portanto, são considerados não ao executar a resolução de sobrecarga e das invocações de método primeiro e terceiro, portanto, selecione os métodos regulares. Quando uma classe declara um método com uma matriz de parâmetros, não é incomum para incluir também algumas das formas expandidas como métodos regulares. Ao fazer isso, é possível evitar a alocação de uma matriz a instância que ocorre quando uma forma expandida de um método com uma matriz de parâmetros é invocada.

Quando o tipo de uma matriz de parâmetros é `object[]`, uma ambiguidade potencial surge entre o formulário normal do método e o formulário expended para um único `object` parâmetro. O motivo para a ambiguidade é que um `object[]` é implicitamente conversível para o tipo `object`. A ambiguidade não apresenta nenhum problema, no entanto, uma vez que ele pode ser resolvido com a inserção de uma conversão se necessário.

O exemplo
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
produz a saída
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

Nas invocações primeiros e últimas do `F`, a forma normal de `F` é aplicável porque existe uma conversão implícita do tipo de argumento para o tipo de parâmetro (ambos são do tipo `object[]`). Portanto, a resolução de sobrecarga seleciona a forma normal de `F`, e o argumento é passado como um parâmetro de valor comum. Em que as invocações de segunda e terceira, a forma normal de `F` não é aplicável porque não existe de nenhuma conversão implícita do tipo de argumento para o tipo de parâmetro (tipo `object` não pode ser implicitamente convertido no tipo `object[]`). No entanto, a forma expandida do `F` é aplicável, portanto, é selecionada por resolução de sobrecarga. Como resultado, um elemento `object[]` é criado por meio da chamada, e o único elemento da matriz é inicializado com o valor do argumento fornecido (que por si só é uma referência a um `object[]`).

### <a name="static-and-instance-methods"></a>Métodos estáticos e de instância

Quando uma declaração de método inclui um `static` modificador, que o método deve ser um método estático. Quando nenhum `static` modificador estiver presente, o método deve ser um método de instância.

Um método estático não funciona em uma instância específica e é um erro de tempo de compilação para se referir a `this` em um método estático.

Um método de instância opera em uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).

Quando um método é referenciado em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é um método estático, `E` deve indicar um tipo que contém `M`e se `M` é um método de instância `E` preciso marcar uma instância de um tipo que contém `M`.

As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Métodos virtuais

Quando uma declaração de método de instância inclui um `virtual` modificador, que o método deve ser um método virtual. Quando nenhum `virtual` modificador estiver presente, o método deve ser um método não virtual.

A implementação de um método não virtual é invariável: A implementação é o mesmo se o método é invocado em uma instância da classe na qual ele é declarado ou uma instância de uma classe derivada. Por outro lado, a implementação de um método virtual pode ser substituída por classes derivadas. O processo de substituição a implementação de um método virtual herdado é conhecido como ***substituindo*** método ([substituir métodos](classes.md#override-methods)).

Em uma invocação de método virtual, o ***tipo de tempo de execução*** da instância para o qual usa essa invocação ocorre determina a implementação real do método para invocar. Em uma invocação de método não virtual, o ***tipo de tempo de compilação*** da instância é o fator determinante. Em termos de precisos, quando um método denominado `N` é invocado com uma lista de argumentos `A` em uma instância com um tipo de tempo de compilação `C` e um tipo de tempo de execução `R` (onde `R` seja `C` ou uma classe derivada de `C`), a invocação é processada da seguinte maneira:

*  Em primeiro lugar, a resolução de sobrecarga é aplicada a `C`, `N`, e `A`para selecionar um método específico `M` do conjunto de métodos declarados em e herdada por `C`. Isso é descrito em [invocações de método](expressions.md#method-invocations).
*  Então, se `M` é um método não virtual, `M` é invocado.
*  Caso contrário, `M` é um método virtual e a implementação mais derivada de `M` com relação ao `R` é invocado.

Para cada método virtual declarado em ou herdadas por uma classe, existe uma ***implementação derivada mais*** do método em relação a essa classe. A implementação mais derivada de um método virtual `M` em relação a uma classe `R` é determinado da seguinte maneira:

*  Se `R` contém o apresentando `virtual` declaração de `M`, em seguida, essa é a implementação mais derivada de `M`.
*  Caso contrário, se `R` contém uma `override` dos `M`, em seguida, essa é a implementação mais derivada de `M`.
*  Caso contrário, a maioria implementação derivada da `M` em relação às `R` é o mesmo que a implementação mais derivada de `M` em relação a classe base direta de `R`.

O exemplo a seguir ilustra as diferenças entre os métodos virtuais e não virtuais:
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

No exemplo, `A` apresenta um método não virtual `F` e um método virtual `G`. A classe `B` apresenta um novo método não virtual `F`, assim, ocultando a herdado `F`e também substitui o método herdado `G`. O exemplo produz a saída:
```
A.F
B.F
B.G
B.G
```

Observe que a instrução `a.G()` invoca `B.G`, e não `A.G`. Isso ocorre porque o tempo de execução de tipo da instância (que é `B`), não o tipo de tempo de compilação da instância (que é `A`), determina a implementação real do método para invocar.

Porque os métodos podem ocultar métodos herdados, é possível que uma classe conter vários métodos virtuais com a mesma assinatura. Isso não apresenta um problema de ambiguidade, desde que todos, exceto o método mais derivado estão ocultos. No exemplo
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
o `C` e `D` classes contêm dois métodos virtuais com a mesma assinatura: aquela introduzido pela `A` e um introduzido por `C`. O método introduzido por `C` oculta o método herdado de `A`. Assim, a declaração de substituição em `D` substitui o método introduzido pela `C`, e não é possível `D` substituir o método introduzido por `A`. O exemplo produz a saída:
```
B.F
B.F
D.F
D.F
```

Observe que é possível invocar o método virtual oculto acessando uma instância de `D` por meio de um menor derivado tipo no qual o método não está oculto.

### <a name="override-methods"></a>Substituir métodos

Quando uma declaração de método de instância inclui um `override` modificador, o método deve ser um ***substituir método***. Um método de substituição substitui um método virtual herdado com a mesma assinatura. Enquanto uma declaração de método virtual apresenta um novo método, uma declaração de método de substituição restringe um método virtual herdado existente fornecendo uma nova implementação do método.

O método substituído por um `override` declaração é conhecida como o ***substituída do método base***. Para um método de substituição `M` declarado em uma classe `C`, o método base substituído é determinado examinando-se cada tipo de classe base de `C`, começando com o tipo de classe base direta de `C` e continuando com cada sucessivas tipo de classe base direta, até em um tipo de classe base pelo menos um método acessível é localizado que tem a mesma assinatura que `M` após a substituição de argumentos de tipo. Para fins de localizar o método base substituído, um método é considerado acessível se estiver `public`, se ele estiver `protected`, se ele for `protected internal`, ou se ele for `internal` e declarado no mesmo programa como `C`.

Um erro de tempo de compilação ocorre, a menos que todos os itens a seguir são verdadeiras para uma declaração de substituição:

*  Um método base substituído pode estar localizado, conforme descrito acima.
*  Não há exatamente um tal método base substituído. Essa restrição tem efeito somente se o tipo de classe base é um tipo construído em que a substituição de argumentos de tipo faz a assinatura dos dois métodos o mesmo.
*  O método base substituído é um virtual, abstrato ou substituir o método. Em outras palavras, o método base substituído não pode ser estático ou não virtual.
*  O método base substituído não é um método lacrado.
*  O método de substituição e o método base substituído têm o mesmo tipo de retorno.
*  A declaração de substituição e o método base substituído têm a mesma acessibilidade declarada. Em outras palavras, uma declaração de substituição não é possível alterar a acessibilidade do método virtual. No entanto, se o método base substituído é protegido interno e é declarada em um assembly diferente do que o assembly que contém o método de substituição e em seguida, o método de substituição declarado acessibilidade deve ser protegida.
*  A declaração de substituição não especifica o tipo de parâmetro-restrições cláusulas. Em vez disso, as restrições são herdadas do método base substituído. Observe que as restrições que são parâmetros de tipo no método substituído podem ser substituídas por argumentos de tipo na restrição herdado. Isso pode levar a restrições que não são válidas quando explicitamente especificado, como tipos de valor ou tipos lacrados.

O exemplo a seguir demonstra como as regras de substituição funcionam para classes genéricas:
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

Uma declaração de substituição pode acessar o método base substituído usando um *base_access* ([acesso de Base](expressions.md#base-access)). No exemplo
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
o `base.PrintFields()` invocação `B` invoca o `PrintFields` método declarado em `A`. Um *base_access* desabilita o mecanismo de invocação virtual e simplesmente trata o método base como um método não virtual. Teve a invocação no `B` foi gravado `((A)this).PrintFields()`, seria recursivamente invocar o `PrintFields` método declarado em `B`, não aquela declarada no `A`, desde que `PrintFields` é virtual e o tipo de tempo de execução do `((A)this)` é `B`.

Apenas, incluindo um `override` can modificador um método substituem outro método. Em todos os outros casos, um método com a mesma assinatura que um método herdado simplesmente oculta o método herdado. No exemplo
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
o `F` método no `B` não inclui um `override` modificador e, portanto, não substitui o `F` método na `A`. Em vez disso, o `F` método no `B` oculta o método na `A`, e um aviso é relatado como a declaração não inclui um `new` modificador.

No exemplo
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
o `F` método no `B` oculta virtual `F` método herdado do `A`. Desde o novo `F` na `B` tem acesso privado, seu escopo inclui apenas o corpo da classe `B` e não se estende para `C`. Portanto, a declaração de `F` na `C` tem permissão para substituir o `F` herdado de `A`.

### <a name="sealed-methods"></a>Métodos lacrados

Quando uma declaração de método de instância inclui um `sealed` modificador, que o método deve ser um ***lacrados método***. Se uma declaração de método de instância inclui o `sealed` modificador, ele também deve incluir o `override` modificador. Usar o `sealed` modificador impede que uma classe derivada ainda mais, substituindo o método.

No exemplo
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
a classe `B` fornece dois métodos de substituição: uma `F` método que tem o `sealed` modificador e um `G` método que não tem. `B`uso do lacrado `modifier` impede `C` substituam ainda mais `F`.

### <a name="abstract-methods"></a>Métodos abstratos

Quando uma declaração de método de instância inclui um `abstract` modificador, que o método deve ser um ***método abstrato***. Embora um método abstrato também é implicitamente um método virtual, ele não pode ter o modificador `virtual`.

Uma declaração de método abstrato apresenta um novo método virtual, mas não fornece uma implementação desse método. Em vez disso, as classes derivadas não abstratas devem fornecer sua própria implementação, substituindo esse método. Como um método abstrato não fornece nenhuma implementação real, o *method_body* de um método abstrato consiste apenas em um ponto e vírgula.

Declarações de método abstrato são permitidas apenas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).

No exemplo
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
o `Shape` classe define o conceito abstrato de um objeto de forma geométrica que pode pintar em si. O `Paint` método é abstrato porque não há nenhuma implementação padrão significativos. O `Ellipse` e `Box` classes são concretas `Shape` implementações. Como essas classes são não-abstrata, eles são necessários para substituir o `Paint` método e fornecer uma implementação real.

É um erro de tempo de compilação para um *base_access* ([acesso de Base](expressions.md#base-access)) para fazer referência a um método abstrato. No exemplo
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
um erro de tempo de compilação é relatado para o `base.F()` invocação porque faz referência a um método abstrato.

Uma declaração de método abstrato tem permissão para substituir um método virtual. Isso permite que uma classe abstrata forçar a nova implementação do método em classes derivadas e torna a implementação original do método não está disponível. No exemplo
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
classe `A` declara um método virtual, a classe `B` substitui esse método com um método abstrato e classe `C` substitui o método abstrato para fornecer sua própria implementação.

### <a name="external-methods"></a>Métodos externos

Quando uma declaração de método inclui um `extern` modificador, que o método deve ser um ***método externo***. Métodos externos são implementados externamente, normalmente usando um idioma diferente do c#. Como uma declaração de método externo não fornece nenhuma implementação real, o *method_body* de um método externo consiste apenas em um ponto e vírgula. Um método externo não pode ser genérico.

O `extern` modificador é normalmente usado em conjunto com um `DllImport` atributo ([interoperação com componentes COM e Win32](attributes.md#interoperation-with-com-and-win32-components)), permitindo que os métodos externos a ser implementada por DLLs (bibliotecas de vínculo dinâmico). O ambiente de execução pode dar suporte a outros mecanismos, no qual as implementações de métodos externos podem ser fornecidas.

Quando um método externo inclui um `DllImport` atributo, a declaração do método também deve incluir um `static` modificador. Este exemplo demonstra o uso do `extern` modificador e o `DllImport` atributo:
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>Métodos parciais (recapitulação)

Quando uma declaração de método inclui um `partial` modificador, que o método deve ser um ***método parcial***. Métodos parciais só podem ser declarados como membros de tipos parciais ([tipos parciais](classes.md#partial-types)) e estão sujeitos a várias restrições. Métodos parciais são descritos na [métodos parciais](classes.md#partial-methods).

### <a name="extension-methods"></a>Métodos de extensão

Quando o primeiro parâmetro de um método inclui o `this` modificador, que o método deve ser um ***método de extensão***. Métodos de extensão só podem ser declarados em classes estáticas não genérico, não aninhadas. O primeiro parâmetro de um método de extensão não pode ter nenhum modificador diferente de `this`, e o tipo de parâmetro não pode ser um tipo de ponteiro.

Este é um exemplo de uma classe estática que declara dois métodos de extensão:
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

Um método de extensão é um método estático normal. Além disso, em que sua classe estática delimitador no escopo, um método de extensão pode ser invocado usando sintaxe de invocação de método de instância ([invocações de método de extensão](expressions.md#extension-method-invocations)), usando a expressão de receptor como o primeiro argumento.

O programa a seguir usa os métodos de extensão declarados acima:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

O `Slice` método está disponível na `string[]`e o `ToInt32` método está disponível no `string`, porque eles foram declarados como métodos de extensão. O significado do programa é o mesmo que as chamadas de método estático normal seguinte, usando:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>Corpo do método

O *method_body* de um método de declaração consiste em um corpo do bloco, um corpo de expressão ou um ponto e vírgula.

O ***tipo de resultado*** de um método é `void` se for o tipo de retorno `void`, ou se o método é assíncrono e o tipo de retorno é `System.Threading.Tasks.Task`. Caso contrário, o tipo de resultado de um método não assíncrono é seu tipo de retorno e o tipo de resultado de um método assíncrono com o tipo de retorno `System.Threading.Tasks.Task<T>` é `T`.

Quando um método tem um `void` resultar de tipo e um corpo do bloco `return` instruções ([a instrução return](statements.md#the-return-statement)) no bloco não são permitidos para especificar uma expressão. Se a execução do bloco de um método void é concluída normalmente (ou seja, controlam fluxos de fora do corpo do método), que o método simplesmente retorna para seu chamador atual.
    
Quando um método tem um `void` resultado e um corpo de expressão, a expressão `E` deve ser um *statement_expression*, e o corpo é exatamente equivalente a um corpo do bloco do formulário `{ E; }`.
    
Quando um método tem um tipo de resultado não nulo e um bloco de corpo, cada `return` instrução do bloco deve especificar uma expressão que é implicitamente conversível para o tipo de resultado. O ponto de extremidade de um corpo do bloco de um método que retorna o valor não deve ser acessível. Em outras palavras, em um método de retorno de valor com um corpo do bloco, o controle não tem permissão para fluir para fora o final do corpo do método.
    
Quando um método tem um tipo de resultado não nulo e um corpo de expressão, a expressão deve ser implicitamente conversível para o tipo de resultado e o corpo é exatamente equivalente a um corpo do bloco do formulário `{ return E; }`.
    
No exemplo
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
o valor de retorno `F` método resulta em um erro de tempo de compilação porque o controle pode fluir para fora o final do corpo do método. O `G` e `H` métodos estão corretos, porque todos os possíveis caminhos de execução terminam com uma instrução return que especifica um valor de retorno. O `I` método está correto, pois seu corpo é equivalente a um bloco de instrução com apenas uma única instrução return nele.

### <a name="method-overloading"></a>Sobrecarga de método

As regras de resolução de sobrecarga de método são descritas em [inferência de tipo](expressions.md#type-inference).

## <a name="properties"></a>Propriedades

Um ***propriedade*** é um membro que fornece acesso a uma característica de um objeto ou uma classe. Exemplos de propriedades incluem o comprimento de uma cadeia de caracteres, o tamanho de uma fonte, a legenda de uma janela, o nome de um cliente e assim por diante. As propriedades são uma extensão natural dos campos — elas são denominadas membros com tipos associados e a sintaxe para acessar os campos e propriedades é o mesmo. No entanto, diferentemente dos campos, as propriedades não denotam locais de armazenamento. Em vez disso, as propriedades têm ***acessadores*** que especificam as instruções a serem executadas quando os valores forem lidos ou gravados. Propriedades, portanto, fornecem um mecanismo para associar as ações com a leitura e gravação de atributos de um objeto; Além disso, eles permitem que esses atributos a ser computado.

As propriedades são declaradas usando *property_declaration*s:

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

Um *property_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos e de instância](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([Substituir métodos](classes.md#override-methods)), `sealed` ([lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([Métodos externos](classes.md#external-methods)) modificadores.

Declarações de propriedade estão sujeitos às mesmas regras de declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.

O *tipo* de uma propriedade de declaração especifica o tipo da propriedade introduzida pela declaração e o *member_name* Especifica o nome da propriedade. A menos que a propriedade é uma implementação de membro de interface explícita, o *member_name* é simplesmente uma *identificador*. Para uma implementação de membro de interface explícita ([implementações de membros de interface explícita](interfaces.md#explicit-interface-member-implementations)), o *member_name* consiste em um *interface_type* seguido por um " `.`"e uma *identificador*.

O *tipo* de uma propriedade deve ser pelo menos tão acessível quanto a própria propriedade ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Um *property_body* qualquer um pode consistir de uma ***corpo do acessador*** ou uma ***corpo da expressão***. Em um corpo de acessador *accessor_declarations*, que deve ser colocada em "`{`"e"`}`" tokens, declare acessadores ([acessadores](classes.md#accessors)) da propriedade. Os acessadores de especificam as instruções executáveis associadas com a leitura e gravação de propriedade.

Um corpo de expressão consiste `=>` seguido por um *expressão* `E` e um ponto e vírgula é exatamente equivalente para o corpo da instrução `{ get { return E; } }`e apenas, portanto, pode ser usado para especificar apenas de getter propriedades em que o resultado do getter é determinado por uma única expressão.

Um *property_initializer* só podem ser designados para uma propriedade implementada automaticamente ([implementadas automaticamente propriedades](classes.md#automatically-implemented-properties)) e faz com que a inicialização do campo subjacente de tais as propriedades com o valor fornecido pelo *expressão*.

Embora a sintaxe para acessar uma propriedade é o mesmo que para um campo, uma propriedade não é classificada como uma variável. Portanto, não é possível passar uma propriedade como uma `ref` ou `out` argumento.

Quando uma declaração de propriedade inclui um `extern` modificador, a propriedade deve ser um ***propriedade externa***. Como uma declaração de propriedade externa não fornece nenhuma implementação real, cada um dos seus *accessor_declarations* consiste em um ponto e vírgula.

### <a name="static-and-instance-properties"></a>Propriedades estáticos e de instância

Quando uma declaração de propriedade inclui um `static` modificador, a propriedade deve ser um ***propriedade estática***. Quando nenhum `static` modificador estiver presente, a propriedade deve ser um ***propriedade da instância***.

Uma propriedade estática não está associada uma instância específica e é um erro de tempo de compilação para se referir a `this` nos acessadores de uma propriedade estática.

Uma propriedade de instância está associada uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores da propriedade.

Quando uma propriedade é referenciada em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é uma propriedade estática, `E` deve indicar um tipo que contém `M`e se `M` é uma propriedade de instância, E preciso marcar uma instância de um tipo que contém `M`.

As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="accessors"></a>Acessadores

O *accessor_declarations* de uma propriedade, especifique as instruções executáveis associadas com a leitura e gravação a essa propriedade.

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

As declarações de acessador consistem em uma *get_accessor_declaration*, um *set_accessor_declaration*, ou ambos. Cada declaração de acessador consiste o token `get` ou `set` seguido de um recurso opcional *accessor_modifier* e um *accessor_body*.

O uso de *accessor_modifier*s é regido pelos seguintes restrições:

*  Uma *accessor_modifier* não pode ser usado em uma interface ou em uma implementação de membro de interface explícita.
*  Para uma propriedade ou indexador que não tem nenhum `override` modificador, uma *accessor_modifier* é permitido somente se a propriedade ou o indexador tiver tanto um `get` e `set` acessador e, em seguida, é permitida apenas em um desses acessadores.
*  Para uma propriedade ou indexador que inclui um `override` modificador, um acessador deve corresponder a *accessor_modifier*, se houver, do acessador que está sendo substituído.
*  O *accessor_modifier* deve declarar a acessibilidade é estritamente mais restritiva que a acessibilidade declarada de propriedade ou indexador em si. Para ser preciso:
   * Se a propriedade ou indexador tem uma acessibilidade declarada de `public`, o *accessor_modifier* pode ser `protected internal`, `internal`, `protected`, ou `private`.
   * Se a propriedade ou indexador tem uma acessibilidade declarada de `protected internal`, o *accessor_modifier* pode ser `internal`, `protected`, ou `private`.
   * Se a propriedade ou indexador tem uma acessibilidade declarada de `internal` ou `protected`, o *accessor_modifier* deve ser `private`.
   * Se a propriedade ou indexador tem uma acessibilidade declarada de `private`, nenhum *accessor_modifier* pode ser usado.

Para `abstract` e `extern` propriedades, o *accessor_body* para cada acessador especificado é simplesmente um ponto e vírgula. Uma propriedade não-abstrata, não-externo pode ter cada *accessor_body* ser um ponto e vírgula, caso em que ele é um ***propriedade implementada automaticamente*** ([implementada automaticamente propriedades ](classes.md#automatically-implemented-properties)). Uma propriedade implementada automaticamente deve ter pelo menos um acessador get. Para os acessadores de qualquer outra não-abstrata, não extern propriedade, o *accessor_body* é um *bloco* que especifica as instruções a serem executadas quando o acessador correspondente é invocado.

Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade. Exceto como o destino de uma atribuição, quando uma propriedade é referenciada em uma expressão, o `get` acessador da propriedade é invocado para calcular o valor da propriedade ([valores das expressões](expressions.md#values-of-expressions)). O corpo de uma `get` acessador deve obedecer às regras para retornar o valor métodos descritos [corpo do método](classes.md#method-body). Em particular, todos os `return` instruções no corpo de um `get` acessador deve especificar uma expressão que é implicitamente conversível para o tipo de propriedade. Além disso, o ponto de extremidade de um `get` acessador não pode estar acessível.

Um `set` acessador corresponde a um método com um parâmetro de valor único do tipo de propriedade e um `void` tipo de retorno. O parâmetro implícito de um `set` acessador é sempre denominado `value`. Quando uma propriedade é referenciada como o destino de uma atribuição ([operadores de atribuição](expressions.md#assignment-operators)), ou como o operando da `++` ou `--` ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators), [ Incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)), o `set` acessador é invocado com um argumento (cujo valor é o do lado direito da atribuição ou o operando das `++` ou `--` operador) que fornece o novo valor ([atribuição simples](expressions.md#simple-assignment)). O corpo de uma `set` acessador deve estar de acordo com as regras para `void` métodos descritos [corpo do método](classes.md#method-body). Em particular, `return` instruções de `set` corpo do acessador não são permitidos para especificar uma expressão. Uma vez que um `set` acessador implicitamente tem um parâmetro chamado `value`, é um erro de tempo de compilação para uma declaração de constante ou variável local em um `set` acessador esse nome.

Com base na presença ou ausência do `get` e `set` acessadores, uma propriedade é classificado da seguinte maneira:

*  Uma propriedade que inclui tanto uma `get` acessador e um `set` acessador deve ser um ***leitura-gravação*** propriedade.
*  Uma propriedade que tem apenas um `get` acessador deve ser um ***somente leitura*** propriedade. Ele é um erro de tempo de compilação para uma propriedade somente leitura ser o destino de uma atribuição.
*  Uma propriedade que tem apenas um `set` acessador deve ser um ***somente gravação*** propriedade. Exceto como o destino de uma atribuição, ele é um erro de tempo de compilação para fazer referência a uma propriedade somente gravação em uma expressão.

No exemplo
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
o `Button` controle declara um público `Caption` propriedade. O `get` acessador do `Caption` propriedade retorna a cadeia de caracteres armazenada em particular `caption` campo. O `set` acessador verifica se o novo valor é diferente do valor atual e, nesse caso, ele armazena o novo valor e redesenha o controle. Propriedades geralmente seguem o padrão mostrado acima: O `get` acessador simplesmente retorna um valor armazenado em um campo privado e o `set` acessador modifica esse campo particular e, em seguida, executa ações adicionais necessárias para o estado de uma atualização completa do objeto.

Dada a `Button` classe acima, o seguinte é um exemplo de uso do `Caption` propriedade:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Aqui, o `set` acessador é invocado, atribuindo um valor para a propriedade e o `get` acessador é invocado por fazer referência à propriedade em uma expressão.

O `get` e `set` acessadores de uma propriedade não são membros diferentes e não é possível declarar os acessadores de uma propriedade separadamente. Como tal, não é possível que os dois acessadores de uma propriedade de leitura / gravação ter acessibilidade diferente. O exemplo
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
não declara uma única propriedade de leitura / gravação. Em vez disso, ele declara duas propriedades com o mesmo nome, um somente leitura e somente gravação. Como dois membros declarados na mesma classe não podem ter o mesmo nome, o exemplo faz com que ocorra um erro de tempo de compilação.

Quando uma classe derivada não declara uma propriedade com o mesmo nome como uma propriedade herdada, a propriedade derivada oculta a propriedade herdada com relação à leitura e gravação. No exemplo
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
o `P` propriedade na `B` oculta a `P` propriedade no `A` em relação à leitura e gravação. Dessa forma, nas instruções
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
a atribuição ao `b.P` causa um erro de tempo de compilação a ser relatado, desde o somente leitura `P` propriedade na `B` oculta a somente gravação `P` propriedade no `A`. No entanto, observe que uma conversão pode ser usada para acessar o oculto `P` propriedade.

Diferentemente dos campos públicos, propriedades fornecem uma separação entre o estado interno de um objeto e sua interface pública. Considere o exemplo:
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Aqui, o `Label` classe usa dois `int` campos, `x` e `y`, para armazenar seu local. O local é publicamente exposta como uma `X` e uma `Y` propriedade e como um `Location` propriedade do tipo `Point`. Se, em uma versão futura do `Label`, ele se torna mais conveniente armazenar o local como um `Point` internamente, a alteração pode ser feita sem afetar a interface pública da classe:
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Tinha `x` e `y` em vez disso, foram `public readonly` campos, teria sido impossível fazer uma alteração para o `Label` classe.

Expõe estado por meio das propriedades não é necessariamente menos eficiente do que expor campos diretamente. Em particular, quando uma propriedade é não virtual e contém apenas uma pequena quantidade de código, o ambiente de execução pode substituir chamadas para acessadores com o código real de acessadores. Esse processo é conhecido como ***inlining***, e torna o acesso de propriedade tão eficiente quanto o acesso de campo, mas preserva a maior flexibilidade de propriedades.

Desde a invocação de um `get` acessador é conceitualmente equivalente ao ler o valor de um campo, ele é considerado um estilo ruim de programação para `get` acessadores ter efeitos colaterais observáveis. No exemplo
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
O valor da `Next` propriedade depende do número de vezes que a propriedade foi acessada anteriormente. Assim, acessar a propriedade produz um efeito colateral observável, e a propriedade deve ser implementada como um método em vez disso.

A convenção de "sem efeitos colaterais" para `get` acessadores não significa que `get` acessadores sempre devem ser escritos para simplesmente retornar valores armazenados nos campos. Na verdade, `get` acessadores geralmente calcular o valor de uma propriedade ao acessar vários campos ou invocar métodos. No entanto, projetado corretamente `get` acessador não executará nenhuma ação que causam alterações observáveis no estado do objeto.

Propriedades podem ser usadas para atrasar a inicialização de um recurso até o momento em que ele é referenciado pela primeira vez. Por exemplo:
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

O `Console` classe contém três propriedades: `In`, `Out`, e `Error`, que representam a entrada padrão, saída e dispositivos de erro, respectivamente. Ao expor esses membros como propriedades, o `Console` classe pode atrasar sua inicialização até que eles são realmente usados. Por exemplo, após a primeira referenciando o `Out` propriedade, como em
```csharp
Console.Out.WriteLine("hello, world");
```
subjacente `TextWriter` para o dispositivo de saída é criado. Porém, se o aplicativo não faz referência à `In` e `Error` propriedades, em seguida, não há objetos são criados para esses dispositivos.

### <a name="automatically-implemented-properties"></a>Propriedades implementadas automaticamente

Uma propriedade implementada automaticamente (ou ***auto-propriedade*** de forma abreviada), é uma propriedade de não-externo não abstrata com corpos de acessador somente-e-vírgula. Propriedades automáticas devem ter um acessador get e, opcionalmente, podem ter um acessador set.

Quando uma propriedade é especificada como uma propriedade implementada automaticamente, um campo oculto existente está automaticamente disponível para a propriedade e os acessadores são implementados para ler e gravar a esse campo existente. Se a propriedade automática não possui nenhum acessador set, o campo de suporte é considerado `readonly` ([campos somente leitura](classes.md#readonly-fields)). Assim como um `readonly` campo, uma propriedade de automática apenas de getter também pode ser atribuída a no corpo de um construtor da classe delimitadora. Essa atribuição atribui diretamente para o campo de suporte da propriedade somente leitura.

Uma propriedade automática, opcionalmente, pode ter um *property_initializer*, que é aplicado diretamente ao campo de suporte como um *variable_initializer* ([inicializadores de variável](classes.md#variable-initializers)) .

O exemplo a seguir:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
é equivalente à declaração a seguir:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

O exemplo a seguir:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
é equivalente à declaração a seguir:
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

Observe que as atribuições para o campo somente leitura são legais, porque eles ocorrem dentro do construtor.


### <a name="accessibility"></a>Acessibilidade

Se tiver um acessador de um *accessor_modifier*, o domínio de acessibilidade ([domínios acessibilidade](basic-concepts.md#accessibility-domains)) do acessador é determinado usando a acessibilidade declarada do *accessor_modifier* . Se um acessador não tem um *accessor_modifier*, o domínio de acessibilidade do acessador é determinado com base a acessibilidade declarada de propriedade ou indexador.

A presença de um *accessor_modifier* nunca afeta a pesquisa de membro ([operadores](expressions.md#operators)) ou resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)). Os modificadores de propriedade ou indexador sempre determinam qual propriedade ou indexador está vinculado, independentemente do contexto do acesso.

Depois que uma determinada propriedade ou indexador tiver sido selecionado, os domínios de acessibilidade dos acessadores específicos envolvidos são usados para determinar se esse uso é válido:

*  Se o uso é como um valor ([valores de expressões](expressions.md#values-of-expressions)), o `get` acessador deve existir e estar acessível.
*  Se o uso é como o destino de uma atribuição simple ([atribuição simples](expressions.md#simple-assignment)), o `set` acessador deve existir e estar acessível.
*  Se o uso é como o destino de atribuição composta ([atribuição composta](expressions.md#compound-assignment)), ou como o destino da `++` ou `--` operadores ([membros de função](expressions.md#function-members).9, [ Expressões de invocação](expressions.md#invocation-expressions)), ambos os `get` acessadores e o `set` acessador deve existir e estar acessível.

No exemplo a seguir, a propriedade `A.Text` está oculto pela propriedade `B.Text`, mesmo em contextos em que apenas o `set` acessador é chamado. Por outro lado, a propriedade `B.Count` não é acessível a classe `M`, portanto, a propriedade acessível `A.Count` é usado em vez disso.

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

Um acessador que é usado para implementar uma interface não pode ter um *accessor_modifier*. Se apenas um acessador é usado para implementar uma interface, o outro acessador pode ser declarado com um *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Acessadores de propriedade abstrata, substituição e virtuais, selado

Um `virtual` declaração de propriedade que especifica que os acessadores da propriedade são virtuais. O `virtual` modificador se aplica a ambos os acessadores de uma propriedade de leitura / gravação — não é possível apenas um acessador de uma propriedade de leitura / gravação seja virtual.

Um `abstract` declaração de propriedade especifica que os acessadores da propriedade são virtuais, mas não fornece uma implementação real dos acessadores. Em vez disso, as classes derivadas não abstratas devem fornecer sua própria implementação para os acessadores, substituindo a propriedade. Como um acessador de uma declaração de propriedade abstrata não fornece nenhuma implementação real, sua *accessor_body* consiste apenas em um ponto e vírgula.

Uma declaração de propriedade que inclui ambos os `abstract` e `override` modificadores Especifica que a propriedade é abstrata e substitui uma propriedade base. Os acessadores de tal propriedade também são abstratos.

Declarações de propriedade abstrata são permitidas apenas em classes abstratas ([classes abstratas](classes.md#abstract-classes)). Os acessadores de uma propriedade herdada de virtual podem ser substituídos em uma classe derivada incluindo uma declaração de propriedade que especifica um `override` diretiva. Isso é conhecido como um ***substituindo a declaração de propriedade***. Uma declaração de propriedade de substituição não declara uma nova propriedade. Em vez disso, ele simplesmente é especialista as implementações de acessadores de uma propriedade virtual existente.

Uma declaração de propriedade de substituição deve especificar o exato mesmo modificadores de acessibilidade, o tipo e o nome que a propriedade herdada. Se a propriedade herdada tiver apenas um único acessador (ou seja, se a propriedade herdada for somente leitura ou somente gravação), a propriedade de substituição deve incluir somente o acessador. Se a propriedade herdada inclui ambos os acessadores (ou seja, se a propriedade herdada for leitura / gravação), a propriedade de substituição pode incluir um único acessador ou ambos os acessadores.

Uma declaração de propriedade de substituição pode incluir o `sealed` modificador. Uso desse modificador impede que uma classe derivada ainda mais substituindo a propriedade. Os acessadores de uma propriedade lacrado também são lacrados.

Exceto pelas diferenças na declaração e chamada de sintaxe, substituição sealed, virtual e acessadores abstratos se comporta exatamente como virtual, selada, substituição e métodos abstratos. Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [substituir métodos](classes.md#override-methods), [lacrados métodos](classes.md#sealed-methods), e [métodos abstratos](classes.md#abstract-methods) se aplicam como se acessadores eram os métodos de um formulário correspondente:

*  Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade e os modificadores mesmos que a propriedade recipiente.
*  Um `set` acessador corresponde a um método com um parâmetro de valor único de tipo de propriedade, um `void` retornar o tipo e os modificadores mesmos que a propriedade recipiente.

No exemplo
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` é uma propriedade somente leitura virtual, `Y` é uma propriedade de leitura / gravação virtual, e `Z` é uma propriedade de leitura / gravação abstrata. Porque `Z` é abstrato, a classe continente `A` também deve ser declarada como abstrata.

Uma classe que deriva de `A` é mostrado abaixo:
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

Aqui, as declarações das `X`, `Y`, e `Z` estão substituindo declarações de propriedade. Cada declaração de propriedade corresponde exatamente os modificadores de acessibilidade, o tipo e o nome da propriedade herdada correspondente. O `get` acessador da `X` e o `set` acessador de `Y` usam o `base` palavra-chave para acessar os acessadores herdados. A declaração de `Z` substitui os dois acessadores abstratos — portanto, não há nenhum membro de função abstract pendentes no `B`, e `B` tem permissão para ser uma classe não abstrata.

Quando uma propriedade é declarada como um `override`, qualquer acessadores substituídos devem estar acessíveis ao código de substituição. Além disso, a acessibilidade declarada da propriedade ou indexador em si e dos acessadores, deve corresponder do membro substituído e acessadores. Por exemplo:
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>Eventos

Uma ***evento*** é um membro que permite que um objeto ou classe para fornecer notificações. Os clientes podem anexar o código executável para eventos fornecendo ***manipuladores de eventos***.

Eventos são declarados usando *event_declaration*s:

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

Uma *event_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos e de instância](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([Substituir métodos](classes.md#override-methods)), `sealed` ([lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([Métodos externos](classes.md#external-methods)) modificadores.

Declarações de evento estão sujeitos às mesmas regras de declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.

O *tipo* de um evento declaração deve ser um *delegate_type* ([tipos de referência](types.md#reference-types)) e que *delegate_type* deve ter pelo menos como acessível quanto o próprio evento ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Uma declaração de evento pode incluir *event_accessor_declarations*. No entanto, se não existir, para não-externo e eventos de não-abstrata, o compilador fornece-os automaticamente ([eventos semelhantes a campo](classes.md#field-like-events)); para eventos de extern, os acessadores são fornecidos externamente.

Uma declaração de evento que omite *event_accessor_declarations* define um ou mais eventos — uma para cada um dos *variable_declarator*s. Os atributos e modificadores que se aplicam a todos os membros declarados, como uma *event_declaration*.

É um erro de tempo de compilação para um *event_declaration* de incluir ambas as `abstract` modificador e delimitado por chaves *event_accessor_declarations*.

Quando uma declaração de evento inclui um `extern` modificador, o evento deve ser um ***evento externo***. Como uma declaração de evento externo fornece nenhuma implementação real, ele é um erro para que ele inclua ambos os `extern` modificador e *event_accessor_declarations*.

É um erro de tempo de compilação para um *variable_declarator* de uma declaração de evento com um `abstract` ou `external` modificador para incluir um *variable_initializer*.

Um evento pode ser usado como o operando esquerdo do `+=` e `-=` operadores ([atribuição de evento](expressions.md#event-assignment)). Esses operadores são usados, respectivamente, para anexar manipuladores de eventos ou remover manipuladores de eventos de um evento, e os modificadores de acesso do evento de controlam os contextos em que essas operações são permitidas.

Uma vez que `+=` e `-=` são as únicas operações que são permitidas em um evento fora do tipo que declara o evento, o código externo podem adicionar e remover manipuladores para um evento, mas não é possível de qualquer outra maneira de obter ou modificar a lista subjacente do evento manipuladores.

Em uma operação do formulário `x += y` ou `x -= y`, quando `x` é um evento e a referência ocorre fora do tipo que contém a declaração de `x`, o resultado da operação tem tipo `void` (em vez de o tipo de `x`, com o valor de `x` após a atribuição). Essa regra proíbe código externo indiretamente examinando o delegado subjacente de um evento.

O exemplo a seguir mostra como os manipuladores de eventos são anexados a instâncias do `Button` classe:
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

Aqui, o `LoginDialog` construtor de instância cria dois `Button` instâncias e anexa os manipuladores de eventos para o `Click` eventos.

### <a name="field-like-events"></a>Eventos semelhantes a campo

Dentro do texto do programa de classe ou struct que contém a declaração de um evento, determinados eventos podem ser usados como campos. Para ser usado dessa forma, um evento não deve ser `abstract` ou `extern`e não deve incluir explicitamente *event_accessor_declarations*. Esse evento pode ser usado em qualquer contexto que permite que um campo. O campo contém um delegado ([delegados](delegates.md)) que se refere à lista de manipuladores de eventos que foram adicionados ao evento. Se nenhum manipulador de eventos tiver sido adicionado, o campo contém `null`.

No exemplo
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` é usado como um campo dentro de `Button` classe. Como demonstrado no exemplo, o campo pode ser examinado, modificado e usado em expressões de invocação do delegado. O `OnClick` método na `Button` classe "gera" o `Click` eventos. A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento — assim, não há constructos de linguagem especial para gerar eventos. Observe que a invocação de delegado é precedida por uma verificação que garante que o delegado for não nulo.

Fora da declaração do `Button` classe, o `Click` membro só pode ser usado no lado esquerdo das `+=` e `-=` operadores, como em
```csharp
b.Click += new EventHandler(...);
```
que anexa um delegado à lista de invocação de `Click` evento, e
```csharp
b.Click -= new EventHandler(...);
```
remover um delegado da lista de invocação do `Click` eventos.

Ao compilar um evento de campo, o compilador automaticamente cria o armazenamento para manter o delegado e cria acessadores para o evento que adicionar ou removem manipuladores de eventos para o campo de delegado. As operações de adição e remoção são thread-safe e pode (mas não é necessárias) ser feito durante a manutenção do bloqueio ([instrução lock](statements.md#the-lock-statement)) no objeto de recipiente para um evento de instância, ou o objeto de tipo ([anônimo expressões de criação do objeto](expressions.md#anonymous-object-creation-expressions)) para um evento estático.

Portanto, uma instância declaração de evento do formulário:
```csharp
class X
{
    public event D Ev;
}
```
será compilada para algo equivalente a:
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
Dentro da classe `X`, faz referência às `Ev` no lado esquerdo das `+=` e `-=` operadores fazem com que a adicionar e remover acessadores a ser invocado. Todas as outras referências a `Ev` são compilados para referenciar o campo oculto `__Ev` em vez disso ([acesso de membro](expressions.md#member-access)). O nome "`__Ev`" é arbitrário; o campo oculto pode ter qualquer nome ou sem nome em todos os.

### <a name="event-accessors"></a>Acessadores de eventos

Declarações de evento normalmente omitir *event_accessor_declarations*, como no `Button` exemplo acima. Uma situação para fazer o tema envolve o caso em que o custo de armazenamento de um campo por evento não é aceitável. Nesses casos, uma classe pode incluir *event_accessor_declarations* e usar um mecanismo privado para armazenar a lista de manipuladores de eventos.

O *event_accessor_declarations* de um evento, especifique as instruções executáveis associadas ao adicionar e remover manipuladores de eventos.

As declarações de acessador consistem em uma *add_accessor_declaration* e uma *remove_accessor_declaration*. Cada declaração de acessador consiste o token `add` ou `remove` seguido por um *bloco*. O *bloco* associado com um *add_accessor_declaration* Especifica as instruções para executar quando um manipulador de eventos é adicionado e o *bloco* associado com um *remove_accessor_declaration* Especifica as instruções para executar quando um manipulador de eventos é removido.

Cada *add_accessor_declaration* e *remove_accessor_declaration* corresponde a um método com um parâmetro de valor único do tipo de evento e um `void` tipo de retorno. O parâmetro implícito de um acessador de evento é chamado `value`. Quando um evento é usado em uma atribuição de evento, o acessador de evento apropriado é usado. Especificamente, se for o operador de atribuição `+=` acessador adicionar é usado, e se o operador de atribuição é `-=` , o acessador de remoção será usado. Em ambos os casos, o operando à direita do operador de atribuição é usado como o argumento para o acessador do evento. O bloco de um *add_accessor_declaration* ou um *remove_accessor_declaration* devem obedecer às regras para `void` métodos descritos [corpo do método](classes.md#method-body). Em particular, `return` instruções em tal um bloco não são permitidas para especificar uma expressão.

Uma vez que um acessador de evento implicitamente tem um parâmetro chamado `value`, ele é um erro de tempo de compilação para um local constante ou variável declarada em um acessador de evento esse nome.

No exemplo
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
o `Control` classe implementa um mecanismo de armazenamento interno para eventos. O `AddEventHandler` método associa um valor de delegado com uma chave, o `GetEventHandler` método retorna o delegado atualmente associado a uma chave e o `RemoveEventHandler` método Remove um delegado como um manipulador de eventos para o evento especificado. Presumivelmente, o mecanismo de armazenamento subjacente foi projetado, de modo que não há nenhum custo para associar um `null` delegar o valor com uma chave e, portanto, os eventos sem tratamento não consumam nenhum armazenamento.

### <a name="static-and-instance-events"></a>Eventos estáticos e de instância

Quando uma declaração de evento inclui um `static` modificador, o evento deve ser um ***evento estático***. Quando nenhum `static` modificador estiver presente, o evento deve ser um ***eventos de instância***.

Um evento estático não está associado uma instância específica e é um erro de tempo de compilação para se referir a `this` nos acessadores de um evento estático.

Um evento de instância está associado uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores de evento.

Quando um evento é referenciado em uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `E.M`, se `M` é um evento estático, `E` deve indicar um tipo que contém `M`e se `M` é um evento de instância, E preciso marcar uma instância de um tipo que contém `M`.

As diferenças entre static e membros de instância são discutidos mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Acessadores de evento abstrato, substituição e virtuais, selado

Um `virtual` declaração de evento especifica que os acessadores de evento são virtuais. O `virtual` modificador se aplica a ambos os acessadores de um evento.

Um `abstract` declaração de evento especifica que os acessadores do evento são virtuais, mas não fornece uma implementação real dos acessadores. Em vez disso, as classes derivadas não abstratas devem fornecer sua própria implementação para os acessadores, substituindo o evento. Como uma declaração de evento abstrata não fornece nenhuma implementação real, ele não pode fornecer delimitada por chaves *event_accessor_declarations*.

Uma declaração de evento que inclui ambos os `abstract` e `override` modificadores Especifica que o evento é abstrato e substitui um evento de base. Os acessadores de um evento também são abstratos.

Declarações de evento abstrato são permitidas apenas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).

Os acessadores de um evento virtual herdado podem ser substituídos em uma classe derivada incluindo uma declaração de evento que especifica um `override` modificador. Isso é conhecido como um ***substituindo a declaração de evento***. Uma declaração de evento de substituição não declara um novo evento. Em vez disso, ele simplesmente é especialista as implementações de acessadores de um evento virtual existente.

Uma declaração de evento de substituição deve especificar o exato mesmo modificadores de acessibilidade, o tipo e o nome que o evento substituído.

Uma declaração de evento de substituição pode incluir o `sealed` modificador. Uso desse modificador impede que uma classe derivada ainda mais a substituição de evento. Os acessadores de um evento lacrado também são lacrados.

É um erro de tempo de compilação para uma declaração de evento de substituição incluir um `new` modificador.

Exceto pelas diferenças na declaração e chamada de sintaxe, substituição sealed, virtual e acessadores abstratos se comporta exatamente como virtual, selada, substituição e métodos abstratos. Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [substituir métodos](classes.md#override-methods), [lacrados métodos](classes.md#sealed-methods), e [métodos abstratos](classes.md#abstract-methods) se aplicam como se acessadores eram os métodos de um formulário correspondente. Cada acessador corresponde a um método com um parâmetro de valor único de tipo de evento, um `void` retornar o tipo e os modificadores mesmos que o evento recipiente.

## <a name="indexers"></a>Indexadores

Uma ***indexador*** é um membro que permite que um objeto a serem indexados da mesma forma como uma matriz. Indexadores são declarados usando *indexer_declaration*s:

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

Uma *indexer_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)), o `new` ([o novo modificador](classes.md#the-new-modifier)), `virtual` ([métodos virtuais](classes.md#virtual-methods)), `override` ([substituir métodos](classes.md#override-methods) ), `sealed` ([Lacrados métodos](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)), e `extern` ([métodos externos](classes.md#external-methods)) modificadores.

Declarações de indexador estão sujeitos às mesmas regras de declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores, com a exceção de que o modificador estático não é permitida em uma declaração do indexador.

Os modificadores `virtual`, `override`, e `abstract` são mutuamente exclusivos, exceto em um caso. O `abstract` e `override` modificadores podem ser usados juntos para que um indexador abstrato pode substituir um virtual.

O *tipo* de um indexador declaração especifica o tipo de elemento do indexador introduzido por declaração. A menos que o indexador é uma implementação de membro de interface explícita, o *tipo* é seguido da palavra-chave `this`. Para uma implementação de membro de interface explícita, o *tipo* é seguido por um *interface_type*, um "`.`" e a palavra-chave `this`. Ao contrário de outros membros, os indexadores não tem nomes definidos pelo usuário.

O *formal_parameter_list* Especifica os parâmetros do indexador. Lista de parâmetros formais de um indexador corresponde de um método ([parâmetros de método](classes.md#method-parameters)), exceto que deve ser especificado pelo menos um parâmetro e que o `ref` e `out` modificadores de parâmetro não são permitidos. .

O *tipo* de um indexador e cada um dos tipos referenciados nas *formal_parameter_list* deve ser pelo menos tão acessíveis quanto o próprio indexador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Uma *indexer_body* qualquer um pode consistir de uma ***corpo do acessador*** ou uma ***corpo da expressão***. Em um corpo de acessador *accessor_declarations*, que deve ser colocada em "`{`"e"`}`" tokens, declare acessadores ([acessadores](classes.md#accessors)) da propriedade. Os acessadores de especificam as instruções executáveis associadas com a leitura e gravação de propriedade.

Um corpo de expressão consiste em "`=>`" seguido por uma expressão `E` e um ponto e vírgula é exatamente equivalente para o corpo da instrução `{ get { return E; } }`e pode, portanto, apenas ser usado para especificar indexadores somente getter onde está o resultado do getter fornecido por uma única expressão.

Embora a sintaxe para acessar um elemento do indexador é o mesmo que para um elemento de matriz, um elemento do indexador não é classificado como uma variável. Assim, não é possível passar um elemento do indexador como um `ref` ou `out` argumento.

Lista de parâmetros formais de um indexador define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do indexador. Especificamente, a assinatura de um indexador consiste em número e tipos de seus parâmetros formais. O tipo de elemento e os nomes dos parâmetros formais não fazem parte da assinatura do indexador.

A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.

Indexadores e propriedades são muito semelhantes em conceito, mas diferem das seguintes maneiras:

*  Uma propriedade é identificada por seu nome, enquanto um indexador é identificado por sua assinatura.
*  Uma propriedade é acessada por meio de um *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)), enquanto um indexador elemento é acessado por meio de um *element_access* ([acesso ao indexador](expressions.md#indexer-access)).
*  Uma propriedade pode ser um `static` membro, enquanto um indexador é sempre um membro de instância.
*  Um `get` acessador de uma propriedade corresponde a um método sem parâmetros, enquanto um `get` acessador de um indexador corresponde a um método com a mesma lista de parâmetro formal que o indexador.
*  Um `set` acessador de uma propriedade corresponde a um método com um parâmetro único chamado `value`, enquanto um `set` acessador de um indexador corresponde a um método com a mesma lista de parâmetro formal que o indexador, além de um parâmetro adicional chamado `value`.
*  É um erro de tempo de compilação para um acessador de indexador declarar uma variável local com o mesmo nome como um parâmetro de indexador.
*  Em uma declaração de propriedade de substituição, a propriedade herdada é acessada usando a sintaxe `base.P`, onde `P` é o nome da propriedade. Em uma declaração de indexador de substituição, o indexador herdado é acessado usando a sintaxe `base[E]`, onde `E` é uma lista separada por vírgulas de expressões.
*  Não há nenhum conceito de um "indexador implementado automaticamente". É um erro ter um indexador não-abstrata, não externa com acessadores de ponto e vírgula.

Além dessas diferenças, todas as regras definidas em [acessadores](classes.md#accessors) e [implementadas automaticamente propriedades](classes.md#automatically-implemented-properties) se aplicam a acessadores de indexador, bem como para acessadores de propriedade.

Quando uma declaração de indexador inclui um `extern` modificador, o indexador é considerado uma ***indexador externo***. Como uma declaração de indexador externo não fornece nenhuma implementação real, cada um dos seus *accessor_declarations* consiste em um ponto e vírgula.

O exemplo a seguir declara um `BitArray` classe que implementa um indexador para acessar os bits individuais na matriz de bits.
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

Uma instância da `BitArray` classe consome significativamente menos memória do que um correspondente `bool[]` (já que cada valor do primeiro ocupa apenas um bit em vez do último de um byte), mas permite que as mesmas operações que um `bool[]`.

O seguinte `CountPrimes` classe usa um `BitArray` e o algoritmo de "sieve" clássico para calcular o número de números primos entre 1 e um máximo de determinado:
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

Observe que a sintaxe para acessar os elementos do `BitArray` é exatamente a mesma usada para um `bool[]`.

O exemplo a seguir mostra uma classe de 26 * 10 grade que tem um indexador com dois parâmetros. O primeiro parâmetro deve ser uma letra maiuscula ou letra minúscula no intervalo A-Z e a segunda é necessário para ser um número inteiro no intervalo de 0 a 9.

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>Sobrecarga de indexador

As regras de resolução de sobrecarga do indexador são descritas em [inferência de tipo](expressions.md#type-inference).

## <a name="operators"></a>Operadores

Uma ***operador*** é um membro que define o significado de um operador de expressão que pode ser aplicado a instâncias da classe. Operadores são declarados usando *operator_declaration*s:

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | 'right_shift' | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Há três categorias de operadores sobrecarregáveis: operadores unários ([operadores unários](classes.md#unary-operators)), operadores binários ([operadores binários](classes.md#binary-operators)) e os operadores de conversão ([operadores de conversão ](classes.md#conversion-operators)).

O *operator_body* é um vírgula, um ***corpo da instrução*** ou uma ***corpo da expressão***. Um corpo de declaração consiste em uma *bloco*, que especifica as instruções para executar quando o operador é invocado. O *bloco* deve obedecer às regras para retornar o valor métodos descritos [corpo do método](classes.md#method-body). Consiste em um corpo de expressão `=>` seguido por uma expressão e um ponto e vírgula e denota uma única expressão para executar quando o operador é invocado.

Para `extern` operadores, o *operator_body* consiste simplesmente em um ponto e vírgula. Para todos os outros operadores, o *operator_body* é um corpo do bloco ou um corpo de expressão.

As seguintes regras se aplicam a todas as declarações de operador:

*  Uma declaração do operador deve incluir um `public` e um `static` modificador.
*  O parâmetro (s) de um operador deve ser parâmetros de valor ([parâmetros de valores](variables.md#value-parameters)). É um erro de tempo de compilação para uma declaração do operador especificar `ref` ou `out` parâmetros.
*  A assinatura de um operador ([operadores unários](classes.md#unary-operators), [operadores binários](classes.md#binary-operators), [operadores de conversão](classes.md#conversion-operators)) deve ser diferente das assinaturas de todos os outros operadores declarados no mesma classe.
*  Todos os tipos referenciados em uma declaração do operador devem ser pelo menos tão acessíveis quanto o próprio operador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).
*  É um erro para o mesmo modificador aparecer várias vezes em uma declaração do operador.

Cada categoria de operador impõe restrições adicionais, conforme descrito nas seções a seguir.

Como outros membros, os operadores declarados em uma classe base são herdados por classes derivadas. Como declarações de operador sempre exigem a classe ou struct no qual o operador é declarado para participar na assinatura do operador, não é possível que um operador declarado em uma classe derivada ocultar um operador declarado em uma classe base. Portanto, o `new` modificador nunca é necessária e, portanto, nunca permitido, em uma declaração do operador.

Informações adicionais sobre os operadores unários e binários podem ser encontradas no [operadores](expressions.md#operators).

Informações adicionais sobre os operadores de conversão podem ser encontradas no [conversões definidas pelo usuário](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Operadores unários

As seguintes regras se aplicam às declarações de operador unário, onde `T` indica o tipo de instância da classe ou struct que contém a declaração do operador:

*  Um unário `+`, `-`, `!`, ou `~` operador deve levar um único parâmetro do tipo `T` ou `T?` e pode retornar qualquer tipo.
*  Um unário `++` ou `--` operador deve levar um único parâmetro do tipo `T` ou `T?` e deve retornar o mesmo tipo ou um tipo derivado dele.
*  Um unário `true` ou `false` operador deve levar um único parâmetro do tipo `T` ou `T?` e deve retornar tipo `bool`.

A assinatura de um operador unário consiste o token de operador (`+`, `-`, `!`, `~`, `++`, `--`, `true`, ou `false`) e o tipo do parâmetro formal único. O tipo de retorno não é parte de assinatura de um operador unário, nem é o nome do parâmetro formal.

O `true` e `false` operadores unários exigem declaração por pares. Ocorrerá um erro de tempo de compilação se uma classe declara um destes operadores sem declarar também o outro. O `true` e `false` operadores são descritos mais detalhadamente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators) e [expressões Boolianas](expressions.md#boolean-expressions).

O exemplo a seguir mostra uma implementação e uso subsequente de `operator ++` para uma classe de vetor de inteiro:
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

Observe como o método do operador retorna o valor produzido, adicionando 1 ao operando, assim como o incremento de sufixo e operadores de decremento ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators)) e o incremento de prefixo e de decremento operadores ([incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)). Ao contrário em C++, esse método precisa não modificar o valor do operando diretamente. Na verdade, modificando o valor do operando violar a semântica padrão de operador de incremento de sufixo.

### <a name="binary-operators"></a>Operadores binários

As seguintes regras se aplicam às declarações de operador binário, onde `T` indica o tipo de instância da classe ou struct que contém a declaração do operador:

*  Um operador de deslocamento não binário deve levar dois parâmetros, pelo menos um dos quais deve ter o tipo `T` ou `T?`e pode retornar qualquer tipo.
*  Um binário `<<` ou `>>` operador deve levar dois parâmetros, o primeiro deles deve ser do tipo `T` ou `T?` e o segundo dos quais deve ser do tipo `int` ou `int?`e pode retornar qualquer tipo.

A assinatura de um operador binário consiste o token de operador (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, ou `<=`) e os tipos dos dois parâmetros formais. O tipo de retorno e os nomes dos parâmetros formais não fazem parte da assinatura de um operador binário.

Determinados operadores binários requerem uma declaração por pares. Para cada declaração de qualquer operador de um par, deve haver uma declaração correspondente do operador do par. Duas declarações do operador correspondam quando tiverem o mesmo tipo de retorno e o mesmo tipo para cada parâmetro. Os operadores a seguir exigem uma declaração por pares:

*  `operator ==` e `operator !=`
*  `operator >` e `operator <`
*  `operator >=` e `operator <=`

### <a name="conversion-operators"></a>Operadores de conversão

Uma declaração do operador de conversão apresenta uma ***conversão definida pelo usuário*** ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)) que amplia as conversões implícitas e explícitas predefinidas.

Uma declaração do operador de conversão que inclui o `implicit` palavra-chave introduz uma conversão implícita definidas pelo usuário. Conversões implícitas podem ocorrer em uma variedade de situações, incluindo as invocações de função de membro, expressões de conversão e atribuições. Isso é descrito posteriormente em [conversões implícitas](conversions.md#implicit-conversions).

Uma declaração do operador de conversão que inclui o `explicit` palavra-chave introduz uma conversão explícita definida pelo usuário. Conversões explícitas podem ocorrer em expressões de conversão e são descritas mais detalhadamente no [conversões explícitas](conversions.md#explicit-conversions).

Um operador de conversão converte de um tipo de fonte, indicado pelo tipo de parâmetro de operador de conversão, para um tipo de destino, indicado pelo tipo de retorno do operador de conversão.

Para um tipo de origem especificado `S` e o tipo de destino `T`, se `S` ou `T` são tipos que permitem valor nulos, deixe `S0` e `T0` consulte seus tipos base, caso contrário `S0` e `T0` são igual a `S` e `T` , respectivamente. Uma classe ou struct é permitida para declarar uma conversão de um tipo de fonte `S` para um tipo de destino `T` somente se todos os itens a seguir forem verdadeiras:

*  `S0` e `T0` são de tipos diferentes.
*  Tanto `S0` ou `T0` é o tipo de classe ou struct em que a declaração do operador ocorre.
*  Nem `S0` nem `T0` é um *interface_type*.
*  Excluindo conversões definidas pelo usuário, uma conversão não existe na `S` à `T` ou do `T` para `S`.

Para fins dessas regras, qualquer tipo de parâmetros associados `S` ou `T` são considerados tipos exclusivos que têm nenhuma relação de herança com outros tipos e quaisquer restrições no tipo desses parâmetros serão ignorados.

No exemplo
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
a primeira operátoru duas é permitidas porque, para as finalidades [indexadores](classes.md#indexers).3, `T` e `int` e `string` respectivamente são considerados tipos exclusivos sem relação alguma com. No entanto, o terceiro operador é um erro, porque `C<T>` é a classe base de `D<T>`.

Na segunda regra que segue um operador de conversão deve converter para ou do tipo de classe ou estrutura na qual o operador é declarado. Por exemplo, é possível para um tipo de classe ou struct `C` para definir uma conversão de `C` para `int` e do `int` para `C`, mas não contra `int` para `bool`.

Não é possível redefinir diretamente uma conversão predefinida. Assim, os operadores de conversão não são permitidos para converter de ou para o `object` porque já existem conversões implícitas e explícitas entre `object` e todos os outros tipos. Da mesma forma, nem a origem ou os tipos de destino de uma conversão podem ser um tipo base dos outros, uma vez que uma conversão, em seguida, seria já existe.

No entanto, é possível declarar operadores em tipos genéricos que, para argumentos de tipo específico, especificam as conversões que já existem como conversões predefinidas. No exemplo
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Quando digita `object` é especificado como um argumento de tipo para `T`, o segundo operador declara uma conversão que já existe (implícito e, portanto, também um valor explícito, existe conversão de qualquer tipo para `object`).

Em casos em que uma conversão predefinida existe entre dois tipos, as conversões definidas pelo usuário entre esses tipos são ignoradas. Especificamente:

*  Se uma conversão implícita predefinida ([conversões implícitas](conversions.md#implicit-conversions)) do tipo `S` digitar `T`, todas as conversões definidas por usuário (implícitas ou explícitas) do `S` para `T` são ignorados.
*  Se uma conversão explícita predefinida ([conversões explícitas](conversions.md#explicit-conversions)) do tipo `S` digitar `T`, nenhuma conversão explícita definida pelo usuário do `S` para `T` são ignorados. Além disso:

Se `T` é um tipo de interface, definido pelo usuário conversões implícitas de `S` para `T` são ignorados.

Caso contrário, definido pelo usuário conversões implícitas de `S` para `T` ainda são consideradas.

Para todos os tipos, mas `object`, os operadores são declarados pelo `Convertible<T>` tipo acima não entram em conflito com conversões predefinidas. Por exemplo:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

No entanto, para o tipo `object`, conversões predefinidas ocultar as conversões definidas pelo usuário em todos os casos, mas um:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Conversões definidas pelo usuário não são permitidas para converter de ou para o *interface_type*s. Em particular, essa restrição garante que nenhum transformações definidas pelo usuário ocorram durante a conversão em um *interface_type*e que uma conversão para um *interface_type* terá êxito apenas se o objeto Na verdade, que está sendo convertido implementa especificado *interface_type*.

A assinatura de um operador de conversão consiste no tipo de origem e o tipo de destino. (Observe que isso é o único formulário de membro para o qual o tipo de retorno participa na assinatura.) O `implicit` ou `explicit` classificação de um operador de conversão não é parte da assinatura do operador. Dessa forma, uma classe ou struct não pode declarar tanto uma `implicit` e um `explicit` operador de conversão com os mesmos tipos de origem e destino.

Em geral, as conversões implícitas definidas pelo usuário devem ser criadas para nunca lançam exceções e perder informações. Se uma conversão definida pelo usuário pode gerar exceções (por exemplo, porque o argumento de origem está fora do intervalo) ou perda de informações (por exemplo, descartando os bits de ordem superior) e, em seguida, essa conversão deve ser definida como uma conversão explícita.

No exemplo
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
a conversão de `Digit` à `byte` é implícito porque nunca gera exceções ou perde informações, mas a conversão de `byte` à `Digit` é explícito desde `Digit` pode representar apenas um subconjunto dos possíveis valores de um `byte`.

## <a name="instance-constructors"></a>Construtores de instância

Um ***construtor de instância*** é um membro que implementa as ações necessárias para inicializar uma instância de uma classe. Construtores de instância são declaradas usando *constructor_declaration*s:

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

Um *constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), uma combinação válida de as quatro modificadores de acesso ([modificadores de acesso ](classes.md#access-modifiers)) e uma `extern` ([métodos externos](classes.md#external-methods)) modificador. Uma declaração de construtor não é permitida para incluir o modificador mesmo várias vezes.

O *identificador* de uma *constructor_declarator* deve nomear a classe na qual o construtor de instância é declarado. Se nenhum outro nome for especificado, ocorrerá um erro de tempo de compilação.

Opcional *formal_parameter_list* de uma instância do construtor está sujeito às mesmas regras que o *formal_parameter_list* de um método ([métodos](classes.md#methods)). Lista de parâmetros formais define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um construtor de instância e controla o processo pelo qual resolução de sobrecarga ([inferência de tipo](expressions.md#type-inference)) seleciona um determinado Construtor de instância em uma invocação.

Cada um dos tipos referenciados nos *formal_parameter_list* de uma instância de construtor deve ser pelo menos tão acessíveis quanto o próprio construtor ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Opcional *constructor_initializer* Especifica outro construtor de instância para invocar antes de executar as instruções fornecidas na *constructor_body* desse construtor de instância. Isso é descrito posteriormente em [inicializadores de construtor](classes.md#constructor-initializers).

Quando uma declaração de construtor inclui um `extern` modificador, o construtor deve ser um ***externo construtor***. Como uma declaração de construtor externo não fornece nenhuma implementação real, sua *constructor_body* consiste em um ponto e vírgula. Para todos os outros construtores, o *constructor_body* consiste em um *bloco* que especifica as instruções para inicializar uma nova instância da classe. Isso corresponde exatamente a *bloco* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).

Construtores de instância não são herdadas. Dessa forma, uma classe não tem nenhum construtor de instância que não sejam aqueles realmente declarados na classe. Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão é fornecido automaticamente ([1&gt;construtores padrão](classes.md#default-constructors)).

Construtores de instância são invocados pelo *object_creation_expression*s ([expressões de criação do objeto](expressions.md#object-creation-expressions)) e por meio das *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicializadores de construtores

Todos os construtores de instância (exceto aqueles para a classe `object`) implicitamente incluem uma invocação de outro construtor de instância imediatamente anterior a *constructor_body*. O construtor para invocar implicitamente é determinado pelo *constructor_initializer*:

*  Um inicializador do construtor de instância do formulário `base(argument_list)` ou `base()` faz com que um construtor de instância da classe base direta a ser invocado. Esse construtor está selecionado, usando *argument_list* se estiverem presentes e as regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution). O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessível contidos na classe base direta ou o construtor padrão ([1&gt;construtores padrão](classes.md#default-constructors)), se nenhum construtor de instância é declaradas no classe base direta. Se esse conjunto está vazio ou se um construtor de instância única melhor não pode ser identificado, ocorrerá um erro de tempo de compilação.
*  Um inicializador do construtor de instância do formulário `this(argument-list)` ou `this()` faz com que um construtor de instância da classe em si a ser invocado. O construtor está selecionado, usando *argument_list* se estiverem presentes e as regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution). O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessível declarados na classe em si. Se esse conjunto está vazio ou se um construtor de instância única melhor não pode ser identificado, ocorrerá um erro de tempo de compilação. Se uma declaração de construtor de instância inclui um inicializador de construtor que invoca o construtor em si, ocorrerá um erro de tempo de compilação.

Se um construtor de instância não tem nenhum inicializador de construtor, um inicializador de construtor do formulário `base()` é fornecido implicitamente. Portanto, uma declaração de construtor de instância do formulário
```csharp
C(...) {...}
```
é exatamente equivalente a
```csharp
C(...): base() {...}
```

O escopo dos parâmetros concedida pela *formal_parameter_list* de um construtor de instância declaração inclui o inicializador do construtor de declaração. Portanto, um inicializador de construtor tem permissão para acessar os parâmetros do construtor. Por exemplo:
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

Um inicializador de construtor de instância não é possível acessar a instância que está sendo criada. Portanto, é um erro de tempo de compilação para fazer referência a `this` em uma expressão de argumento o inicializador do construtor, como é um erro de tempo de compilação para uma expressão de argumento fazer referência a qualquer membro de instância por meio de um *simple_name*.

### <a name="instance-variable-initializers"></a>Inicializadores de variável de instância

Quando um construtor de instância não tem nenhum inicializador de construtor, ou ele tem um inicializador de construtor do formulário `base(...)`, esse construtor implicitamente executa as inicializações especificadas pelo *variable_initializer*s de os campos de instância é declarado na sua classe. Isso corresponde a uma sequência de atribuições que são executados imediatamente após a entrada para o construtor e antes da invocação do construtor de classe base direta implícita. Os inicializadores de variável são executados na ordem textual em que aparecem na declaração da classe.

### <a name="constructor-execution"></a>Execução do construtor

Inicializadores de variável são transformados em instruções de atribuição, e essas instruções de atribuição são executadas antes da invocação do construtor de instância de classe base. Essa ordenação garantirá que todos os campos de instância são inicializados por seus inicializadores de variável antes de quaisquer declarações que têm acesso a essa instância são executadas.

O exemplo
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
Quando `new B()` é usado para criar uma instância de `B`, a seguinte saída é produzida:
```
x = 1, y = 0
```

O valor de `x` é 1, porque o inicializador de variável é executado antes que o construtor de instância de classe base seja invocado. No entanto, o valor de `y` é 0 (o valor padrão de um `int`) porque a atribuição ao `y` não é executada até depois que o construtor de classe base retorna.

É útil pensar em inicializadores de construtor e inicializadores de variável de instância como instruções que são automaticamente inseridas antes do *constructor_body*. O exemplo
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
contém várias inicializadores de variável; Ele também contém os inicializadores de construtor de ambos os formulários (`base` e `this`). O exemplo corresponde ao código mostrado abaixo, onde cada comentário indica uma instrução inserida automaticamente (a sintaxe usada para as invocações de construtor automaticamente inserido não é válido, mas serve apenas para ilustrar o mecanismo).

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>Construtores padrão

Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão é fornecido automaticamente. Esse construtor padrão simplesmente invoca o construtor sem parâmetros da classe base direta. Se a classe é abstrata a acessibilidade declarada para o construtor padrão é protegida. Caso contrário, a acessibilidade declarada para o construtor padrão é pública. Portanto, o construtor padrão sempre é do formulário

```csharp
protected C(): base() {}
```
ou
```csharp
public C(): base() {}
```
onde `C` é o nome da classe. Se a resolução de sobrecarga não puder determinar um melhor candidato exclusivo para o inicializador do construtor de classe base ocorrerá um erro de tempo de compilação.

No exemplo
```csharp
class Message
{
    object sender;
    string text;
}
```
um construtor padrão é fornecido porque a classe não contém nenhuma declaração de construtor de instância. Portanto, o exemplo é precisamente equivalente a
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Construtores particulares

Quando uma classe `T` declara apenas construtores de instância privada, não é possível para as classes fora do texto de programa do `T` derivar `T` ou para criar diretamente instâncias do `T`. Assim, se uma classe contém apenas membros estáticos e não se destina a ser instanciado, adicionar um construtor de instância privada vazio impedirá instanciação. Por exemplo:
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

O `Trig` classe grupos constantes e métodos relacionados, mas não se destina a ser instanciado. Portanto, ele declara um construtor de instância única de privado vazio. Construtor de pelo menos uma instância deve ser declarado para suprimir a geração automática de um construtor padrão.

### <a name="optional-instance-constructor-parameters"></a>Parâmetros do construtor de instância opcional

O `this(...)` formulário de inicializador de construtor é comumente usado em conjunto com sobrecarga para implementar os parâmetros do construtor de instância opcional. No exemplo
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
os construtores de dois instância primeiro simplesmente fornecem os valores padrão para os argumentos ausentes. Ambos usam um `this(...)` inicializador de construtor para invocar o terceiro construtor de instância, o que realmente faz o trabalho de inicializar a nova instância. O efeito é que um dos parâmetros do construtor opcional:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Construtores estáticos

Um ***construtor estático*** é um membro que implementa as ações necessárias para inicializar um tipo de classe fechado. Construtores estáticos são declarados usando *static_constructor_declaration*s:

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

Um *static_constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e um `extern` modificador ([métodos externos](classes.md#external-methods)).

O *identificador* de uma *static_constructor_declaration* deve nomear a classe na qual o construtor estático é declarado. Se nenhum outro nome for especificado, ocorrerá um erro de tempo de compilação.

Quando uma declaração de construtor estático inclui um `extern` modificador, o construtor estático é considerado uma ***externo construtor estático***. Como uma declaração de construtor estático externo não fornece nenhuma implementação real, sua *static_constructor_body* consiste em um ponto e vírgula. Para todas as outras declarações de construtor estático, o *static_constructor_body* consiste em um *bloco* que especifica as instruções para executar a fim de inicializar a classe. Isso corresponde exatamente a *method_body* de um método estático com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).

Construtores estáticos não são herdados e não podem ser chamados diretamente.

O construtor estático para um tipo de classe fechado é executado no máximo uma vez em um determinado domínio de aplicativo. A execução de um construtor estático é disparada pela primeira dos seguintes eventos ocorra dentro de um domínio de aplicativo:

*  Uma instância do tipo de classe é criada.
*  Qualquer um dos membros estáticos do tipo de classe são referenciados.

Se uma classe contém o `Main` método ([inicialização do aplicativo](basic-concepts.md#application-startup)) em que a execução começa, o construtor estático para essa classe é executado antes de `Main` método é chamado.

Para inicializar um novo tipo de classes fechado, primeiro um novo conjunto de campos estáticos ([campos estáticos e de instância](classes.md#static-and-instance-fields)) para esse tipo específico de fechado é criado. Cada um dos campos estáticos é inicializada com seu valor padrão ([valores padrão](variables.md#default-values)). Em seguida, os inicializadores de campo estático ([inicialização do campo estático](classes.md#static-field-initialization)) são executadas para esses campos estáticos. Por fim, o construtor estático é executado.

O exemplo
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
deve produzir a saída:
```
Init A
A.F
Init B
B.F
```
porque a execução de `A`do construtor estático é disparado pela chamada para `A.F`e a execução de `B`do construtor estático é disparado pela chamada para `B.F`.

É possível criar dependências circulares que permitem que os campos estáticos com inicializadores de variável a ser observado em seu estado de valor padrão.

O exemplo
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
produz a saída
```
X = 1, Y = 2
```

Para executar o `Main` método, o sistema pela primeira vez é executado o inicializador para `B.Y`, antes de classe `B`do construtor estático. `Y`do inicializador faz com que `A`do construtor estático para ser executado porque o valor de `A.X` é referenciado. O construtor estático de `A` por sua vez prossegue para calcular o valor da `X`e fazer buscas caso o valor padrão de `Y`, que é zero. `A.X` Portanto, é inicializado como 1. O processo de execução `A`do construtor estático e inicializadores de campo estático, em seguida, for concluído, retornando para o cálculo do valor inicial do `Y`, cujo resultado se torna 2.

Como o construtor estático é executado exatamente uma vez para cada fechado o tipo de classe construída, é um local conveniente para impor verificações de tempo de execução no parâmetro de tipo não podem ser verificados em tempo de compilação por meio de restrições ([parâmetro de tipo restrições de](classes.md#type-parameter-constraints)). Por exemplo, o tipo a seguir usa um construtor estático para impor que o argumento de tipo é um enum:
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>Destruidores

Um ***destruidor*** é um membro que implementa as ações necessárias para destruir uma instância de uma classe. Um destruidor for declarado usando um *destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

Um *destructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)).

O *identificador* de uma *destructor_declaration* deve nomear a classe em que o destruidor for declarado. Se nenhum outro nome for especificado, ocorrerá um erro de tempo de compilação.

Quando uma declaração de destruidor inclui um `extern` modificador, o destruidor deve ser um ***destruidor externo***. Como uma declaração de destruidor externo não fornece nenhuma implementação real, sua *destructor_body* consiste em um ponto e vírgula. Para todos os destruidores, o *destructor_body* consiste em um *bloco* que especifica as instruções a executar para destruir uma instância da classe. Um *destructor_body* corresponde exatamente a *method_body* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).

Destruidores não são herdados. Portanto, uma classe não possui nenhum destruidor diferente daquele que podem ser declarados nessa classe.

Uma vez que um destruidor deve não ter nenhum parâmetro, não pode ser sobrecarregado, portanto, uma classe pode ter, no máximo, um destruidor.

Os destruidores são invocados automaticamente e não podem ser invocados explicitamente. Uma instância não estiver qualificada para destruição quando ele não é mais possível para qualquer código usar essa instância. Execução do destruidor para a instância pode ocorrer a qualquer momento depois que a instância se tornar qualificada para destruição. Quando uma instância é destruída, os destruidores na cadeia de herança da instância são chamados na ordem, do mais derivado para menos derivado. Um destruidor pode ser executado em qualquer thread. Para mais informações sobre as regras que determinam quando e como um destruidor é executado, consulte [gerenciamento automático de memória](basic-concepts.md#automatic-memory-management).

A saída do exemplo
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
uma vez que os destruidores em uma cadeia de herança são chamados na ordem, do mais derivado para menos derivado.

Os destruidores são implementados, substituindo o método virtual `Finalize` em `System.Object`. Programas em c# não tem permissão para substituir este método ou ligue para (ou substituições dele) diretamente. Por exemplo, o programa
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
contém dois erros.

O compilador se comporta como se esse método e substituições do mesmo, não existem em todos os. Portanto, este programa:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
é válido, e o método mostrado oculta `System.Object`do `Finalize` método.

Para obter uma discussão sobre o comportamento quando uma exceção for lançada de um destruidor, consulte [como exceções são tratadas](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterators

Um membro da função ([membros de função](expressions.md#function-members)) implementado usando um bloco de iteradores ([blocos](statements.md#blocks)) é chamado um ***iterador***.

Um bloco de iteradores pode ser usado como o corpo de um membro da função, desde que o tipo de retorno do membro da função correspondente é uma das interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) ou uma das interfaces enumeráveis ([Interfaces enumeráveis](classes.md#enumerable-interfaces)). Ele pode ocorrer como um *method_body*, *operator_body* ou *accessor_body*, enquanto eventos, construtores de instância, estáticas construtores e destruidores não podem ser implementadas como iteradores.

Quando um membro da função é implementado usando um bloco de iteradores, ele é um erro de tempo de compilação para a lista de parâmetros formais do membro da função para especificar qualquer `ref` ou `out` parâmetros.

### <a name="enumerator-interfaces"></a>Interfaces de enumerador

O ***interfaces de enumerador*** são a interface não genérica `System.Collections.IEnumerator` e em todas as instanciações da interface genérica `System.Collections.Generic.IEnumerator<T>`. Por questão de brevidade, neste capítulo essas interfaces são referenciadas como `IEnumerator` e `IEnumerator<T>`, respectivamente.

### <a name="enumerable-interfaces"></a>Interfaces enumeráveis

O ***interfaces enumeráveis*** são a interface não genérica `System.Collections.IEnumerable` e em todas as instanciações da interface genérica `System.Collections.Generic.IEnumerable<T>`. Por questão de brevidade, neste capítulo essas interfaces são referenciadas como `IEnumerable` e `IEnumerable<T>`, respectivamente.

### <a name="yield-type"></a>Tipo de rendimento

Um iterador produz uma sequência de valores, todos do mesmo tipo. Esse tipo é chamado de ***yield tipo*** do iterador.

*  O tipo de rendimento de um iterador que retorna `IEnumerator` ou `IEnumerable` é `object`.
*  O tipo de rendimento de um iterador que retorna `IEnumerator<T>` ou `IEnumerable<T>` é `T`.

### <a name="enumerator-objects"></a>Objetos de enumerador

Quando um membro da função retornando um enumerador de tipo de interface é implementado usando um bloco de iteradores, invocar o membro da função não é imediatamente executado o código no bloco de iterador. Em vez disso, uma ***objeto enumerador*** é criada e retornada. Esse objeto encapsula o código especificado no bloco de iterador e a execução do código no bloco iterador ocorre quando o objeto de enumerador `MoveNext` método é invocado. Um objeto de enumerador tem as seguintes características:

*  Ele implementa `IEnumerator` e `IEnumerator<T>`, onde `T` é o tipo de yield do iterador.
*  Ele implementa `System.IDisposable`.
*  Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor da instância passada para o membro da função.
*  Ele tem quatro estados possíveis, ***antes de***, ***executando***, ***suspenso***, e ***depois***e é inicialmente no ***antes***  estado.

Um objeto de enumerador normalmente é uma instância de uma classe de enumerador gerado pelo compilador que encapsula o código no bloco de iterador e implementa as interfaces de enumerador, mas outros métodos de implementação são possíveis. Se uma classe de enumerador é gerada pelo compilador, essa classe será ser aninhada, direta ou indiretamente, na classe que contém o membro da função, ela tem acessibilidade privada e ele terá um nome reservado para uso pelo compilador ([identificadores ](lexical-structure.md#identifiers)).

Um objeto de enumerador pode implementar mais interfaces daqueles especificados acima.

As seções a seguir descrevem o comportamento exato do `MoveNext`, `Current`, e `Dispose` membros a `IEnumerable` e `IEnumerable<T>` fornecidas por um objeto de enumerador de implementações de interface.

Observe que não têm suporte para objetos de enumerador a `IEnumerator.Reset` método. Invocar esse método faz com que um `System.NotSupportedException` seja lançada.

#### <a name="the-movenext-method"></a>O método MoveNext

O `MoveNext` método de um objeto de enumerador encapsula o código de um bloco de iteradores. Invocar o `MoveNext` método executa o código no bloco de iteradores e conjuntos de `Current` propriedade do objeto enumerador conforme apropriado. Precisa ação executada pelo `MoveNext` depende do estado do objeto enumerador quando `MoveNext` é invocado:

*  Se for o estado do objeto enumerador ***antes de***, chamar `MoveNext`:
   * Altera o estado para ***executando***.
   * Inicializa os parâmetros (incluindo `this`) do bloco de iterador com os valores de argumento e o valor de instância salvo quando o objeto de enumerador foi inicializado.
   * Executa o bloco de iteradores desde o início até que a execução é interrompida (conforme descrito abaixo).
*  Se for o estado do objeto enumerador ***em execução***, o resultado da invocação `MoveNext` não está especificado.
*  Se for o estado do objeto enumerador ***suspensos***, chamar `MoveNext`:
   * Altera o estado para ***executando***.
   * Restaura os valores de todas as variáveis locais e parâmetros (inclusive este) para os valores salvos quando a execução do bloco iterador foi suspenso pela última vez. Observe que o conteúdo de todos os objetos referenciados por essas variáveis pode ter mudado desde a chamada anterior a MoveNext.
   * Retoma a execução do bloco de iterador imediatamente após o `yield return` instrução que causou a suspensão de execução e continua até que a execução é interrompida (conforme descrito abaixo).
*  Se for o estado do objeto enumerador ***após***, chamar `MoveNext` retorna `false`.


Quando `MoveNext` executa o bloco de iteradores, execução pode ser interrompida de quatro maneiras: por um `yield return` instrução, por um `yield break` instrução, encontrando o final do bloco de iterador e por uma exceção sendo lançada e propagada da bloco de iteradores.

*  Quando um `yield return` instrução for encontrada ([a instrução yield](statements.md#the-yield-statement)):
   * A expressão fornecida na instrução é avaliada implicitamente convertida no tipo de rendimento e atribuída ao `Current` propriedade do objeto enumerador.
   * Execução do corpo do iterador é suspenso. Os valores de todos os parâmetros e variáveis locais (incluindo `this`) são salvas, como é o local deste `yield return` instrução. Se o `yield return` instrução está dentro de um ou mais `try` bloqueia associado `finally` blocos não são executados no momento.
   * O estado do objeto enumerador é alterado para ***suspenso***.
   * O `MoveNext` retorno do método `true` para seu chamador, que indica que a iteração avançado com êxito para o próximo valor.
*  Quando um `yield break` instrução for encontrada ([a instrução yield](statements.md#the-yield-statement)):
   * Se o `yield break` instrução está dentro de um ou mais `try` bloqueia associado `finally` blocos são executados.
   * O estado do objeto enumerador é alterado para ***depois de***.
   * O `MoveNext` retorno do método `false` para seu chamador, que indica que a iteração foi concluída.
*  Quando o final do corpo do iterador for encontrado:
   * O estado do objeto enumerador é alterado para ***depois de***.
   * O `MoveNext` retorno do método `false` para seu chamador, que indica que a iteração foi concluída.
*  Quando uma exceção é lançada e propagada para fora do bloco de iterador:
   * Apropriado `finally` blocos no corpo do iterador serão foram executados pela propagação de exceção.
   * O estado do objeto enumerador é alterado para ***depois de***.
   * A propagação de exceção continuará para o chamador do `MoveNext` método.

#### <a name="the-current-property"></a>A propriedade atual

Um objeto de enumerador `Current` propriedade é afetada pela `yield return` instruções no bloco de iterador.

Quando um objeto de enumerador está no ***suspensos*** de estado, o valor de `Current` é o valor definido pela chamada anterior para `MoveNext`. Quando um objeto de enumerador está no ***antes de***, ***executando***, ou ***depois*** afirma, o resultado de acessar `Current` não está especificado.

Para um iterador com um rendimento tipo diferente de `object`, o resultado de acessando `Current` por meio do objeto de enumerador `IEnumerable` implementação corresponde ao acessar `Current` por meio do objeto de enumerador `IEnumerator<T>` implementação e convertendo o resultado para `object`.

#### <a name="the-dispose-method"></a>O método Dispose

O `Dispose` método é usado para limpar a iteração, colocando o objeto enumerador o ***depois*** estado.

*  Se for o estado do objeto enumerador ***antes de***, chamar `Dispose` altera o estado para ***depois***.
*  Se for o estado do objeto enumerador ***em execução***, o resultado da invocação `Dispose` não está especificado.
*  Se for o estado do objeto enumerador ***suspensos***, chamar `Dispose`:
   * Altera o estado para ***executando***.
   * Executa qualquer finalmente blocos como se o último executado `yield return` instrução foram um `yield break` instrução. Se isso faz com que uma exceção seja gerada e propagadas para fora do corpo do iterador, o estado do objeto enumerador é definido como ***após*** e a exceção é propagada para o chamador do `Dispose` método.
   * Altera o estado para ***depois de***.
*  Se for o estado do objeto enumerador ***após***, chamar `Dispose` não tem nenhum efeito.

### <a name="enumerable-objects"></a>Objetos enumeráveis

Quando um membro da função retornando um tipo enumerável interface é implementado usando um bloco de iteradores, invocar o membro da função não é imediatamente executado o código no bloco de iterador. Em vez disso, uma ***objeto enumerável*** é criada e retornada. O objeto enumerável `GetEnumerator` método retorna um objeto enumerador que encapsula o código especificado no bloco de iterador e a execução do código no bloco iterador ocorre quando o objeto de enumerador `MoveNext` método é invocado. Um objeto enumerável tem as seguintes características:

*  Ele implementa `IEnumerable` e `IEnumerable<T>`, onde `T` é o tipo de yield do iterador.
*  Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor da instância passada para o membro da função.

Um objeto enumerável normalmente é uma instância de uma classe enumerable gerado pelo compilador que encapsula o código no bloco de iterador e implementa as interfaces enumeráveis, mas outros métodos de implementação são possíveis. Se uma classe enumerable é gerada pelo compilador, essa classe será ser aninhada, direta ou indiretamente, na classe que contém o membro da função, ela tem acessibilidade privada e ele terá um nome reservado para uso pelo compilador ([identificadores ](lexical-structure.md#identifiers)).

Um objeto enumerável pode implementar mais interfaces daqueles especificados acima. Em particular, também pode implementar um objeto enumerável `IEnumerator` e `IEnumerator<T>`, habilitá-la para servir como um enumerável e um enumerador. Esse tipo de implementação, a primeira vez que um objeto enumerável `GetEnumerator` método é invocado, o objeto enumerável em si é retornado. As invocações subsequentes do objeto enumerável `GetEnumerator`, se houver, retornar uma cópia do objeto enumerável. Assim, cada retornada enumerador tem seu próprio estado e as alterações em um enumerador não afetará outro.

#### <a name="the-getenumerator-method"></a>O método GetEnumerator

Um objeto enumerável fornece uma implementação do `GetEnumerator` métodos do `IEnumerable` e `IEnumerable<T>` interfaces. Os dois `GetEnumerator` métodos compartilham uma implementação comum que adquire e retorna um objeto de enumerador disponíveis. O objeto de enumerador é inicializada com os valores de argumento e a instância valor salvo quando o objeto enumerável foi inicializado, mas caso contrário, as funções de objeto de enumerador conforme descrito em [objetos do enumerador](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Exemplo de implementação

Esta seção descreve uma possível implementação de iteradores em termos de construções c# padrão. A implementação descrita aqui baseia-se os mesmos princípios usados pelo compilador Microsoft c#, mas ele não é uma implementação de conformidade ou o único possível.

O seguinte `Stack<T>` classe implementa seu `GetEnumerator` método usando um iterador. O iterador enumera os elementos da pilha na parte superior para a parte inferior.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerado pelo compilador que encapsula o código no bloco de iterador, conforme mostrado a seguir.

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

A conversão anterior, o código no bloco iterador é transformado em uma máquina de estado e colocado no `MoveNext` método da classe de enumerador. Além disso, a variável local `i` será transformado em um campo no objeto de enumerador para que ele possa continuar existindo entre invocações de `MoveNext`.

O exemplo a seguir imprime uma simple tabela de multiplicação de números inteiros de 1 a 10. O `FromTo` método no exemplo retorna um objeto enumerável e é implementado usando um iterador.

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

O `FromTo` método pode ser convertido em uma instanciação de uma classe enumerable gerado pelo compilador que encapsula o código no bloco de iterador, conforme mostrado a seguir.

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

A classe enumerable implementa as interfaces enumeráveis e as interfaces de enumerador, habilitá-la para servir como um enumerável e um enumerador. Na primeira vez o `GetEnumerator` método é invocado, o objeto enumerável em si é retornado. As invocações subsequentes do objeto enumerável `GetEnumerator`, se houver, retornar uma cópia do objeto enumerável. Assim, cada retornada enumerador tem seu próprio estado e as alterações em um enumerador não afetará outro. O `Interlocked.CompareExchange` método é usado para garantir a operação de thread-safe.

O `from` e `to` parâmetros são transformados em campos na classe enumerable. Porque `from` é modificada no bloco de iterador, adicional `__from` campo é introduzido para conter o valor inicial fornecido para `from` em cada enumerador.

O `MoveNext` método lança um `InvalidOperationException` se ele é chamado quando `__state` é `0`. Isso protege contra o uso do objeto enumerável como um objeto de enumerador sem primeiro chamar `GetEnumerator`.

O exemplo a seguir mostra uma classe simples de árvore. O `Tree<T>` classe implementa seu `GetEnumerator` método usando um iterador. O iterador enumera os elementos da árvore em ordem de infixo.

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerado pelo compilador que encapsula o código no bloco de iterador, conforme mostrado a seguir.

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Os de temporários gerados pelo compilador usados na `foreach` instruções são elevadas para o `__left` e `__right` campos do objeto enumerador. O `__state` campo do objeto enumerador é atualizado com cuidado para que o correto `Dispose()` método será chamado corretamente se uma exceção é lançada. Observe que não é possível escrever o código traduzido com simples `foreach` instruções.

## <a name="async-functions"></a>Funções assíncronas

Um método ([métodos](classes.md#methods)) ou função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)) com o `async` modificador é chamado um ***função assíncrona***. Em geral, o termo ***async*** é usado para descrever qualquer tipo de função que tem o `async` modificador.

É um erro de tempo de compilação para a lista de parâmetros formais de uma função assíncrona para especificar qualquer `ref` ou `out` parâmetros.

O *return_type* de async método deve ser `void` ou uma ***tipo de tarefa***. Os tipos de tarefa são `System.Threading.Tasks.Task` e tipos construídos de `System.Threading.Tasks.Task<T>`. Por questão de brevidade, neste capítulo esses tipos são referenciados como `Task` e `Task<T>`, respectivamente. Um método assíncrono retorna um tipo de tarefa deve ser o retorno de tarefa.

A definição exata dos tipos de tarefa é definido pela implementação, mas do ponto de vista da linguagem um tipo de tarefa está em um dos Estados incompletos, teve êxito ou falha. Uma tarefa com falha registra uma exceção pertinente. Uma bem-sucedida `Task<T>` registra um resultado do tipo `T`. Tipos de tarefa são aguardável e, portanto, podem ser os operandos de expressões await ([expressões Await](expressions.md#await-expressions)).

Uma invocação de função async tem a capacidade de suspender a avaliação por meio de expressões await ([expressões Await](expressions.md#await-expressions)) em seu corpo. Avaliação pode ser retomada posteriormente no ponto de suspensão await expressão por meio de um ***delegado de continuação***. O delegado de continuação é do tipo `System.Action`, e quando ele é chamado, a avaliação da invocação de função async será retomada da expressão await onde parou. O ***chamador atual*** de uma função assíncrona invocação é o chamador original, se a invocação da função nunca foi suspenso ou o chamador mais recente do delegado de continuação caso contrário.

### <a name="evaluation-of-a-task-returning-async-function"></a>Avaliação de uma função de retorno de tarefa de async

Invocação de uma função de retorno de tarefa de async faz com que uma instância do tipo de tarefa retornada seja gerado. Isso é chamado de ***retornar task*** da função async. A tarefa é inicialmente em um estado incompleto.

O corpo da função de async é avaliado até que ele seja suspenso (por atingir uma expressão await) ou for encerrado, no qual o ponto de controle é retornado ao chamador, junto com a tarefa de retorno.

Quando o corpo da função async é encerrado, a tarefa de retornada é movida para fora do estado incompleto:

*  Se o corpo da função for encerrado como resultado de se atingir uma instrução return ou ao final do corpo, qualquer valor de resultado é registrada no task de retorno, que é colocado em um estado de êxito.
*  Se o corpo da função será encerrado devido a uma exceção não tratada ([a instrução throw](statements.md#the-throw-statement)) a exceção é registrada no task de retorno que é colocado em um estado de falha.

### <a name="evaluation-of-a-void-returning-async-function"></a>Avaliação de uma função de async que retornam void

Se for o tipo de retorno da função async `void`, avaliação difere acima da seguinte maneira: porque nenhuma tarefa é retornada, a função se comunica em vez disso, conclusão e exceções para o thread atual ***sincronização contexto***. A definição exata de contexto de sincronização é dependente da implementação, mas é uma representação de "onde" o thread atual está em execução. O contexto de sincronização é notificado quando a avaliação de uma função de async que retornam void começa, for concluída com êxito ou faz com que uma exceção não percebida seja lançada.

Isso permite que o contexto para controlar a execução de funções de async que retornam void quantos sob ele e decidir como propagar exceções provenientes proveito delas.
