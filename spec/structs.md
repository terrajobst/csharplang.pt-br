---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229477"
---
# <a name="structs"></a>Structs

Structs são semelhantes às classes que representam estruturas de dados que podem conter membros de dados e os membros da função. No entanto, diferentemente das classes, structs são tipos de valor e não precisam de alocação de heap. Uma variável de um tipo de struct diretamente contém os dados do struct, enquanto que uma variável de um tipo de classe contém uma referência aos dados, o último conhecido como um objeto.

Os structs são particularmente úteis para estruturas de dados pequenas que têm semântica de valor. Números complexos, pontos em um sistema de coordenadas ou pares chave-valor em um dicionário são exemplos de structs. Chave para essas estruturas de dados é que eles têm alguns membros de dados que eles não exigem o uso de herança ou identidade referencial, e que podem ser convenientemente implementados usando a semântica de valor em que a atribuição copia o valor em vez de referência.

Conforme descrito em [tipos simples](types.md#simple-types), os tipos simples fornecidos por c#, como `int`, `double`, e `bool`, são, na verdade, todos os tipos de struct. Como esses tipos predefinidos são structs, também é possível usar a sobrecarga de operador para implementar os novos tipos de "primitivos" na linguagem c# e estruturas. Dois exemplos de tais tipos são fornecidos no final deste capítulo ([exemplos de Struct](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Declarações de struct

Um *struct_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara um novo struct:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

Um *struct_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *struct_modifier*s ([modificadores de Struct](structs.md#struct-modifiers)), seguido por um recurso opcional `partial` modificador, seguido da palavra-chave `struct` e uma *identificador* que nomeia o struct, seguido por um opcional *type_parameter_list* especificação ([parâmetros de tipo](classes.md#type-parameters)), seguido por um recurso opcional *struct_interfaces* especificação ([Modificador parcial](structs.md#partial-modifier))), seguido por um recurso opcional *type_parameter_constraints_clause*especificação s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), seguido de um *struct_body* ([corpo de Struct](structs.md#struct-body)), opcionalmente seguido por um ponto e vírgula.

### <a name="struct-modifiers"></a>Modificadores de struct

Um *struct_declaration* opcionalmente pode incluir uma sequência de modificadores de struct:

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

Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de struct.

Os modificadores de uma declaração de struct têm o mesmo significado que aquelas de uma declaração de classe ([declarações de classe](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Modificador parcial

O `partial` modificador indica que isso *struct_declaration* é uma declaração de tipo parcial. Várias declarações de estrutura parcial com o mesmo nome dentro de uma declaração de namespace ou tipo delimitador se combinam para formar uma declaração de struct, seguindo as regras especificadas na [tipos parciais](classes.md#partial-types).

### <a name="struct-interfaces"></a>Interfaces de struct

Uma declaração de struct pode incluir um *struct_interfaces* especificação, nesse caso o struct é considerado implementar diretamente os tipos de interface fornecido.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementações de interface são discutidas mais detalhadamente em [implementações de Interface](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Corpo de struct

O *struct_body* de um struct define os membros de struct.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Membros de struct

Os membros de um struct consistem em membros introduzidos por seus *struct_member_declaration*s e os membros herdados do tipo `System.ValueType`.

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

Exceto pelas diferenças observadas na [diferenças de classe e struct](structs.md#class-and-struct-differences), as descrições dos membros de classe fornecidos na [membros de classe](classes.md#class-members) por meio do [iteradores](classes.md#iterators) se aplicam a struct membros também.

## <a name="class-and-struct-differences"></a>Diferenças de classe e struct

Structs são diferentes das classes de várias maneiras importantes:

*  Structs são tipos de valor ([semântica de valor](structs.md#value-semantics)).
*  Todos os tipos de structs herdam implicitamente da classe `System.ValueType` ([herança](structs.md#inheritance)).
*  Atribuição para uma variável de um tipo de struct cria uma cópia do valor que está sendo atribuído ([atribuição](structs.md#assignment)).
*  O valor padrão de um struct é o valor produzido pela configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null` ([valores padrão](structs.md#default-values)).
*  Operações de conversão boxing e unboxing são usadas para converter entre um tipo de struct e `object` ([conversões Boxing e unboxing](structs.md#boxing-and-unboxing)).
*  O significado dos `this` é diferente para structs ([esse acesso](expressions.md#this-access)).
*  Declarações de campo de instância para um struct não são permitidas para incluir os inicializadores de variável ([inicializadores de campo](structs.md#field-initializers)).
*  Um struct não é permitido para declarar um construtor de instância sem parâmetros ([construtores](structs.md#constructors)).
*  Um struct não é permitido declarar um destruidor ([destruidores](structs.md#destructors)).

### <a name="value-semantics"></a>Semântica de valor

Structs são tipos de valor ([tipos de valor](types.md#value-types)) e são considerados como tendo a semântica de valor. Classes, por outro lado, são tipos de referência ([tipos de referência](types.md#reference-types)) e são considerados como tendo a semântica de referência.

Uma variável de um tipo de struct diretamente contém os dados do struct, enquanto que uma variável de um tipo de classe contém uma referência aos dados, o último conhecido como um objeto. Quando um struct `B` contém um campo de instância do tipo `A` e `A` é um tipo de struct, ele é um erro de tempo de compilação para `A` depender `B` ou um tipo construído a partir `B`. Um struct `X` ***depende diretamente*** um struct `Y` se `X` contém um campo de instância do tipo `Y`. Dada esta definição, o conjunto completo de estruturas dos quais depende de um struct é o fechamento transitivo do ***depende diretamente*** relação.  Por exemplo
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
é um erro porque cada um dos tipos `A`, `B`, e `C` dependem uns aos outros.

Com classes, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível operações em uma variável afetem o objeto referenciado por outra variável. Com structs, as variáveis têm sua própria cópia dos dados (exceto no caso de `ref` e `out` variáveis de parâmetro), e não é possível que operações em um afetem o outro. Além disso, como structs não são tipos de referência, não é possível para valores de um tipo de struct para ser `null`.

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
gera o valor `10`. A atribuição de `a` à `b` cria uma cópia do valor, e `b` , portanto, não é afetado pela atribuição para `a.x`. Tinha `Point` em vez disso, foi declarado como uma classe, a saída seria `100` porque `a` e `b` fará referência ao mesmo objeto.

### <a name="inheritance"></a>Herança

Todos os tipos de structs herdam implicitamente da classe `System.ValueType`, que, por sua vez, herda da classe `object`. Uma declaração de struct pode especificar uma lista de interfaces implementadas, mas não é possível para uma declaração de struct especificar uma classe base.

Tipos de struct nunca são abstratos e sempre implicitamente são lacrados. O `abstract` e `sealed` modificadores, portanto, não são permitidos em uma declaração de struct.

Uma vez que não há suporte para herança para structs, a acessibilidade declarada de um membro de struct não pode ser `protected` ou `protected internal`.

Membros da função em um struct não podem ser `abstract` ou `virtual`e o `override` modificador é permitido somente para substituir métodos herdados de `System.ValueType`.

### <a name="assignment"></a>Atribuição

Atribuição para uma variável de um tipo de struct cria uma cópia do valor que está sendo atribuído. Isso difere da atribuição a uma variável de um tipo de classe, que copia a referência, mas não o objeto identificado por referência.

Semelhante a uma atribuição, quando um struct é passado como um valor de parâmetro ou retornado como o resultado de um membro da função, uma cópia do struct é criada. Um struct pode ser passado por referência a um membro da função usando um `ref` ou `out` parâmetro.

Quando uma propriedade ou indexador de um struct é o destino de uma atribuição, a expressão de instância associada com o acesso de propriedade ou indexador deve ser classificada como uma variável. Se a expressão de instância é classificada como um valor, ocorrerá um erro de tempo de compilação. Isso é descrito mais detalhadamente nas [atribuição simples](expressions.md#simple-assignment).

### <a name="default-values"></a>Valores padrão

Conforme descrito em [valores padrão](variables.md#default-values), vários tipos de variáveis são inicializados automaticamente com seus valores padrão quando eles são criados. Para variáveis de tipos de classe e outros tipos de referência, o valor padrão é `null`. No entanto, como structs são tipos de valor não podem ser `null`, o valor padrão de um struct é o valor produzido pela configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`.

Referindo-se para o `Point` struct declarado acima, o exemplo
```csharp
Point[] a = new Point[100];
```
inicializa cada `Point` na matriz para o valor produzido pela configuração de `x` e `y` campos como zero.

O valor padrão de um struct corresponde ao valor retornado pelo construtor padrão do struct ([1&gt;construtores padrão](types.md#default-constructors)). Diferentemente de uma classe, um struct não é permitido para declarar um construtor de instância sem parâmetros. Em vez disso, cada struct implicitamente tem um construtor de instância sem parâmetros que sempre retorna o valor resultante da configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`.

Estruturas devem ser projetadas para considerar o estado de inicialização padrão em um estado válido. No exemplo
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
o construtor de instância definidos pelo usuário protege contra valores nulos apenas onde ele é chamado explicitamente. Em casos em que um `KeyValuePair` variável está sujeito a inicialização de valor padrão, o `key` e `value` campos será nulos e o struct deve estar preparado para lidar com esse estado.

### <a name="boxing-and-unboxing"></a>Conversões boxing e unboxing

Um valor de um tipo de classe pode ser convertido no tipo `object` ou a um tipo de interface é implementado pela classe simplesmente tratando a referência como outro tipo em tempo de compilação. Da mesma forma, um valor do tipo `object` ou um valor de um tipo de interface pode ser convertido para um tipo de classe sem alterar a referência (mas obviamente uma verificação de tipo de tempo de execução é necessária neste caso).

Como structs não são tipos de referência, essas operações são implementadas diferente para tipos de struct. Quando um valor de um tipo de struct é convertido para o tipo `object` ou para um tipo de interface é implementado pelo struct, uma operação de conversão boxing ocorre. Da mesma forma, quando um valor do tipo `object` ou um valor de um tipo de interface é convertido para um tipo de struct, uma operação de conversão unboxing. A principal diferença das mesmas operações em tipos de classe é a conversão boxing e unboxing copia o valor do struct dentro ou fora da instância do box. Portanto, após uma operação de conversão unboxing ou conversão boxing, as alterações feitas ao struct não Demarcado não são refletidas na estrutura demarcada.

Quando um tipo de struct substitui um método virtual herdado de `System.Object` (como `Equals`, `GetHashCode`, ou `ToString`), a invocação do método virtual por meio de uma instância do tipo struct não faz com que a conversão boxing ocorra. Isso é verdadeiro mesmo quando o struct é usado como um parâmetro de tipo e a invocação ocorrer através de uma instância do tipo de parâmetro de tipo. Por exemplo:
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
```
1
2
3
```

Embora seja um estilo ruim para `ToString` para ter efeitos colaterais, o exemplo demonstra que nenhuma conversão boxing ocorreu para as três invocações de `x.ToString()`.

Da mesma forma, a conversão boxing nunca implicitamente ocorre ao acessar um membro em um parâmetro de tipo restrita. Por exemplo, suponha que uma interface `ICounter` contém um método `Increment` que pode ser usado para modificar um valor. Se `ICounter` é usado como uma restrição, a implementação do `Increment` método for chamado com uma referência à variável que `Increment` foi chamado em nunca uma cópia demarcada.

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

A primeira chamada para `Increment` modifica o valor na variável `x`. Isso não é equivalente a segunda chamada para `Increment`, que modifica o valor em uma cópia demarcada do `x`. Assim, a saída do programa é:
```
0
1
1
```

Para obter mais detalhes sobre conversões boxing e unboxing, consulte [conversões Boxing e unboxing](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Significado deste

Dentro de um construtor de instância ou um membro da função de instância de uma classe, `this` é classificado como um valor. Assim, enquanto `this` pode ser usado para fazer referência à instância para o qual o membro da função foi invocado, não é possível atribuir a `this` em um membro da função de uma classe.

Dentro de um construtor de instância de um struct `this` corresponde a um `out` parâmetro do tipo struct e dentro de um membro da função de instância de um struct `this` corresponde a um `ref` parâmetro do tipo struct. Em ambos os casos `this` é classificado como uma variável, e é possível modificar a estrutura inteira para o qual o membro da função foi invocado, atribuindo a `this` ou passando-o como um `ref` ou `out` parâmetro.

### <a name="field-initializers"></a>Inicializadores de campo

Conforme descrito em [valores padrão](structs.md#default-values), o valor padrão de um struct consiste o valor resultante da configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo para `null`. Por esse motivo, um struct não permite que as declarações de campo de instância para incluir os inicializadores de variável. Essa restrição se aplica somente a campos de instância. Campos estáticos de um struct podem incluir inicializadores de variável.

O exemplo
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
é um erro porque as declarações de campo de instância incluem inicializadores de variável.

### <a name="constructors"></a>Construtores

Diferentemente de uma classe, um struct não é permitido para declarar um construtor de instância sem parâmetros. Em vez disso, cada struct implicitamente tem um construtor de instância sem parâmetros que sempre retorna o valor resultante da configuração de todos os campos de tipo de valor para seus valores padrão e todas as referências de campos de tipo como nulo ([padrão construtores](types.md#default-constructors)). Um struct pode declarar construtores de instância ter parâmetros. Por exemplo
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
ambos criam uma `Point` com `x` e `y` inicializados em zero.

Um construtor de instância struct não podem incluir um inicializador de construtor do formulário `base(...)`.

Se o construtor de instância struct não especificar um inicializador de construtor, o `this` variable corresponde a um `out` parâmetro do tipo struct e é semelhante a um `out` parâmetro, `this` deve ser definitivamente atribuído ( [Atribuição definitiva](variables.md#definite-assignment)) em todos os locais em que o construtor retorna. Se o construtor de instância struct Especifica um inicializador de construtor, o `this` variable corresponde a um `ref` parâmetro do tipo struct e é semelhante a um `ref` parâmetro, `this` é considerado atribuído definitivamente em entrada para o corpo do construtor. Considere a implementação do construtor de instância abaixo:
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

Nenhuma função de membro de instância (incluindo os acessadores set das propriedades `X` e `Y`) pode ser chamado até que todos os campos da estrutura que está sendo construído tiveram sido atribuídos definitivamente. A única exceção envolve propriedades implementadas automaticamente ([implementadas automaticamente propriedades](classes.md#automatically-implemented-properties)). As regras de atribuição definitiva ([expressões de atribuição simples](variables.md#simple-assignment-expressions)) isentar especificamente a atribuição a uma propriedade automática de um tipo de struct dentro de um construtor de instância desse tipo struct: essa atribuição é considerada um definitiva atribuição de campo de suporte ocultos da propriedade automática. Portanto, o seguinte é permitido:

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

Um struct não é permitido declarar um destruidor.

### <a name="static-constructors"></a>Construtores estáticos

Construtores estáticos para structs siga com muita frequência as mesmas regras de classes. A execução de um construtor estático para um tipo de struct é disparada pela primeira dos seguintes eventos ocorra dentro de um domínio de aplicativo:

*  Um membro estático do tipo struct é referenciado.
*  Um construtor explicitamente declarado do tipo struct é chamado.

A criação de valores padrão ([valores padrão](structs.md#default-values)) do struct tipos não dispara o construtor estático. (Um exemplo disso é o valor inicial de elementos em uma matriz.)

## <a name="struct-examples"></a>Exemplos de struct

A seguir mostra dois exemplos significativos do uso de `struct` tipos para criar tipos que podem ser usados da mesma forma para os tipos predefinidos da linguagem, mas com semântica modificada.

### <a name="database-integer-type"></a>Tipo de inteiro de banco de dados

O `DBInt` abaixo de struct implementa um tipo de inteiro que pode representar o conjunto completo de valores da `int` tipo, além de um estado adicional que indica um valor desconhecido. Um tipo com essas características é comumente usado em bancos de dados.

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

### <a name="database-boolean-type"></a>Tipo de banco de dados booliano

O `DBBool` struct abaixo implementa um tipo de lógico de três valores. Os possíveis valores desse tipo são `DBBool.True`, `DBBool.False`, e `DBBool.Null`, onde o `Null` membro indica um valor desconhecido. Esses tipos de lógicos de três valores são usados em bancos de dados.

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
