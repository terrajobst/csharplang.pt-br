---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488772"
---
# <a name="interfaces"></a>Interfaces

Uma interface define um contrato. Uma classe ou struct que implementa uma interface deve cumprir o contrato. Uma interface pode herdar de várias interfaces base e uma classe ou struct pode implementar várias interfaces.

Interfaces podem conter métodos, propriedades, eventos e indexadores. A própria interface não fornece implementações dos membros que ela define. A interface simplesmente especifica os membros que devem ser fornecidos por classes ou estruturas que implementam a interface.

## <a name="interface-declarations"></a>Declarações de interface

Uma *interface_declaration* é um *type_declaration* ([as declarações de tipo](namespaces.md#type-declarations)) que declara um novo tipo de interface.

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

Uma *interface_declaration* consiste em um conjunto opcional de *atributos* ([atributos](attributes.md)), seguido por um conjunto opcional de *interface_modifier*s ([Interface modificadores](interfaces.md#interface-modifiers)), seguido por um recurso opcional `partial` modificador, seguido da palavra-chave `interface` e uma *identificador* que nomeia a interface, seguido por um recurso opcional *variant_type_parameter_list* especificação ([listas de parâmetros de tipo de variante](interfaces.md#variant-type-parameter-lists)), seguido por um recurso opcional *interface_base* especificação ([interfaces Base](interfaces.md#base-interfaces)), seguido por um recurso opcional *type_parameter_constraints_clause*especificação s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) , seguido por um *interface_body* ([Interface corpo](interfaces.md#interface-body)), opcionalmente seguido por um ponto e vírgula.

### <a name="interface-modifiers"></a>Modificadores de interface

Uma *interface_declaration* opcionalmente pode incluir uma sequência de modificadores de interface:

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

Ele é um erro de tempo de compilação para o mesmo modificador aparecer várias vezes em uma declaração de interface.

O `new` modificador é permitido somente em interfaces definidas dentro de uma classe. Especifica que a interface oculta um membro herdado com o mesmo nome, conforme descrito em [o novo modificador](classes.md#the-new-modifier).

O `public`, `protected`, `internal`, e `private` modificadores controlam a acessibilidade da interface. Dependendo do contexto no qual ocorre a declaração de interface, apenas alguns desses modificadores podem ser permitida ([declarado acessibilidade](basic-concepts.md#declared-accessibility)).

### <a name="partial-modifier"></a>Modificador parcial

O `partial` modificador indica que isso *interface_declaration* é uma declaração de tipo parcial. Combinam várias declarações de interface parcial com o mesmo nome dentro de uma declaração de namespace ou tipo delimitador à declaração de uma interface de formulário, seguindo as regras especificadas na [tipos parciais](classes.md#partial-types).

### <a name="variant-type-parameter-lists"></a>Listas de parâmetros de tipo de variante

Listas de parâmetros de tipo de variante podem ocorrer somente em tipos de interface e delegados. A diferença da comum *type_parameter_list*s é opcional *variance_annotation* em cada parâmetro de tipo.

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

Se for a anotação de variância `out`, o parâmetro de tipo será considerado ***covariante***. Se for a anotação de variância `in`, o parâmetro de tipo será considerado ***contravariante***. Se não houver nenhuma anotação de variância, o parâmetro de tipo deve ser ***invariável***.

No exemplo
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` é covariante, `Y` é contravariante e `Z` é invariável.

#### <a name="variance-safety"></a>Segurança de variação

A ocorrência de anotações de variância na lista de parâmetros de tipo de um tipo restringe os locais onde os tipos podem ocorrer dentro da declaração de tipo.

Um tipo `T` está ***saída unsafe*** se tiver mantido por um dos seguintes:

*  `T` é um parâmetro de tipo contravariantes
*  `T` é um tipo de matriz com um tipo de elemento não segura de saída
*  `T` é um tipo de interface ou delegado `S<A1,...,Ak>` construídos a partir de um tipo genérico `S<X1,...,Xk>` where para pelo menos um `Ai` mantém por um dos seguintes:
   * `Xi` é covariante ou invariável e `Ai` não é seguro de saída.
   * `Xi` é contravariante ou invariável e `Ai` segura para a entrada.
   
Um tipo `T` está ***entrada unsafe*** se tiver mantido por um dos seguintes:

*  `T` é um parâmetro de tipo covariantes
*  `T` é um tipo de matriz com um tipo de elemento de entrada-unsafe
*  `T` é um tipo de interface ou delegado `S<A1,...,Ak>` construídos a partir de um tipo genérico `S<X1,...,Xk>` where para pelo menos um `Ai` mantém por um dos seguintes:
   * `Xi` é covariante ou invariável e `Ai` não é seguro de entrada.
   * `Xi` é contravariante ou invariável e `Ai` não é seguro de saída.

Intuitivamente, um tipo de saída-unsafe é proibido em uma posição de saída e um tipo de entrada-unsafe for proibido em uma posição de entrada.

É um tipo ***saída-safe*** se não for inseguro saída, e ***safe entrada*** se não for inseguro de entrada.

#### <a name="variance-conversion"></a>Conversão de variância

A finalidade de anotações de variância é fornecer conversões mais branda (mas ainda fortemente tipado) em tipos de interface e delegados. Para isso terminar as definições de implícita ([conversões implícitas](conversions.md#implicit-conversions)) e as conversões explícitas ([conversões explícitas](conversions.md#explicit-conversions)) fazer usam a noção da convertibilidade de variância, que é definida da seguinte maneira:

Um tipo `T<A1,...,An>` é convertido por variância em um tipo `T<B1,...,Bn>` se `T` é uma interface ou um tipo de delegado declarado com os parâmetros de tipo de variante `T<X1,...,Xn>`e para cada parâmetro de tipo de variante `Xi` um dos seguintes contém:

*  `Xi` é covariante e existe uma conversão implícita de referência ou a identidade do `Ai` para `Bi`
*  `Xi` é contravariante e uma referência implícita ou existe conversão de identidade do `Bi` para `Ai`
*  `Xi` é invariável e uma identidade existe conversão do `Ai` para `Bi`

### <a name="base-interfaces"></a>Interfaces base

Uma interface pode herdar de zero ou mais tipos de interface, que são chamados de ***interfaces base explícitas*** da interface. Quando uma interface tem um ou mais interfaces base explícitas, em seguida, na declaração de interface, o identificador de interface é seguido por dois-pontos e vírgulas lista dos tipos de base de interface separada.

```antlr
interface_base
    : ':' interface_type_list
    ;
```

Para um tipo de interface construída, as interfaces base explícitas são formadas retirando as declarações explícitas de interface base na declaração de tipo genérico e substituir, para cada *type_parameter* na interface de base declaração, correspondente *type_argument* do tipo construído.

As interfaces base explícitas de uma interface devem ser pelo menos tão acessíveis quanto a própria interface ([restrições de acessibilidade](basic-concepts.md#accessibility-constraints)). Por exemplo, ele é um erro de tempo de compilação para especificar uma `private` ou `internal` interface na *interface_base* de um `public` interface.

Ele é um erro de tempo de compilação para uma interface herdada direta ou indiretamente de si mesma.

O ***interfaces base*** de uma interface são as interfaces base explícitas e suas interfaces base. Em outras palavras, o conjunto de interfaces base é o fechamento transitivo completo as interfaces base explícitas, suas interfaces base explícitas e assim por diante. Uma interface herda todos os membros de suas interfaces de base. No exemplo
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
as interfaces base da `IComboBox` estão `IControl`, `ITextBox`, e `IListBox`.

Em outras palavras, o `IComboBox` acima da interface herda membros `SetText` e `SetItems` , bem como `Paint`.

Cada interface base de uma interface deve ser segura de saída ([safety variação](interfaces.md#variance-safety)). Uma classe ou struct que implementa uma interface implicitamente também implementa todas as interfaces de base da interface.

### <a name="interface-body"></a>Corpo de interface

O *interface_body* de uma interface define os membros da interface.

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>Membros de interface

Os membros de uma interface são os membros herdados de interfaces base e os membros declarados pela interface em si.

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

Uma declaração de interface pode declarar zero ou mais membros. Os membros de uma interface devem ser métodos, propriedades, eventos ou indexadores. Uma interface não pode conter constantes, campos, operadores, construtores de instância, destruidores ou tipos, nem pode conter de uma interface membros estáticos de qualquer tipo.

Todos os membros de interface implicitamente têm acesso público. É um erro de tempo de compilação para declarações de membro de interface incluir qualquer modificador. Em particular, os membros de interfaces não podem ser declarados com os modificadores `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, ou `static`.

O exemplo
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
declara uma interface que contém um dos tipos possíveis de membros: Um método, uma propriedade, um evento e um indexador.

Uma *interface_declaration* cria um novo espaço de declaração ([declarações](basic-concepts.md#declarations)) e o *interface_member_declaration*s imediatamente contido pelo *interface_declaration* introduzir novos membros nesse espaço de declaração. As seguintes regras se aplicam a *interface_member_declaration*s:

*  O nome de um método deve ser diferente dos nomes de todas as propriedades e eventos declarados na mesma interface. Além disso, a assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)) de um método deve ser diferente das assinaturas de todos os outros métodos declarados na mesma interface, e dois métodos declarados na mesma interface podem não ter assinaturas que diferem somente por `ref` e `out`.
*  O nome de uma propriedade ou evento deve ser diferente dos nomes de todos os outros membros declarados na mesma interface.
*  A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma interface.

Os membros herdados de uma interface são especificamente não faz parte do espaço de declaração da interface. Portanto, uma interface pode declarar um membro com o mesmo nome ou assinatura como um membro herdado. Quando isso ocorrer, o membro de interface derivado deve ocultar o membro de interface base. Ocultar um membro herdado não é considerado um erro, mas ele faz com que o compilador emita um aviso. Para suprimir o aviso, a declaração do membro de interface derivado deve incluir um `new` modificador para indicar que o membro derivado destina-se para ocultar o membro base. Este tópico é discutido mais detalhadamente em [ocultando por meio da herança](basic-concepts.md#hiding-through-inheritance).

Se um `new` modificador é incluído em uma declaração que não oculta um membro herdado, um aviso será emitido para esse efeito. Esse aviso é suprimido, removendo o `new` modificador.

Observe que os membros na classe `object` não é, estritamente falando, os membros de qualquer interface ([membros da Interface](interfaces.md#interface-members)). No entanto, os membros na classe `object` estão disponíveis por meio de pesquisa de membro em qualquer tipo de interface ([pesquisa de membro](expressions.md#member-lookup)).

### <a name="interface-methods"></a>Métodos de interface

Métodos de interface são declarados usando *interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

O *atributos*, *return_type*, *identificador*, e *formal_parameter_list* de uma declaração de método de interface têm o mesmo que significa que as de uma declaração de método em uma classe ([métodos](classes.md#methods)). Uma declaração de método de interface não é permitida especificar um corpo de método e a declaração, portanto, sempre termine com um ponto e vírgula.

Cada tipo de parâmetro formal de um método de interface deve ser de entrada-safe ([safety variação](interfaces.md#variance-safety)), e o tipo de retorno deve ser `void` ou saída-safe. Além disso, cada restrição de tipo de classe, a restrição de tipo de interface e a restrição de parâmetro de tipo em qualquer parâmetro de tipo do método devem ser seguro de entrada.

Essas regras garantem que qualquer covariant ou contravariant uso da interface permanece fortemente tipado. Por exemplo,
```csharp
interface I<out T> { void M<U>() where U : T; }
```
é ilegal porque o uso de `T` como uma restrição de parâmetro de tipo em `U` não for seguro de entrada.

Não foram essa restrição em vigor seria possível de violar a segurança de tipos da seguinte maneira:
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
Isso é, na verdade, uma chamada para `C.M<E>`. Mas essa chamada requer que `E` derivam `D`, portanto, a segurança de tipo seja violada aqui.

### <a name="interface-properties"></a>Propriedades de interface

Propriedades de interface são declaradas usando *interface_property_declaration*s:

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

O *atributos*, *tipo*, e *identificador* de uma declaração de propriedade de interface têm o mesmo significado que aquelas de uma declaração de propriedade em uma classe ([ Propriedades](classes.md#properties)).

Os acessadores de uma declaração de propriedade de interface correspondem aos acessadores de uma declaração de propriedade de classe ([acessadores](classes.md#accessors)), exceto que o corpo do acessador deve sempre ser um ponto e vírgula. Portanto, os acessadores simplesmente indicam se a propriedade é leitura / gravação, somente leitura ou somente gravação.

O tipo de uma propriedade de interface deve ser segura de saída se houver um acessador get e deve ser a entrada segura se não houver um acessador set.

### <a name="interface-events"></a>Eventos de interface

Eventos de interface são declarados usando *interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

O *atributos*, *tipo*, e *identificador* de uma declaração de evento de interface têm o mesmo significado que aquelas de uma declaração de evento em uma classe ([eventos ](classes.md#events)).

O tipo de um evento de interface deve ser seguro de entrada.

### <a name="interface-indexers"></a>Indexadores de interface

Indexadores de interface são declarados usando *interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

O *atributos*, *tipo*, e *formal_parameter_list* de uma declaração de interface do indexador têm o mesmo significado que aquelas de uma declaração de indexador em uma classe ([ Indexadores](classes.md#indexers)).

Os acessadores de uma declaração de interface do indexador correspondem aos acessadores de uma declaração de indexador de classe ([indexadores](classes.md#indexers)), exceto que o corpo do acessador deve sempre ser um ponto e vírgula. Assim, os acessadores simplesmente indicam se o indexador é leitura / gravação, somente leitura ou somente gravação.

Todos os tipos de parâmetro formal de um indexador de interface devem ser de entrada-safe. Além disso, qualquer `out` ou `ref` tipos de parâmetro formal também devem ser segura de saída. Observe que mesmo `out` parâmetros devem ser de entrada-safe, devido a uma limitação da plataforma subjacente da execução.

O tipo de um indexador de interface deve ser segura de saída se houver um acessador get e deve ser a entrada segura se não houver um acessador set.

### <a name="interface-member-access"></a>Acesso de membro de interface

Os membros de interface são acessados por meio do acesso de membro ([acesso de membro](expressions.md#member-access)) e acesso do indexador ([acesso ao indexador](expressions.md#indexer-access)) expressões do formulário `I.M` e `I[A]`, onde `I` é um tipo de interface `M` é um método, propriedade ou evento desse tipo de interface, e `A` é uma lista de argumentos do indexador.

Para interfaces que são estritamente herança única (a cada interface na cadeia de herança tem exatamente zero ou uma interface base direta), os efeitos da pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)), invocação de método ([ As invocações de método](expressions.md#method-invocations)) e o acesso do indexador ([acesso do indexador](expressions.md#indexer-access)) as regras são exatamente as mesmas para classes e estruturas: Mais derivado ocultar membros menos derivados membros com o mesmo nome ou assinatura. No entanto, para as interfaces de herança múltipla, as ambiguidades podem ocorrer quando dois ou mais interfaces base não relacionadas declaram membros com o mesmo nome ou assinatura. Esta seção mostra vários exemplos de tais situações. Em todos os casos, as conversões explícitas podem ser usados para resolver as ambiguidades.

No exemplo
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
as duas primeiras instruções causam erros de tempo de compilação, porque a pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `Count` em `IListCounter` é ambíguo. Conforme ilustrado pelo exemplo, a ambiguidade é resolvida com a conversão `x` para o tipo de interface base adequada. Tal conversão ter sem custos de tempo de execução — eles simplesmente consistem em exibindo a instância como um tipo menos derivado no tempo de compilação.

No exemplo
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
a invocação `n.Add(1)` seleciona `IInteger.Add` aplicando as regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution). Da mesma forma, a invocação `n.Add(1.0)` seleciona `IDouble.Add`. Quando conversões explícitas são inseridas, há apenas um candidato método e, portanto, nenhuma ambiguidade.

No exemplo
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
o `IBase.F` membro é ocultado pelo `ILeft.F` membro. A invocação `d.F(1)` , portanto, seleciona `ILeft.F`, embora `IBase.F` parece não estar oculto no caminho de acesso que o orienta ao longo das `IRight`.

A regra intuitiva para ocultação de nas interfaces de herança múltipla é simplesmente isso: Se um membro é ocultado em qualquer caminho de acesso, ele ficará oculta em todos os caminhos de acesso. Porque o caminho de acesso de `IDerived` à `ILeft` para `IBase` oculta `IBase.F`, o membro é ocultado também no caminho de acesso de `IDerived` para `IRight` para `IBase`.

## <a name="fully-qualified-interface-member-names"></a>Nomes de membro de interface totalmente qualificado

Às vezes, um membro de interface é referenciado por seu ***nome totalmente qualificado***. O nome totalmente qualificado de um membro de interface consiste do nome da interface na qual o membro é declarado, seguido por um ponto, seguido do nome do membro. O nome totalmente qualificado de um membro faz referência a interface na qual o membro é declarado. Por exemplo, dadas as declarações
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
o nome totalmente qualificado da `Paint` está `IControl.Paint` e o nome totalmente qualificado do `SetText` é `ITextBox.SetText`.

No exemplo acima, não é possível se referir `Paint` como `ITextBox.Paint`.

Quando uma interface é parte de um namespace, o nome totalmente qualificado de um membro de interface inclui o nome do namespace. Por exemplo
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

Aqui, o nome totalmente qualificado do `Clone` método é `System.ICloneable.Clone`.

## <a name="interface-implementations"></a>Implementações de interfaces

Interfaces podem ser implementados por classes e estruturas. Para indicar que uma classe ou struct implementa diretamente uma interface, o identificador de interface está incluído na lista de classe base da classe ou struct. Por exemplo:
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

Uma classe ou struct que implementa diretamente uma interface diretamente também implementa todas as interfaces de base da interface implicitamente. Isso é verdadeiro mesmo se a classe ou struct explicitamente não lista todas as interfaces base na lista de classes base. Por exemplo:
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

Aqui, a classe `TextBox` implementa ambos `IControl` e `ITextBox`.

Quando uma classe `C` diretamente implementa uma interface, todas as classes derivadas C também implementa a interface implicitamente. As interfaces base especificadas em uma declaração de classe podem ser tipos de interface construída ([construído tipos](types.md#constructed-types)). Uma interface de base não pode ser um parâmetro de tipo por conta própria, embora isso pode envolver os parâmetros de tipo que estão no escopo. O código a seguir ilustra como uma classe pode implementar e estender tipos construídos:
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

As interfaces de base de uma declaração de classe genérica devem satisfazer a regra de exclusividade descrita em [exclusividade de interfaces implementadas](interfaces.md#uniqueness-of-implemented-interfaces).

### <a name="explicit-interface-member-implementations"></a>Implementações de membros de interface explícita

Para fins de implementação de interfaces, uma classe ou struct pode declarar ***implementações de membros de interface explícita***. Uma implementação de membro de interface explícita é uma declaração de método, propriedade, evento ou indexador que faz referência a um nome de membro de interface totalmente qualificado. Por exemplo
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

Aqui `IDictionary<int,T>.this` e `IDictionary<int,T>.Add` é implementações de membro de interface explícita.

Em alguns casos, o nome de um membro de interface não pode ser apropriado para a classe de implementação, nesse caso, o membro de interface pode ser implementado usando a implementação de membro de interface explícita. Uma classe que implementa uma abstração de arquivo, por exemplo, provavelmente seria implementar uma `Close` função de membro que tem o efeito de liberar o recurso de arquivo e implementar o `Dispose` método da `IDisposable` usando a interface explícita da interface implementação de membro:
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

Não é possível acessar uma implementação de membro de interface explícita por meio de seu nome totalmente qualificado em uma invocação de método, acesso de propriedade ou acesso do indexador. Uma implementação de membro de interface explícita só pode ser acessada por meio de uma instância da interface e, nesse caso é referenciada apenas por seu nome de membro.

É um erro de tempo de compilação para uma implementação de membro de interface explícita incluir os modificadores de acesso, e ele é um erro de tempo de compilação para incluir os modificadores `abstract`, `virtual`, `override`, ou `static`.

Implementações de membros de interface explícita têm características de acessibilidade diferente de outros membros. Como as implementações de membros de interface explícita nunca são acessíveis por meio de seus nomes totalmente qualificados em uma invocação de método ou um acesso de propriedade, eles são de certa forma privada. No entanto, desde que eles podem ser acessados por meio de uma instância da interface, eles estão em um sentido público também.

Implementações de membros de interface explícita têm duas finalidades principais:

*  Como as implementações de membros de interface explícita não são acessíveis por meio de instâncias de classe ou struct, eles permitem que as implementações de interface a ser excluído da interface pública de uma classe ou struct. Isso é particularmente útil quando uma classe ou struct implementa uma interface interna que não interessa para um consumidor dessa classe ou struct.
*  Implementações de membros de interface explícita permitem desambiguidade de membros de interface com a mesma assinatura. Sem as implementações de membros de interface explícita seria impossível para uma classe ou struct têm implementações diferentes de membros com a mesma assinatura de interface e tipo de retorno, como ela seria impossível para uma classe ou struct ter qualquer implementação em todos os membros de interface com a mesma assinatura, mas com diferentes tipos de retorno.

Para uma implementação de membro de interface explícita seja válida, a classe ou struct deve nomear uma interface em sua lista de classes base que contém um membro cujo nome totalmente qualificado, tipo e tipos de parâmetro corresponderem exatamente do membro de interface explícita implementação. Assim, na classe
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
a declaração de `IComparable.CompareTo` resulta em um erro de tempo de compilação porque `IComparable` não estiver na lista de classes base `Shape` e não é uma interface de base de `ICloneable`. Da mesma forma, nas declarações
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
a declaração de `ICloneable.Clone` na `Ellipse` resulta em um erro de tempo de compilação porque `ICloneable` não listados explicitamente na lista de classes base `Ellipse`.

O nome totalmente qualificado de um membro de interface deve fazer referência a interface em que o membro foi declarado. Dessa forma, nas declarações
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
a implementação de membro de interface explícita `Paint` deve ser escrito como `IControl.Paint`.

### <a name="uniqueness-of-implemented-interfaces"></a>Exclusividade de interfaces implementadas

As interfaces implementadas por uma declaração de tipo genérico devem permanecer exclusivas para todos os possíveis tipos construídos. Sem essa regra, seria impossível determinar o método correto para chamar para determinados tipos construídos. Por exemplo, suponha que uma declaração de classe genérica puderem ser escrita da seguinte maneira:
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

Foram isso permitido, seria impossível determinar qual código deve ser executado no caso a seguir:
```csharp
I<int> x = new X<int,int>();
x.F();
```

Para determinar se a lista de interface de uma declaração de tipo genérico é válida, as etapas a seguir são executadas:

*  Deixe `L` a lista de interfaces especificada diretamente em uma classe genérica, struct ou declaração de interface `C`.
*  Adicionar ao `L` quaisquer base interfaces das interfaces já está sendo `L`.
*  Remova todas as duplicatas de `L`.
*  Se qualquer possível construído criado a partir do tipo `C` seria, depois de argumentos de tipo são substituídos na `L`, fazer com que duas interfaces no `L` ser idênticos, em seguida, a declaração de `C` é inválido. Declarações de restrição não são consideradas ao determinar todos os possíveis tipos construídos.

Na declaração da classe `X` acima, a lista de interfaces `L` consiste `I<U>` e `I<V>`. A declaração é inválida porque qualquer tipo com construído `U` e `V` sendo do mesmo tipo faria com que essas duas interfaces são tipos idênticos.

É possível para interfaces especificadas em níveis diferentes de herança para unificar:
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

Esse código é válido, embora `Derived<U,V>` implementa ambos `I<U>` e `I<V>`. O código
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
invoca o método no `Derived`, já que `Derived<int,int>` efetivamente reimplementa `I<int>` ([Reimplementação da Interface](interfaces.md#interface-re-implementation)).

### <a name="implementation-of-generic-methods"></a>Implementação de métodos genéricos

Quando um método genérico implicitamente implementa um método de interface fornecidas para cada parâmetro de tipo de método deve ser equivalente em ambas as declarações (após qualquer tipo de interface parâmetros são substituídos com os argumentos de tipo apropriado), em que as restrições de método parâmetros de tipo são identificados por posições ordinais, da esquerda para a direita.

Quando um método genérico explicitamente implementa um método de interface, no entanto, não são permitidas restrições sobre o método de implementação. Em vez disso, as restrições são herdadas do método de interface

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

O método `C.F<T>` implicitamente implementa `I<object,C,string>.F<T>`. Nesse caso, `C.F<T>` não são necessários (nem permitido) para especificar a restrição `T:object` , pois `object` é uma restrição implícita em todos os parâmetros de tipo. O método `C.G<T>` implicitamente implementa `I<object,C,string>.G<T>` porque as restrições coincide com a interface, depois que os parâmetros de tipo de interface são substituídos com os argumentos de tipo correspondente. A restrição para o método `C.H<T>` é um erro porque tipos lacrados (`string` nesse caso) não podem ser usados como restrições. Omitindo a restrição também seria um erro, pois as restrições de implementações de método de interface implícita são necessárias para corresponder. Portanto, é impossível implementar implicitamente `I<object,C,string>.H<T>`. Esse método de interface só pode ser implementado usando uma implementação de membro de interface explícita:
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

Neste exemplo, a implementação de membro de interface explícita invoca um método público ter restrições estritamente mais fracas. Observe que a atribuição da `t` à `s` é válido desde `T` herda de uma restrição de `T:string`, mesmo que essa restrição não é expressada no código-fonte.

### <a name="interface-mapping"></a>Mapeamento de interface

Uma classe ou struct deve fornecer implementações de todos os membros das interfaces que são listados na lista de classe base da classe ou struct. O processo de localização de implementações de membros de interface em uma classe ou struct a implementação é conhecido como ***mapeamento de interface***.

Mapeamento de interface para uma classe ou struct `C` localiza uma implementação para cada membro de cada interface especificada na lista de classes base `C`. A implementação de um membro de interface específica `I.M`, onde `I` é a interface na qual o membro `M` é declarado, é determinada pelo exame de cada classe ou struct `S`, começando com `C` e Repetir para cada classe base sucessiva do `C`, até que uma correspondência for encontrada:

*  Se `S` contém uma declaração de uma implementação de membro de interface explícita corresponde `I` e `M`, em seguida, esse membro é a implementação de `I.M`.
*  Caso contrário, se `S` contém uma declaração de um membro público não estático que corresponde à `M`, em seguida, esse membro é a implementação de `I.M`. Se mais de um membro correspondências, não está especificado qual membro é a implementação do `I.M`. Essa situação pode ocorrer somente se `S` for um tipo construído em que os dois membros conforme declarado no tipo genérico têm assinaturas diferentes, mas os argumentos de tipo que seja idêntico de suas assinaturas.

Ocorrerá um erro de tempo de compilação se implementações não podem ser localizadas para todos os membros de todas as interfaces especificados na lista de classes base `C`. Observe que os membros de uma interface incluem aqueles membros que são herdados de interfaces base.

Para fins de mapeamento de interface, um membro da classe `A` corresponde a um membro de interface `B` quando:

*  `A` e `B` são métodos e o nome, tipo, e listas de parâmetros formais de `A` e `B` são idênticos.
*  `A` e `B` são propriedades, o nome e tipo de `A` e `B` são idênticas, e `A` tem o mesmos acessadores, conforme `B` (`A` é permitido ter acessadores adicionais se não for uma interface explícita implementação de membro).
*  `A` e `B` são os eventos e o nome e o tipo de `A` e `B` são idênticos.
*  `A` e `B` são os indexadores, o tipo e listas de parâmetros formais das `A` e `B` são idênticas, e `A` tem o mesmos acessadores, conforme `B` (`A` é permitido ter acessadores adicionais se não for um implementação de membro de interface explícita).

Implicações importantes do algoritmo de mapeamento de interface são:

*  Implementações de membros de interface explícita têm precedência sobre os outros membros na mesma classe ou struct ao determinar o membro de classe ou struct que implementa um membro de interface.
*  Membros não não públicas nem estáticos participarem no mapeamento de interface.

No exemplo
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
o `ICloneable.Clone` membro do `C` torna-se a implementação de `Clone` em `ICloneable` como as implementações de membros de interface explícita têm precedência sobre os outros membros.

Se uma classe ou struct implementa duas ou mais interfaces que contém um membro com o mesmo nome, tipo e tipos de parâmetro, é possível mapear cada um desses membros de interface para um único membro de classe ou struct. Por exemplo
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

Aqui, o `Paint` métodos de ambos `IControl` e `IForm` são mapeadas para o `Paint` método na `Page`. É claro também é possível ter implementações de membros de interface explícita separada para os dois métodos.

Se uma classe ou struct implementa uma interface que contém membros ocultos, alguns membros necessariamente precisa ser implementados por meio de implementações de membro de interface explícita. Por exemplo
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

Uma implementação dessa interface exige pelo menos uma implementação de membro de interface explícita e levaria a uma das seguintes formas
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

Quando uma classe implementa várias interfaces que têm a mesma interface base, pode haver apenas uma implementação da interface base. No exemplo
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
não é possível ter implementações separadas para o `IControl` nomeado na lista de classes base, o `IControl` herdadas por `ITextBox`e o `IControl` herdada por `IListBox`. Na verdade, não há nenhuma noção de uma identidade separada para essas interfaces. Em vez disso, as implementações de `ITextBox` e `IListBox` compartilham a mesma implementação de `IControl`, e `ComboBox` simplesmente é considerado para implementar as interfaces de três `IControl`, `ITextBox`, e `IListBox`.

Os membros de uma classe base participarem no mapeamento de interface. No exemplo
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
o método `F` na `Class1` é usado em `Class2`da implementação de `Interface1`.

### <a name="interface-implementation-inheritance"></a>Herança de implementação de interface

Uma classe herda todas as implementações de interface fornecidas por suas classes base.

Sem explicitamente ***reimplementação*** uma interface, uma classe derivada não é possível de qualquer maneira alterar os mapeamentos de interface que herda de suas classes base. Por exemplo, nas declarações
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
o `Paint` método na `TextBox` oculta a `Paint` método na `Control`, mas não altera o mapeamento de `Control.Paint` até `IControl.Paint`e chamadas para `Paint` por meio da classe serão instâncias e instâncias de interface ter os seguintes efeitos
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

No entanto, quando um método de interface é mapeado em um método virtual em uma classe, é possível que as classes derivadas substituir o método virtual e alterar a implementação da interface. Por exemplo, reescrever as declarações acima para
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
os seguintes efeitos agora serão observados
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

Uma vez que as implementações de membros de interface explícita não podem ser declaradas como virtuais, não é possível substituir uma implementação de membro de interface explícita. No entanto, é perfeitamente válido para uma implementação de membro de interface explícita chamar outro método, e que outro método pode ser declarado virtual para permitir que classes para substituí-lo derivadas. Por exemplo
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

Aqui, as classes derivadas `Control` possível especializar a implementação de `IControl.Paint` , substituindo o `PaintControl` método.

### <a name="interface-re-implementation"></a>Reimplementação da interface

Uma classe que herda de uma implementação de interface tem permissão para ***reimplementar*** a interface incluindo-o na lista de classe base.

Uma nova implementação de uma interface segue exatamente as mesma interface regras de mapeamento como uma implementação inicial de uma interface. Portanto, o mapeamento de interface herdada não tem efeito depende do mapeamento de interface estabelecida para a reimplementação da interface. Por exemplo, nas declarações
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
o fato de que `Control` mapeia `IControl.Paint` até `Control.IControl.Paint` não afeta a reimplementação na `MyControl`, que mapeia `IControl.Paint` até `MyControl.Paint`.

Herdado de declarações de membro público e o membro de interface explícita herdada declarações participarem no processo de mapeamento de interface de interfaces implementadas novamente. Por exemplo
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

Aqui, a implementação de `IMethods` na `Derived` mapeia os métodos de interface em `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, e `Base.I`.

Quando uma classe implementa uma interface, ele implicitamente também implementa todas as interfaces de base dessa interface. Da mesma forma, uma nova implementação de uma interface também é implicitamente uma Reimplementação de todas as interfaces de base da interface. Por exemplo
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

Aqui, a nova implementação da `IDerived` também reimplementa `IBase`, o mapeamento `IBase.F` em `D.F`.

### <a name="abstract-classes-and-interfaces"></a>Interfaces e classes abstratas

Como uma classe não abstrata, uma classe abstrata deve fornecer implementações de todos os membros das interfaces que são listados na lista de classe base da classe. No entanto, uma classe abstrata é permitida para mapear os métodos de interface em métodos abstratos. Por exemplo
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

Aqui, a implementação de `IMethods` mapeia `F` e `G` em métodos abstratos, que deve ser substituído em classes não abstratas que derivam de `C`.

Observe que as implementações de membros de interface explícita não podem ser abstratas, mas as implementações de membros de interface explícita certamente têm permissão para chamar métodos abstratos. Por exemplo
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

Aqui, as classes não abstratas que derivam de `C` seria necessário para substituir `FF` e `GG`, fornecendo assim a implementação real da `IMethods`.
