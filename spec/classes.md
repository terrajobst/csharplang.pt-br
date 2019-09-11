---
ms.openlocfilehash: 2c87cafb8591b9dff2aa517b65af80ab263c7faa
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876896"
---
# <a name="classes"></a>Classes

Uma classe é uma estrutura de dados que pode conter membros de dados (constantes e campos), membros de função (métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e tipos aninhados. Os tipos de classe dão suporte à herança, um mecanismo no qual uma classe derivada pode estender e especializar uma classe base.

## <a name="class-declarations"></a>Declarações de classe

Um *class_declaration* é um *type_declaration* ([declarações de tipo](namespaces.md#type-declarations)) que declara uma nova classe.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Um *class_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *class_modifier*s ([modificadores de classe](classes.md#class-modifiers)), seguido por um `partial` modificador opcional, seguido pelo palavra `class` -chave e um *identificador* que nomeia a classe, seguido por um *type_parameter_list* opcional ([parâmetros de tipo](classes.md#type-parameters)), seguido por uma especificação *class_base* opcional (especificação de[base de classe ](classes.md#class-base-specification)), seguido por um conjunto opcional de *type_parameter_constraints_clause*s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), seguido por um *class_body* ([corpo de classe](classes.md#class-body)), opcionalmente seguido por um ponto-e-vírgula.

Uma declaração de classe não pode fornecer *type_parameter_constraints_clause*s, a menos que ele também forneça um *type_parameter_list*.

Uma declaração de classe que fornece um *type_parameter_list* é uma ***declaração de classe genérica***. Além disso, qualquer classe aninhada dentro de uma declaração de classe genérica ou uma declaração struct genérica é, em si, uma declaração de classe genérica, já que parâmetros de tipo para o tipo recipiente devem ser fornecidos para criar um tipo construído.

### <a name="class-modifiers"></a>Modificadores de classe

Um *class_declaration* pode, opcionalmente, incluir uma sequência de modificadores de classe:

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

É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de classe.

O `new` modificador é permitido em classes aninhadas. Ele especifica que a classe oculta um membro herdado com o mesmo nome, conforme descrito no [novo modificador](classes.md#the-new-modifier). É um erro de tempo de compilação para que `new` o modificador apareça em uma declaração de classe que não seja uma declaração de classe aninhada.

Os `public`modificadores `internal`, `protected`, e`private` controlam a acessibilidade da classe. Dependendo do contexto no qual a declaração de classe ocorre, alguns desses modificadores podem não ser permitidos ([acessibilidade declarada](basic-concepts.md#declared-accessibility)).

Os `abstract`modificadores e`static`são discutidos nas seções a seguir. `sealed`

#### <a name="abstract-classes"></a>Classes abstratas

O `abstract` modificador é usado para indicar que uma classe está incompleta e que se destina a ser usada somente como uma classe base. Uma classe abstrata difere de uma classe não abstrata das seguintes maneiras:

*  Uma classe abstrata não pode ser instanciada diretamente e é um erro de tempo de compilação para usar o `new` operador em uma classe abstrata. Embora seja possível ter variáveis e valores cujos tipos de tempo de compilação sejam abstratos, tais variáveis e valores serão necessariamente `null` ou contêm referências a instâncias de classes não abstratas derivadas dos tipos abstratos.
*  Uma classe abstrata é permitida (mas não obrigatória) para conter membros abstratos.
*  Uma classe abstrata não pode ser selada.

Quando uma classe não abstrata é derivada de uma classe abstrata, a classe não abstrata deve incluir implementações reais de todos os membros abstratos herdados, substituindo assim esses membros abstratos. No exemplo
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
a classe `A` abstract introduz um método `F`abstract. A `B` classe introduz um método `G`adicional, mas como não fornece uma implementação de `F`, `B` também deve ser declarada abstrata. As `C` substituições `F` de classe e fornecem uma implementação real. Como não há membros abstratos no `C`, `C` o é permitido (mas não obrigatório) para ser não abstrato.

#### <a name="sealed-classes"></a>Classes lacradas

O `sealed` modificador é usado para impedir a derivação de uma classe. Um erro em tempo de compilação ocorrerá se uma classe selada for especificada como a classe base de outra classe.

Uma classe selada também não pode ser uma classe abstrata.

O `sealed` modificador é usado principalmente para evitar derivação não intencional, mas também permite determinadas otimizações de tempo de execução. Em particular, como uma classe selada é conhecida por nunca ter classes derivadas, é possível transformar invocações de membro de função virtual em instâncias de classe seladas em invocações não virtuais.

#### <a name="static-classes"></a>Classes estáticas

O `static` modificador é usado para marcar a classe que está sendo declarada como uma ***classe estática***. Uma classe estática não pode ser instanciada, não pode ser usada como um tipo e pode conter somente membros estáticos. Somente uma classe estática pode conter declarações de métodos de extensão ([métodos de extensão](classes.md#extension-methods)).

Uma declaração de classe estática está sujeita às seguintes restrições:

*  Uma classe estática não pode incluir um `sealed` modificador ou `abstract` . No entanto, observe que, como uma classe estática não pode ser instanciada ou derivada de, ela se comporta como se fosse selada e abstrata.
*  Uma classe estática pode não incluir uma especificação de *class_base* ([especificação de base de classe](classes.md#class-base-specification)) e não pode especificar explicitamente uma classe base ou uma lista de interfaces implementadas. Uma classe estática herda implicitamente do tipo `object`.
*  Uma classe estática só pode conter membros estáticos ([membros estáticos e de instância](classes.md#static-and-instance-members)). Observe que as constantes e os tipos aninhados são classificados como membros estáticos.
*  Uma classe estática não pode ter membros `protected` ou `protected internal` a acessibilidade declarada.

É um erro de tempo de compilação para violar qualquer uma dessas restrições.

Uma classe estática não tem construtores de instância. Não é possível declarar um construtor de instância em uma classe estática e nenhum construtor de instância padrão ([construtores padrão](classes.md#default-constructors)) é fornecido para uma classe estática.

Os membros de uma classe estática não são estáticos automaticamente e as declarações de membro devem incluir explicitamente `static` um modificador (exceto para constantes e tipos aninhados). Quando uma classe é aninhada em uma classe externa estática, a classe aninhada não é uma classe estática, a menos `static` que inclua explicitamente um modificador.

__Referenciando tipos de classe estática__

Um *namespace_or_type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)) tem permissão para fazer referência a uma classe estática se

*  O *namespace_or_type_name* é o `T` em um *namespace_or_type_name* do formulário `T.I`ou
*  O *namespace_or_type_name* é o `T` em um *typeof_expression* ([lista de argumentos](expressions.md#argument-lists)1) do formulário `typeof(T)`.

Um *primary_expression* ([membros da função](expressions.md#function-members)) tem permissão para fazer referência a uma classe estática se

*  O *primary_expression* é o `E` em um *member_access* ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) do `E.I`formulário.

Em qualquer outro contexto, é um erro de tempo de compilação para fazer referência a uma classe estática. Por exemplo, é um erro para uma classe estática ser usada como uma classe base, um tipo constituinte ([tipos aninhados](classes.md#nested-types)) de um membro, um argumento de tipo genérico ou uma restrição de parâmetro de tipo. Da mesma forma, uma classe estática não pode ser usada em um tipo de matriz, um `new` tipo de ponteiro, uma expressão, `is` uma expressão de `as` conversão, uma `sizeof` expressão, uma expressão, uma expressão ou uma expressão de valor padrão.

### <a name="partial-modifier"></a>Modificador parcial

O `partial` modificador é usado para indicar que esse *class_declaration* é uma declaração de tipo parcial. Várias declarações de tipo parcial com o mesmo nome dentro de um namespace delimitador ou declaração de tipo são combinadas para formar uma declaração de tipo, seguindo as regras especificadas em [tipos parciais](classes.md#partial-types).

Ter a declaração de uma classe distribuída em segmentos separados de texto do programa pode ser útil se esses segmentos são produzidos ou mantidos em diferentes contextos. Por exemplo, uma parte de uma declaração de classe pode ser gerada pela máquina, enquanto a outra é criada manualmente. A separação textual dos dois impede que as atualizações de um entrem em conflito com as atualizações do outro.

### <a name="type-parameters"></a>Parâmetros de tipo

Um parâmetro de tipo é um identificador simples que denota um espaço reservado para um argumento de tipo fornecido para criar um tipo construído. Um parâmetro de tipo é um espaço reservado formal para um tipo que será fornecido posteriormente. Por outro lado, um argumento de tipo ([argumentos de tipo](types.md#type-arguments)) é o tipo real que é substituído pelo parâmetro de tipo quando um tipo construído é criado.

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

Cada parâmetro de tipo em uma declaração de classe define um nome no espaço de declaração ([declarações](basic-concepts.md#declarations)) dessa classe. Portanto, ele não pode ter o mesmo nome que outro parâmetro de tipo ou um membro declarado nessa classe. Um parâmetro de tipo não pode ter o mesmo nome que o próprio tipo.

### <a name="class-base-specification"></a>Especificação de base de classe

Uma declaração de classe pode incluir uma especificação *class_base* , que define a classe base direta da classe e as interfaces ([interfaces](interfaces.md)) diretamente implementadas pela classe.

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

A classe base especificada em uma declaração de classe pode ser um tipo de classe construído ([tipos construídos](types.md#constructed-types)). Uma classe base não pode ser um parâmetro de tipo por conta própria, embora possa envolver os parâmetros de tipo que estão no escopo.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Classes base

Quando um *class_type* é incluído no *class_base*, ele especifica a classe base direta da classe que está sendo declarada. Se uma declaração de classe não tiver nenhum *class_base*, ou se o *class_base* listar apenas os tipos de interface, a classe base `object`direta será considerada. Uma classe herda membros de sua classe base direta, conforme descrito em [herança](classes.md#inheritance).

No exemplo
```csharp
class A {}

class B: A {}
```
a `A` classe é considerada como a classe base direta de `B`e `B` é considerada derivada de `A`. Como `A` o não especifica explicitamente uma classe base direta, sua classe base direta é `object`implicitamente.

Para um tipo de classe construída, se uma classe base for especificada na declaração de classe genérica, a classe base do tipo construído será obtida pela substituição, para cada *type_parameter* na declaração de classe base, o type_argument correspondentedo tipo construído. Dadas as declarações de classe genéricas
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
a classe base do tipo `G<int>` construído `B<string,int[]>`seria.

A classe base direta de um tipo de classe deve ser pelo menos acessível como o próprio tipo de classe ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)). Por exemplo, é um erro de tempo de compilação para uma `public` classe derivar de uma `private` classe `internal` ou.

A classe base direta de um tipo de classe não deve ser um dos seguintes tipos: `System.Array` `System.MulticastDelegate`, `System.Delegate` `System.Enum`,, ou `System.ValueType`. Além disso, uma declaração de classe genérica `System.Attribute` não pode usar como uma classe base direta ou indireta.

Ao determinar o significado da `A` especificação de classe base direta de uma classe `B`, a classe base direta de `B` é temporariamente considerada. `object` Intuitivamente, isso garante que o significado de uma especificação de classe base não possa depender recursivamente por si só. O Exemplo:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
está em erro porque, `A<C.B>` na especificação da classe base, a classe base direta de `C` é considerada `object`como sendo e, portanto (pelas regras de [namespace e nomes](basic-concepts.md#namespace-and-type-names)de `C` tipo) não é considerada como um membro `B`.

As classes base de um tipo de classe são a classe base direta e suas classes base. Em outras palavras, o conjunto de classes base é o fechamento transitivo da relação de classe base direta. Fazendo referência ao exemplo acima, as classes base do `B` são `A` e `object`. No exemplo
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
as classes base do `D<int>` são `C<int[]>`, `B<IComparable<int[]>>`, `A`, e `object`.

Exceto para classe `object`, cada tipo de classe tem exatamente uma classe base direta. A `object` classe não tem nenhuma classe base direta e é a classe base definitiva de todas as outras classes.

Quando uma classe `B` deriva de uma classe `A`, é um `B`erro de tempo de compilação para `A` o qual depender. Uma classe ***depende diretamente*** de sua classe base direta (se houver) e ***depende diretamente*** da classe na qual ela é imediatamente aninhada (se houver). Dada essa definição, o conjunto completo de classes sobre as quais uma classe depende é o fechamento reflexivo e transitivo da relação ***depende diretamente*** de.

O exemplo
```csharp
class A: A {}
```
é errado porque a classe depende dele mesmo. Da mesma forma, o exemplo
```csharp
class A: B {}
class B: C {}
class C: A {}
```
é um erro porque as classes dependem circularmente. Por fim, o exemplo
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
resulta em um erro de tempo de compilação `A` porque depende `B.C` de (sua classe base direta `B` ), que depende (sua `A`classe de circunscrição imediatamente), que depende circularmente.

Observe que uma classe não depende das classes que estão aninhadas dentro dela. No exemplo
```csharp
class A
{
    class B: A {}
}
```
`B`depende de `A` (porque `A` é sua classe base direta e sua classe imediatamente delimitadora), `B` mas `A` não depende (porque `B` não é uma classe base nem uma classe de circunscrição de `A`). Portanto, o exemplo é válido.

Não é possível derivar de uma `sealed` classe. No exemplo
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
`A`a `B` classeestácomerroporquetentaderivardaclasse.`sealed`

#### <a name="interface-implementations"></a>Implementações de interfaces

Uma especificação *class_base* pode incluir uma lista de tipos de interface, caso em que a classe é mencionada para implementar diretamente os tipos de interface fornecidos. Implementações de interface são discutidas mais detalhadamente em [implementações de interface](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Restrições de parâmetro de tipo

As declarações de tipo genérico e de método podem opcionalmente especificar restrições de parâmetro de tipo, incluindo *type_parameter_constraints_clause*s.

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

Cada *type_parameter_constraints_clause* consiste no token `where`, seguido pelo nome de um parâmetro de tipo, seguido por dois-pontos e pela lista de restrições para esse parâmetro de tipo. Pode haver no máximo uma `where` cláusula para cada parâmetro de tipo e as `where` cláusulas podem ser listadas em qualquer ordem. Como os `get` tokens e `set` em um acessador `where` de propriedade, o token não é uma palavra-chave.

A lista de restrições fornecida em uma `where` cláusula pode incluir qualquer um dos seguintes componentes, nesta ordem: uma única restrição primária, uma ou mais restrições secundárias e a restrição de construtor, `new()`.

Uma restrição PRIMARY pode ser um tipo de classe ou a ***restrição*** `class` de tipo de referência ou a ***restrição*** `struct`de tipo de valor. Uma restrição secundária pode ser um *type_parameter* ou um *interface_type*.

A restrição de tipo de referência especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de referência. Todos os tipos de classe, tipos de interface, tipos delegados, tipos de matriz e parâmetros de tipo conhecidos como um tipo de referência (conforme definido abaixo) atendem a essa restrição.

A restrição de tipo de valor especifica que um argumento de tipo usado para o parâmetro de tipo deve ser um tipo de valor não anulável. Todos os tipos de struct não anuláveis, tipos de enumeração e parâmetros de tipo que têm a restrição de tipo de valor atendem a essa restrição. Observe que, embora seja classificado como um tipo de valor, um tipo anulável ([tipos anuláveis](types.md#nullable-types)) não satisfaz a restrição de tipo de valor. Um parâmetro de tipo com a restrição de tipo de valor também não pode ter o *constructor_constraint*.

Os tipos de ponteiro nunca podem ser argumentos de tipo e não são considerados para satisfazer as restrições de tipo de referência ou tipo de valor.

Se uma restrição for um tipo de classe, um tipo de interface ou um parâmetro de tipo, esse tipo especificará um "tipo base" mínimo que cada argumento de tipo usado para esse parâmetro de tipo deve dar suporte a. Sempre que um tipo construído ou um método genérico é usado, o argumento de tipo é verificado em relação às restrições no parâmetro de tipo em tempo de compilação. O argumento de tipo fornecido deve atender às condições descritas em [satisfazer restrições](types.md#satisfying-constraints).

Uma restrição *class_type* deve atender às seguintes regras:

*  O tipo deve ser um tipo de classe.
*  O tipo não deve ser `sealed`.
*  O tipo não deve ser um dos seguintes `System.Array`tipos: `System.Enum`, `System.Delegate`, ou `System.ValueType`.
*  O tipo não deve ser `object`. Como todos os tipos derivam de `object`, essa restrição não teria nenhum efeito se fosse permitida.
*  No máximo uma restrição para um determinado parâmetro de tipo pode ser um tipo de classe.

Um tipo especificado como uma restrição *interface_type* deve atender às seguintes regras:

*  O tipo deve ser um tipo de interface.
*  Um tipo não deve ser especificado mais de uma vez em uma `where` determinada cláusula.

Em ambos os casos, a restrição pode envolver qualquer um dos parâmetros de tipo do tipo associado ou declaração de método como parte de um tipo construído e pode envolver o tipo que está sendo declarado.

Qualquer classe ou tipo de interface especificado como uma restrição de parâmetro de tipo deve ser pelo menos como acessível ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)) como o tipo genérico ou o método que está sendo declarado.

Um tipo especificado como uma restrição *type_parameter* deve atender às seguintes regras:

*  O tipo deve ser um parâmetro de tipo.
*  Um tipo não deve ser especificado mais de uma vez em uma `where` determinada cláusula.

Além disso, não deve haver ciclos no grafo de dependência dos parâmetros de tipo, em que a dependência é uma relação transitiva definida por:

*  Se um `T` parâmetro de tipo for usado como uma restrição para o `S` parâmetro `S` de tipo, ***dependerá de*** `T`.
*  Se um parâmetro `S` de tipo depender de um parâmetro `T` de `T` tipo e depender de um `U` parâmetro `S` de tipo, ***dependerá de*** `U`.

Dada essa relação, é um erro de tempo de compilação para um parâmetro de tipo depender de si mesmo (direta ou indiretamente).

Todas as restrições devem ser consistentes entre os parâmetros de tipo dependentes. Se o parâmetro `S` de tipo depender do `T` parâmetro de tipo, então:

*  `T`Não deve ter a restrição de tipo de valor. Caso contrário `T` , é efetivamente lacrado, portanto `S` `T`, seria forçado a ser o mesmo tipo de, eliminando a necessidade de dois parâmetros de tipo.
*  Se `S` o tiver a restrição de tipo `T` de valor, então não deverá ter uma restrição *class_type* .
*  Se `S` tiver uma restrição `A` class_type e `T` tiver uma restrição `B` class_type, deverá haver uma conversão de identidade ou conversão de referência implícita `A` de para `B`ou uma conversão de referência implícita `B` de `A`para.
*  Se `S` também depender do parâmetro `U` de tipo `U` e tiver uma restrição `A` class_type `T` e tiver uma restrição `B` class_type, deverá haver uma conversão de identidade ou conversão de referência implícita `A` de `B` para ou uma conversão de referência `B` implícita `A`de para.

É válido para `S` que o tenha a restrição de tipo de `T` valor e tenha a restrição de tipo de referência. Efetivamente, isso `T` limita os tipos `System.Object`, `System.ValueType`, `System.Enum`e qualquer tipo de interface.

Se a `where` cláusula de um parâmetro de tipo incluir uma restrição de Construtor (que tem `new()`o formulário), será possível usar o `new` operador para criar instâncias do tipo ([expressões de criação de objeto](expressions.md#object-creation-expressions)). Qualquer argumento de tipo usado para um parâmetro de tipo com uma restrição de construtor deve ter um construtor público sem parâmetros (esse construtor existe implicitamente para qualquer tipo de valor) ou ser um parâmetro de tipo que tenha a restrição de tipo de valor ou restrição de Construtor (consulte [Restrições de parâmetro de tipo](classes.md#type-parameter-constraints) para obter detalhes).

Veja a seguir exemplos de restrições:
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

O exemplo a seguir está em erro porque causa uma circularidade no grafo de dependência dos parâmetros de tipo:
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

A ***classe base efetiva*** de um parâmetro `T` de tipo é definida da seguinte maneira:

*  Se `T` o não tiver restrições primárias ou restrições de parâmetro de tipo, sua `object`classe base efetiva será.
*  Se `T` tiver a restrição de tipo de valor, sua classe base `System.ValueType`efetiva será.
*  Se `T` o tiver uma restrição `C` class_type, mas não houver restrições *type_parameter* , sua classe `C`base efetiva será.
*  Se `T` não tiver nenhuma restrição *class_type* , mas tiver uma ou mais restrições *type_parameter* , sua classe base efetiva será o tipo mais abrangedo ([operadores de conversão levantados](conversions.md#lifted-conversion-operators)) no conjunto de classes base efetivas de seu *type_* restrições de parâmetro. As regras de consistência asseguram que exista um tipo mais abrangente.
*  Se `T` o tiver uma restrição *class_type* e uma ou mais restrições *type_parameter* , sua classe base efetiva será o tipo mais abrangedo ([operadores de conversão levantados](conversions.md#lifted-conversion-operators)) no conjunto que consiste em *class_type* restrição de `T` e as classes base efetivas de suas restrições *type_parameter* . As regras de consistência asseguram que exista um tipo mais abrangente.
*  Se `T` o tiver a restrição de tipo de referência, mas não houver restrições *class_type* , `object`sua classe base efetiva será.

Para fins dessas regras, se T `V` tiver uma restrição que seja um *value_type*, use em vez disso, o tipo `V` base mais específico é um *class_type*. Isso nunca pode ocorrer em uma restrição explicitamente determinada, mas pode ocorrer quando as restrições de um método genérico são implicitamente herdadas por uma declaração de método de substituição ou uma implementação explícita de um método de interface.

Essas regras garantem que a classe base efetiva seja sempre uma *class_type*.

O ***conjunto de interfaces efetivas*** de um `T` parâmetro de tipo é definido da seguinte maneira:

*  Se `T` não tiver nenhum *secondary_constraints*, seu conjunto de interface eficaz estará vazio.
*  Se `T` tiver restrições *interface_type* , mas nenhuma restrição de *type_parameter* , seu conjunto de interface eficaz será seu conjunto de restrições *interface_type* .
*  Se `T` o não tiver restrições *interface_type* , mas tiver restrições *type_parameter* , seu conjunto de interface eficaz será a União dos conjuntos de interface efetiva de suas restrições *type_parameter* .
*  Se `T` o tiver restrições de *interface_type* e restrições de *type_parameter* , seu conjunto de interface eficaz será a União de seu conjunto de restrições de *interface_type* e os conjuntos de interface efetivos de seu *type_parameter* restrições.

Um parâmetro de tipo é ***conhecido como um tipo de referência*** se ele tiver a restrição de tipo de referência ou se sua classe `object` base `System.ValueType`efetiva não for ou.

Os valores de um tipo de parâmetro de tipo restrito podem ser usados para acessar os membros de instância implícitos pelas restrições. No exemplo
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
os métodos de `IPrintable` podem ser invocados diretamente `x` no `T` porque o é restrito a sempre implementar `IPrintable`.

### <a name="class-body"></a>Corpo da classe

O *class_body* de uma classe define os membros dessa classe.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Tipos parciais

Uma declaração de tipo pode ser dividida em várias ***declarações de tipo parciais***. A declaração de tipo é construída de suas partes seguindo as regras nesta seção, momento ela é tratada como uma única declaração durante o restante do processamento de tempo de compilação e tempo de execução do programa.

Um *class_declaration*, *struct_declaration* ou *interface_declaration* representa uma declaração de tipo parcial se ele incluir `partial` um modificador. `partial`Não é uma palavra-chave e atua apenas como um modificador se aparecer imediatamente antes de uma das palavras- `class`chave `struct` , `interface` ou em uma declaração de tipo ou antes do `void` tipo em uma declaração de método. Em outros contextos, ele pode ser usado como um identificador normal.

Cada parte de uma declaração de tipo parcial deve incluir `partial` um modificador. Ele deve ter o mesmo nome e ser declarado no mesmo namespace ou em uma declaração de tipo que as outras partes. O `partial` modificador indica que partes adicionais da declaração de tipo podem existir em outro lugar, mas a existência dessas partes adicionais não é um requisito; ela é válida para um tipo com uma única declaração para `partial` incluir o modificador.

Todas as partes de um tipo parcial devem ser compiladas em conjunto, de modo que as partes possam ser mescladas em tempo de compilação em uma única declaração de tipo. Tipos parciais especificamente não permitem que tipos já compilados sejam estendidos.

Tipos aninhados podem ser declarados em várias partes `partial` usando o modificador. Normalmente, o tipo recipiente é declarado usando `partial` também, e cada parte do tipo aninhado é declarada em uma parte diferente do tipo recipiente.

O `partial` modificador não é permitido em declarações delegate ou enum.

### <a name="attributes"></a>Atributos

Os atributos de um tipo parcial são determinados pela combinação, em uma ordem não especificada, os atributos de cada uma das partes. Se um atributo for colocado em várias partes, será equivalente a especificar o atributo várias vezes no tipo. Por exemplo, as duas partes:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
são equivalentes a uma declaração como:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Os atributos nos parâmetros de tipo são combinados de maneira semelhante.

### <a name="modifiers"></a>Modificadores

Quando uma declaração de tipo parcial inclui uma especificação de acessibilidade `public`( `protected`os `internal`modificadores `private` ,, e), ela deve concordar com todas as outras partes que incluem uma especificação de acessibilidade. Se nenhuma parte de um tipo parcial incluir uma especificação de acessibilidade, o tipo receberá a acessibilidade padrão apropriada (a[acessibilidade declarada](basic-concepts.md#declared-accessibility)).

Se uma ou mais declarações parciais de um tipo aninhado `new` incluírem um modificador, nenhum aviso será relatado se o tipo aninhado ocultar um membro herdado ([ocultando a herança](basic-concepts.md#hiding-through-inheritance)).

Se uma ou mais declarações parciais de uma classe incluírem um `abstract` modificador, a classe será considerada abstrata ([classes abstratas](classes.md#abstract-classes)). Caso contrário, a classe será considerada não abstrata.

Se uma ou mais declarações parciais de uma classe incluírem um `sealed` modificador, a classe será considerada selada ([classes seladas](classes.md#sealed-classes)). Caso contrário, a classe será considerada sem lacre.

Observe que uma classe não pode ser abstrata e selada.

Quando o `unsafe` modificador é usado em uma declaração de tipo parcial, somente essa parte específica é considerada um contexto não seguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parâmetros de tipo e restrições

Se um tipo genérico for declarado em várias partes, cada parte deverá declarar os parâmetros de tipo. Cada parte deve ter o mesmo número de parâmetros de tipo e o mesmo nome para cada parâmetro de tipo, em ordem.

Quando uma declaração de tipo genérico parcial inclui restrições`where` (cláusulas), as restrições devem concordar com todas as outras partes que incluem restrições. Especificamente, cada parte que inclui restrições deve ter restrições para o mesmo conjunto de parâmetros de tipo e, para cada parâmetro de tipo, os conjuntos de restrições primária, secundária e de construtor devem ser equivalentes. Dois conjuntos de restrições são equivalentes se contiverem os mesmos membros. Se nenhuma parte de um tipo genérico parcial especificar restrições de parâmetro de tipo, os parâmetros de tipo serão considerados irrestrito.

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
está correto porque essas partes que incluem restrições (as duas primeiras) efetivamente especificam o mesmo conjunto de restrições primárias, secundárias e de construtor para o mesmo conjunto de parâmetros de tipo, respectivamente.

### <a name="base-class"></a>Classe base

Quando uma declaração de classe parcial inclui uma especificação de classe base, ela deve concordar com todas as outras partes que incluem uma especificação de classe base. Se nenhuma parte de uma classe parcial incluir uma especificação de classe base, a classe base `System.Object` se tornará ([classes base](classes.md#base-classes)).

### <a name="base-interfaces"></a>Interfaces base

O conjunto de interfaces base para um tipo declarado em várias partes é a União das interfaces base especificadas em cada parte. Uma interface base específica só pode ser nomeada uma vez em cada parte, mas é permitida para várias partes nomear as mesmas interfaces base. Deve haver apenas uma implementação dos membros de qualquer interface base fornecida.

No exemplo
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
o conjunto de interfaces base para a `C` classe `IA`é `IB`, e `IC`.

Normalmente, cada parte fornece uma implementação das interfaces declaradas nessa parte; no entanto, isso não é um requisito. Uma parte pode fornecer a implementação para uma interface declarada em uma parte diferente:
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

Com exceção dos métodos parciais ([métodos parciais](classes.md#partial-methods)), o conjunto de membros de um tipo declarado em várias partes é simplesmente a União do conjunto de membros declarado em cada parte. Os corpos de todas as partes da declaração de tipo compartilham o mesmo espaço de declaração ([declarações](basic-concepts.md#declarations)), e o escopo de cada membro ([escopos](basic-concepts.md#scopes)) se estende aos corpos de todas as partes. O domínio de acessibilidade de qualquer membro sempre inclui todas as partes do tipo delimitador; um `private` membro declarado em uma parte pode ser acessado livremente de outra parte. É um erro de tempo de compilação para declarar o mesmo membro em mais de uma parte do tipo, a menos que esse membro seja um tipo com `partial` o modificador.

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

A ordenação de membros dentro de um tipo é raramente C# significativa para o código, mas pode ser significativa ao fazer a interface com outras linguagens e ambientes. Nesses casos, a ordenação de membros dentro de um tipo declarado em várias partes é indefinida.

### <a name="partial-methods"></a>Métodos parciais

Os métodos parciais podem ser definidos em uma parte de uma declaração de tipo e implementados em outro. A implementação é opcional; Se nenhuma parte implementar o método parcial, a declaração de método parcial e todas as chamadas para ele serão removidas da declaração de tipo resultante da combinação das partes.

Os métodos parciais não podem definir modificadores de acesso, `private`mas são implicitamente. O tipo de retorno deve `void`ser, e seus parâmetros não podem `out` ter o modificador. O identificador `partial` será reconhecido como uma palavra-chave especial em uma declaração de método somente se aparecer imediatamente `void` antes do tipo; caso contrário, ele poderá ser usado como um identificador normal. Um método parcial não pode implementar explicitamente métodos de interface.

Há dois tipos de declarações de método parciais: Se o corpo da declaração do método for um ponto e vírgula, a declaração será considerada uma ***declaração de método parcial de definição***. Se o corpo for fornecido como um *bloco*, a declaração será considerada uma declaração de ***método parcial de implementação***. Em todas as partes de uma declaração de tipo, pode haver apenas uma definição de declaração de método parcial com uma determinada assinatura, e pode haver apenas uma implementação de declaração de método parcial com uma determinada assinatura. Se uma declaração de método parcial de implementação for fornecida, uma declaração de método parcial de definição correspondente deverá existir e as declarações deverão corresponder conforme especificado no seguinte:

* As declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem), nome do método, número de parâmetros de tipo e número de parâmetros.
* Os parâmetros correspondentes nas declarações devem ter os mesmos modificadores (embora não necessariamente na mesma ordem) e os mesmos tipos (diferenças de módulo em nomes de parâmetro de tipo).
* Os parâmetros de tipo correspondentes nas declarações devem ter as mesmas restrições (diferenças de módulo em nomes de parâmetro de tipo).

Uma declaração de método parcial de implementação pode aparecer na mesma parte da declaração de método parcial de definição correspondente.

Apenas um método parcial de definição participa da resolução de sobrecarga. Portanto, se uma declaração de implementação for ou não determinada, as expressões de invocação podem ser resolvidas para invocações do método parcial. Como um método parcial sempre retorna `void`, essas expressões de invocação sempre serão instruções de expressão. Além disso, como um método parcial é implicitamente `private`, essas instruções sempre ocorrerão dentro de uma das partes da declaração de tipo dentro da qual o método parcial é declarado.

Se nenhuma parte de uma declaração de tipo parcial contiver uma declaração de implementação para um determinado método parcial, qualquer instrução de expressão invocando-a será simplesmente removida da declaração de tipo combinado. Portanto, a expressão de invocação, incluindo quaisquer expressões constituintes, não tem nenhum efeito no tempo de execução. O próprio método parcial também é removido e não será um membro da declaração de tipo combinado.

Se existir uma declaração de implementação para um determinado método parcial, as invocações dos métodos parciais serão mantidas. O método parcial fornece a elevação para uma declaração de método semelhante à declaração de método parcial de implementação, exceto para o seguinte:

* O `partial` modificador não está incluído
* Os atributos na declaração de método resultante são os atributos combinados da definição e a declaração de método parcial de implementação em ordem não especificada. Duplicatas não são removidas.
* Os atributos nos parâmetros da declaração de método resultante são os atributos combinados dos parâmetros correspondentes da definição e a declaração de método parcial de implementação em ordem não especificada. Duplicatas não são removidas.

Se uma declaração de definição, mas não uma declaração de implementação, for fornecida para um método parcial M, as seguintes restrições serão aplicáveis:

* É um erro de tempo de compilação para criar um delegate para o método ([expressões de criação de delegado](expressions.md#delegate-creation-expressions)).
* É um erro de tempo de compilação para fazer referência `M` a dentro de uma função anônima que é convertida em um tipo[de árvore de expressão (avaliação de conversões de função anônima para tipos de árvore de expressão](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* As expressões que ocorrem como parte de uma invocação `M` do não afetam o estado de atribuição definido ([atribuição definitiva](variables.md#definite-assignment)), o que pode levar a erros em tempo de compilação.
* `M`Não pode ser o ponto de entrada para um aplicativo ([inicialização do aplicativo](basic-concepts.md#application-startup)).

Os métodos parciais são úteis para permitir que uma parte de uma declaração de tipo Personalize o comportamento de outra parte, por exemplo, uma que seja gerada por uma ferramenta. Considere a seguinte declaração de classe parcial:
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

Se essa classe for compilada sem nenhuma outra parte, a definição de declarações de método parcial e suas invocações será removida e a declaração de classe combinada resultante será equivalente à seguinte:
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

Suponha que outra parte seja fornecida, no entanto, que fornece declarações de implementação dos métodos parciais:
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

Em seguida, a declaração de classe combinada resultante será equivalente à seguinte:
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

Embora cada parte de um tipo extensível deva ser declarada dentro do mesmo namespace, as partes normalmente são escritas em diferentes declarações de namespace. Portanto, diretivas `using` diferentes ([usando diretivas](namespaces.md#using-directives)) podem estar presentes para cada parte. Ao interpretar nomes simples ([inferência de tipos](expressions.md#type-inference)) dentro de uma parte, `using` somente as diretivas das declarações de namespace que envolvem essa parte são consideradas. Isso pode resultar no mesmo identificador com significados diferentes em diferentes partes:
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

Os membros de uma classe consistem nos membros introduzidos por seus *class_member_declaration*s e nos membros herdados da classe base direta.

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

*  Constantes, que representam valores constantes associados à classe ([constantes](classes.md#constants)).
*  Campos, que são as variáveis da classe ([campos](classes.md#fields)).
*  Métodos, que implementam os cálculos e as ações que podem ser executadas pela classe ([métodos](classes.md#methods)).
*  Propriedades, que definem características nomeadas e as ações associadas à leitura e à gravação dessas características ([Propriedades](classes.md#properties)).
*  Eventos, que definem as notificações que podem ser geradas pela classe ([eventos](classes.md#events)).
*  Indexadores, que permitem que as instâncias da classe sejam indexadas da mesma forma (sintaticamente) como matrizes ([indexadores](classes.md#indexers)).
*  Operadores, que definem os operadores de expressão que podem ser aplicados a instâncias da classe ([operadores](classes.md#operators)).
*  Construtores de instância, que implementam as ações necessárias para inicializar instâncias da classe ([construtores de instância](classes.md#instance-constructors))
*  Destruidores, que implementam as ações a serem executadas antes que as instâncias da classe sejam descartadas permanentemente ([destruidores](classes.md#destructors)).
*  Construtores estáticos, que implementam as ações necessárias para inicializar a própria classe ([construtores estáticos](classes.md#static-constructors)).
*  Tipos, que representam os tipos que são locais para a classe ([tipos aninhados](classes.md#nested-types)).

Membros que podem conter código executável são coletivamente conhecidos como *membros da função* do tipo de classe. Os membros da função de um tipo de classe são os métodos, as propriedades, os eventos, os indexadores, os operadores, os construtores de instância, os destruidores e os construtores estáticos desse tipo de classe.

Um *class_declaration* cria um novo espaço de declaração ([declarações](basic-concepts.md#declarations)) e os *class_member_declaration*s imediatamente contidos pelo *class_declaration* introduzem novos membros nesse espaço de declaração. As regras a seguir se aplicam a *class_member_declaration*s:

*  Construtores de instância, destruidores e construtores estáticos devem ter o mesmo nome que a classe de circunscrição imediata. Todos os outros membros devem ter nomes que diferem do nome da classe imediatamente delimitadora.
*  O nome de uma constante, campo, propriedade, evento ou tipo deve ser diferente dos nomes de todos os outros membros declarados na mesma classe.
*  O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe. Além disso, a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe podem não ter assinaturas que diferem exclusivamente por `ref` e `out`.
*  A assinatura de um construtor de instância deve ser diferente das assinaturas de todos os outros construtores de instância declarados na mesma classe, e dois construtores declarados na mesma classe podem não ter assinaturas que `ref` diferem exclusivamente pelo and. `out`
*  A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.
*  A assinatura de um operador deve ser diferente das assinaturas de todos os outros operadores declarados na mesma classe.

Os membros herdados de um tipo de classe ([herança](classes.md#inheritance)) não fazem parte do espaço de declaração de uma classe. Assim, uma classe derivada tem permissão para declarar um membro com o mesmo nome ou assinatura que um membro herdado (que em vigor oculta o membro herdado).

### <a name="the-instance-type"></a>O tipo de instância

Cada declaração de classe tem um tipo de associado associado ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)), o ***tipo de instância***. Para uma declaração de classe genérica, o tipo de instância é formado criando um tipo construído ([tipos construídos](types.md#constructed-types)) da declaração de tipo, com cada um dos argumentos de tipo fornecidos sendo o parâmetro de tipo correspondente. Como o tipo de instância usa os parâmetros de tipo, ele só pode ser usado quando os parâmetros de tipo estão no escopo; ou seja, dentro da declaração de classe. O tipo de instância é o tipo `this` de para código escrito dentro da declaração de classe. Para classes não genéricas, o tipo de instância é simplesmente a classe declarada. O exemplo a seguir mostra várias declarações de classe junto com seus tipos de instância: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Membros de tipos construídos

Os membros não herdados de um tipo construído são obtidos pela substituição, para cada *type_parameter* na declaração de membro, o *type_argument* correspondente do tipo construído. O processo de substituição é baseado no significado semântico das declarações de tipo e não é simplesmente a substituição textual.

Por exemplo, considerando a declaração de classe genérica
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
o tipo `Gen<int[],IComparable<string>>` construído tem os seguintes membros:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

O `a` tipo do membro na Declaração `Gen` de classe genérica é "matriz bidimensional de `T`", portanto, o tipo do membro `a` no tipo construído acima é "matriz bidimensional de uma matriz unidimensional de `int`", ou `int[,][]`.

Em membros da função de instância, o `this` tipo de é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da declaração que a contém.

Todos os membros de uma classe genérica podem usar parâmetros de tipo de qualquer classe delimitadora, seja diretamente ou como parte de um tipo construído. Quando um tipo construído específico fechado ([tipos abertos e fechados](types.md#open-and-closed-types)) é usado em tempo de execução, cada uso de um parâmetro de tipo é substituído pelo argumento de tipo real fornecido para o tipo construído. Por exemplo:
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

Uma classe ***herda*** os membros de seu tipo de classe base direta. Herança significa que uma classe implicitamente contém todos os membros de seu tipo de classe base direta, exceto para construtores de instância, destruidores e construtores estáticos da classe base. Alguns aspectos importantes da herança são:

*  A herança é transitiva. Se `C` é derivado de `B`, e `B` é derivado de `A`, `C` herda os membros declarados `B` em, bem como os membros declarados em. `A`
*  Uma classe derivada estende sua classe base direta. Uma classe derivada pode adicionar novos membros aos que ela herda, mas ela não pode remover a definição de um membro herdado.
*  Construtores de instância, destruidores e construtores estáticos não são herdados, mas todos os outros membros são, independentemente de sua acessibilidade declarada ([acesso de membro](basic-concepts.md#member-access)). No entanto, dependendo da acessibilidade declarada, os membros herdados podem não estar acessíveis em uma classe derivada.
*  Uma classe derivada pode ***ocultar*** membros herdados ([ocultando a herança](basic-concepts.md#hiding-through-inheritance)) declarando novos membros com o mesmo nome ou assinatura. No entanto, observe que ocultar um membro herdado não remove esse membro — ele simplesmente torna esse membro inacessível diretamente por meio da classe derivada.
*  Uma instância de uma classe contém um conjunto de todos os campos de instância declarados na classe e suas classes base, e uma conversão implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe de um tipo de classe derivada para qualquer um de seus tipos de classe base. Assim, uma referência a uma instância de uma classe derivada pode ser tratada como uma referência a uma instância de qualquer uma de suas classes base.
*  Uma classe pode declarar métodos, propriedades e indexadores virtuais, e classes derivadas podem substituir a implementação desses membros da função. Isso permite que as classes exibam o comportamento polimórfico em que as ações executadas por uma invocação de membro de função variam dependendo do tipo de tempo de execução da instância por meio da qual esse membro de função é invocado.

O membro herdado de um tipo de classe construído são os membros do tipo de classe base imediato ([classes base](classes.md#base-classes)), que é encontrado substituindo os argumentos de tipo do tipo construído para cada ocorrência dos parâmetros de tipo correspondentes noespecificação de class_base. Esses membros, por sua vez, são transformados substituindo, para cada *type_parameter* na declaração de membro, o *type_argument* correspondente da especificação *class_base* .

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

No exemplo acima, o `D<int>` tipo construído tem um membro `public int G(string s)` não herdado obtido pela substituição do argumento `int` de tipo para o parâmetro `T`de tipo. `D<int>`também tem um membro herdado da Declaração `B`de classe. Esse membro herdado é determinado pela primeira vez que determina o `B<int[]>` tipo `D<int>` de classe base `int` de `T` substituindo para na especificação `B<T[]>`de classe base. Em seguida, como um argumento de `B`tipo `int[]` para, é substituído `U` por `public U F(long index)`no, produzindo o membro `public int[] F(long index)`herdado.

### <a name="the-new-modifier"></a>O novo modificador

Um *class_member_declaration* tem permissão para declarar um membro com o mesmo nome ou assinatura de um membro herdado. Quando isso ocorre, o membro da classe derivada é dito para ***ocultar*** o membro da classe base. Ocultar um membro herdado não é considerado um erro, mas faz com que o compilador emita um aviso. Para suprimir o aviso, a declaração do membro da classe derivada pode incluir `new` um modificador para indicar que o membro derivado deve ocultar o membro base. Este tópico é abordado mais detalhadamente em [ocultar por meio de herança](basic-concepts.md#hiding-through-inheritance).

Se um `new` modificador for incluído em uma declaração que não oculta um membro herdado, um aviso para esse efeito será emitido. Esse aviso é suprimido com a remoção `new` do modificador.

### <a name="access-modifiers"></a>Modificadores de acesso

Um *class_member_declaration* pode ter qualquer um dos cinco tipos possíveis de acessibilidade declarada ([acessibilidade declarada](basic-concepts.md#declared-accessibility)) `protected`: `internal` `public`, `protected internal`, `private`, ou. Exceto para a `protected internal` combinação, é um erro de tempo de compilação para especificar mais de um modificador de acesso. Quando um *class_member_declaration* não inclui nenhum modificador de acesso, `private` é assumido.

### <a name="constituent-types"></a>Tipos constituintes

Os tipos que são usados na declaração de um membro são chamados de tipos constituintes desse membro. Os tipos constituintes possíveis são o tipo de uma constante, campo, propriedade, evento ou indexador, o tipo de retorno de um método ou operador e os tipos de parâmetro de um construtor de método, indexador, operador ou instância. Os tipos constituintes de um membro devem ser pelo menos tão acessíveis quanto o próprio membro ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Membros estáticos e de instância

Os membros de uma classe são membros ***estáticos*** ou ***membros de instância***. Em geral, é útil considerar os membros estáticos como pertencentes a tipos de classe e membros de instância como pertencentes a objetos (instâncias de tipos de classe).

Quando um campo, método, propriedade, evento, operador ou declaração de Construtor inclui um `static` modificador, ele declara um membro estático. Além disso, uma constante ou declaração de tipo declara implicitamente um membro estático. Os membros estáticos têm as seguintes características:

*  `M` Quando um membro estático é referenciado em um *member_access* (acesso de[membro](expressions.md#member-access)) do `E.M`formulário `E` , deve-se indicar `M`um tipo contendo. É um erro de tempo de compilação para `E` indicar uma instância.
*  Um campo estático identifica exatamente um local de armazenamento a ser compartilhado por todas as instâncias de um determinado tipo de classe fechado. Independentemente de quantas instâncias de um determinado tipo de classe fechada forem criadas, há apenas uma cópia de um campo estático.
*  Um membro de função estática (método, propriedade, evento, operador ou Construtor) não opera em uma instância específica e é um erro de tempo de compilação para fazer referência a `this` esse membro de função.

Quando um campo, método, propriedade, evento, indexador, Construtor ou declaração de destruidor não inclui um `static` modificador, ele declara um membro de instância. (Um membro de instância é, às vezes, chamado de membro não estático.) Os membros da instância têm as seguintes características:

*  Quando um membro `M` de instância é referenciado em um *member_access* ([acesso de membro](expressions.md#member-access)) `E.M`do `E` formulário, deve-se indicar uma instância `M`de um tipo que contém. É um erro de tempo de ligação para `E` indicar um tipo.
*  Cada instância de uma classe contém um conjunto separado de todos os campos de instância da classe.
*  Um membro de função de instância (método, propriedade, indexador, Construtor de instância ou destruidor) opera em uma determinada instância da classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).

O exemplo a seguir ilustra as regras para acessar membros estáticos e de instância:
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

O `F` método mostra que em um membro da função de instância, um *Simple_name* ([nomes simples](expressions.md#simple-names)) pode ser usado para acessar membros de instância e membros estáticos. O `G` método mostra que em um membro de função estática, é um erro de tempo de compilação para acessar um membro de instância por meio de um *Simple_name*. O `Main` método mostra que em um *member_access* ([acesso de membro](expressions.md#member-access)), os membros da instância devem ser acessados por meio de instâncias e os membros estáticos devem ser acessados por meio de tipos.

### <a name="nested-types"></a>Tipos aninhados

Um tipo declarado em uma declaração de classe ou struct é chamado de ***tipo aninhado***. Um tipo declarado em uma unidade de compilação ou namespace é chamado de ***tipo não aninhado***.

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
Class `B` é um tipo aninhado porque é declarado dentro de `A`Class e Class `A` é um tipo não aninhado porque é declarado em uma unidade de compilação.

#### <a name="fully-qualified-name"></a>Nome totalmente qualificado

O nome totalmente qualificado ([nomes totalmente qualificados](basic-concepts.md#fully-qualified-names)) para um tipo aninhado `S.N` é `S` onde é o nome totalmente qualificado do tipo no qual o `N` tipo é declarado.

#### <a name="declared-accessibility"></a>Acessibilidade declarada

Tipos não aninhados podem ter `public` ou `internal` declarar acessibilidade e ter `internal` declarado acessibilidade por padrão. Os tipos aninhados também podem ter esses formulários de acessibilidade declarados, além de uma ou mais formas adicionais de acessibilidade declarada, dependendo se o tipo recipiente é uma classe ou estrutura:

*  Um tipo aninhado declarado em uma classe pode ter qualquer uma das cinco formas de acessibilidade declaradas `protected internal`( `protected``public`, `internal`,, `private`ou) e, como outros membros da classe, o `private` padrão é declarado acessibilidade.
*  Um tipo aninhado declarado em um struct pode ter qualquer uma das três formas de acessibilidade declarada `internal`(`public`, `private`ou) e, como outros membros de struct, usa `private` acessibilidade declarada por padrão.

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
declara uma classe `Node`aninhada privada.

#### <a name="hiding"></a>Ocultar

Um tipo aninhado pode ocultar ([ocultar o nome](basic-concepts.md#name-hiding)) um membro base. O `new` modificador é permitido em declarações de tipo aninhadas para que a ocultação possa ser expressa explicitamente. O exemplo
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
mostra uma classe `M` aninhada que oculta o método `M` definido em `Base`.

#### <a name="this-access"></a>Este acesso

Um tipo aninhado e seu tipo recipiente não têm uma relação especial com relação a *this_access* ([esse acesso](expressions.md#this-access)). Especificamente, `this` dentro de um tipo aninhado não pode ser usado para fazer referência a membros de instância do tipo recipiente. Nos casos em que um tipo aninhado precisa de acesso aos membros da instância de seu tipo recipiente, o acesso pode ser `this` fornecido fornecendo o para a instância do tipo recipiente como um argumento de construtor para o tipo aninhado. O exemplo a seguir
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
mostra essa técnica. Uma instância do `C` cria uma instância do `Nested` e passa seu próprio `this` Construtor `Nested`para o para fornecer acesso subsequente aos `C`membros da instância do.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Acesso a membros privados e protegidos do tipo recipiente

Um tipo aninhado tem acesso a todos os membros que são acessíveis para seu tipo recipiente, incluindo membros do tipo recipiente que têm e `private` `protected` têm acessibilidade declarada. O exemplo
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
mostra uma classe `C` que contém uma classe `Nested`aninhada. Em `Nested`, o método `G` chama o método `F` estático definido em `C`e `F` tem acessibilidade definida como particular.

Um tipo aninhado também pode acessar membros protegidos definidos em um tipo base de seu tipo recipiente. No exemplo
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
a `Derived.Nested` classe aninhada acessa o método `F` protegido definido na `Derived`classe base do, `Base`, chamando por meio de uma instância `Derived`do.

#### <a name="nested-types-in-generic-classes"></a>Tipos aninhados em classes genéricas

Uma declaração de classe genérica pode conter declarações de tipo aninhadas. Os parâmetros de tipo da classe delimitadora podem ser usados dentro dos tipos aninhados. Uma declaração de tipo aninhado pode conter parâmetros de tipo adicionais que se aplicam somente ao tipo aninhado.

Cada declaração de tipo contida em uma declaração de classe genérica é implicitamente uma declaração de tipo genérico. Ao gravar uma referência a um tipo aninhado em um tipo genérico, o tipo construído que o contém, incluindo seus argumentos de tipo, deve ser nomeado. No entanto, de dentro da classe externa, o tipo aninhado pode ser usado sem qualificação; o tipo de instância da classe externa pode ser usado implicitamente ao construir o tipo aninhado. O exemplo a seguir mostra três maneiras corretas diferentes de se referir a um `Inner`tipo construído criado a partir de; as duas primeiras são equivalentes:
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

Embora seja um estilo de programação inadequado, um parâmetro de tipo em um tipo aninhado pode ocultar um membro ou parâmetro de tipo declarado no tipo externo:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Nomes de membros reservados

Para facilitar a implementação C# de tempo de execução subjacente, para cada declaração de membro de origem que seja uma propriedade, um evento ou um indexador, a implementação deve reservar duas assinaturas de método com base no tipo de declaração de membro, seu nome e seu tipo. É um erro de tempo de compilação para um programa declarar um membro cuja assinatura corresponde a uma dessas assinaturas reservadas, mesmo que a implementação de tempo de execução subjacente não faça uso dessas reservas.

Os nomes reservados não introduzem declarações, portanto, eles não participam da pesquisa de membros. No entanto, as assinaturas de método reservado associadas de uma declaração participam da herança ([herança](classes.md#inheritance)) e podem ser `new` ocultadas com o modificador ([o novo modificador](classes.md#the-new-modifier)).

A reserva desses nomes serve para três finalidades:

*  Para permitir que a implementação subjacente use um identificador comum como um nome de método para obter ou definir o acesso C# ao recurso de idioma.
*  Para permitir que outras linguagens interoperem usando um identificador comum como um nome de método para obter ou definir C# o acesso ao recurso de idioma.
*  Para ajudar a garantir que a origem aceita por um compilador em conformidade é aceita por outro, tornando-se as especificidades dos nomes de membro reservados consistentes em todas as C# implementações.

A declaração de um destruidor ([destruidores](classes.md#destructors)) também faz com que uma assinatura seja reservada ([nomes de membros reservados para destruidores](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Nomes de membro reservados para propriedades

Para uma propriedade `P` ([Propriedades](classes.md#properties)) do tipo `T`, as seguintes assinaturas são reservadas:

```csharp
T get_P();
void set_P(T value);
```

Ambas as assinaturas são reservadas, mesmo que a propriedade seja somente leitura ou somente gravação.

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
uma classe `A` define uma propriedade `P`somente leitura, reservando, assim, assinaturas `get_P` para `set_P` os métodos e. Uma classe `B` deriva de `A` e oculta essas duas assinaturas reservadas. O exemplo produz a saída:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Nomes de membro reservados para eventos

Para um evento `E` ([eventos](classes.md#events)) do tipo `T`delegate, as seguintes assinaturas são reservadas:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Nomes de membro reservados para indexadores

Para um indexador ([indexadores](classes.md#indexers)) do tipo `T` com a lista `L`de parâmetros, as seguintes assinaturas são reservadas:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Ambas as assinaturas são reservadas, mesmo que o indexador seja somente leitura ou somente gravação.

Além disso, o `Item` nome do membro é reservado.

#### <a name="member-names-reserved-for-destructors"></a>Nomes de membro reservados para destruidores

Para uma classe que contém um destruidor ([destruidores](classes.md#destructors)), a assinatura a seguir é reservada:
```csharp
void Finalize();
```

## <a name="constants"></a>Constantes

Uma ***constante*** é um membro de classe que representa um valor constante: um valor que pode ser calculado em tempo de compilação. Um *constant_declaration* introduz uma ou mais constantes de um determinado tipo.

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

Um *constant_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um `new` modificador ([o novo modificador](classes.md#the-new-modifier)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)). Os atributos e os modificadores se aplicam a todos os membros declarados pelo *constant_declaration*. Embora constantes sejam consideradas membros estáticos, um *constant_declaration* não requer nem permite um `static` modificador. É um erro para que o mesmo modificador apareça várias vezes em uma declaração de constante.

O *tipo* de um *constant_declaration* especifica o tipo dos membros introduzidos pela declaração. O tipo é seguido por uma lista de *constant_declarator*s, cada um dos quais introduz um novo membro. Um *constant_declarator* consiste em um *identificador* que nomeia o membro, seguido por um token`=`"", seguido por um *constant_expression* ([expressões constantes](expressions.md#constant-expressions)) que fornece o valor do membro.

O *tipo* especificado em uma declaração de constante deve `sbyte`ser `byte` `int`, `short` `ushort` `uint` ,`long`,,,,,,, ,`float` `ulong` `char` `double`, ,`decimal` ,`string`,um enum_type ou um *reference_type*. `bool` Cada *constant_expression* deve produzir um valor do tipo de destino ou de um tipo que possa ser convertido para o tipo de destino por uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).

O *tipo* de uma constante deve ser pelo menos tão acessível quanto a própria constante ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O valor de uma constante é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou um *member_access* ([acesso de membro](expressions.md#member-access)).

Uma constante pode participar de um *constant_expression*. Portanto, uma constante pode ser usada em qualquer construção que exija um *constant_expression*. Exemplos dessas construções incluem `case` rótulos, `goto case` instruções, `enum` declarações de membro, atributos e outras declarações de constantes.

Conforme descrito em [expressões constantes](expressions.md#constant-expressions), um *constant_expression* é uma expressão que pode ser totalmente avaliada em tempo de compilação. Como a única maneira de criar um valor não nulo de um *reference_type* diferente `string` de é aplicar o `new` operador e, como o `new` operador não é permitido em um *constant_expression*, o único valor possível para constantes de *reference_type*s diferentes de `string` is `null`.

Quando é desejado um nome simbólico para um valor constante, mas quando o tipo desse valor não é permitido em uma declaração de constante, ou quando o valor não pode ser computado em tempo de compilaçãopor um constant_expression `readonly` , um campo ([campos somente leitura ](classes.md#readonly-fields)) pode ser usado em vez disso.

Uma declaração constante que declara várias constantes é equivalente a várias declarações de constantes únicas com os mesmos atributos, modificadores e tipo. Por exemplo
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

As constantes podem depender de outras constantes no mesmo programa, contanto que as dependências não sejam de natureza circular. O compilador organiza automaticamente para avaliar as declarações constantes na ordem apropriada. No exemplo
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
`A.Y`o compilador primeiro avalia, depois `B.Z`avalia e `A.X`, finalmente, avalia, produzindo os valores `10`, `11`e `12`. Declarações constantes podem depender de constantes de outros programas, mas essas dependências só são possíveis em uma direção. Fazendo referência ao exemplo acima, se `A` e `B` foram declarados em programas separados, seria `B.Z`possível `A.X` depender, mas `B.Z` , em seguida, não depender `A.Y`simultaneamente.

## <a name="fields"></a>Campos

Um ***campo*** é um membro que representa uma variável associada a um objeto ou classe. Um *field_declaration* introduz um ou mais campos de um determinado tipo.

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

Um *field_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), um `new` modificador ([o novo modificador](classes.md#the-new-modifier)), uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) `static` e um modificadores ([campos estáticos e de instância](classes.md#static-and-instance-fields)). Além disso, um *field_declaration* pode incluir um `readonly` modificador ([campos somente leitura](classes.md#readonly-fields)) `volatile` ou um modificador ([campos voláteis](classes.md#volatile-fields)), mas não ambos. Os atributos e os modificadores se aplicam a todos os membros declarados pelo *field_declaration*. É um erro para que o mesmo modificador apareça várias vezes em uma declaração de campo.

O *tipo* de um *field_declaration* especifica o tipo dos membros introduzidos pela declaração. O tipo é seguido por uma lista de *variable_declarator*s, cada um dos quais introduz um novo membro. Um *variable_declarator* consiste em um *identificador* que nomeia esse membro, opcionalmente seguido por um token`=`"" e um *variable_initializer* ([inicializadores de variável](classes.md#variable-initializers)) que fornece o valor inicial desse membro.

O *tipo* de um campo deve ser pelo menos tão acessível quanto o próprio campo ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O valor de um campo é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou um *member_access* ([acesso de membro](expressions.md#member-access)). O valor de um campo não ReadOnly é modificado usando uma *atribuição* ([operadores de atribuição](expressions.md#assignment-operators)). O valor de um campo não ReadOnly pode ser obtido e modificado usando o incremento de sufixo e os operadores de diminuição ([operadores de incremento e diminuição de sufixo](expressions.md#postfix-increment-and-decrement-operators)) e os operadores de incremento de prefixo e decréscimo ([incremento de prefixo e decréscimo operadores](expressions.md#prefix-increment-and-decrement-operators)).

Uma declaração de campo que declara vários campos é equivalente a várias declarações de campos únicos com os mesmos atributos, modificadores e tipo. Por exemplo
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

Quando uma declaração de campo inclui `static` um modificador, os campos introduzidos pela declaração são ***campos estáticos***. Quando nenhum `static` modificador está presente, os campos introduzidos pela declaração são ***campos de instância***. Campos estáticos e campos de instância são dois dos vários tipos de variáveis ([variáveis](variables.md)) com C#suporte do e às vezes eles são chamados de ***variáveis estáticas*** e ***variáveis de instância***, respectivamente.

Um campo estático não faz parte de uma instância específica; em vez disso, ele é compartilhado entre todas as instâncias de um tipo fechado ([tipos abertos e fechados](types.md#open-and-closed-types)). Não importa quantas instâncias de um tipo de classe fechada são criadas, há apenas uma cópia de um campo estático para o domínio de aplicativo associado.

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

Quando um campo é referenciado em *um member_access* ([acesso de membro](expressions.md#member-access)) do `E.M`formulário, `M` se for um campo estático `E` , deverá indicar um tipo `M`contendo, e `M` se for um campo de instância, e deverá denota uma instância de um tipo que `M`contém.

As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Campos somente leitura

Quando um *field_declaration* inclui um `readonly` modificador, os campos introduzidos pela declaração são ***campos somente leitura***. Atribuições diretas a campos ReadOnly só podem ocorrer como parte dessa declaração ou em um construtor de instância ou construtor estático na mesma classe. (Um campo ReadOnly pode ser atribuído a várias vezes nesses contextos.) Especificamente, as atribuições diretas a um `readonly` campo são permitidas apenas nos seguintes contextos:

*  No *variable_declarator* que introduz o campo (incluindo um *variable_initializer* na declaração).
*  Para um campo de instância, nos construtores de instância da classe que contém a declaração de campo; para um campo estático, no construtor estático da classe que contém a declaração de campo. Esses são também os únicos contextos nos quais é válido passar um `readonly` campo como um `out` parâmetro ou `ref` .

A tentativa de atribuir a um `readonly` campo ou passá-lo como `out` um `ref` parâmetro ou em qualquer outro contexto é um erro em tempo de compilação.

#### <a name="using-static-readonly-fields-for-constants"></a>Usando campos somente leitura estáticos para constantes

Um `static readonly` campo é útil quando um nome simbólico para um valor constante é desejado, mas quando o tipo do valor não é permitido em uma `const` declaração ou quando o valor não pode ser computado em tempo de compilação. No exemplo
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
os `Black`Membros `White` `const` ,, ,`Green`e não`Blue` podem ser declarados como membros porque seus valores não podem ser computados em tempo de compilação. `Red` No entanto, `static readonly` declará-las, em vez disso, tem muito o mesmo efeito.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Controle de versão de constantes e campos somente leitura estáticos

Os campos Constants e ReadOnly têm semântica de versão binária diferente. Quando uma expressão faz referência a uma constante, o valor da constante é obtido em tempo de compilação, mas quando uma expressão faz referência a um campo ReadOnly, o valor do campo não é obtido até o tempo de execução. Considere um aplicativo que consiste em dois programas separados:
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

Os `Program1` namespaces e `Program2` denotam dois programas que são compilados separadamente. Como `Program1.Utils.X` é declarado como um campo somente leitura estático, a saída do valor `Console.WriteLine` pela instrução não é conhecida em tempo de compilação, mas é obtida em tempo de execução. Portanto, se o valor de `X` for alterado e `Program1` for recompilado, a `Console.WriteLine` instrução produzirá o novo valor, mesmo se `Program2` não for recompilado. No entanto `X` , era uma constante, o valor `X` de teria sido obtido no momento `Program2` em que foi compilado e permaneceria inalterado por alterações no `Program1` até que `Program2` seja recompilado.

### <a name="volatile-fields"></a>Campos voláteis

Quando um *field_declaration* inclui um `volatile` modificador, os campos introduzidos por essa declaração são ***campos voláteis***.

Para campos não voláteis, as técnicas de otimização que reordenam instruções podem levar a resultados inesperados e imprevisíveis em programas multi-threaded que acessam campos sem sincronização, como o fornecido pelo *lock_statement* ([o instrução Lock](statements.md#the-lock-statement)). Essas otimizações podem ser executadas pelo compilador, pelo sistema de tempo de execução ou por hardware. Para campos voláteis, essas otimizações de reordenação são restritas:

*  Uma leitura de um campo volátil é chamada de ***leitura volátil***. Uma leitura volátil tem "adquirir semântica"; ou seja, é garantido que ocorra antes de qualquer referência à memória que ocorra depois dela na sequência de instruções.
*  Uma gravação de um campo volátil é chamada de ***gravação volátil***. Uma gravação volátil tem "semântica de liberação"; ou seja, é garantido que ocorra após qualquer referência de memória antes da instrução de gravação na sequência de instruções.

Essas restrições garantem que todos os threads observem as gravações voláteis executadas por qualquer outro thread na ordem em que elas foram realizadas. Uma implementação de conformidade não é necessária para fornecer uma única ordenação total de gravações voláteis, como visto de todos os threads de execução. O tipo de um campo volátil deve ser um dos seguintes:

*  Um *reference_type*.
*  O tipo `byte` `sbyte` ,`uint` ,,`System.UIntPtr`,,,, ,`bool`, ou`System.IntPtr`. `short` `ushort` `int` `char` `float`
*  Um *enum_type* tem um tipo base de enumeração `byte`de `sbyte` `short` `ushort`,,,, ou `uint`. `int`

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

Neste exemplo, o método `Main` inicia um novo thread que executa o método. `Thread2` Esse método armazena um valor em um campo não volátil chamado `result`e, em seguida, armazena `true` no campo `finished`volátil. O thread principal aguarda que o campo `finished` seja definido como `true`e, em seguida, lê o `result`campo. Como `finished` foi declarado `volatile`, o thread principal deve ler o valor `143` do campo `result`. Se o campo `finished` não tivesse sido declarado `volatile`, seria `result` permitido que o repositório fosse visível `finished`para o thread principal após o repositório e, portanto, para o thread principal ler o valor `0` do campo `result`. Declarar `finished` como um `volatile` campo impede qualquer inconsistência.

### <a name="field-initialization"></a>Inicialização de campo

O valor inicial de um campo, seja um campo estático ou um campo de instância, é o valor padrão ([valores padrão](variables.md#default-values)) do tipo do campo. Não é possível observar o valor de um campo antes que essa inicialização padrão tenha ocorrido, e um campo nunca é "não inicializado". O exemplo
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
Porque `b` e`i` são inicializados automaticamente para valores padrão.

### <a name="variable-initializers"></a>Inicializadores de variável

Declarações de campo podem incluir *variable_initializer*s. Para campos estáticos, inicializadores de variáveis correspondem a instruções de atribuição que são executadas durante a inicialização da classe. Para campos de instância, inicializadores de variáveis correspondem a instruções de atribuição que são executadas quando uma instância da classe é criada.

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
Porque uma atribuição `x` ocorre quando inicializadores de campo estáticos são executados e `i` atribuições `s` e ocorrem quando os inicializadores de campo de instância são executados.

A inicialização de valor padrão descrita na [inicialização do campo](classes.md#field-initialization) ocorre para todos os campos, incluindo campos que têm inicializadores variáveis. Assim, quando uma classe é inicializada, todos os campos estáticos nessa classe são inicializados primeiro com seus valores padrão e, em seguida, os inicializadores de campo estático são executados em ordem textual. Da mesma forma, quando uma instância de uma classe é criada, todos os campos de instância nessa instância são inicializados primeiro com seus valores padrão e, em seguida, os inicializadores de campo de instância são executados na ordem textual.

É possível que campos estáticos com inicializadores de variáveis sejam observados em seu estado de valor padrão. No entanto, isso é fortemente desencorajado como uma questão de estilo. O exemplo
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
exibe esse comportamento. Apesar das definições circulares de a e b, o programa é válido. Isso resulta na saída
```
a = 1, b = 2
```
Porque os `a` campos estáticos `b` e são inicializados para `0` (o valor `int`padrão de) antes de seus inicializadores serem executados. Quando o inicializador `a` for executado, o valor `b` de será zero e, `a` portanto, será `1`inicializado para. Quando o inicializador `b` for executado, o valor `a` de já `1`será e, `b` portanto, será `2`inicializado para.

#### <a name="static-field-initialization"></a>Inicialização de campo estático

Os inicializadores de variável de campo estático de uma classe correspondem a uma sequência de atribuições que são executadas na ordem textual na qual aparecem na declaração de classe. Se existir um construtor estático ([construtores estáticos](classes.md#static-constructors)) na classe, a execução dos inicializadores de campo estáticos ocorrerá imediatamente antes da execução desse construtor estático. Caso contrário, os inicializadores de campo estáticos são executados em uma hora dependente da implementação antes do primeiro uso de um campo estático dessa classe. O exemplo
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
como a execução do `X`inicializador e `Y`do inicializador do pode ocorrer em qualquer ordem; eles só são restritos a ocorrer antes das referências a esses campos. No entanto, no exemplo:
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
como as regras para quando os construtores estáticos são executados (conforme definido em [construtores estáticos](classes.md#static-constructors)) `B`fornecem esse construtor estático (e `B`, portanto, inicializadores de campo estáticos `A`) devem ser executados antes do estático inicializadores de construtor e de campo.

#### <a name="instance-field-initialization"></a>Inicialização do campo de instância

Os inicializadores de variável de campo de instância de uma classe correspondem a uma sequência de atribuições que são executadas imediatamente após a entrada para qualquer um dos construtores de instância ([inicializadores de Construtor](classes.md#constructor-initializers)) dessa classe. Os inicializadores de variável são executados na ordem textual em que aparecem na declaração de classe. O processo de criação e inicialização da instância de classe é descrito mais detalhadamente em [construtores de instância](classes.md#instance-constructors).

Um inicializador de variável para um campo de instância não pode fazer referência à instância que está sendo criada. Portanto, é um erro de tempo de compilação a ser `this` referenciado em um inicializador de variável, pois é um erro de tempo de compilação para um inicializador de variável referenciar qualquer membro de instância por meio de um *Simple_name*. No exemplo
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
o inicializador de `y` variável para resulta em um erro de tempo de compilação porque faz referência a um membro da instância que está sendo criada.

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

Um *method_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), `new` o ([o novo modificador](classes.md#the-new-modifier)), `static` ([estático e métodos de instância](classes.md#static-and-instance-methods)) `virtual` ,[(métodos virtuais](classes.md#virtual-methods)) `override` , (métodos de[substituição](classes.md#override-methods)), `sealed` ([métodos lacrados](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)) e `extern`([Métodos externos](classes.md#external-methods)) modificadores.

Uma declaração tem uma combinação válida de modificadores se todas as seguintes opções forem verdadeiras:

*  A declaração inclui uma combinação válida de modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)).
*  A declaração não inclui o mesmo modificador várias vezes.
*  A declaração inclui, no máximo, um dos seguintes modificadores `static`: `virtual`, e `override`.
*  A declaração inclui, no máximo, um dos seguintes modificadores `new` : `override`e.
*  Se a declaração incluir o `abstract` modificador, a declaração não incluirá nenhum dos seguintes modificadores: `static`, `virtual` `sealed` ou `extern`.
*  Se a declaração incluir o `private` modificador, a declaração não incluirá nenhum dos seguintes modificadores: `virtual`, `override`ou `abstract`.
*  Se a declaração incluir o `sealed` modificador, a declaração também incluirá `override` o modificador.
*  Se a declaração incluir o `partial` modificador, ele não incluirá nenhum dos seguintes modificadores: `protected` `new` `virtual`, `sealed` `private` `public` `internal`,,,,,, `override` , `abstract`ou .`extern`

Um método que tem o `async` modificador é uma função assíncrona e segue as regras descritas em [funções assíncronas](classes.md#async-functions).

O *return_type* de uma declaração de método especifica o tipo do valor calculado e retornado pelo método. O *return_type* é `void` se o método não retornar um valor. Se a declaração incluir o `partial` modificador, o tipo de retorno deverá `void`ser.

O *member_name* especifica o nome do método. A menos que o método seja uma implementação de membro de interface explícita ([implementações explícitas de membro de interface](interfaces.md#explicit-interface-member-implementations)), o *member_name* é simplesmente um *identificador*. Para uma implementação de membro de interface explícita, o *member_name* consiste em um *interface_type* seguido por`.`um "" e um *identificador*.

O *type_parameter_list* opcional especifica os parâmetros de tipo do método ([parâmetros de tipo](classes.md#type-parameters)). Se um *type_parameter_list* for especificado, o método será um ***método genérico***. Se o método tiver um `extern` modificador, um *type_parameter_list* não poderá ser especificado.

O *formal_parameter_list* opcional especifica os parâmetros do método ([parâmetros do método](classes.md#method-parameters)).

Os *type_parameter_constraints_clause*opcionais especificam restrições em parâmetros de tipo individuais ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) e só podem ser especificados se um *type_parameter_list* também for fornecido e o método não tiver um `override` modificador.

O *return_type* e cada um dos tipos referenciados no *formal_parameter_list* de um método devem ser pelo menos tão acessíveis quanto o próprio método ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O *method_body* é um ponto e vírgula, um ***corpo de instrução*** ou um corpo de ***expressão***. Um corpo de instrução consiste em um *bloco*, que especifica as instruções a serem executadas quando o método é invocado. Um corpo de `=>` expressão consiste em seguido por uma *expressão* e um ponto e vírgula e denota uma única expressão a ser executada quando o método é invocado. 

Para `abstract` métodos `extern` e, o *method_body* consiste simplesmente de um ponto e vírgula. Para `partial` métodos, o *method_body* pode consistir em um ponto e vírgula, um corpo de bloco ou um corpo de expressão. Para todos os outros métodos, o *method_body* é um corpo de bloco ou um corpo de expressão.

Se o *method_body* consistir em um ponto e vírgula, a declaração pode não `async` incluir o modificador.

O nome, a lista de parâmetros de tipo e a lista de parâmetros formais de um método definem a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do método. Especificamente, a assinatura de um método consiste em seu nome, o número de parâmetros de tipo e o número, os modificadores e os tipos de seus parâmetros formais. Para essas finalidades, qualquer parâmetro de tipo do método que ocorre no tipo de um parâmetro formal é identificado não por seu nome, mas por sua posição ordinal na lista de argumentos de tipo do método. O tipo de retorno não faz parte da assinatura de um método, nem os nomes dos parâmetros de tipo ou dos parâmetros formais.

O nome de um método deve ser diferente dos nomes de todos os outros não-métodos declarados na mesma classe. Além disso, a assinatura de um método deve diferir das assinaturas de todos os outros métodos declarados na mesma classe, e dois métodos declarados na mesma classe podem não ter assinaturas que `ref` diferem exclusivamente por e. `out`

As *type_parameter*s do método estão no escopo em todo o *method_declaration*e podem ser usadas para formar tipos em todo esse escopo em *return_type*, *method_body*e *type_parameter_constraints_clause*s, mas não em *atributos*.

Todos os parâmetros formais e parâmetros de tipo devem ter nomes diferentes.

### <a name="method-parameters"></a>Parâmetros do método

Os parâmetros de um método, se houver, são declarados pelo *formal_parameter_list*do método.

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

A lista de parâmetros formais consiste em um ou mais parâmetros separados por vírgulas, dos quais apenas o último pode ser um *parameter_array*.

Um *fixed_parameter* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um opcional `ref`, `out` ou `this` modificador, um *tipo*, um *identificador* e um *default_ opcional argumento*. Cada *fixed_parameter* declara um parâmetro do tipo fornecido com o nome fornecido. O `this` modificador designa o método como um método de extensão e só é permitido no primeiro parâmetro de um método estático. Os métodos de extensão são descritos mais detalhadamente em [métodos de extensão](classes.md#extension-methods).

Um *fixed_parameter* com um *default_argument* é conhecido como um ***parâmetro opcional***, enquanto que um *fixed_parameter* sem um *default_argument* é um ***parâmetro necessário***. Um parâmetro necessário pode não aparecer após um parâmetro opcional em um *formal_parameter_list*.

Um `ref` parâmetro `out` ou não pode ter um *default_argument*. A *expressão* em um *default_argument* deve ser uma das seguintes:

*  a *constant_expression*
*  uma expressão do formulário `new S()` em que `S` é um tipo de valor
*  uma expressão do formulário `default(S)` em que `S` é um tipo de valor

A *expressão* deve ser conversível implicitamente por uma identidade ou conversão anulável para o tipo do parâmetro.

Se os parâmetros opcionais ocorrerem em uma declaração de método parcial de implementação ([métodos parciais](classes.md#partial-methods)), uma implementação de membro de interface explícita ([implementações explícitas de membro de interface](interfaces.md#explicit-interface-member-implementations)) ou em uma declaração de indexador de parâmetro único ([ Indexadores](classes.md#indexers)) que o compilador deve dar a um aviso, já que esses membros nunca podem ser invocados de forma a permitir que os argumentos sejam omitidos.

Um *parameter_array* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), um `params` modificador, um *array_type*e um *identificador*. Uma matriz de parâmetros declara um único parâmetro do tipo de matriz fornecido com o nome fornecido. O *array_type* de uma matriz de parâmetros deve ser um tipo de matriz de dimensão única ([tipos de matriz](arrays.md#array-types)). Em uma invocação de método, uma matriz de parâmetros permite que um único argumento do tipo de matriz fornecido seja especificado ou que zero ou mais argumentos do tipo de elemento de matriz sejam especificados. Matrizes de parâmetros são descritas em mais detalhes em [matrizes de parâmetros](classes.md#parameter-arrays).

Um *parameter_array* pode ocorrer após um parâmetro opcional, mas não pode ter um valor padrão – a omissão de argumentos para um *parameter_array* resultaria na criação de uma matriz vazia.

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

No *formal_parameter_list* para `M`, `i` é um parâmetro de referência obrigatório, `d` é um parâmetro de valor obrigatório `b`, `s`, `o` e `t` são parâmetros de valor opcional e `a` é uma matriz de parâmetros.

Uma declaração de método cria um espaço de declaração separado para parâmetros, parâmetros de tipo e variáveis locais. Os nomes são introduzidos nesse espaço de declaração pela lista de parâmetros de tipo e pela lista de parâmetros formais do método e por declarações de variáveis locais no *bloco* do método. É um erro para dois membros de um espaço de declaração de método ter o mesmo nome. É um erro para o espaço de declaração de método e o espaço de declaração de variável local de um espaço de declaração aninhada para conter elementos com o mesmo nome.

Uma invocação de método ([invocações de método](expressions.md#method-invocations)) cria uma cópia, específica para essa invocação, dos parâmetros formais e das variáveis locais do método, e a lista de argumentos da invocação atribui valores ou referências de variáveis ao formal recém-criado parâmetro. Dentro do *bloco* de um método, parâmetros formais podem ser referenciados por seus identificadores em expressões *Simple_name* ([nomes simples](expressions.md#simple-names)).

Há quatro tipos de parâmetros formais:

*  Parâmetros de valor, que são declarados sem nenhum modificador.
*  Parâmetros de referência, que são declarados com o `ref` modificador.
*  Parâmetros de saída, que são declarados com o `out` modificador.
*  Matrizes de parâmetros, que são declaradas com o `params` modificador.

Conforme descrito em [assinaturas e sobrecargas](basic-concepts.md#signatures-and-overloading), os `ref` modificadores e `out` são parte da assinatura de um método, mas o `params` modificador não é.

#### <a name="value-parameters"></a>Parâmetros de valor

Um parâmetro declarado sem nenhum modificador é um parâmetro de valor. Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento correspondente fornecido na invocação do método.

Quando um parâmetro formal é um parâmetro de valor, o argumento correspondente em uma invocação de método deve ser uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de parâmetro formal.

Um método tem permissão para atribuir novos valores a um parâmetro de valor. Essas atribuições afetam apenas o local de armazenamento local representado pelo parâmetro value — elas não têm nenhum efeito sobre o argumento real fornecido na invocação do método.

#### <a name="reference-parameters"></a>Parâmetros de referência

Um parâmetro declarado com um `ref` modificador é um parâmetro de referência. Ao contrário de um parâmetro de valor, um parâmetro de referência não cria um novo local de armazenamento. Em vez disso, um parâmetro de referência representa o mesmo local de armazenamento que a variável fornecida como o argumento na invocação do método.

Quando um parâmetro formal é um parâmetro de referência, o argumento correspondente em uma invocação de método deve consistir `ref` na palavra-chave seguida por um *variable_reference* ([regras exatas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo Digite como o parâmetro formal. Uma variável deve ser definitivamente atribuída antes de poder ser passada como um parâmetro de referência.

Dentro de um método, um parâmetro de referência é sempre considerado definitivamente atribuído.

Um método declarado como iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de referência.

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

Para a invocação de `Swap` em `Main`, `x` representa `i` e `y` representa `j`. Assim, a invocação tem o efeito de trocar os valores de `i` e. `j`

Em um método que usa parâmetros de referência, é possível que vários nomes representem o mesmo local de armazenamento. No exemplo
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
a invocação do `F` no `G` passa uma referência para `s` o `a` e `b`o. Portanto, para essa invocação, os nomes `s`, `a`e `b` todos fazem referência ao mesmo local de armazenamento e as três atribuições modificam o campo `s`de instância.

#### <a name="output-parameters"></a>Parâmetros de saída

Um parâmetro declarado com um `out` modificador é um parâmetro de saída. Semelhante a um parâmetro de referência, um parâmetro de saída não cria um novo local de armazenamento. Em vez disso, um parâmetro de saída representa o mesmo local de armazenamento que a variável fornecida como o argumento na invocação do método.

Quando um parâmetro formal é um parâmetro de saída, o argumento correspondente em uma invocação de método deve consistir `out` na palavra-chave seguida por um *variable_reference* ([regras exatas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) do mesmo Digite como o parâmetro formal. Uma variável não precisa ser definitivamente atribuída antes de poder ser passada como um parâmetro de saída, mas após uma invocação em que uma variável foi passada como um parâmetro de saída, a variável é considerada definitivamente atribuída.

Dentro de um método, assim como uma variável local, um parâmetro de saída é inicialmente considerado não atribuído e deve ser definitivamente atribuído antes de seu valor ser usado.

Cada parâmetro de saída de um método deve ser definitivamente atribuído antes do retorno do método.

Um método declarado como um método parcial ([métodos parciais](classes.md#partial-methods)) ou um iterador ([iteradores](classes.md#iterators)) não pode ter parâmetros de saída.

Os parâmetros de saída normalmente são usados em métodos que produzem vários valores de retorno. Por exemplo:
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

Observe que as `dir` variáveis `name` e podem ser desatribuídas antes de serem passadas para `SplitPath`e que elas sejam consideradas definitivamente atribuídas após a chamada.

#### <a name="parameter-arrays"></a>Matrizes de parâmetros

Um parâmetro declarado com um `params` modificador é uma matriz de parâmetros. Se uma lista de parâmetros formais incluir uma matriz de parâmetros, ela deverá ser o último parâmetro na lista e deverá ser de um tipo de matriz unidimensional. Por exemplo, os tipos `string[]` e `string[][]` podem ser usados como o tipo de uma matriz de parâmetros, mas o `string[,]` tipo não pode. Não é possível combinar o `params` modificador com os `ref` modificadores e `out`.

Uma matriz de parâmetros permite que argumentos sejam especificados de uma das duas maneiras em uma invocação de método:

*  O argumento fornecido para uma matriz de parâmetros pode ser uma única expressão conversível implicitamente ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetros. Nesse caso, a matriz de parâmetros funciona precisamente como um parâmetro de valor.
*  Como alternativa, a invocação pode especificar zero ou mais argumentos para a matriz de parâmetros, em que cada argumento é uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros. Nesse caso, a invocação cria uma instância do tipo de matriz de parâmetros com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecidos e usa a instância de matriz recém-criada como o real argumento.

Exceto para permitir um número variável de argumentos em uma invocação, uma matriz de parâmetros é precisamente equivalente a um parâmetro de valor ([parâmetros de valor](classes.md#value-parameters)) do mesmo tipo.

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

A primeira invocação de `F` simplesmente passa a matriz `a` como um parâmetro de valor. A segunda invocação de `F` cria automaticamente um quatro elementos `int[]` com os valores de elemento fornecidos e passa essa instância de matriz como um parâmetro de valor. Da mesma forma, a terceira invocação de `F` cria um elemento `int[]` zero e passa essa instância como um parâmetro de valor. A segunda e terceira invocações são precisamente equivalentes a escrever:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Ao executar a resolução de sobrecarga, um método com uma matriz de parâmetros pode ser aplicável em seu formato normal ou em sua forma expandida ([membro de função aplicável](expressions.md#applicable-function-member)). A forma expandida de um método só estará disponível se a forma normal do método não for aplicável e somente se um método aplicável com a mesma assinatura do formulário expandido ainda não estiver declarado no mesmo tipo.

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

No exemplo, duas das possíveis formas expandidas do método com uma matriz de parâmetros já estão incluídas na classe como métodos regulares. Portanto, esses formulários expandidos não são considerados ao executar a resolução de sobrecarga e a primeira e terceira invocações de método, portanto, selecionam os métodos regulares. Quando uma classe declara um método com uma matriz de parâmetros, não é incomum também incluir alguns dos formulários expandidos como métodos regulares. Ao fazer isso, é possível evitar a alocação de uma instância de matriz que ocorre quando uma forma expandida de um método com uma matriz de parâmetros é invocada.

Quando o tipo de uma matriz de parâmetros `object[]`é, ocorre uma possível ambiguidade entre a forma normal do método e o formulário informado para um único `object` parâmetro. O motivo da ambiguidade é que um `object[]` é implicitamente conversível no tipo. `object` No entanto, a ambiguidade não apresenta nenhum problema, pois ela pode ser resolvida com a inserção de uma conversão, se necessário.

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

Na primeira e última invocações do `F`, a forma normal do `F` é aplicável porque existe uma conversão implícita do tipo de argumento para o tipo de parâmetro (ambos são do tipo `object[]`). Portanto, a resolução de sobrecarga seleciona a forma `F`normal de e o argumento é passado como um parâmetro de valor regular. Na segunda e terceira invocações, a forma normal de `F` não é aplicável porque não existe nenhuma conversão implícita do tipo de argumento para o tipo de parâmetro (o tipo `object` não pode ser convertido implicitamente `object[]`no tipo). No entanto, a forma `F` expandida do é aplicável, portanto, é selecionada pela resolução de sobrecarga. Como resultado, um elemento `object[]` One é criado pela invocação, e o único elemento da matriz é inicializado com o valor do argumento fornecido (que, por sua vez, é uma referência a um `object[]`).

### <a name="static-and-instance-methods"></a>Métodos estáticos e de instância

Quando uma declaração de método inclui `static` um modificador, esse método é considerado um método estático. Quando nenhum `static` modificador está presente, o método é considerado um método de instância.

Um método estático não funciona em uma instância específica e é um erro de tempo de compilação para fazer referência a `this` um método estático.

Um método de instância opera em uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)).

Quando um método é referenciado em *um member_access* ([acesso de membro](expressions.md#member-access)) do `E.M`formulário, `M` se for um método estático `E` , deverá denotar `M`um tipo contendo `M` e, se for um método de instância, deve-se indicar uma instância de um tipo que contém `M`. `E`

As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Métodos virtuais

Quando uma declaração de método de instância `virtual` inclui um modificador, esse método é considerado um método virtual. Quando nenhum `virtual` modificador está presente, o método é considerado um método não virtual.

A implementação de um método não virtual é invariável: A implementação é a mesma se o método é invocado em uma instância da classe na qual ela é declarada ou uma instância de uma classe derivada. Por outro lado, a implementação de um método virtual pode ser substituída por classes derivadas. O processo de substituição da implementação de um método virtual herdado é conhecido como ***substituição*** desse método ([métodos de substituição](classes.md#override-methods)).

Em uma invocação de método virtual, o ***tipo de tempo de execução*** da instância para o qual a invocação ocorre determina a implementação do método real a ser invocado. Em uma invocação de método não virtual, o ***tipo de tempo de compilação*** da instância é o fator determinante. Em termos precisos, quando um método `N` chamado é invocado com uma `A` lista de argumentos em uma instância com um tipo `C` de tempo de compilação e um `R` tipo de `R` tempo de `C` execução (onde é ou uma classe derivada de `C`), a invocação é processada da seguinte maneira:

*  Primeiro, a resolução de sobrecarga é `C`aplicada `N`a, `A`e, para selecionar um método `M` específico do conjunto de métodos declarado em e herdado `C`por. Isso é descrito em [invocações de método](expressions.md#method-invocations).
*  Em seguida, `M` se for um método não virtual, `M` será invocado.
*  Caso contrário `M` , é um método virtual e a implementação mais derivada de `M` com relação a `R` é invocada.

Para cada método virtual declarado em ou herdado por uma classe, existe uma ***implementação mais derivada*** do método em relação a essa classe. A implementação mais derivada de um método `M` virtual em relação a uma classe `R` é determinada da seguinte maneira:

*  Se `R` contiver a `virtual` declaração de `M`introdução de, essa será a implementação mais derivada `M`de.
*  Caso contrário, `R` se contiver `M`um `override` de, essa será a implementação mais derivada `M`de.
*  Caso contrário, a implementação mais derivada `M` de em relação `R` ao é igual à implementação mais derivada de `M` em relação à classe base direta do `R`.

O exemplo a seguir ilustra as diferenças entre métodos virtuais e não virtuais:
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

No exemplo, `A` apresenta um método `F` não virtual e um método `G`virtual. A classe `B` introduz um novo método `F`não virtual, ocultando o herdado `F`e também substitui o método `G`herdado. O exemplo produz a saída:
```
A.F
B.F
B.G
B.G
```

Observe que a instrução `a.G()` `B.G`invoca, não `A.G`. Isso ocorre porque o tipo de tempo de execução da instância (que é `B`), não o tipo de tempo de compilação da instância (que é `A`), determina a implementação do método real a ser invocado.

Como os métodos têm permissão para ocultar os métodos herdados, é possível que uma classe contenha vários métodos virtuais com a mesma assinatura. Isso não apresenta um problema de ambiguidade, pois todos, exceto o método mais derivado, estão ocultos. No exemplo
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
as `C` classes `D` e contêm dois métodos virtuais com a mesma assinatura: Aquele introduzido pelo `A` e aquele introduzido pelo `C`. O método introduzido por `C` oculta o método herdado de `A`. Assim, a declaração de substituição `D` em substitui o método introduzido `C`pelo, e não é possível `D` substituir o método introduzido pelo `A`. O exemplo produz a saída:
```
B.F
B.F
D.F
D.F
```

Observe que é possível invocar o método virtual oculto acessando uma instância do `D` por meio de um tipo menos derivado no qual o método não está oculto.

### <a name="override-methods"></a>Métodos de substituição

Quando uma declaração de método de instância `override` inclui um modificador, o método é considerado um ***método de substituição***. Um método override substitui um método virtual herdado pela mesma assinatura. Enquanto uma declaração de método virtual apresenta um novo método, uma declaração de método de substituição restringe um método virtual herdado existente fornecendo uma nova implementação do método.

O método substituído por uma `override` declaração é conhecido como ***método base substituído***. Para um método `M` override declarado em uma classe `C`, o método base substituído é determinado examinando cada tipo de classe base `C`de, começando com o tipo de `C` classe base direta e continuando com cada sucesso o tipo de classe base direta, até em um determinado tipo de classe base, pelo menos um método acessível está localizado, que `M` tem a mesma assinatura que após a substituição dos argumentos de tipo. Para fins de localização do método base substituído, um método é considerado acessível `public`se for `protected` `protected internal`, se for, se for, ou se `internal` for e declarado no mesmo programa que `C`o.

Um erro de tempo de compilação ocorre a menos que todos os itens a seguir sejam verdadeiros para uma declaração de substituição:

*  Um método base substituído pode ser localizado conforme descrito acima.
*  Há exatamente um desses métodos de base substituído. Essa restrição só terá efeito se o tipo de classe base for um tipo construído em que a substituição de argumentos de tipo torna a assinatura de dois métodos o mesmo.
*  O método base substituído é um método virtual, abstract ou override. Em outras palavras, o método base substituído não pode ser estático ou não virtual.
*  O método base substituído não é um método lacrado.
*  O método override e o método base substituído têm o mesmo tipo de retorno.
*  A declaração de substituição e o método base substituído têm a mesma acessibilidade declarada. Em outras palavras, uma declaração de substituição não pode alterar a acessibilidade do método virtual. No entanto, se o método base substituído for protegido internamente e for declarado em um assembly diferente do assembly que contém o método override, a acessibilidade declarada do método de substituição deverá ser protegida.
*  A declaração de substituição não especifica cláusulas de tipo-parâmetro-Constraints. Em vez disso, as restrições são herdadas do método base substituído. Observe que as restrições que são parâmetros de tipo no método substituído podem ser substituídas por argumentos de tipo na restrição herdada. Isso pode levar a restrições que não são legais quando explicitamente especificados, como tipos de valor ou tipos lacrados.

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

Uma declaração de substituição pode acessar o método base substituído usando um *base_access* ([acesso de base](expressions.md#base-access)). No exemplo
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
a `base.PrintFields()` invocação no `B` invoca o `PrintFields` método declarado em `A`. Um *base_access* desabilita o mecanismo de invocação virtual e simplesmente trata o método base como um método não virtual. A invocação `B` foi escrita `A` `PrintFields` `((A)this).PrintFields()`, ela invocaria recursivamente o método declarado em `B`, e não o declarado em, pois `PrintFields` é virtual e o tipo de tempo de execução de `((A)this)` é .`B`

Somente ao incluir um `override` modificador, um método pode substituir outro método. Em todos os outros casos, um método com a mesma assinatura que um método herdado simplesmente oculta o método herdado. No exemplo
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
o `F` método no `B` não inclui um `override` modificador e, portanto, não substitui `F` o método `A`em. Em vez disso `F` , o `B` método em oculta o método `A`em e um aviso é relatado porque a declaração não inclui um `new` modificador.

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
o `F` método em `B` oculta o método virtual `F` herdado de `A`. Como o novo `F` no `B` tem acesso privado, seu escopo inclui apenas o corpo da classe `B` de e não se estende `C`para. Portanto, a declaração de `F` no `C` tem permissão para substituir o `F` herdado `A`de.

### <a name="sealed-methods"></a>Métodos lacrados

Quando uma declaração de método de instância `sealed` inclui um modificador, esse método é considerado um ***método lacrado***. Se uma declaração de método de instância `sealed` incluir o modificador, ele também `override` deverá incluir o modificador. O uso do `sealed` modificador impede que uma classe derivada substitua ainda mais o método.

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
a classe `B` fornece dois métodos de substituição: `F` um método que tem `sealed` o modificador `G` e um método que não tem. `B`o uso do `modifier` desealed `C` impede a substituição `F`adicional.

### <a name="abstract-methods"></a>Métodos abstratos

Quando uma declaração de método de instância `abstract` inclui um modificador, esse método é considerado um ***método abstrato***. Embora um método abstract também seja implicitamente um método virtual, ele não pode ter o `virtual`modificador.

Uma declaração de método abstract apresenta um novo método virtual, mas não fornece uma implementação desse método. Em vez disso, as classes derivadas não abstratas são necessárias para fornecer sua própria implementação substituindo esse método. Como um método abstract não fornece implementação real, o *method_body* de um método abstract simplesmente consiste em um ponto-e-vírgula.

Declarações de método abstract só são permitidas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).

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
a `Shape` classe define a noção abstrata de um objeto de forma geométrica que pode ser pintada. O `Paint` método é abstrato porque não há nenhuma implementação padrão significativa. As `Ellipse` classes `Box` e são implementações concretas `Shape` . Como essas classes não são abstratas, elas são necessárias para substituir o `Paint` método e fornecer uma implementação real.

É um erro de tempo de compilação para um *base_access* ([acesso de base](expressions.md#base-access)) para referenciar um método abstrato. No exemplo
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
um erro de tempo de compilação é relatado para `base.F()` a invocação porque faz referência a um método abstrato.

Uma declaração de método abstract tem permissão para substituir um método virtual. Isso permite que uma classe abstrata force a reimplementação do método em classes derivadas e torna a implementação original do método indisponível. No exemplo
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
classe `A` declara um método virtual, a classe `B` substitui esse método por um método abstrato e a classe `C` substitui o método abstract para fornecer sua própria implementação.

### <a name="external-methods"></a>Métodos externos

Quando uma declaração de método inclui `extern` um modificador, esse método é considerado um ***método externo***. Os métodos externos são implementados externamente, normalmente usando uma linguagem C#diferente de. Como uma declaração de método externo não fornece implementação real, o *method_body* de um método externo simplesmente consiste em um ponto-e-vírgula. Um método externo não pode ser genérico.

O `extern` modificador é normalmente usado em conjunto com `DllImport` um atributo ([interoperação com componentes com e Win32](attributes.md#interoperation-with-com-and-win32-components)), permitindo que métodos externos sejam implementados por DLLs (bibliotecas de vínculo dinâmico). O ambiente de execução pode dar suporte a outros mecanismos nos quais implementações de métodos externos podem ser fornecidas.

Quando um método externo inclui um `DllImport` atributo, a declaração de método também deve incluir `static` um modificador. Este exemplo demonstra o uso do `extern` modificador e do `DllImport` atributo:
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

Quando uma declaração de método inclui `partial` um modificador, esse método é considerado um ***método parcial***. Os métodos parciais só podem ser declarados como membros de tipos parciais ([tipos parciais](classes.md#partial-types)) e estão sujeitos a várias restrições. Os métodos parciais são descritos mais detalhadamente em [métodos parciais](classes.md#partial-methods).

### <a name="extension-methods"></a>Métodos de extensão

Quando o primeiro parâmetro de um método inclui o `this` modificador, esse método é considerado um ***método de extensão***. Os métodos de extensão só podem ser declarados em classes estáticas não genéricas e não aninhadas. O primeiro parâmetro de um método de extensão não pode ter nenhum modificador `this`diferente de e o tipo de parâmetro não pode ser um tipo de ponteiro.

Veja a seguir um exemplo de uma classe estática que declara dois métodos de extensão:
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

Um método de extensão é um método estático regular. Além disso, onde sua classe estática delimitadora está no escopo, um método de extensão pode ser invocado usando a sintaxe de invocação do método de instância ([invocações de método de extensão](expressions.md#extension-method-invocations)), usando a expressão Receiver como o primeiro argumento.

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

O `Slice` método está disponível `string[]`no, e o `ToInt32` método está disponível em `string`, porque eles foram declarados como métodos de extensão. O significado do programa é o mesmo que o seguinte, usando chamadas de método estáticos comuns:
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

O *method_body* de uma declaração de método consiste em um corpo de bloco, um corpo de expressão ou um ponto-e-vírgula.

O ***tipo de resultado*** de um método `void` é se o tipo de `void`retorno for ou se o método for Async e o tipo de `System.Threading.Tasks.Task`retorno for. Caso contrário, o tipo de resultado de um método não assíncrono é seu tipo de retorno, e o tipo de resultado de um método assíncrono `System.Threading.Tasks.Task<T>` com `T`tipo de retorno é.

Quando um método tem um `void` tipo de resultado e um corpo de `return` bloco,[as instruções (a instrução return](statements.md#the-return-statement)) no bloco não têm permissão para especificar uma expressão. Se a execução do bloco de um método void for concluída normalmente (ou seja, o controle flui para fora do final do corpo do método), esse método simplesmente retorna ao seu chamador atual.
    
Quando um método tem um `void` resultado e um corpo de expressão, a `E` expressão deve ser um *statement_expression*e o corpo é exatamente equivalente a um corpo de bloco do formulário `{ E; }`.
    
Quando um método tem um tipo de resultado não void e um corpo de bloco, `return` cada instrução no bloco deve especificar uma expressão que é implicitamente conversível para o tipo de resultado. O ponto de extremidade de um corpo de bloco de um método de retorno de valor não deve ser acessível. Em outras palavras, em um método de retorno de valor com um corpo de bloco, o controle não tem permissão para fluir para fora do final do corpo do método.
    
Quando um método tem um tipo de resultado não void e um corpo de expressão, a expressão deve ser conversível implicitamente no tipo de resultado e o corpo é exatamente equivalente a um corpo de bloco do `{ return E; }`formulário.
    
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
o método de retorno `F` de valor resulta em um erro de tempo de compilação porque o controle pode fluir para fora do final do corpo do método. Os `G` métodos `H` e estão corretos porque todos os caminhos de execução possíveis terminam em uma instrução Return que especifica um valor de retorno. O `I` método está correto, pois seu corpo é equivalente a um bloco de instrução com apenas uma única instrução de retorno.

### <a name="method-overloading"></a>Sobrecarga de método

As regras de resolução de sobrecarga de método são descritas em [inferência de tipos](expressions.md#type-inference).

## <a name="properties"></a>Propriedades

Uma ***Propriedade*** é um membro que fornece acesso a uma característica de um objeto ou de uma classe. Exemplos de propriedades incluem o comprimento de uma cadeia de caracteres, o tamanho de uma fonte, a legenda de uma janela, o nome de um cliente e assim por diante. As propriedades são uma extensão natural dos campos – ambos são membros nomeados com tipos associados e a sintaxe para acessar campos e propriedades é a mesma. No entanto, diferentemente dos campos, as propriedades não denotam locais de armazenamento. Em vez disso, as propriedades têm ***acessadores*** que especificam as instruções a serem executadas quando os valores forem lidos ou gravados. Portanto, as propriedades fornecem um mecanismo para associar ações com a leitura e gravação de atributos de um objeto; Além disso, eles permitem que esses atributos sejam computados.

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

Um *property_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), `new` o ([o novo modificador](classes.md#the-new-modifier)), `static` ([estático e métodos de instância](classes.md#static-and-instance-methods)) `virtual` ,[(métodos virtuais](classes.md#virtual-methods)) `override` , (métodos de[substituição](classes.md#override-methods)), `sealed` ([métodos lacrados](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)) e `extern`([Métodos externos](classes.md#external-methods)) modificadores.

As declarações de propriedade estão sujeitas às mesmas regras que as declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.

O *tipo* de uma declaração de propriedade especifica o tipo da propriedade introduzida pela declaração e *member_name* especifica o nome da propriedade. A menos que a propriedade seja uma implementação de membro de interface explícita, o *member_name* é simplesmente um *identificador*. Para uma implementação explícita de membro de interface ([implementações explícitas de membro de interface](interfaces.md#explicit-interface-member-implementations)), o *member_name* consiste em`.`um *interface_type* seguido por um "" e um *identificador*.

O *tipo* de uma propriedade deve ser pelo menos tão acessível quanto a própria propriedade ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Um *property_body* pode consistir em um ***corpo de acessador*** ou um ***corpo de expressão***. Em um corpo de acessador, *accessor_declarations*, que deve ser`{`colocado entre os`}`tokens "" e "", declare os acessadores ([acessadores](classes.md#accessors)) da propriedade. Os acessadores especificam as instruções Executáveis associadas à leitura e gravação da propriedade.

Um corpo de `=>` expressão que consiste em seguido por uma *expressão* `E` e um ponto-e-vírgula é exatamente equivalente ao corpo `{ get { return E; } }`da instrução e, portanto, só pode ser usado para especificar propriedades somente getter em que o resultado de o getter é fornecido por uma única expressão.

Um *property_initializer* só pode ser fornecido para uma propriedade implementada automaticamente ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)) e causa a inicialização do campo subjacente de tais propriedades com o valor fornecido pela *expressão* .

Embora a sintaxe para acessar uma propriedade seja a mesma de um campo, uma propriedade não é classificada como uma variável. Portanto, não é possível passar uma propriedade como um `ref` argumento ou. `out`

Quando uma declaração de propriedade inclui `extern` um modificador, a propriedade é considerada uma ***Propriedade externa***. Como uma declaração de propriedade externa não fornece implementação real, cada uma de suas *accessor_declarations* consiste em um ponto-e-vírgula.

### <a name="static-and-instance-properties"></a>Propriedades de instância e estática

Quando uma declaração de propriedade inclui `static` um modificador, a propriedade é considerada uma ***propriedade estática***. Quando nenhum `static` modificador estiver presente, a propriedade será considerada uma ***propriedade de instância***.

Uma propriedade estática não está associada a uma instância específica e é um erro de tempo de compilação para se referir `this` aos acessadores de uma propriedade estática.

Uma propriedade de instância é associada a uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores dessa propriedade.

Quando uma propriedade é referenciada em *um member_access* ([acesso de membro](expressions.md#member-access)) do `E.M`formulário, `M` se é uma propriedade estática `E` , deve denotar `M`um tipo que `M` contém e se é uma instância Propriedade, E deve indicar uma instância de um tipo que `M`contém.

As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="accessors"></a>Acessadores

O *accessor_declarations* de uma propriedade especifica as instruções Executáveis associadas à leitura e gravação dessa propriedade.

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

As declarações de acessador consistem em um *get_accessor_declaration*, um *set_accessor_declaration*ou ambos. Cada declaração de acessador consiste `get` no `set` token ou seguido por um *accessor_modifier* opcional e um *accessor_body*.

O uso de *accessor_modifier*s é regido pelas seguintes restrições:

*  Um *accessor_modifier* não pode ser usado em uma interface ou em uma implementação de membro de interface explícita.
*  Para uma propriedade ou um indexador que não `override` tenha nenhum modificador, um *accessor_modifier* será permitido somente se a propriedade ou o indexador `set` tiver um e um `get` acessador e, em seguida, for permitido somente em um desses acessadores.
*  Para uma propriedade ou um indexador que inclui `override` um modificador, um acessador deve corresponder ao *accessor_modifier*, se houver, do acessador que está sendo substituído.
*  O *accessor_modifier* deve declarar uma acessibilidade que seja estritamente mais restritiva do que a acessibilidade declarada da propriedade ou do indexador em si. Para ser preciso:
   * Se a `public`propriedade ou o indexador tiver uma acessibilidade declarada, o *accessor_modifier* poderá `protected internal`ser `internal`, `protected`, ou `private`.
   * Se a propriedade ou o `protected internal`indexador tiver uma acessibilidade declarada, o *accessor_modifier* poderá `internal`ser `protected`, ou `private`.
   * Se a propriedade ou o indexador tiver uma acessibilidade declarada `internal` de ou `protected`, o `private` *accessor_modifier* deverá ser.
   * Se a propriedade ou o indexador tiver uma acessibilidade declarada de `private`, nenhum *accessor_modifier* poderá ser usado.

Para `abstract` propriedades `extern` e, o *accessor_body* para cada acessador especificado é simplesmente um ponto e vírgula. Uma propriedade não abstrata que não seja externa pode ter cada *accessor_body* um ponto-e-vírgula; nesse caso, é uma ***propriedade implementada automaticamente*** ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)). Uma propriedade implementada automaticamente deve ter pelo menos um acessador get. Para os acessadores de qualquer outra propriedade não abstrata e não externa, o *accessor_body* é um *bloco* que especifica as instruções a serem executadas quando o acessador correspondente é invocado.

Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade. Exceto como o destino de uma atribuição, quando uma propriedade é referenciada em uma expressão, `get` o acessador da propriedade é invocado para calcular o valor da propriedade ([valores de expressões](expressions.md#values-of-expressions)). O corpo de um `get` acessador deve estar em conformidade com as regras para métodos de retorno de valor descritos no [corpo do método](classes.md#method-body). Em particular, todas `return` as instruções no corpo de um `get` acessador devem especificar uma expressão que é implicitamente conversível para o tipo de propriedade. Além disso, o ponto de `get` extremidade de um acessador não deve ser acessível.

Um `set` acessador corresponde a um método com um parâmetro de valor único do tipo de `void` Propriedade e um tipo de retorno. O parâmetro implícito de um `set` acessador é `value`sempre denominado. Quando uma propriedade é referenciada como o destino de uma atribuição[(operadores de atribuição](expressions.md#assignment-operators)) ou como o operando `++` de `--` ou (operadores de[incremento de sufixo e decréscimo](expressions.md#postfix-increment-and-decrement-operators), [incremento de prefixo e diminuir os operadores](expressions.md#prefix-increment-and-decrement-operators)), o acessador é invocado com um argumento (cujo valor é o lado direito da atribuição ou o operando `++` do operador or `--` ) que fornece o novo valor ([atribuição simples](expressions.md#simple-assignment)). `set` O corpo de um `set` acessador deve estar em conformidade `void` com as regras para métodos descritos no [corpo do método](classes.md#method-body). Em particular, `return` as instruções no `set` corpo do acessador não têm permissão para especificar uma expressão. Como um `set` acessador implicitamente tem um `value`parâmetro chamado, ele é um erro de tempo de compilação para uma variável local ou uma `set` declaração de constante em um acessador para ter esse nome.

Com base na presença ou ausência de `get` `set` acessadores, uma propriedade é classificada da seguinte maneira:

*  Uma propriedade que inclui um `get` acessador e um `set` acessador é considerada uma propriedade de ***leitura/gravação*** .
*  Uma propriedade que tem apenas um `get` acessador é considerada uma propriedade ***somente leitura*** . É um erro de tempo de compilação para uma propriedade somente leitura ser o destino de uma atribuição.
*  Uma propriedade que tem apenas um `set` acessador é considerada uma propriedade ***somente gravação*** . Exceto como o destino de uma atribuição, é um erro de tempo de compilação para referenciar uma propriedade somente gravação em uma expressão.

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
o `Button` controle declara uma propriedade pública `Caption` . O `get` acessador `Caption` da propriedade retorna a cadeia de caracteres armazenada no campo particular `caption` . O `set` acessador verifica se o novo valor é diferente do valor atual e, nesse caso, armazena o novo valor e redesenha o controle. As propriedades geralmente seguem o padrão mostrado acima: O `get` acessador simplesmente retorna um valor armazenado em um campo particular, `set` e o acessador modifica esse campo particular e, em seguida, executa quaisquer ações adicionais necessárias para atualizar totalmente o estado do objeto.

Considerando a `Button` classe acima, veja a seguir um exemplo de uso `Caption` da propriedade:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Aqui, o `set` acessador é invocado atribuindo um valor à propriedade, e o `get` acessador é invocado referenciando a propriedade em uma expressão.

O `get` e`set` os acessadores de uma propriedade não são membros distintos e não é possível declarar os acessadores de uma propriedade separadamente. Assim, não é possível que os dois acessadores de uma propriedade de leitura/gravação tenham acessibilidade diferente. O exemplo
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
não declara uma única propriedade de leitura/gravação. Em vez disso, ele declara duas propriedades com o mesmo nome, uma somente leitura e uma somente gravação. Como dois membros declarados na mesma classe não podem ter o mesmo nome, o exemplo causa a ocorrência de um erro de tempo de compilação.

Quando uma classe derivada declara uma propriedade com o mesmo nome de uma propriedade herdada, a propriedade derivada oculta a propriedade herdada em relação à leitura e à gravação. No exemplo
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
a `P` Propriedade em `B` oculta a `P` Propriedade em `A` com relação à leitura e gravação. Portanto, nas instruções
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
a atribuição para `b.P` causa a reportação de um erro em tempo de compilação, pois a `P` propriedade somente `B` leitura no oculta a propriedade somente `P` gravação no `A`. Observe, no entanto, que uma conversão pode ser usada para acessar `P` a Propriedade Hidden.

Ao contrário dos campos públicos, as propriedades fornecem uma separação entre o estado interno de um objeto e sua interface pública. Considere o exemplo:
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

Aqui, a `Label` classe usa dois `int` campos `x` e `y`, para armazenar seu local. O local é exposto `X` publicamente como um e uma `Y` Propriedade e como uma `Location` Propriedade do tipo `Point`. Se, em uma versão futura do `Label`, for mais conveniente armazenar o local como um `Point` internamente, a alteração poderá ser feita sem afetar a interface pública da classe:
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

Tinha `x` `public readonly` `Label` e `y` , em vez de campos, seria impossível fazer essa alteração na classe.

Expor o estado por meio de propriedades não é necessariamente qualquer menos eficiente do que expor os campos diretamente. Em particular, quando uma propriedade é não virtual e contém apenas uma pequena quantidade de código, o ambiente de execução pode substituir chamadas para acessadores pelo código real dos acessadores. Esse processo é conhecido como ***inlining***e torna o acesso à propriedade tão eficiente quanto o acesso ao campo, mas preserva a maior flexibilidade das propriedades.

Como invocar um `get` acessador é conceitualmente equivalente a ler o valor de um campo, ele é considerado um estilo `get` de programação insatisfatório para que os acessadores tenham efeitos colaterais observáveis. No exemplo
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
o valor da `Next` Propriedade depende do número de vezes que a propriedade foi acessada anteriormente. Portanto, o acesso à propriedade produz um efeito colateral observável, e a propriedade deve ser implementada como um método em vez disso.

A Convenção "sem efeitos colaterais" para `get` acessadores não significa `get` que os acessadores sempre devem ser escritos para simplesmente retornar valores armazenados em campos. Na verdade `get` , os acessadores geralmente calculam o valor de uma propriedade acessando vários campos ou invocando métodos. No entanto, um `get` acessador projetado corretamente não executa nenhuma ação que cause alterações observáveis no estado do objeto.

As propriedades podem ser usadas para atrasar a inicialização de um recurso até o momento em que ele é referenciado pela primeira vez. Por exemplo:
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

A `Console` classe contém três propriedades `Out`, `In`, e `Error`, que representam os dispositivos de entrada, saída e erro padrão, respectivamente. Ao expor esses membros como propriedades, a `Console` classe pode atrasar a inicialização até que elas sejam realmente usadas. Por exemplo, após a primeira referência `Out` à propriedade, como em
```csharp
Console.Out.WriteLine("hello, world");
```
o subjacente `TextWriter` para o dispositivo de saída é criado. Mas se o aplicativo não fizer nenhuma referência às `In` propriedades `Error` e, nenhum objeto será criado para esses dispositivos.

### <a name="automatically-implemented-properties"></a>Propriedades implementadas automaticamente

Uma propriedade implementada automaticamente (ou ***Propriedade automática*** para curto), é uma propriedade não abstrata não abstraida com corpos de acessadores somente de ponto e vírgula. As propriedades automáticas devem ter um acessador get e, opcionalmente, podem ter um acessador set.

Quando uma propriedade é especificada como uma propriedade implementada automaticamente, um campo de apoio oculto fica automaticamente disponível para a propriedade e os acessadores são implementados para ler e gravar nesse campo de backup. Se a propriedade automática não tiver nenhum acessador set, o campo de backup `readonly` será considerado ([campos somente leitura](classes.md#readonly-fields)). Assim como um `readonly` campo, uma propriedade automática somente getter também pode ser atribuída ao corpo de um construtor da classe delimitadora. Tal atribuição atribui diretamente ao campo de apoio somente leitura da propriedade.

Uma propriedade automática pode, opcionalmente, ter um *property_initializer*, que é aplicado diretamente ao campo de backup como um *variable_initializer* ([inicializadores de variável](classes.md#variable-initializers)).

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

Observe que as atribuições para o campo ReadOnly são legais, pois elas ocorrem dentro do construtor.


### <a name="accessibility"></a>Acessibilidade

Se um acessador tiver um *accessor_modifier*, o domínio de acessibilidade ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)) do acessador será determinado usando a acessibilidade declarada do *accessor_modifier*. Se um acessador não tiver um *accessor_modifier*, o domínio de acessibilidade do acessador será determinado da acessibilidade declarada da propriedade ou do indexador.

A presença de um *accessor_modifier* nunca afeta a pesquisa de Membros ([operadores](expressions.md#operators)) ou a resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)). Os modificadores na propriedade ou no indexador sempre determinam a qual propriedade ou indexador está associado, independentemente do contexto do acesso.

Depois que uma determinada propriedade ou um indexador tiver sido selecionado, os domínios de acessibilidade dos acessadores específicos envolvidos serão usados para determinar se esse uso é válido:

*  Se o uso for como um valor ([valores de expressões](expressions.md#values-of-expressions)), o `get` acessador deverá existir e estar acessível.
*  Se o uso for como o destino de uma atribuição simples ([atribuição simples](expressions.md#simple-assignment)), o `set` acessador deverá existir e estar acessível.
*  Se o uso for como o destino da atribuição composta ([atribuição composta](expressions.md#compound-assignment)) ou como o destino dos `++` operadores ou `--` (membros de[função](expressions.md#function-members). 9, expressões de `get` [invocação](expressions.md#invocation-expressions)), os acessadores e o `set` acessador deve existir e estar acessível.

No exemplo a seguir, a propriedade `A.Text` é ocultada pela propriedade `B.Text`, mesmo em contextos onde apenas o `set` acessador é chamado. Por outro lado, a `B.Count` propriedade não é acessível para `M`classe, portanto, a `A.Count` Propriedade acessível é usada em seu lugar.

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

Um acessador que é usado para implementar uma interface pode não ter um *accessor_modifier*. Se apenas um acessador for usado para implementar uma interface, o outro acessador poderá ser declarado com um *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Acessadores de propriedade virtual, sealed, override e abstract

Uma `virtual` declaração de propriedade especifica que os acessadores da propriedade são virtuais. O `virtual` modificador se aplica a ambos os acessadores de uma propriedade de leitura/gravação — não é possível que apenas um acessador de uma propriedade de leitura/gravação seja virtual.

Uma `abstract` declaração de propriedade especifica que os acessadores da propriedade são virtuais, mas não fornecem uma implementação real dos acessadores. Em vez disso, as classes derivadas não abstratas são necessárias para fornecer sua própria implementação para os acessadores, substituindo a propriedade. Como um acessador de uma declaração de propriedade abstrata não fornece implementação real, seu *accessor_body* simplesmente consiste em um ponto-e-vírgula.

Uma declaração de propriedade que inclui os `abstract` modificadores e `override` especifica que a propriedade é abstrata e substitui uma propriedade base. Os acessadores de tal propriedade também são abstratos.

As declarações de propriedade abstratas só são permitidas em classes abstratas ([classes abstratas](classes.md#abstract-classes)). Os acessadores de uma propriedade virtual herdada podem ser substituídos em uma classe derivada, incluindo uma declaração de `override` propriedade que especifica uma diretiva. Isso é conhecido como uma ***declaração de propriedade de substituição***. Uma declaração de propriedade de substituição não declara uma nova propriedade. Em vez disso, ele simplesmente especializa as implementações dos acessadores de uma propriedade virtual existente.

Uma declaração de propriedade de substituição deve especificar exatamente os mesmos modificadores de acessibilidade, tipo e nome que a propriedade herdada. Se a propriedade Inherited tiver apenas um único acessador (ou seja, se a propriedade herdada for somente leitura ou somente gravação), a propriedade de substituição deverá incluir somente esse acessador. Se a propriedade Inherited incluir os acessadores (ou seja, se a propriedade herdada for Read-Write), a propriedade de substituição poderá incluir um único acessador ou ambos os acessadores.

Uma declaração de propriedade de substituição pode `sealed` incluir o modificador. O uso desse modificador impede que uma classe derivada substitua ainda mais a propriedade. Os acessadores de uma propriedade selada também são lacrados.

Exceto pelas diferenças na sintaxe de declaração e invocação, os acessadores virtual, sealed, override e abstract se comportam exatamente como métodos virtuais, lacrados, de substituição e abstratos. Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [métodos de substituição](classes.md#override-methods), métodos [lacrados](classes.md#sealed-methods)e [métodos abstratos](classes.md#abstract-methods) se aplicam como se os acessadores fossem métodos de um formulário correspondente:

*  Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade e os mesmos modificadores que a propriedade recipiente.
*  Um `set` acessador corresponde a um método com um parâmetro de valor único do tipo de `void` Propriedade, um tipo de retorno e os mesmos modificadores que a propriedade recipiente.

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
`X`é uma propriedade somente leitura virtual, `Y` é uma propriedade de leitura/gravação virtual e `Z` é uma propriedade de leitura/gravação abstrata. Como `Z` é abstrato, a classe `A` recipiente também deve ser declarada abstrata.

Uma classe derivada de `A` é mostrada abaixo:
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

Aqui, as declarações de `X`, `Y`e `Z` estão substituindo declarações de propriedade. Cada declaração de propriedade corresponde exatamente aos modificadores de acessibilidade, ao tipo e ao nome da propriedade herdada correspondente. O `get` acessador do `set` `X` e o `Y` acessador do usam a `base` palavra-chave para acessar os acessadores herdados. A declaração de `Z` substitui ambos os acessadores abstratos – portanto, não há nenhum membro de `B`função abstrata `B` pendente no e é permitido ser uma classe não abstrata.

Quando uma propriedade é declarada `override`como um, todos os acessadores substituídos devem ser acessíveis para o código de substituição. Além disso, a acessibilidade declarada da propriedade ou do indexador em si, e dos acessadores, deve corresponder à do membro e aos acessadores substituídos. Por exemplo:
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

Um ***evento*** é um membro que permite que um objeto ou classe forneça notificações. Os clientes podem anexar código executável para eventos fornecendo ***manipuladores de eventos***.

Os eventos são declarados usando *event_declaration*s:

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

Um *event_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), `new` o ([o novo modificador](classes.md#the-new-modifier)), `static` ([estático e métodos de instância](classes.md#static-and-instance-methods)) `virtual` ,[(métodos virtuais](classes.md#virtual-methods)) `override` , (métodos de[substituição](classes.md#override-methods)), `sealed` ([métodos lacrados](classes.md#sealed-methods)), `abstract` ([métodos abstratos](classes.md#abstract-methods)) e `extern`([Métodos externos](classes.md#external-methods)) modificadores.

As declarações de evento estão sujeitas às mesmas regras que as declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores.

O *tipo* de uma declaração de evento deve ser um *delegate_type* ([tipos de referência](types.md#reference-types)) e esse *delegate_type* deve ser pelo menos acessível como o próprio evento ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Uma declaração de evento pode incluir *event_accessor_declarations*. No entanto, se não for, para eventos não-extern e não abstratos, o compilador fornecerá esses itens automaticamente ([eventos semelhantes a campo](classes.md#field-like-events)); para eventos extern, os acessadores são fornecidos externamente.

Uma declaração de evento que omite *event_accessor_declarations* define um ou mais eventos, um para cada um dos *variable_declarator*s. Os atributos e os modificadores se aplicam a todos os membros declarados por tal *event_declaration*.

É um erro de tempo de compilação para um *event_declaration* incluir o modificador `abstract` e o *event_accessor_declarations*delimitado por chave.

Quando uma declaração de evento inclui `extern` um modificador, o evento é considerado um ***evento externo***. Como uma declaração de evento externo não fornece implementação real, é um erro para que ela inclua o modificador e o `extern` *event_accessor_declarations*.

É um erro de tempo de compilação para um *variable_declarator* de uma declaração de evento com `abstract` um `external` modificador ou para incluir um *variable_initializer*.

Um evento pode ser usado como o operando esquerdo dos operadores `+=` e `-=` (atribuição de[evento](expressions.md#event-assignment)). Esses operadores são usados, respectivamente, para anexar manipuladores de eventos ou para remover manipuladores de eventos de um evento, e os modificadores de acesso do evento controlam os contextos nos quais essas operações são permitidas.

Como `+=` e`-=` são as únicas operações que são permitidas em um evento fora do tipo que declara o evento, o código externo pode adicionar e remover manipuladores para um evento, mas não pode obter ou modificar a lista subjacente de eventos manipuladores.

Em uma operação do formulário `x += y` ou `x -= y`, quando `x` é um evento e a referência ocorre fora do tipo que contém a declaração de `x`, o resultado da operação tem o tipo `void` (em vez de ter o tipo de `x`, com o valor de `x` após a atribuição). Essa regra proíbe que o código externo examine indiretamente o delegado subjacente de um evento.

O exemplo a seguir mostra como os manipuladores de eventos são anexados `Button` a instâncias da classe:
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

Aqui, o `LoginDialog` Construtor de instância cria `Button` duas instâncias e anexa manipuladores de `Click` eventos aos eventos.

### <a name="field-like-events"></a>Eventos do tipo campo

Dentro do texto do programa da classe ou struct que contém a declaração de um evento, determinados eventos podem ser usados como campos. Para ser usado dessa forma, um evento não deve ser `abstract` ou `extern`e não deve incluir explicitamente *event_accessor_declarations*. Tal evento pode ser usado em qualquer contexto que permita um campo. O campo contém um delegado ([delegados](delegates.md)) que se refere à lista de manipuladores de eventos que foram adicionados ao evento. Se nenhum manipulador de eventos tiver sido adicionado, o campo conterá `null`.

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
`Click`é usado como um campo dentro da `Button` classe. Como demonstra o exemplo, o campo pode ser examinado, modificado e usado em expressões de invocação de delegado. O `OnClick` método `Click` na classe "gera" o evento. `Button` A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento — assim, não há constructos de linguagem especial para gerar eventos. Observe que a invocação de delegado é precedida por uma verificação que garante que o delegado não seja nulo.

`Button` Fora da declaração da classe, o `Click` membro só pode ser usado no lado esquerdo dos `+=` operadores e `-=` , como em
```csharp
b.Click += new EventHandler(...);
```
que acrescenta um delegado à lista de invocação do `Click` evento e
```csharp
b.Click -= new EventHandler(...);
```
que remove um delegado da lista de invocação do `Click` evento.

Ao compilar um evento do tipo campo, o compilador cria automaticamente o armazenamento para manter o delegado e cria acessadores para o evento que adiciona ou remove manipuladores de eventos ao campo delegado. As operações de adição e remoção são thread-safe e podem (mas não são necessárias) serem realizadas enquanto mantém o bloqueio ([a instrução Lock](statements.md#the-lock-statement)) no objeto recipiente para um evento de instância ou o tipo Object ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) para um evento estático.

Assim, uma declaração de evento de instância do formulário:
```csharp
class X
{
    public event D Ev;
}
```
será compilado para algo equivalente a:
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
Dentro da classe `X`, referências à `Ev` no lado esquerdo dos `+=` operadores e `-=` fazem com que os acessadores adicionar e remover sejam invocados. Todas as outras referências `Ev` a são compiladas para referenciar `__Ev` o campo oculto em vez disso ([acesso de membro](expressions.md#member-access)). O nome "`__Ev`" é arbitrário; o campo oculto pode ter qualquer nome ou nenhum nome.

### <a name="event-accessors"></a>Acessadores de eventos

As`Button` declarações de evento normalmente omitem *event_accessor_declarations*, como no exemplo acima. Uma situação para isso envolve o caso em que o custo de armazenamento de um campo por evento não é aceitável. Nesses casos, uma classe pode incluir *event_accessor_declarations* e usar um mecanismo privado para armazenar a lista de manipuladores de eventos.

O *event_accessor_declarations* de um evento especifica as instruções Executáveis associadas à adição e remoção de manipuladores de eventos.

As declarações de acessador consistem em um *add_accessor_declaration* e um *remove_accessor_declaration*. Cada declaração de acessador consiste `add` no `remove` token ou seguido por um *bloco*. O *bloco* associado a um *add_accessor_declaration* especifica as instruções a serem executadas quando um manipulador de eventos é adicionado e o *bloco* associado a um *remove_accessor_declaration* especifica as instruções a serem executadas Quando um manipulador de eventos é removido.

Cada *add_accessor_declaration* e *remove_accessor_declaration* corresponde a um método com um parâmetro de valor único do tipo de evento e `void` um tipo de retorno. O parâmetro implícito de um acessador de `value`eventos é nomeado. Quando um evento é usado em uma atribuição de evento, o acessador de evento apropriado é usado. Especificamente, se o operador de atribuição `+=` for, o acessador Add será usado e, se o `-=` operador de atribuição for, o acessador de remoção será usado. Em ambos os casos, o operando do lado direito do operador de atribuição é usado como o argumento para o acessador de eventos. O bloco de um *add_accessor_declaration* ou *remove_accessor_declaration* deve estar em conformidade com as regras `void` para métodos descritos no [corpo do método](classes.md#method-body). Em particular, `return` instruções nesse bloco não são permitidas para especificar uma expressão.

Como um acessador de evento implicitamente tem `value`um parâmetro chamado, ele é um erro de tempo de compilação para uma variável local ou constante declarada em um acessador de evento para ter esse nome.

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
a `Control` classe implementa um mecanismo de armazenamento interno para eventos. O `AddEventHandler` método associa um valor delegado a uma chave, o `GetEventHandler` método retorna o delegado atualmente associado a uma chave e o `RemoveEventHandler` método Remove um delegado como um manipulador de eventos para o evento especificado. Supostamente, o mecanismo de armazenamento subjacente foi projetado de forma que não há nenhum custo para associar um `null` valor delegado a uma chave e, portanto, eventos sem tratamento não consomem nenhum armazenamento.

### <a name="static-and-instance-events"></a>Eventos estáticos e de instância

Quando uma declaração de evento inclui `static` um modificador, o evento é considerado um ***evento estático***. Quando nenhum `static` modificador está presente, o evento é considerado um ***evento de instância***.

Um evento estático não está associado a uma instância específica e é um erro de tempo de compilação para se referir `this` aos acessadores de um evento estático.

Um evento de instância é associado a uma determinada instância de uma classe, e essa instância pode ser acessada como `this` ([esse acesso](expressions.md#this-access)) nos acessadores desse evento.

Quando um evento é referenciado em *um member_access* ([acesso de membro](expressions.md#member-access)) do `E.M`formulário, se for um evento estático `E` , deve-se `M` indicar `M`um tipo que `M` contém e se é um evento de instância, e deve denota uma instância de um tipo que `M`contém.

As diferenças entre os membros estático e de instância são discutidas mais detalhadamente em [membros estáticos e de instância](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Acessadores de evento virtual, sealed, override e abstract

Uma `virtual` declaração de evento especifica que os acessadores desse evento são virtuais. O `virtual` modificador se aplica a ambos os acessadores de um evento.

Uma `abstract` declaração de evento especifica que os acessadores do evento são virtuais, mas não fornecem uma implementação real dos acessadores. Em vez disso, as classes derivadas não abstratas são necessárias para fornecer sua própria implementação para os acessadores, substituindo o evento. Como uma declaração de evento abstract não fornece implementação real, ela não pode fornecer *event_accessor_declarations*delimitado por chaves.

Uma declaração de evento que inclui os `abstract` modificadores e `override` especifica que o evento é abstrato e substitui um evento base. Os acessadores de tal evento também são abstratos.

As declarações de evento abstract só são permitidas em classes abstratas ([classes abstratas](classes.md#abstract-classes)).

Os acessadores de um evento virtual herdado podem ser substituídos em uma classe derivada, incluindo uma declaração de `override` evento que especifica um modificador. Isso é conhecido como uma ***declaração de evento de substituição***. Uma declaração de evento de substituição não declara um novo evento. Em vez disso, ele simplesmente especializa as implementações dos acessadores de um evento virtual existente.

Uma declaração de evento de substituição deve especificar exatamente os mesmos modificadores de acessibilidade, tipo e nome que o evento substituído.

Uma declaração de evento de substituição pode `sealed` incluir o modificador. O uso desse modificador impede que uma classe derivada substitua o evento. Os acessadores de um evento lacrado também são lacrados.

É um erro de tempo de compilação para uma declaração de evento de substituição para `new` incluir um modificador.

Exceto pelas diferenças na sintaxe de declaração e invocação, os acessadores virtual, sealed, override e abstract se comportam exatamente como métodos virtuais, lacrados, de substituição e abstratos. Especificamente, as regras descritas em [métodos virtuais](classes.md#virtual-methods), [métodos de substituição](classes.md#override-methods), métodos [lacrados](classes.md#sealed-methods)e [métodos abstratos](classes.md#abstract-methods) se aplicam como se os acessadores fossem métodos de um formulário correspondente. Cada acessador corresponde a um método com um parâmetro de valor único do tipo de `void` evento, um tipo de retorno e os mesmos modificadores que o evento recipiente.

## <a name="indexers"></a>Indexadores

Um ***indexador*** é um membro que permite que um objeto seja indexado da mesma maneira que uma matriz. Indexadores são declarados usando *indexer_declaration*s:

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

Um *indexer_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)) e uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)), `new` o ([o novo modificador](classes.md#the-new-modifier)), `virtual` ([ Métodos virtuais](classes.md#virtual-methods)), `override` ([métodos de substituição](classes.md#override-methods)) `sealed` , os modificadores ( `abstract` [métodos lacrados](classes.md#sealed-methods)), ( `extern` [métodos abstratos](classes.md#abstract-methods)) e ([métodos externos](classes.md#external-methods)).

As declarações do indexador estão sujeitas às mesmas regras que as declarações de método ([métodos](classes.md#methods)) em relação a combinações válidas de modificadores, com a única exceção de que o modificador estático não é permitido em uma declaração de indexador.

Os modificadores `virtual`, `override`e `abstract` são mutuamente exclusivos, exceto em um caso. Os `abstract` modificadores e `override` podem ser usados juntos para que um indexador abstrato possa substituir um virtual.

O *tipo* de uma declaração de indexador especifica o tipo de elemento do indexador introduzido pela declaração. A menos que o indexador seja uma implementação de membro de interface explícita, o *tipo* é `this`seguido pela palavra-chave. Para uma implementação de membro de interface explícita, o *tipo* é seguido por um *interface_type*,`.`um "" e a `this`palavra-chave. Ao contrário de outros membros, os indexadores não têm nomes definidos pelo usuário.

O *formal_parameter_list* especifica os parâmetros do indexador. A lista de parâmetros formais de um indexador corresponde ao de um método ([parâmetros de método](classes.md#method-parameters)), exceto pelo menos um parâmetro que deve ser especificado e que `ref` os `out` modificadores e de parâmetro não são permitidos.

O *tipo* de um indexador e cada um dos tipos referenciados no *formal_parameter_list* devem ser pelo menos tão acessíveis quanto o próprio indexador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

Um *indexer_body* pode consistir em um ***corpo de acessador*** ou um ***corpo de expressão***. Em um corpo de acessador, *accessor_declarations*, que deve ser`{`colocado entre os`}`tokens "" e "", declare os acessadores ([acessadores](classes.md#accessors)) da propriedade. Os acessadores especificam as instruções Executáveis associadas à leitura e gravação da propriedade.

Um corpo de expressão que consiste em`=>`"" seguido por uma `E` expressão e um ponto-e-vírgula é exatamente `{ get { return E; } }`equivalente ao corpo da instrução e, portanto, só pode ser usado para especificar indexadores somente getter em que o resultado do getter é fornecido por uma única expressão.

Embora a sintaxe para acessar um elemento do indexador seja a mesma que para um elemento de matriz, um elemento de indexador não é classificado como uma variável. Portanto, não é possível passar um elemento do indexador como um `ref` argumento ou. `out`

A lista de parâmetros formais de um indexador define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) do indexador. Especificamente, a assinatura de um indexador consiste no número e tipos de seus parâmetros formais. O tipo de elemento e os nomes dos parâmetros formais não fazem parte da assinatura de um indexador.

A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma classe.

Indexadores e propriedades são muito semelhantes em conceito, mas diferem das seguintes maneiras:

*  Uma propriedade é identificada por seu nome, enquanto um indexador é identificado por sua assinatura.
*  Uma propriedade é acessada por meio de um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou um *member_access* ([acesso de membro](expressions.md#member-access)), enquanto um elemento de indexador é acessado por meio de um *element_access* ([acesso do indexador](expressions.md#indexer-access)).
*  Uma propriedade pode ser um `static` membro, enquanto um indexador é sempre um membro de instância.
*  Um `get` acessador de uma propriedade corresponde a um método sem parâmetros, enquanto `get` um acessador de um indexador corresponde a um método com a mesma lista de parâmetros formais que o indexador.
*  Um `set` acessador de uma propriedade corresponde a um método com um único `value`parâmetro chamado, `set` enquanto que um acessador de um indexador corresponde a um método com a mesma lista de parâmetros formais que o indexador, além de um parâmetro adicional nomeado `value`.
*  É um erro de tempo de compilação para um acessador de indexador declarar uma variável local com o mesmo nome que um parâmetro de indexador.
*  Em uma declaração de propriedade de substituição, a propriedade herdada é acessada `P` usando a sintaxe `base.P`, onde é o nome da propriedade. Em uma declaração de substituição do indexador, o indexador herdado é acessado `E` usando a sintaxe `base[E]`, em que é uma lista separada por vírgulas de expressões.
*  Não há nenhum conceito de "indexador implementado automaticamente". É um erro ter um indexador não-abstrato e não externo com acessadores de ponto e vírgula.

Além dessas diferenças, todas as regras definidas em [acessadores](classes.md#accessors) e [Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties) se aplicam a acessadores indexadores, bem como a acessadores de propriedade.

Quando uma declaração de indexador inclui `extern` um modificador, o indexador é considerado um ***indexador externo***. Como uma declaração externa do indexador não fornece nenhuma implementação real, cada uma de suas *accessor_declarations* consiste em um ponto-e-vírgula.

O exemplo a seguir declara uma `BitArray` classe que implementa um indexador para acessar os bits individuais na matriz de bits.
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

Uma instância da `BitArray` classe consome substancialmente menos memória do que uma correspondente `bool[]` (já que cada valor do primeiro ocupa apenas um bit em vez do último byte), mas permite as mesmas operações que um. `bool[]`

A classe `CountPrimes` a seguir usa `BitArray` um e o algoritmo clássico "sieve" para calcular o número de primos entre 1 e um determinado máximo:
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

Observe que a sintaxe para acessar elementos do `BitArray` é exatamente a mesma do para um. `bool[]`

O exemplo a seguir mostra uma classe de grade 26 * 10 que tem um indexador com dois parâmetros. O primeiro parâmetro deve ser uma letra maiúscula ou minúscula no intervalo de A-Z e o segundo deve ser um número inteiro no intervalo de 0-9.

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

### <a name="indexer-overloading"></a>Sobrecarga do indexador

As regras de resolução de sobrecarga do indexador são descritas em [inferência de tipos](expressions.md#type-inference).

## <a name="operators"></a>Operadores

Um ***operador*** é um membro que define o significado de um operador de expressão que pode ser aplicado a instâncias da classe. Os operadores são declarados usando *operator_declaration*s:

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
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
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

Há três categorias de operadores que podem ser sobrecarregados: Operadores unários ([operadores unários](classes.md#unary-operators)), operadores binários ([operadores binários](classes.md#binary-operators)) e operadores de conversão ([operadores de conversão](classes.md#conversion-operators)).

O *operator_body* é um ponto e vírgula, um ***corpo de instrução*** ou um corpo de ***expressão***. Um corpo de instrução consiste em um *bloco*, que especifica as instruções a serem executadas quando o operador é invocado. O *bloco* deve estar em conformidade com as regras para métodos de retorno de valor descritos no [corpo do método](classes.md#method-body). Um corpo de `=>` expressão consiste em seguido por uma expressão e um ponto e vírgula e denota uma única expressão a ser executada quando o operador é invocado.

Para `extern` operadores, o *operator_body* consiste simplesmente de um ponto e vírgula. Para todos os outros operadores, o *operator_body* é um corpo de bloco ou um corpo de expressão.

As regras a seguir se aplicam a todas as declarações de operador:

*  Uma declaração de operador deve incluir um `public` e um `static` modificador.
*  Os parâmetros de um operador devem ser parâmetros de valor (parâmetros de[valor](variables.md#value-parameters)). É um erro de tempo de compilação para uma declaração de operador especificar `ref` ou `out` parâmetros.
*  A assinatura de um operador ([operadores unários](classes.md#unary-operators), [operadores binários](classes.md#binary-operators), [operadores de conversão](classes.md#conversion-operators)) deve ser diferente das assinaturas de todos os outros operadores declarados na mesma classe.
*  Todos os tipos referenciados em uma declaração de operador devem ser pelo menos tão acessíveis quanto o próprio operador ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).
*  É um erro para que o mesmo modificador apareça várias vezes em uma declaração de operador.

Cada categoria de operador impõe restrições adicionais, conforme descrito nas seções a seguir.

Como outros membros, os operadores declarados em uma classe base são herdados por classes derivadas. Como as declarações de operador sempre exigem a classe ou struct em que o operador é declarado para participar da assinatura do operador, não é possível que um operador declarado em uma classe derivada oculte um operador declarado em uma classe base. Assim, o `new` modificador nunca é necessário e, portanto, nunca é permitido em uma declaração de operador.

Informações adicionais sobre operadores unários e binários podem ser encontradas em [operadores](expressions.md#operators).

Informações adicionais sobre operadores de conversão podem ser encontradas em [conversões definidas pelo usuário](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Operadores unários

As regras a seguir se aplicam a declarações de `T` operador unários, onde denota o tipo de instância da classe ou struct que contém a declaração do operador:

*  Um operador `+` `-`unário `~` `T` ,, ou deve usar um único parâmetro do tipo ou `T?` pode retornar qualquer tipo. `!`
*  Um operador `++` or `--` unário deve usar um único parâmetro do `T` tipo `T?` ou deve retornar o mesmo tipo ou um tipo derivado dele.
*  Um operador `true` or `false` unário deve usar um único parâmetro do `T` tipo `T?` ou deve retornar o `bool`tipo.

A assinatura de um operador unário consiste no token do operador`+`( `-` `!`, `--` `~` `++`,,,, `true`, ou `false`) e no tipo do único parâmetro formal. O tipo de retorno não faz parte da assinatura de um operador unário, nem é o nome do parâmetro formal.

Os `true` operadores `false` e unários exigem declarações emparelhadas. Ocorrerá um erro em tempo de compilação se uma classe declarar um desses operadores sem também declarar o outro. Os `true` operadores `false` e são descritos mais detalhadamente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators) e [expressões booleanas](expressions.md#boolean-expressions).

O exemplo a seguir mostra uma implementação e o uso `operator ++` subsequente de para uma classe de vetor de inteiro:
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

Observe como o método Operator retorna o valor produzido pela adição de 1 ao operando, assim como os operadores de incremento de sufixo e decréscimo ([incremento de sufixo e diminuição de operadores](expressions.md#postfix-increment-and-decrement-operators)) e os operadores de incremento de prefixo e decréscimo ([prefixo operadores de incremento e decréscimo](expressions.md#prefix-increment-and-decrement-operators)). Diferentemente C++do no, esse método não precisa modificar o valor de seu operando diretamente. Na verdade, modificar o valor do operando violaria a semântica padrão do operador de incremento de sufixo.

### <a name="binary-operators"></a>Operadores binários

As regras a seguir se aplicam a declarações de `T` operador binários, onde denota o tipo de instância da classe ou struct que contém a declaração do operador:

*  Um operador binário non-Shift deve usar dois parâmetros, pelo menos um dos quais deve ter tipo `T` ou `T?`, e pode retornar qualquer tipo.
*  Um operador `<<` ou `>>` binário deve usar dois parâmetros, o primeiro deles deve ter o tipo `T` ou `T?` e o segundo deve ter o tipo `int` ou `int?`, e pode retornar qualquer tipo.

A assinatura de um operador binário consiste no token do operador (`+` `*`, `-` `&` `|` `/` `%` ,,`^`,,,,,,, `>>` `<<` `==` ,,,`<=`, ou`>=`) e os tipos dos dois parâmetros formais. `<` `!=` `>` O tipo de retorno e os nomes dos parâmetros formais não fazem parte da assinatura de um operador binário.

Determinados operadores binários exigem declarações emparelhadas. Para cada declaração de qualquer operador de um par, deve haver uma declaração correspondente do outro operador do par. Duas declarações de operador correspondem quando têm o mesmo tipo de retorno e o mesmo tipo para cada parâmetro. Os seguintes operadores exigem a declaração de pares:

*  `operator ==` e `operator !=`
*  `operator >` e `operator <`
*  `operator >=` e `operator <=`

### <a name="conversion-operators"></a>Operadores de conversão

Uma declaração de operador de conversão apresenta uma ***conversão definida pelo usuário*** ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)) que aumenta as conversões implícitas e explícitas predefinidas.

Uma declaração de operador de conversão que `implicit` inclui a palavra-chave apresenta uma conversão implícita definida pelo usuário. Conversões implícitas podem ocorrer em várias situações, incluindo invocações de membro de função, expressões de conversão e atribuições. Isso é descrito mais detalhadamente em [conversões implícitas](conversions.md#implicit-conversions).

Uma declaração de operador de conversão que `explicit` inclui a palavra-chave apresenta uma conversão explícita definida pelo usuário. Conversões explícitas podem ocorrer em expressões de conversão e são descritas em [conversões explícitas](conversions.md#explicit-conversions).

Um operador de conversão converte de um tipo de origem, indicado pelo tipo de parâmetro do operador de conversão, em um tipo de destino, indicado pelo tipo de retorno do operador de conversão.

Para um `S` determinado tipo de origem e tipo `T`de destino, se `S` ou `T` forem tipos anuláveis `T0` , avise `S0` e faça referência aos seus `S0` tipos `T0` subjacentes, caso contrário, e serão igual a `S` e `T` respectivamente. Uma classe ou estrutura tem permissão para declarar uma conversão de um tipo `S` de origem para um tipo `T` de destino somente se todas as seguintes opções forem verdadeiras:

*  `S0`e `T0` são tipos diferentes.
*  `S0` Ou`T0` é o tipo de classe ou struct no qual a declaração do operador ocorre.
*  Nem `S0` nem `T0` é um *interface_type*.
*  Excluindo conversões definidas `S` pelo usuário, uma conversão não existe de `T` ou `T` `S`para.

Para os fins dessas regras, quaisquer parâmetros de tipo associados `S` a ou `T` são considerados como tipos exclusivos que não têm nenhuma relação de herança com outros tipos, e quaisquer restrições nesses parâmetros de tipo são ignoradas.

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
as primeiras duas declarações de operador são permitidas porque, para fins de [indexadores](classes.md#indexers)3 `T` e `int` , `string` respectivamente, são consideradas tipos exclusivos sem nenhuma relação. No entanto, o terceiro operador é um `C<T>` erro porque é a classe `D<T>`base de.

Da segunda regra, ela segue que um operador de conversão deve converter de ou para o tipo class ou struct no qual o operador é declarado. Por exemplo, é `C` possível que um tipo de classe ou estrutura defina uma conversão de `C` para `int` e `int` de `C`para, mas não de `int` para `bool`.

Não é possível redefinir diretamente uma conversão predefinida. Assim, os operadores de conversão não têm permissão para converter de `object` ou para porque as conversões implícitas e explícitas já existem entre `object` o e todos os outros tipos. Da mesma forma, nem os tipos de origem nem de destino de uma conversão podem ser um tipo base do outro, já que uma conversão já existirá.

No entanto, é possível declarar operadores em tipos genéricos que, para argumentos de tipo específicos, especificar conversões que já existem como conversões predefinidas. No exemplo
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Quando Type `object` é especificado como um argumento de tipo `T`para, o segundo operador declara uma conversão que já existe (um implícito e, portanto, também uma conversão explícita existe de qualquer tipo para tipo `object`).

Nos casos em que existe uma conversão predefinida entre dois tipos, todas as conversões definidas pelo usuário entre esses tipos serão ignoradas. Especificamente:

*  Se uma conversão implícita predefinida ([conversões implícitas](conversions.md#implicit-conversions)) `S` existir de tipo para tipo `T`, todas as conversões definidas pelo usuário (implícitas ou explícitas) `S` de `T` para serão ignoradas.
*  Se uma conversão explícita predefinida ([conversões explícitas](conversions.md#explicit-conversions)) `S` existir de tipo para tipo `T`, todas as conversões explícitas definidas pelo usuário do `S` para `T` serão ignoradas. Além

Se `T` for um tipo de interface, conversões implícitas definidas pelo usuário `S` de `T` para serão ignoradas.

Caso contrário, conversões implícitas definidas pelo usuário `S` de `T` para o ainda serão consideradas.

Para todos os tipos `object`, mas os operadores declarados `Convertible<T>` pelo tipo acima não entram em conflito com conversões predefinidas. Por exemplo:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

No entanto, `object`para conversões de tipo predefinidas, oculte as conversões definidas pelo usuário em todos os casos, exceto uma:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

As conversões definidas pelo usuário não têm permissão para converter de ou para *interface_type*s. Em particular, essa restrição garante que nenhuma transformação definida pelo usuário ocorra durante a conversão para um *interface_type*e que uma conversão em um *interface_type* só terá sucesso se o objeto que está sendo convertido realmente implementar o *interface_type*especificado.

A assinatura de um operador de conversão consiste no tipo de origem e no tipo de destino. (Observe que essa é a única forma de membro para a qual o tipo de retorno participa na assinatura.) A `implicit` classificação `explicit` ou de um operador de conversão não faz parte da assinatura do operador. Assim, uma classe ou struct não pode declarar um `implicit` operador de `explicit` conversão e um com os mesmos tipos de origem e destino.

Em geral, as conversões implícitas definidas pelo usuário devem ser projetadas para nunca gerar exceções e nunca perderem informações. Se uma conversão definida pelo usuário puder dar origem às exceções (por exemplo, porque o argumento de origem está fora do intervalo) ou a perda de informações (como descartar bits de ordem superior), essa conversão deve ser definida como uma conversão explícita.

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
a conversão de `Digit` para `byte` é implícita porque ela nunca gera exceções ou perde informações, mas a conversão de `byte` para `Digit` é explícita, `Digit` já que só pode representar um subconjunto do possível valores de a `byte`.

## <a name="instance-constructors"></a>Construtores de instância

Um ***construtor de instância*** é um membro que implementa as ações necessárias para inicializar uma instância de uma classe. Construtores de instância são declarados usando *constructor_declaration*s:

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

Um *constructor_declaration* pode incluir um conjunto de *atributos* ([atributos](attributes.md)), uma combinação válida dos quatro modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) e um `extern` modificador ([métodos externos](classes.md#external-methods)). Uma declaração de construtor não tem permissão para incluir o mesmo modificador várias vezes.

O *identificador* de um *constructor_declarator* deve nomear a classe na qual o construtor de instância é declarado. Se qualquer outro nome for especificado, ocorrerá um erro em tempo de compilação.

O *formal_parameter_list* opcional de um construtor de instância está sujeito às mesmas regras que o *formal_parameter_list* de um método ([métodos](classes.md#methods)). A lista de parâmetros formais define a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um construtor de instância e governa o processo pelo qual a resolução de sobrecarga ([inferência de tipos](expressions.md#type-inference)) seleciona um construtor de instância específico em uma invocação.

Cada um dos tipos referenciados no *formal_parameter_list* de um construtor de instância deve ser pelo menos acessível como o próprio Construtor ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)).

O *constructor_initializer* opcional especifica outro construtor de instância para invocar antes de executar as instruções fornecidas no *constructor_body* deste construtor de instância. Isso é descrito mais detalhadamente em [inicializadores de Construtor](classes.md#constructor-initializers).

Quando uma declaração de Construtor inclui `extern` um modificador, o construtor é considerado um ***Construtor externo***. Como uma declaração de Construtor externo não fornece nenhuma implementação real, seu *constructor_body* consiste em um ponto-e-vírgula. Para todos os outros construtores, o *constructor_body* consiste em um *bloco* que especifica as instruções para inicializar uma nova instância da classe. Isso corresponde exatamente ao *bloco* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).

Construtores de instância não são herdados. Portanto, uma classe não tem construtores de instância diferentes daqueles realmente declarados na classe. Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão será fornecido automaticamente ([construtores padrão](classes.md#default-constructors)).

Os construtores de instância são invocados por *object_creation_expression*s ([expressões de criação de objeto](expressions.md#object-creation-expressions)) e por meio de *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicializadores de construtores

Todos os construtores de instância (exceto aqueles para `object`classe) incluem implicitamente uma invocação de outro construtor de instância imediatamente antes de *constructor_body*. O construtor para invocar implicitamente é determinado pelo *constructor_initializer*:

*  Um inicializador de construtor de instância `base(argument_list)` do `base()` formulário ou faz com que um construtor de instância da classe base direta seja invocado. Esse construtor é selecionado usando *argument_list* , se presente, e as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution). O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessíveis contidos na classe base direta ou no construtor padrão ([construtores padrão](classes.md#default-constructors)), se nenhum construtor de instância for declarado na classe base direta. Se esse conjunto estiver vazio, ou se um único Construtor de instância recomendada não puder ser identificado, ocorrerá um erro em tempo de compilação.
*  Um inicializador de construtor de instância `this(argument-list)` do `this()` formulário ou faz com que um construtor de instância da própria classe seja invocado. O construtor é selecionado usando *argument_list* , se presente, e as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution). O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessíveis declarados na própria classe. Se esse conjunto estiver vazio, ou se um único Construtor de instância recomendada não puder ser identificado, ocorrerá um erro em tempo de compilação. Se uma declaração de construtor de instância incluir um inicializador de construtor que invoca o Construtor em si, ocorrerá um erro em tempo de compilação.

Se um construtor de instância não tiver nenhum inicializador de construtor, um inicializador de construtor do formulário `base()` será fornecido implicitamente. Portanto, uma declaração de construtor de instância do formulário
```csharp
C(...) {...}
```
é exatamente equivalente a
```csharp
C(...): base() {...}
```

O escopo dos parâmetros fornecidos pelo *formal_parameter_list* de uma declaração de construtor de instância inclui o inicializador de construtor dessa declaração. Assim, um inicializador de construtor tem permissão para acessar os parâmetros do construtor. Por exemplo:
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

Um inicializador de construtor de instância não pode acessar a instância que está sendo criada. Portanto, é um erro de tempo de compilação a `this` ser referenciado em uma expressão de argumento do inicializador de construtor, pois é um erro de tempo de compilação para uma expressão de argumento referenciar qualquer membro de instância por meio de um *Simple_name*.

### <a name="instance-variable-initializers"></a>Inicializadores de variável de instância

Quando um construtor de instância não tem nenhum inicializador de construtor ou tem um inicializador de `base(...)`Construtor do formulário, esse construtor executa implicitamente as inicializações especificadas pelos *variable_initializer*s dos campos de instância declarado em sua classe. Isso corresponde a uma sequência de atribuições que são executadas imediatamente após a entrada para o construtor e antes da invocação implícita do construtor da classe base direta. Os inicializadores de variável são executados na ordem textual em que aparecem na declaração de classe.

### <a name="constructor-execution"></a>Execução do Construtor

Inicializadores de variáveis são transformados em instruções de atribuição, e essas instruções de atribuição são executadas antes da invocação do construtor da instância da classe base. Essa ordenação garante que todos os campos de instância sejam inicializados por seus inicializadores variáveis antes que qualquer instrução que tenha acesso a essa instância seja executada.

Dado o exemplo
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
Quando `new B()` é usado para criar uma instância do `B`, a seguinte saída é produzida:
```
x = 1, y = 0
```

O valor de `x` é 1 porque o inicializador de variável é executado antes que o construtor da instância da classe base seja invocado. No entanto, o `y` valor de é 0 (o valor padrão `int`de um) porque a `y` atribuição a não é executada até que o construtor da classe base seja retornado.

É útil considerar inicializadores de variável de instância e inicializadores de construtor como instruções que são inseridas automaticamente antes de *constructor_body*. O exemplo
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
contém vários inicializadores de variável; Ele também contém inicializadores de construtor de ambos os`base` formulários `this`(e). O exemplo corresponde ao código mostrado abaixo, onde cada comentário indica uma instrução inserida automaticamente (a sintaxe usada para as invocações de Construtor inseridas automaticamente não é válida, mas meramente serve para ilustrar o mecanismo).

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

Se uma classe não contiver nenhuma declaração de construtor de instância, um construtor de instância padrão será fornecido automaticamente. Esse construtor padrão simplesmente invoca o construtor sem parâmetros da classe base direta. Se a classe for abstrata, a acessibilidade declarada para o construtor padrão será protegida. Caso contrário, a acessibilidade declarada para o construtor padrão é pública. Portanto, o construtor padrão sempre está no formato

```csharp
protected C(): base() {}
```
ou
```csharp
public C(): base() {}
```
em `C` que é o nome da classe. Se a resolução de sobrecarga não puder determinar um melhor candidato exclusivo para o inicializador de construtor da classe base, ocorrerá um erro em tempo de compilação.

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

Quando uma classe `T` declara apenas construtores de instância privada, não é possível para classes fora do texto do programa de `T` para derivar de `T` ou para criar instâncias diretamente do `T`. Portanto, se uma classe contiver apenas membros estáticos e não tiver a intenção de ser instanciada, a adição de um construtor de instância particular vazio impedirá a instanciação. Por exemplo:
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

A `Trig` classe agrupa métodos e constantes relacionados, mas não deve ser instanciada. Portanto, ele declara um único Construtor de instância particular vazio. Pelo menos um construtor de instância deve ser declarado para suprimir a geração automática de um construtor padrão.

### <a name="optional-instance-constructor-parameters"></a>Parâmetros opcionais do construtor de instância

A `this(...)` forma de inicializador de construtor é comumente usada em conjunto com sobrecarga para implementar parâmetros de construtor de instância opcionais. No exemplo
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
os primeiros dois construtores de instância fornecem apenas os valores padrão para os argumentos ausentes. Ambos usam um `this(...)` inicializador de construtor para invocar o terceiro construtor de instância, que realmente faz o trabalho de inicializar a nova instância. O efeito é o dos parâmetros de Construtor opcionais:
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

O *identificador* de um *static_constructor_declaration* deve nomear a classe na qual o construtor estático é declarado. Se qualquer outro nome for especificado, ocorrerá um erro em tempo de compilação.

Quando uma declaração de construtor estático inclui `extern` um modificador, o construtor estático é considerado um ***construtor estático externo***. Como uma declaração de construtor estático externa não fornece nenhuma implementação real, seu *static_constructor_body* consiste em um ponto-e-vírgula. Para todas as outras declarações de construtor estático, o *static_constructor_body* consiste em um *bloco* que especifica as instruções a serem executadas para inicializar a classe. Isso corresponde exatamente ao *method_body* de um método estático com um tipo `void` de retorno ([corpo do método](classes.md#method-body)).

Construtores estáticos não são herdados e não podem ser chamados diretamente.

O construtor estático para um tipo de classe fechada é executado no máximo uma vez em um determinado domínio de aplicativo. A execução de um construtor estático é disparada pelo primeiro dos seguintes eventos para ocorrer dentro de um domínio de aplicativo:

*  Uma instância do tipo de classe é criada.
*  Qualquer um dos membros estáticos do tipo de classe é referenciado.

Se uma classe contiver `Main` o método ([inicialização do aplicativo](basic-concepts.md#application-startup)) no qual a execução começa, o construtor estático para aquela classe é `Main` executado antes de o método ser chamado.

Para inicializar um novo tipo de classe fechada, primeiro um novo conjunto de campos estáticos ([campos estáticos e de instância](classes.md#static-and-instance-fields)) para esse tipo fechado específico é criado. Cada um dos campos estáticos é inicializado para seu valor padrão ([valores padrão](variables.md#default-values)). Em seguida, os inicializadores de campo estáticos ([inicialização de campo estático](classes.md#static-field-initialization)) são executados para esses campos estáticos. Por fim, o construtor estático é executado.

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
Porque a execução do `A`construtor estático do é disparada pela chamada `A.F`para, e a execução `B`do construtor estático do é disparada pela `B.F`chamada para.

É possível construir dependências circulares que permitem que campos estáticos com inicializadores de variáveis sejam observados em seu estado de valor padrão.

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

Para executar o `Main` método, o sistema primeiro executa o inicializador `B.Y`para, antes do `B`construtor estático da classe. `Y`o inicializador `A`faz com que o construtor estático seja executado porque o `A.X` valor de é referenciado. O construtor estático de `A` , por sua vez, prossegue para calcular o `X`valor de e, ao fazer isso, busca o valor `Y`padrão de, que é zero. `A.X`é, portanto, inicializado como 1. O processo de execução `A`de inicializadores de campo estático e construtor estático é concluído, retornando ao cálculo do valor inicial de `Y`, o resultado é 2.

Como o construtor estático é executado exatamente uma vez para cada tipo de classe construída fechada, é um local conveniente para impor verificações de tempo de execução no parâmetro de tipo que não pode ser verificado em tempo de compilação via restrições ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) . Por exemplo, o tipo a seguir usa um construtor estático para impor que o argumento de tipo seja uma enumeração:
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

Um ***destruidor*** é um membro que implementa as ações necessárias para destruir uma instância de uma classe. Um destruidor é declarado usando um *destructor_declaration*:

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

O *identificador* de um *destructor_declaration* deve nomear a classe na qual o destruidor é declarado. Se qualquer outro nome for especificado, ocorrerá um erro em tempo de compilação.

Quando uma declaração de destruidor inclui `extern` um modificador, diz-se que o destruidor é um ***destruidor externo***. Como uma declaração de destruidor externo não fornece implementação real, seu *destructor_body* consiste em um ponto-e-vírgula. Para todos os outros destruidores, o *destructor_body* consiste em um *bloco* que especifica as instruções a serem executadas a fim de destruir uma instância da classe. Um *destructor_body* corresponde exatamente ao *method_body* de um método de instância com um `void` tipo de retorno ([corpo do método](classes.md#method-body)).

Os destruidores não são herdados. Portanto, uma classe não tem destruidores além daquele que pode ser declarado nessa classe.

Como um destruidor é necessário para não ter parâmetros, ele não pode ser sobrecarregado, portanto, uma classe pode ter, no máximo, um destruidor.

Os destruidores são invocados automaticamente e não podem ser invocados explicitamente. Uma instância fica qualificada para destruição quando não é mais possível que qualquer código use essa instância. A execução do destruidor para a instância pode ocorrer a qualquer momento depois que a instância for qualificada para destruição. Quando uma instância é destruída, os destruidores na cadeia de herança dessa instância são chamados, em ordem, da mais derivada para a menos derivada. Um destruidor pode ser executado em qualquer thread. Para obter mais informações sobre as regras que regem quando e como um destruidor é executado, consulte [gerenciamento automático de memória](basic-concepts.md#automatic-memory-management).

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
como os destruidores em uma cadeia de herança são chamados em ordem, da mais derivada para a menos derivada.

Os destruidores são implementados substituindo o `Finalize` método `System.Object`virtual em. C#os programas não têm permissão para substituir esse método ou chamá-lo (ou substituí-lo) diretamente. Por exemplo, o programa
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

O compilador se comporta como se esse método e as substituições, não existem. Portanto, este programa:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
é válido e o método mostrado oculta `System.Object` `Finalize` o método.

Para obter uma discussão sobre o comportamento quando uma exceção é gerada de um destruidor, consulte [como as exceções são tratadas](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterators

Um membro de função ([membros da função](expressions.md#function-members)) implementado usando um bloco do iterador ([blocos](statements.md#blocks)) é chamado de ***iterador***.

Um bloco do iterador pode ser usado como o corpo de um membro da função, desde que o tipo de retorno do membro da função correspondente seja uma das interfaces do enumerador ([interfaces do enumerador](classes.md#enumerator-interfaces)) ou uma das interfaces enumeráveis ([interfaces enumeráveis](classes.md#enumerable-interfaces)) . Ele pode ocorrer como um *method_body*, *operator_body* ou *accessor_body*, enquanto eventos, construtores de instância, construtores estáticos e destruidores não podem ser implementados como iteradores.

Quando um membro de função é implementado usando um bloco iterador, ele é um erro de tempo de compilação para a lista de parâmetros formais do membro `ref` da `out` função para especificar qualquer parâmetro ou.

### <a name="enumerator-interfaces"></a>Interfaces do enumerador

As ***interfaces do enumerador*** são a interface `System.Collections.IEnumerator` não genérica e todas as instanciações da interface `System.Collections.Generic.IEnumerator<T>`genérica. Por questão de brevidade, neste capítulo, essas interfaces são referenciadas como `IEnumerator` e `IEnumerator<T>`, respectivamente.

### <a name="enumerable-interfaces"></a>Interfaces enumeráveis

As ***interfaces enumeráveis*** são a interface `System.Collections.IEnumerable` não genérica e todas as instanciações da interface `System.Collections.Generic.IEnumerable<T>`genérica. Por questão de brevidade, neste capítulo, essas interfaces são referenciadas como `IEnumerable` e `IEnumerable<T>`, respectivamente.

### <a name="yield-type"></a>Tipo yield

Um iterador produz uma sequência de valores, todos do mesmo tipo. Esse tipo é chamado de ***tipo yield*** do iterador.

*  O tipo yield de um iterador que `IEnumerator` retorna `IEnumerable` ou `object`é.
*  O tipo yield de um iterador que `IEnumerator<T>` retorna `IEnumerable<T>` ou `T`é.

### <a name="enumerator-objects"></a>Objetos do enumerador

Quando um membro de função que retorna um tipo de interface de enumerador é implementado usando um bloco de iterador, invocar o membro da função não executa imediatamente o código no bloco do iterador. Em vez disso, um ***objeto enumerador*** é criado e retornado. Esse objeto encapsula o código especificado no bloco do iterador e a execução do código no bloco do iterador ocorre quando o método do `MoveNext` objeto enumerador é invocado. Um objeto enumerador tem as seguintes características:

*  Ele implementa `IEnumerator` e `IEnumerator<T>`, onde `T` é o tipo yield do iterador.
*  Ele implementa `System.IDisposable`.
*  Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor de instância passado para o membro da função.
*  Ele tem quatro Estados potenciais, ***antes***, ***em execução***, ***suspenso***e ***depois***, e está inicialmente no estado ***anterior*** .

Um objeto enumerador normalmente é uma instância de uma classe enumeradora gerada pelo compilador que encapsula o código no bloco iterador e implementa as interfaces do enumerador, mas outros métodos de implementação são possíveis. Se uma classe de enumerador for gerada pelo compilador, essa classe será aninhada, direta ou indiretamente, na classe que contém o membro da função, ela terá uma acessibilidade privada e terá um nome reservado para uso do compilador ([identificadores](lexical-structure.md#identifiers)).

Um objeto enumerador pode implementar mais interfaces do que aquelas especificadas acima.

As seções a seguir descrevem o comportamento exato dos `MoveNext`Membros `Current`, e `Dispose` , das `IEnumerable` implementações `IEnumerable<T>` de interface e fornecidas por um objeto enumerador.

Observe que os objetos do enumerador não `IEnumerator.Reset` oferecem suporte ao método. Invocar esse método faz com `System.NotSupportedException` que um seja gerado.

#### <a name="the-movenext-method"></a>O método MoveNext

O `MoveNext` método de um objeto de enumerador encapsula o código de um bloco de iterador. Invocar o `MoveNext` método executa o código no bloco do iterador e define `Current` a propriedade do objeto enumerador conforme apropriado. A ação precisa executada `MoveNext` depende do estado do objeto do enumerador quando `MoveNext` o é invocado:

*  Se o estado do objeto enumerador for ***antes***, invocando `MoveNext`:
   * Altera o estado para ***em execução***.
   * Inicializa os parâmetros (incluindo `this`) do bloco do iterador para os valores de argumento e o valor de instância salvos quando o objeto enumerador foi inicializado.
   * Executa o bloco de iteradores desde o início até que a execução seja interrompida (conforme descrito abaixo).
*  Se o estado do objeto enumerador estiver ***em execução***, o resultado da invocação `MoveNext` será não especificado.
*  Se o estado do objeto do enumerador for ***suspenso***, invocando `MoveNext`:
   * Altera o estado para ***em execução***.
   * Restaura os valores de todas as variáveis e parâmetros locais (incluindo isso) para os valores salvos quando a execução do bloco do iterador foi suspensa pela última vez. Observe que o conteúdo de quaisquer objetos referenciados por essas variáveis pode ter sido alterado desde a chamada anterior para MoveNext.
   * Retoma a execução do bloco iterador imediatamente após a `yield return` instrução que causou a suspensão da execução e continua até que a execução seja interrompida (conforme descrito abaixo).
*  Se o estado do objeto enumerador for ***After***, invocando `MoveNext` retorna `false`.


Quando `MoveNext` o executa o bloco do iterador, a execução pode ser interrompida de quatro maneiras: Por uma `yield return` instrução, por uma `yield break` instrução, encontrando o final do bloco do iterador e, por uma exceção sendo gerada e propagada do bloco do iterador.

*  Quando uma `yield return` instrução é encontrada ([a instrução yield](statements.md#the-yield-statement)):
   * A expressão fornecida na instrução é avaliada, implicitamente convertida para o tipo yield e atribuída à `Current` Propriedade do objeto Enumerator.
   * A execução do corpo do iterador é suspensa. Os valores de todas as variáveis e parâmetros locais ( `this`incluindo) são salvos, como é o local `yield return` desta instrução. Se a `yield return` instrução estiver dentro de um ou `try` mais blocos, os `finally` blocos associados não serão executados neste momento.
   * O estado do objeto do enumerador é alterado para ***suspenso***.
   * O `MoveNext` método retorna `true` ao chamador, indicando que a iteração foi avançada com êxito para o próximo valor.
*  Quando uma `yield break` instrução é encontrada ([a instrução yield](statements.md#the-yield-statement)):
   * Se a `yield break` instrução estiver dentro de um ou `try` mais blocos, os `finally` blocos associados serão executados.
   * O estado do objeto enumerador é alterado para ***After***.
   * O `MoveNext` método retorna `false` ao seu chamador, indicando que a iteração foi concluída.
*  Quando o final do corpo do iterador for encontrado:
   * O estado do objeto enumerador é alterado para ***After***.
   * O `MoveNext` método retorna `false` ao seu chamador, indicando que a iteração foi concluída.
*  Quando uma exceção é gerada e propagada para fora do bloco do iterador:
   * Os `finally` blocos apropriados no corpo do iterador serão executados pela propagação de exceção.
   * O estado do objeto enumerador é alterado para ***After***.
   * A propagação de exceção continua para o chamador do `MoveNext` método.

#### <a name="the-current-property"></a>A propriedade atual

A propriedade de `Current` um objeto enumerador é `yield return` afetada por instruções no bloco do iterador.

Quando um objeto de enumerador está no estado ***suspenso*** , o valor `Current` de é o valor definido pela chamada anterior para `MoveNext`. Quando um objeto enumerador está nos Estados ***anterior***, ***em execução***ou ***depois*** , o resultado do acesso `Current` não é especificado.

Para um iterador com um tipo yield diferente `object`de, o resultado do `Current` acesso por meio da implementação `IEnumerable` do objeto enumerador `Current` `IEnumerator<T>` corresponde ao acesso por meio do objeto enumerador implementação e conversão do resultado em `object`.

#### <a name="the-dispose-method"></a>O método Dispose

O `Dispose` método é usado para limpar a iteração, trazendo o objeto enumerador para o estado ***After*** .

*  Se o estado do objeto enumerador for ***antes***, invocar `Dispose` altera o estado para ***After***.
*  Se o estado do objeto enumerador estiver ***em execução***, o resultado da invocação `Dispose` será não especificado.
*  Se o estado do objeto do enumerador for ***suspenso***, invocando `Dispose`:
   * Altera o estado para ***em execução***.
   * Executa qualquer bloco finally como se a última instrução executada `yield return` fosse uma `yield break` instrução. Se isso fizer com que uma exceção seja lançada e propagada do corpo do iterador, o estado do objeto enumerador será definido como ***After*** e a exceção será propagada para o `Dispose` chamador do método.
   * Altera o estado para ***depois***.
*  Se o estado do objeto enumerador for ***After***, a invocação `Dispose` não terá nenhum efeito.

### <a name="enumerable-objects"></a>Objetos enumeráveis

Quando um membro de função que retorna um tipo de interface enumerável é implementado usando um bloco de iterador, invocar o membro da função não executa imediatamente o código no bloco do iterador. Em vez disso, um ***objeto enumerável*** é criado e retornado. O método do `GetEnumerator` objeto Enumerable retorna um objeto enumerador que encapsula o código especificado no bloco do iterador e a execução do código no bloco do iterador ocorre quando o método do `MoveNext` objeto enumerador é invocado. Um objeto enumerável tem as seguintes características:

*  Ele implementa `IEnumerable` e `IEnumerable<T>`, onde `T` é o tipo yield do iterador.
*  Ele é inicializado com uma cópia dos valores de argumento (se houver) e o valor de instância passado para o membro da função.

Um objeto enumerável normalmente é uma instância de uma classe enumerável gerada pelo compilador que encapsula o código no bloco do iterador e implementa as interfaces enumeráveis, mas outros métodos de implementação são possíveis. Se uma classe enumerável for gerada pelo compilador, essa classe será aninhada, direta ou indiretamente, na classe que contém o membro da função, ela terá uma acessibilidade privada e terá um nome reservado para uso do compilador ([identificadores](lexical-structure.md#identifiers)).

Um objeto enumerável pode implementar mais interfaces do que aquelas especificadas acima. Em particular, um objeto enumerável também pode `IEnumerator` implementar `IEnumerator<T>`e, permitindo que ele sirva como um enumerável e um enumerador. Nesse tipo de implementação, na primeira vez que um método de `GetEnumerator` objeto enumerável é invocado, o próprio objeto enumerável é retornado. As invocações subsequentes do objeto `GetEnumerator`Enumerable, se houver, retornam uma cópia do objeto Enumerable. Assim, cada enumerador retornado tem seu próprio Estado e as alterações em um enumerador não afetarão outra.

#### <a name="the-getenumerator-method"></a>O método GetEnumerator

Um objeto enumerável fornece uma implementação dos `GetEnumerator` métodos `IEnumerable` das interfaces e `IEnumerable<T>` . Os dois `GetEnumerator` métodos compartilham uma implementação comum que adquire e retorna um objeto enumerador disponível. O objeto enumerador é inicializado com os valores de argumento e o valor da instância salvos quando o objeto Enumerable foi inicializado, mas, caso contrário, o objeto enumerador funciona conforme descrito em [objetos do enumerador](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Exemplo de implementação

Esta seção descreve uma possível implementação de iteradores em termos de construções C# padrão. A implementação descrita aqui se baseia nos mesmos princípios usados pelo compilador da Microsoft C# , mas não é por isso uma implementação obrigatória ou a única possível.

A classe `Stack<T>` a seguir implementa `GetEnumerator` seu método usando um iterador. O iterador enumera os elementos da pilha na ordem superior para a inferior.

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

O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerada pelo compilador que encapsula o código no bloco do iterador, conforme mostrado a seguir.

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

Na tradução anterior, o código no bloco do iterador é transformado em um computador de estado e `MoveNext` colocado no método da classe do enumerador. Além disso, a variável `i` local é transformada em um campo no objeto Enumerator para que ele possa continuar a existir em invocações de. `MoveNext`

O exemplo a seguir imprime uma tabela de multiplicação simples dos inteiros de 1 a 10. O `FromTo` método no exemplo retorna um objeto enumerável e é implementado usando um iterador.

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

O `FromTo` método pode ser convertido em uma instanciação de uma classe enumerável gerada pelo compilador que encapsula o código no bloco do iterador, conforme mostrado a seguir.

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

A classe Enumerable implementa as interfaces enumeráveis e as interfaces do enumerador, permitindo que ele sirva como um enumerável e um enumerador. Na primeira vez que `GetEnumerator` o método é invocado, o próprio objeto Enumerable é retornado. As invocações subsequentes do objeto `GetEnumerator`Enumerable, se houver, retornam uma cópia do objeto Enumerable. Assim, cada enumerador retornado tem seu próprio Estado e as alterações em um enumerador não afetarão outra. O `Interlocked.CompareExchange` método é usado para garantir a operação thread-safe.

Os `from` parâmetros `to` e são transformados em campos na classe Enumerable. Como `from` é modificado no bloco do iterador, um `__from` campo adicional é introduzido para conter o valor inicial fornecido `from` para em cada enumerador.

O `MoveNext` método lançará `InvalidOperationException` um se for chamado quando `__state` for `0`. Isso protege contra o uso do objeto Enumerable como um objeto enumerador sem primeiro `GetEnumerator`chamar.

O exemplo a seguir mostra uma classe de árvore simples. A `Tree<T>` classe implementa seu `GetEnumerator` método usando um iterador. O iterador enumera os elementos da árvore em ordem de infixo.

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

O `GetEnumerator` método pode ser convertido em uma instanciação de uma classe de enumerador gerada pelo compilador que encapsula o código no bloco do iterador, conforme mostrado a seguir.

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

O compilador gerado temporaries usado nas `foreach` instruções é `__left` dividido nos campos e `__right` do objeto enumerador. O `__state` campo do objeto enumerador é cuidadosamente atualizado para que o método `Dispose()` correto seja chamado corretamente se uma exceção for lançada. Observe que não é possível escrever o código traduzido com instruções simples `foreach` .

## <a name="async-functions"></a>Funções assíncronas

Um método ([métodos](classes.md#methods)) ou uma função anônima ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) com o `async` modificador é chamado de ***função Async***. Em geral, o termo ***Async*** é usado para descrever qualquer tipo de função que tenha o `async` modificador.

É um erro de tempo de compilação para a lista de parâmetros formais de uma função Async para `ref` especificar `out` qualquer parâmetro ou.

O *return_type* de um método assíncrono deve ser `void` um tipo de ***tarefa***ou. Os tipos de tarefa `System.Threading.Tasks.Task` são e os tipos `System.Threading.Tasks.Task<T>`construídos a partir de. Para fins de brevidade, neste capítulo, esses tipos são referenciados como `Task` e `Task<T>`, respectivamente. Um método assíncrono que retorna um tipo de tarefa é considerado como sendo retornado por tarefa.

A definição exata dos tipos de tarefa é a implementação definida, mas, no ponto de vista do idioma, um tipo de tarefa está em um dos Estados incompletos, com êxito ou com falha. Uma tarefa com falha registra uma exceção pertinente. Um êxito `Task<T>` registra um resultado do tipo `T`. Os tipos de tarefa são awaitable e, portanto, podem ser os operandos das expressões Await ([Await expressões](expressions.md#await-expressions)).

Uma invocação de função assíncrona tem a capacidade de suspender a avaliação por meio de expressões Await ([expressões Await](expressions.md#await-expressions)) em seu corpo. Posteriormente, a avaliação pode ser retomada no ponto da expressão Await de suspensão por meio de um ***delegado de continuação***. O delegado de continuação é do tipo `System.Action`e, quando é invocado, a avaliação da invocação de função assíncrona retomará a partir da expressão Await onde ela parou. O ***chamador atual*** de uma invocação de função assíncrona é o chamador original se a invocação de função nunca foi suspensa ou o chamador mais recente do delegado de continuação, caso contrário.

### <a name="evaluation-of-a-task-returning-async-function"></a>Avaliação de uma função assíncrona de retorno de tarefa

A invocação de uma função assíncrona de retorno de tarefa faz com que uma instância do tipo de tarefa retornada seja gerada. Isso é chamado de ***tarefa de retorno*** da função Async. A tarefa está inicialmente em um estado incompleto.

O corpo da função assíncrona é então avaliado até que seja suspenso (atingindo uma expressão Await) ou seja encerrado, no qual o controle de ponto é retornado ao chamador, junto com a tarefa de retorno.

Quando o corpo da função Async é encerrado, a tarefa de retorno é movida do estado incompleto:

*  Se o corpo da função terminar como o resultado de atingir uma instrução de retorno ou o final do corpo, qualquer valor de resultado será registrado na tarefa de retorno, que será colocado em um estado de êxito.
*  Se o corpo da função for encerrado como resultado de uma exceção não percebida ([a instrução Throw](statements.md#the-throw-statement)), a exceção será registrada na tarefa de retorno que será colocada em um estado com falha.

### <a name="evaluation-of-a-void-returning-async-function"></a>Avaliação de uma função assíncrona de retorno nulo

Se o tipo de retorno da função Async for `void`, a avaliação será diferente da seguinte maneira: Como nenhuma tarefa é retornada, a função comunica a conclusão e as exceções ao ***contexto de sincronização***do thread atual. A definição exata do contexto de sincronização é dependente da implementação, mas é uma representação de "onde" o thread atual está em execução. O contexto de sincronização é notificado quando a avaliação de uma função assíncrona de retorno nulo começa, é concluída com êxito ou faz com que uma exceção não percebida seja gerada.

Isso permite que o contexto Mantenha o controle de quantas funções assíncronas de retorno nulo estão em execução e para decidir como propagar exceções saindo delas.
