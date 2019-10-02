---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704036"
---
# <a name="structs"></a>Structs

As estruturas são semelhantes às classes, pois representam estruturas de dados que podem conter membros de dados e membros de função. No entanto, ao contrário das classes, as structs são tipos de valor e não exigem alocação de heap. Uma variável de um tipo struct contém diretamente os dados da estrutura, enquanto uma variável de um tipo de classe contém uma referência aos dados, o último conhecido como um objeto.

Os structs são particularmente úteis para estruturas de dados pequenas que têm semântica de valor. Números complexos, pontos em um sistema de coordenadas ou pares chave-valor em um dicionário são exemplos de structs. A chave para essas estruturas de dados é que elas têm poucos membros de dados, que não exigem o uso de herança ou identidade referencial, e que podem ser convenientemente implementadas usando semântica de valor em que a atribuição copia o valor em vez da referência.

Conforme descrito em [tipos simples](types.md#simple-types), os tipos simples fornecidos pelo C#, como `int`, `double` e `bool`, são, na verdade, todos os tipos de struct. Assim como esses tipos predefinidos são structs, também é possível usar structs e sobrecarga de operador para implementar novos tipos "primitivos" no C# idioma. Dois exemplos desses tipos são fornecidos no final deste capítulo ([exemplos de struct](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Declarações de struct

Um *struct_declaration* é um *type_declaration* ([declarações de tipo](namespaces.md#type-declarations)) que declara uma nova struct:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

Um *struct_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *struct_modifier*s ([modificadores de struct](structs.md#struct-modifiers)), seguido por um modificador opcional `partial`, seguido pelo palavra-chave `struct` e um *identificador* que nomeia a struct, seguido por uma especificação *type_parameter_list* opcional ([parâmetros de tipo](classes.md#type-parameters)), seguida por uma especificação *struct_interfaces* opcional ([parcial ),](structs.md#partial-modifier)seguido por uma especificação opcional *type_parameter_constraints_clause*s (restrições de[parâmetro de tipo](classes.md#type-parameter-constraints)), seguida por um *struct_body* ([corpo de struct](structs.md#struct-body)), opcionalmente seguido por um ponto e vírgula.

### <a name="struct-modifiers"></a>Modificadores de struct

Um *struct_declaration* pode, opcionalmente, incluir uma sequência de modificadores de struct:

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

É um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de struct.

Os modificadores de uma declaração de struct têm o mesmo significado que os de uma declaração de classe ([declarações de classe](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Modificador parcial

O modificador `partial` indica que esse *struct_declaration* é uma declaração de tipo parcial. Várias declarações de struct parciais com o mesmo nome dentro de um namespace delimitador ou declaração de tipo são combinadas para formar uma declaração de struct, seguindo as regras especificadas em [tipos parciais](classes.md#partial-types).

### <a name="struct-interfaces"></a>Interfaces de struct

Uma declaração struct pode incluir uma especificação *struct_interfaces* . nesse caso, o struct é dito para implementar diretamente os tipos de interface fornecidos.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementações de interface são discutidas mais detalhadamente em [implementações de interface](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Corpo da struct

O *struct_body* de uma struct define os membros da estrutura.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Membros de struct

Os membros de uma struct consistem nos membros introduzidos por seus *struct_member_declaration*s e nos membros herdados do tipo `System.ValueType`.

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

Exceto pelas diferenças observadas nas [diferenças de classe e estrutura](structs.md#class-and-struct-differences), as descrições dos membros de classe fornecidos em [membros de classe](classes.md#class-members) por meio de [iteradores](classes.md#iterators) também se aplicam a membros de struct.

## <a name="class-and-struct-differences"></a>Diferenças de classe e struct

As estruturas diferem das classes de várias maneiras importantes:

*  Structs são tipos de valor ([semântica de valor](structs.md#value-semantics)).
*  Todos os tipos de struct herdam implicitamente da classe `System.ValueType` ([herança](structs.md#inheritance)).
*  A atribuição a uma variável de um tipo struct cria uma cópia do valor que está sendo atribuído ([atribuição](structs.md#assignment)).
*  O valor padrão de uma struct é o valor produzido pela configuração de todos os campos de tipo de valor para seu valor padrão e todos os campos de tipo de referência como `null` ([valores padrão](structs.md#default-values)).
*  As operações boxing e unboxing são usadas para converter entre um tipo struct e `object` ([boxing e unboxing](structs.md#boxing-and-unboxing)).
*  O significado de `this` é diferente para structs ([esse acesso](expressions.md#this-access)).
*  As declarações de campo de instância para um struct não têm permissão para incluir inicializadores de variável ([inicializadores de campo](structs.md#field-initializers)).
*  Um struct não tem permissão para declarar um construtor de instância sem parâmetros ([construtores](structs.md#constructors)).
*  Uma struct não tem permissão para declarar um destruidor ([destruidores](structs.md#destructors)).

### <a name="value-semantics"></a>Semântica de valor

Structs são tipos de valor ([tipos de valor](types.md#value-types)) e são considerados como semântica de valor. As classes, por outro lado, são tipos de referência ([tipos de referência](types.md#reference-types)) e são consideradas semânticas de referência.

Uma variável de um tipo struct contém diretamente os dados da estrutura, enquanto uma variável de um tipo de classe contém uma referência aos dados, o último conhecido como um objeto. Quando um struct `B` contém um campo de instância do tipo `A` e `A` é um tipo de struct, é um erro de tempo de compilação para `A` depender de `B` ou um tipo construído a partir de `B`. Um struct `X` ***depende diretamente*** de um struct `Y` se `X` contiver um campo de instância do tipo `Y`. Dada essa definição, o conjunto completo de structs sobre o qual uma estrutura depende é o fechamento transitivo da relação ***depende diretamente*** de.  Por exemplo
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
é um erro porque `Node` contém um campo de instância de seu próprio tipo.  Outro exemplo
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
é um erro porque cada um dos tipos `A`, `B` e `C` dependem uns dos outros.

Com classes, é possível que duas variáveis referenciem o mesmo objeto e, assim, possíveis para operações em uma variável afetem o objeto referenciado pela outra variável. Com as estruturas, cada uma tem sua própria cópia dos dados (exceto no caso de variáveis de parâmetro `ref` e `out`), e não é possível que as operações em um afetem a outra. Além disso, como structs não são tipos de referência, não é possível que os valores de um tipo struct sejam `null`.

Dada a declaração
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
o fragmento de código
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
gera o valor `10`. A atribuição de `a` a `b` cria uma cópia do valor e `b`, portanto, não é afetado pela atribuição a `a.x`. @No__t-0, em vez de ser declarado como uma classe, a saída seria `100` porque `a` e `b` referenciariam o mesmo objeto.

### <a name="inheritance"></a>Herança

Todos os tipos de struct herdam implicitamente da classe `System.ValueType`, que, por sua vez, herda da classe `object`. Uma declaração struct pode especificar uma lista de interfaces implementadas, mas não é possível que uma declaração struct especifique uma classe base.

Os tipos de struct nunca são abstratos e sempre são lacrados implicitamente. Os modificadores `abstract` e `sealed`, portanto, não são permitidos em uma declaração de struct.

Como a herança não tem suporte para structs, a acessibilidade declarada de um Membro struct não pode ser `protected` ou `protected internal`.

Membros de função em um struct não podem ser `abstract` ou `virtual`, e o modificador `override` é permitido apenas para substituir métodos herdados de `System.ValueType`.

### <a name="assignment"></a>Atribuição

A atribuição a uma variável de um tipo struct cria uma cópia do valor que está sendo atribuído. Isso difere da atribuição a uma variável de um tipo de classe, que copia a referência, mas não o objeto identificado pela referência.

Semelhante a uma atribuição, quando uma struct é passada como um parâmetro de valor ou retornado como resultado de um membro de função, uma cópia da estrutura é criada. Uma struct pode ser passada por referência a um membro de função usando um parâmetro `ref` ou `out`.

Quando uma propriedade ou um indexador de uma struct é o destino de uma atribuição, a expressão de instância associada ao acesso de propriedade ou indexador deve ser classificada como uma variável. Se a expressão de instância for classificada como um valor, ocorrerá um erro em tempo de compilação. Isso é descrito em mais detalhes em [atribuição simples](expressions.md#simple-assignment).

### <a name="default-values"></a>Valores padrão

Conforme descrito em [valores padrão](variables.md#default-values), vários tipos de variáveis são inicializados automaticamente para seu valor padrão quando eles são criados. Para variáveis de tipos de classe e outros tipos de referência, esse valor padrão é `null`. No entanto, como structs são tipos de valor que não podem ser `null`, o valor padrão de uma struct é o valor produzido pela configuração de todos os campos de tipo de valor para seu valor padrão e todos os campos de tipo de referência como `null`.

Referindo-se ao struct `Point` declarado acima, o exemplo
```csharp
Point[] a = new Point[100];
```
Inicializa cada `Point` na matriz para o valor produzido definindo os campos `x` e `y` como zero.

O valor padrão de uma struct corresponde ao valor retornado pelo construtor padrão da struct ([construtores padrão](types.md#default-constructors)). Ao contrário de uma classe, uma struct não tem permissão para declarar um construtor de instância sem parâmetros. Em vez disso, cada struct implicitamente tem um construtor de instância sem parâmetros que sempre retorna o valor resultante da definição de todos os campos de tipo de valor para seu valor padrão e todos os campos de tipo de referência como `null`.

As estruturas devem ser projetadas para considerar o estado de inicialização padrão um estado válido. No exemplo
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
o construtor de instância definido pelo usuário protege contra valores nulos somente quando ele é chamado explicitamente. Nos casos em que uma variável `KeyValuePair` está sujeita à inicialização de valor padrão, os campos `key` e `value` serão nulos e o struct deverá estar preparado para lidar com esse estado.

### <a name="boxing-and-unboxing"></a>Conversões boxing e unboxing

Um valor de um tipo de classe pode ser convertido para o tipo `object` ou para um tipo de interface que é implementado pela classe simplesmente tratando a referência como outro tipo em tempo de compilação. Da mesma forma, um valor do tipo `object` ou um valor de um tipo de interface pode ser convertido de volta para um tipo de classe sem alterar a referência (mas é claro que uma verificação de tipo em tempo de execução é necessária nesse caso).

Como structs não são tipos de referência, essas operações são implementadas de forma diferente para tipos struct. Quando um valor de um tipo struct é convertido no tipo `object` ou em um tipo de interface que é implementado pela estrutura, ocorre uma operação boxing. Da mesma forma, quando um valor do tipo `object` ou um valor de um tipo de interface é convertido de volta em um tipo struct, ocorre uma operação de unboxing. Uma diferença importante das mesmas operações em tipos de classe é que a boxing e a unboxing copiam o valor de struct para dentro ou para fora da instância em caixa. Portanto, após uma operação Boxing ou unboxing, as alterações feitas no struct não-Boxed não são refletidas no struct boxed.

Quando um tipo de struct substitui um método virtual herdado de `System.Object` (como `Equals`, `GetHashCode` ou `ToString`), a invocação do método virtual por meio de uma instância do tipo struct não faz com que a Boxing ocorra. Isso é verdadeiro mesmo quando a struct é usada como um parâmetro de tipo e a invocação ocorre por meio de uma instância do tipo de parâmetro de tipo. Por exemplo:
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

A saída do programa é:
```console
1
2
3
```

Embora seja um estilo inadequado para `ToString` ter efeitos colaterais, o exemplo demonstra que nenhuma Boxing ocorreu para as três invocações de `x.ToString()`.

Da mesma forma, a Boxing nunca ocorre implicitamente ao acessar um membro em um parâmetro de tipo restrito. Por exemplo, suponha que uma interface `ICounter` contenha um método `Increment`, que pode ser usado para modificar um valor. Se `ICounter` for usado como uma restrição, a implementação do método `Increment` será chamada com uma referência à variável que `Increment` foi chamada em, nunca em uma cópia em caixa.

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

A primeira chamada para `Increment` modifica o valor na variável `x`. Isso não é equivalente à segunda chamada para `Increment`, que modifica o valor em uma cópia em caixa de `x`. Assim, a saída do programa é:
```console
0
1
1
```

Para obter mais detalhes sobre boxing e unboxing, consulte [boxing e unboxing](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Significado disso

Dentro de um construtor de instância ou membro de função de instância de uma classe, `this` é classificado como um valor. Portanto, embora `this` possa ser usado para fazer referência à instância para a qual o membro de função foi invocado, não é possível atribuir a `this` em um membro de função de uma classe.

Dentro de um construtor de instância de uma struct, `this` corresponde a um parâmetro `out` do tipo struct e dentro de um membro da função de instância de uma struct, `this` corresponde a um parâmetro `ref` do tipo struct. Em ambos os casos, `this` é classificado como uma variável, e é possível modificar toda a estrutura para a qual o membro da função foi invocado, atribuindo a `this` ou passando-o como um parâmetro `ref` ou `out`.

### <a name="field-initializers"></a>Inicializadores de campo

Conforme descrito em [valores padrão](structs.md#default-values), o valor padrão de uma struct consiste no valor que resulta da definição de todos os campos de tipo de valor para seu valor padrão e de todos os campos de tipo de referência como `null`. Por esse motivo, uma struct não permite que declarações de campo de instância incluam inicializadores de variável. Essa restrição se aplica somente a campos de instância. Os campos estáticos de uma estrutura têm permissão para incluir inicializadores de variável.

O exemplo
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
está em erro porque as declarações de campo de instância incluem inicializadores de variável.

### <a name="constructors"></a>Construtores

Ao contrário de uma classe, uma struct não tem permissão para declarar um construtor de instância sem parâmetros. Em vez disso, cada struct implicitamente tem um construtor de instância sem parâmetros que sempre retorna o valor resultante da definição de todos os campos de tipo de valor para seu valor padrão e todos os campos de tipo de referência como NULL ([construtores padrão](types.md#default-constructors)). Uma struct pode declarar construtores de instância com parâmetros. Por exemplo
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Dada a declaração acima, as instruções
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
ambos criam um `Point` com `x` e `y` inicializados como zero.

Um construtor de instância de struct não tem permissão para incluir um inicializador de construtor do formulário `base(...)`.

Se o construtor da instância de struct não especificar um inicializador de construtor, a variável `this` corresponderá a um parâmetro `out` do tipo struct e semelhante a um parâmetro `out`, `this` deverá ser definitivamente atribuído ([atribuição definitiva ](variables.md#definite-assignment)) em cada local em que o construtor retorna. Se o construtor da instância de struct especificar um inicializador de construtor, a variável `this` corresponderá a um parâmetro `ref` do tipo struct e semelhante a um parâmetro `ref`, `this` será considerado definitivamente atribuído na entrada ao corpo do Construtor . Considere a implementação do construtor de instância abaixo:
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

Nenhuma função de membro de instância (incluindo os acessadores set para as propriedades `X` e `Y`) pode ser chamada até que todos os campos da estrutura que está sendo construída tenham sido definitivamente atribuídos. A única exceção envolve Propriedades implementadas automaticamente ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)). As regras de atribuição definidas ([expressões de atribuição simples](variables.md#simple-assignment-expressions)) especificamente isentam a atribuição para uma propriedade automática de um tipo struct dentro de um construtor de instância desse tipo struct: tal atribuição é considerada uma atribuição definitiva da oculta campo de apoio da propriedade automática. Assim, é permitido o seguinte:

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>Destruidores

Um struct não tem permissão para declarar um destruidor.

### <a name="static-constructors"></a>Construtores estáticos

Construtores estáticos para structs seguem a maioria das mesmas regras que para classes. A execução de um construtor estático para um tipo de struct é disparada pelo primeiro dos seguintes eventos para ocorrer dentro de um domínio de aplicativo:

*  Um membro estático do tipo struct é referenciado.
*  Um construtor declarado explicitamente do tipo struct é chamado.

A criação de valores padrão ([valores padrão](structs.md#default-values)) de tipos de struct não dispara o construtor estático. (Um exemplo disso é o valor inicial dos elementos em uma matriz.)

## <a name="struct-examples"></a>Exemplos de struct

O exemplo a seguir mostra dois exemplos significativos de uso de tipos `struct` para criar tipos que podem ser usados de forma semelhante aos tipos predefinidos da linguagem, mas com semântica modificada.

### <a name="database-integer-type"></a>Tipo de inteiro do banco de dados

O struct `DBInt` abaixo implementa um tipo inteiro que pode representar o conjunto completo de valores do tipo `int`, além de um estado adicional que indica um valor desconhecido. Um tipo com essas características é comumente usado em bancos de dados.

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>Tipo booliano do banco de dados

O struct `DBBool` abaixo implementa um tipo lógico de três valores. Os valores possíveis desse tipo são `DBBool.True`, `DBBool.False` e `DBBool.Null`, em que o membro `Null` indica um valor desconhecido. Esses tipos lógicos de três valores são comumente usados em bancos de dados.

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
