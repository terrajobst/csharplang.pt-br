---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488820"
---
# <a name="attributes"></a>Atributos

Grande parte da linguagem c# permite que o programador especificar informações declarativas sobre as entidades definidas no programa. Por exemplo, a acessibilidade de um método em uma classe for especificada, decorando-o com o *method_modifier*s `public`, `protected`, `internal`, e `private`.

C# habilita os programadores a novos tipos de informações declarativas, chamado de estoque ***atributos***. Os programadores podem anexar atributos para várias entidades de programa e recuperar informações de atributo em um ambiente de tempo de execução. Por exemplo, uma estrutura pode definir um `HelpAttribute` atributo que pode ser colocado em determinados elementos do programa (como classes e métodos) para fornecer um mapeamento a partir desses elementos de programa para a documentação.

Os atributos são definidos por meio da declaração de classes de atributos ([classes de atributo](attributes.md#attribute-classes)), que pode ter parâmetros posicionais e nomeados ([Positional e parâmetros nomeados](attributes.md#positional-and-named-parameters)). Atributos anexados a entidades em um programa em C# usando as especificações do atributo ([especificação do atributo](attributes.md#attribute-specification)) e podem ser recuperados em tempo de execução como instâncias de atributo ([instâncias de atributo](attributes.md#attribute-instances)).

## <a name="attribute-classes"></a>Classes de atributo

Uma classe que deriva da classe abstrata `System.Attribute`, seja direta ou indiretamente, é um ***classe de atributo***. A declaração de uma classe de atributo define um novo tipo de ***atributo*** que pode ser colocado em uma declaração. Por convenção, as classes de atributos são nomeadas com um sufixo de `Attribute`. Usos de um atributo podem incluir ou omitir esse sufixo.

### <a name="attribute-usage"></a>Uso do atributo

O atributo `AttributeUsage` ([atributo AttributeUsage o](attributes.md#the-attributeusage-attribute)) é usado para descrever como uma classe de atributo pode ser usada.

`AttributeUsage` tem um parâmetro posicional ([Positional e parâmetros nomeados](attributes.md#positional-and-named-parameters)) que permite que uma classe de atributo especificar os tipos de declaração na qual ele pode ser usado. O exemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

define uma classe de atributo nomeada `SimpleAttribute` que pode ser colocado na *class_declaration*s e *interface_declaration*somente s. O exemplo

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

mostra vários usos do `Simple` atributo. Embora esse atributo é definido com o nome `SimpleAttribute`, quando esse atributo é usado, o `Attribute` sufixo pode ser omitido, resultando no nome curto `Simple`. Assim, o exemplo acima é semanticamente equivalente ao seguinte:

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` tem um parâmetro nomeado ([Positional e parâmetros nomeados](attributes.md#positional-and-named-parameters)) chamado `AllowMultiple`, que indica se o atributo pode ser especificado mais de uma vez para uma determinada entidade. Se `AllowMultiple` para um atributo de classe é true e, em seguida, classe de atributo é um ***classe de atributo multiuso***e pode ser especificado mais de uma vez em uma entidade. Se `AllowMultiple` para um atributo de classe é falsa ou não está especificado, essa classe de atributo é um ***uso classe de atributo único***e podem ser especificadas no máximo uma vez em uma entidade.

O exemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

define uma classe de atributo multiuso chamada `AuthorAttribute`. O exemplo

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

mostra uma declaração de classe com dois usos do `Author` atributo.

`AttributeUsage` tem outro parâmetro nomeado chamado `Inherited`, que indica se o atributo, quando especificado em uma classe base, também é herdado por classes que derivam dessa classe base. Se `Inherited` para um atributo de classe é true e, em seguida, esse atributo é herdado. Se `Inherited` para um atributo de classe é false, em seguida, esse atributo não é herdado. Se não for especificado, o valor padrão é verdadeiro.

Uma classe de atributo `X` não tem um `AttributeUsage` atributo anexado a ele, como em

```csharp
using System;

class X: Attribute {...}
```

é equivalente ao seguinte:

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>Parâmetros posicionais e nomeados

Classes de atributo podem ter ***parâmetros posicionais*** e ***parâmetros nomeados***. Cada construtor de instância pública para uma classe de atributo define uma sequência válida de parâmetros posicionais para essa classe de atributo. Cada campo de leitura / gravação público não estático e a propriedade para uma classe de atributo definem um parâmetro nomeado para a classe de atributo.

O exemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

define uma classe de atributo nomeada `HelpAttribute` que tem um parâmetro posicional, `url`e um parâmetro nomeado, `Topic`. Embora seja não estático e público, a propriedade `Url` não define um parâmetro nomeado, pois ele não é leitura / gravação.

Essa classe de atributo pode ser usado da seguinte maneira:

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>Tipos de parâmetro de atributo

Os tipos de parâmetros posicionais e nomeados para uma classe de atributo são limitados para o ***tipos de parâmetro de atributo***, que são:

*  Um dos seguintes tipos: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.
*  O tipo `object`.
*  O tipo `System.Type`.
*  Um tipo de enumeração fornecido tem acessibilidade pública e os tipos na qual ele está aninhado (se houver) também tem acessibilidade pública ([especificação do atributo](attributes.md#attribute-specification)).
*  Matrizes unidimensionais de tipos mencionados acima.
*  Um argumento de construtor ou um campo público que não tem um desses tipos, não pode ser usado como um parâmetro posicional ou nomeado em uma especificação de atributo.

## <a name="attribute-specification"></a>Especificação de atributo

***Especificação do atributo*** é o aplicativo de um atributo definido anteriormente para uma declaração. Um atributo é uma parte das informações declarativas adicionais que são especificadas para uma declaração. Atributos podem ser especificados no escopo global (para especificar atributos no assembly ou módulo que contém) e para *type_declaration*s ([as declarações de tipo](namespaces.md#type-declarations)), *class_member_declaration* s ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([membros de Interface](interfaces.md#interface-members)), *struct_member _declaration*s ([membros de Struct](structs.md#struct-members)), *enum_member_declaration*s ([membros Enum](enums.md#enum-members)), *accessor_declarations*  ([Acessadores](classes.md#accessors)), *event_accessor_declarations* ([eventos semelhantes a campo](classes.md#field-like-events)), e *formal_parameter_list*s ([parâmetros de método](classes.md#method-parameters)).

Os atributos são especificados em ***seções de atributo***. Uma seção de atributo consiste em um par de colchetes, que envolvem uma lista separada por vírgulas de atributos de um ou mais. A ordem em que atributos são especificados em uma lista desse tipo e a ordem em que seções anexadas à mesma entidade programa são organizadas, não é significativa. Por exemplo, as especificações do atributo `[A][B]`, `[B][A]`, `[A,B]`, e `[B,A]` são equivalentes.

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

Um atributo consiste em um *attribute_name* e uma lista opcional de argumentos posicionais e nomeados. Os argumentos posicionais (se houver) precedem os argumentos nomeados. Um argumento posicional consiste em uma *attribute_argument_expression*; um argumento nomeado consiste em um nome, seguido por um sinal de igual, seguido por um *attribute_argument_expression*, que, juntos , são restringidas pelas mesmas regras de atribuição simples. A ordem dos argumentos nomeados não é significativa.

O *attribute_name* identifica uma classe de atributo. Se o formulário da *attribute_name* é *type_name* e em seguida, esse nome deve se referir a uma classe de atributo. Caso contrário, ocorrerá um erro de tempo de compilação. O exemplo

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

resulta em um erro de tempo de compilação, porque ele tenta usar `Class1` como um atributo de classe quando `Class1` não é uma classe de atributo.

Determinados contextos permitem a especificação de um atributo em mais de um destino. Um programa pode especificar explicitamente o destino, incluindo uma *attribute_target_specifier*. Quando um atributo é colocado no nível global, uma *global_attribute_target_specifier* é necessária. Em todos os outros locais, é aplicada a uma opção razoável, mas um *attribute_target_specifier* pode ser usado para confirmar ou substituir o padrão em certos casos ambíguos (ou apenas afirmar o padrão em casos não ambígua). Portanto, normalmente, *attribute_target_specifier*s pode ser omitido, exceto no nível global. Os contextos potencialmente ambíguos são resolvidos da seguinte maneira:

*  Um atributo especificado no escopo global pode aplicar para o assembly de destino ou o módulo de destino. Nenhum padrão existe para este contexto, então um *attribute_target_specifier* sempre é necessário neste contexto. A presença do `assembly` *attribute_target_specifier* indica que o atributo se aplica ao destino assembly; a presença dos `module` *attribute_target_specifier* indica que o atributo se aplica ao módulo de destino.
*  Um atributo especificado em uma declaração delegate pode aplicar para o delegado que está sendo declarado ou seu valor de retorno. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao delegado. A presença do `type` *attribute_target_specifier* indica que o atributo se aplica ao delegado; a presença dos `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.
*  Um atributo especificado em uma declaração de método pode aplicar para o método que está sendo declarado ou seu valor de retorno. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método. A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `return` *attribute_target_specifier* indica Se o atributo se aplica ao valor de retorno.
*  Um atributo especificado em uma declaração do operador pode aplicar o operador que está sendo declarado ou seu valor de retorno. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao operador. A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao operador; a presença dos `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.
*  Um atributo especificado em uma declaração de evento que omite os acessadores de eventos pode aplicar ao evento que está sendo declarado, para o campo associado (se o evento não é abstrato) ou para adicionar associado e remover métodos. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao evento. A presença do `event` *attribute_target_specifier* indica que o atributo se aplica para o evento; a presença dos `field` *attribute_target_specifier* indica o atributo aplica-se ao campo; e a presença do `method` *attribute_target_specifier* indica que o atributo se aplica aos métodos.
*  Um atributo especificado em uma declaração do acessador get para uma declaração de propriedade ou indexador pode aplicar para o método associado ou seu valor de retorno. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método. A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `return` *attribute_target_specifier* indica Se o atributo se aplica ao valor de retorno.
*  Um atributo especificado em um acessador set de uma declaração de propriedade ou indexador pode aplicar para o método associado ou seu único parâmetro implícito. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método. A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `param` *attribute_target_specifier* indica o atributo aplica-se para o parâmetro; a presença do `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.
*  Um atributo especificado em uma declaração de acessador add ou remove, para uma declaração de evento pode aplicar para o método associado ou seu único parâmetro. Na ausência de um *attribute_target_specifier*, o atributo se aplica ao método. A presença do `method` *attribute_target_specifier* indica que o atributo se aplica ao método; a presença dos `param` *attribute_target_specifier* indica o atributo aplica-se para o parâmetro; a presença do `return` *attribute_target_specifier* indica que o atributo se aplica ao valor de retorno.

Em outros contextos, a inclusão de um *attribute_target_specifier* é permitido, mas desnecessários. Por exemplo, uma declaração de classe pode incluir ou omitir o especificador de `type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

É um erro especificar um inválido *attribute_target_specifier*. Por exemplo, o especificador de `param` não pode ser usado em uma declaração de classe:

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

Por convenção, as classes de atributos são nomeadas com um sufixo de `Attribute`. Uma *attribute_name* do formulário *type_name* pode incluir ou omitir esse sufixo. Se uma classe de atributo for encontrada com e sem esse sufixo, uma ambiguidade está presente, e um erro de tempo de compilação. Se o *attribute_name* está escrito, de modo que sua direita *identificador* é um identificador textual ([identificadores](lexical-structure.md#identifiers)), em seguida, apenas um atributo sem um sufixo é correspondido, permitindo que tal uma ambiguidade de ser resolvido. O exemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

mostra duas classes chamadas de atributo `X` e `XAttribute`. O atributo `[X]` é ambíguo, já que ela pode se referir a um `X` ou `XAttribute`. Usar um identificador textual permite que a intenção exata a ser especificado em tais casos raros. O atributo `[XAttribute]` não é ambígua (embora seria se havia uma classe de atributo nomeada `XAttributeAttribute`!). Se a declaração de classe `X` for removido, os dois atributos se referir à classe de atributo nomeada `XAttribute`, da seguinte maneira:

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

Ele é um erro de tempo de compilação para usar uma classe de atributo de uso único mais de uma vez na mesma entidade. O exemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

resulta em um erro de tempo de compilação, porque ele tenta usar `HelpString`, que é uma classe de atributo de uso único, mais de uma vez na declaração de `Class1`.

Uma expressão `E` é um *attribute_argument_expression* se todas as instruções a seguir forem verdadeiras:

*  O tipo de `E` é um tipo de parâmetro de atributo ([tipos de parâmetro de atributo](attributes.md#attribute-parameter-types)).
*  Em tempo de compilação, o valor de `E` pode ser resolvida para um dos seguintes:
   * Um valor constante.
   * Um objeto `System.Type`.
   * Uma matriz unidimensional do *attribute_argument_expression*s.

Por exemplo:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

Um *typeof_expression* ([o operador typeof](expressions.md#the-typeof-operator)) usada como uma expressão de argumento de atributo pode referenciar um tipo não genérico, um tipo construído fechado ou um tipo genérico não associado, mas ele não pode fazer referência a um Abra o tipo. Isso é para garantir que a expressão pode ser resolvida em tempo de compilação.

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>Instâncias de atributo

Uma ***instância de atributo*** é uma instância que representa um atributo em tempo de execução. Um atributo é definido com uma classe de atributo, argumentos posicionais e argumentos nomeado. Uma instância do atributo é uma instância da classe de atributo que é inicializado com os argumentos posicionais e nomeados.

Recuperação de uma instância do atributo envolve o processamento de tempo de compilação e tempo de execução, conforme descrito nas seções a seguir.

### <a name="compilation-of-an-attribute"></a>Compilação de um atributo

A compilação de um *atributo* com a classe de atributo `T`, *positional_argument_list* `P` e *named_argument_list* `N`, consiste as seguintes etapas:

*  Siga as etapas de processamento de tempo de compilação para compilar uma *object_creation_expression* do formulário `new T(P)`. Essas etapas resultarem em um erro de tempo de compilação, ou determinar um construtor de instância `C` em `T` que pode ser invocado em tempo de execução.
*  Se `C` não tem acessibilidade pública, em seguida, ocorre um erro de tempo de compilação.
*  Para cada *named_argument* `Arg` em `N`:
   * Deixe `Name` ser o *identificador* da *named_argument* `Arg`.
   * `Name` deve identificar um campo público de leitura / gravação não estático ou propriedade em `T`. Se `T` tem não há tal campo ou propriedade, em seguida, ocorre um erro de tempo de compilação.
*  Mantenha as seguintes informações para a instanciação de tempo de execução do atributo: a classe de atributo `T`, o construtor de instância `C` nos `T`, o *positional_argument_list* `P` e o *named_argument_list* `N`.

### <a name="run-time-retrieval-of-an-attribute-instance"></a>Recuperação de tempo de execução de uma instância do atributo

Compilação de um *atributo* resulta em uma classe de atributo `T`, um construtor de instância `C` na `T`, uma *positional_argument_list* `P`e um *named_argument_list* `N`. Dadas essas informações, uma instância de atributo pode ser recuperada em tempo de execução usando as seguintes etapas:

*  Siga as etapas de processamento de tempo de execução para a execução de um *object_creation_expression* do formulário `new T(P)`, usando o construtor de instância `C` conforme determinado no tempo de compilação. Essas etapas resultarem em uma exceção, ou produzir uma instância `O` de `T`.
*  Para cada *named_argument* `Arg` em `N`, na ordem:
   * Deixe `Name` ser o *identificador* da *named_argument* `Arg`. Se `Name` não identifica um campo de leitura / gravação público não estático ou propriedade em `O`, em seguida, uma exceção será lançada.
   * Deixe `Value` ser o resultado da avaliação de *attribute_argument_expression* de `Arg`.
   * Se `Name` identifica um campo na `O`, em seguida, defina esse campo como `Value`.
   * Caso contrário, `Name` identifica uma propriedade em `O`. Defina essa propriedade como `Value`.
   * O resultado será `O`, uma instância da classe de atributo `T` que foi inicializado com o *positional_argument_list* `P` e o *named_argument_list* `N`.

## <a name="reserved-attributes"></a>Atributos reservados

Um pequeno número de atributos afeta o idioma de alguma forma. Esses atributos incluem:

*  `System.AttributeUsageAttribute` ([Atributo AttributeUsage o](attributes.md#the-attributeusage-attribute)), que é usado para descrever as maneiras em que uma classe de atributo pode ser usada.
*  `System.Diagnostics.ConditionalAttribute` ([Condicional o atributo](attributes.md#the-conditional-attribute)), que é usado para definir métodos condicionais.
*  `System.ObsoleteAttribute` ([Atributo obsoleto o](attributes.md#the-obsolete-attribute)), que é usado para marcar um membro como obsoleto.
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` e `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([atributos de informações do chamador](attributes.md#caller-info-attributes)), que são usados para fornecer informações sobre o contexto de chamada aos parâmetros opcionais.

### <a name="the-attributeusage-attribute"></a>O atributo AttributeUsage

O atributo `AttributeUsage` é usado para descrever a maneira na qual a classe de atributo pode ser usada.

Uma classe decorada com o `AttributeUsage` atributo deve derivar de `System.Attribute`, direta ou indiretamente. Caso contrário, ocorrerá um erro de tempo de compilação.

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>O atributo Conditional

O atributo `Conditional` habilita a definição de ***métodos condicionais*** e ***classes de atributo conditional***.

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>Métodos condicionais

Um método decorado com o `Conditional` atributo é um método condicional. O `Conditional` atributo indica uma condição testando um símbolo de compilação condicional. Chamadas para um método condicional são incluídas ou omitidas, dependendo se esse símbolo é definido no ponto de chamada. Se o símbolo é definido, a chamada será incluída; Caso contrário, a chamada (inclusive a avaliação do receptor e parâmetros da chamada) é omitida.

Um método condicional está sujeito às seguintes restrições:

*  O método condicional deve ser um método em uma *class_declaration* ou *struct_declaration*. Um erro de tempo de compilação ocorrerá se o `Conditional` atributo é especificado em um método em uma declaração de interface.
*  O método condicional deve ter um tipo de retorno `void`.
*  O método condicional não deve ser marcado com o `override` modificador. Um método condicional pode ser marcado com o `virtual` modificador, no entanto. Substituições desse método são implicitamente condicionais e não devem ser marcadas explicitamente com um `Conditional` atributo.
*  O método condicional não deve ser uma implementação de um método de interface. Caso contrário, ocorrerá um erro de tempo de compilação.

Além disso, um erro de tempo de compilação ocorre se um método condicional é usado em uma *delegate_creation_expression*. O exemplo

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

declara `Class1.M` como um método condicional. `Class2`do `Test` método chama esse método. Desde o símbolo de compilação condicional `DEBUG` for definido, se `Class2.Test` é chamado, ele chamará `M`. Se o símbolo `DEBUG` não havia sido definido, em seguida, `Class2.Test` não chamaria `Class1.M`.

É importante observar que a inclusão ou exclusão de uma chamada para um método condicional é controlado pelos símbolos de compilação condicional no ponto de chamada. No exemplo

Arquivo `class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

Arquivo `class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

Arquivo `class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

as classes `Class2` e `Class3` cada conter chamadas para o método condicional `Class1.F`, que é condicional com base em ou não `DEBUG` está definido. Como esse símbolo é definido no contexto de `Class2` , mas não `Class3`, a chamada para `F` na `Class2` está incluído, enquanto a chamada para `F` no `Class3` é omitido.

O uso de métodos condicionais em uma cadeia de herança pode ser confuso. Chamadas feitas para um método condicional por meio `base`, no formato `base.M`, estão sujeitas às regras de chamada de método normal de condicional. No exemplo

Arquivo `class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

Arquivo `class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

Arquivo `class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` inclui uma chamada para o `M` definidos em sua classe base. Essa chamada é omitida porque o método base é condicional com base na presença de símbolo `DEBUG`, que é indefinido. Portanto, o método grava no console "`Class2.M executed`" apenas. Uso criterioso de *pp_declaration*s pode eliminar esses problemas.

#### <a name="conditional-attribute-classes"></a>Classes de atributo Conditional

Uma classe de atributo ([classes de atributo](attributes.md#attribute-classes)) decorado com um ou mais `Conditional` atributos é uma ***classe de atributo condicional***. Uma classe de atributo condicional, portanto, é associada com os símbolos de compilação condicional declarados em seu `Conditional` atributos. Neste exemplo:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

declara `TestAttribute` como uma classe de atributo conditional associada com os símbolos condicionais compilações `ALPHA` e `BETA`.

Especificações de atributos ([especificação do atributo](attributes.md#attribute-specification)) de um atributo conditional serão incluídas se um ou mais dos seus símbolos de compilação condicional associado é definido no ponto de especificação, caso contrário, o atributo especificação é omitida.

É importante observar que a inclusão ou exclusão de uma especificação de atributo de uma classe de atributo condicional é controlado pelos símbolos de compilação condicional no ponto a especificação. No exemplo

Arquivo `test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

Arquivo `class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

Arquivo `class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

as classes `Class1` e `Class2` são cada decorada com o atributo `Test`, que é condicional com base em ou não `DEBUG` é definido. Como esse símbolo é definido no contexto de `Class1` , mas não `Class2`, a especificação da `Test` atributo `Class1` está incluído, enquanto a especificação da `Test` o atributo em `Class2` for omitido.

### <a name="the-obsolete-attribute"></a>O atributo obsoleto

O atributo `Obsolete` é usado para marcar tipos e membros de tipos que não devem mais ser usados.

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

Se um programa usa um tipo ou membro é decorado com o `Obsolete` atributo, o compilador emite um aviso ou erro. Especificamente, o compilador emite um aviso se nenhum erro de parâmetro for fornecido, ou se o parâmetro de erro é fornecido e tem o valor `false`. O compilador emitirá um erro se o parâmetro de erro for especificado e tem o valor `true`.

No exemplo

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

a classe `A` decorada com o `Obsolete` atributo. Cada uso de `A` em `Main` resulta em um aviso de que inclui a mensagem especificada, "Esta classe é obsoleta; Use a classe B."

### <a name="caller-info-attributes"></a>Atributos de informações do chamador

Para fins, como registro em log e relatórios, às vezes é útil para um membro da função obter determinadas informações de tempo de compilação sobre o código de chamada. Os atributos de informações do chamador fornecem uma maneira para transmitir essas informações de forma transparente.

Quando um parâmetro opcional é anotado com um dos atributos de informações do chamador, omitir o argumento correspondente em uma chamada não necessariamente fazer com que o valor do parâmetro padrão a serem substituídos. Em vez disso, se as informações especificadas sobre o contexto de chamada estiverem disponíveis, essas informações serão passadas como o valor do argumento.

Por exemplo:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

Uma chamada para `Log()` sem argumentos imprimiria o linha número e caminho do arquivo da chamada, bem como o nome do membro no qual ocorreu a chamada.

Atributos de informações do chamador podem ocorrer em parâmetros opcionais em qualquer lugar, inclusive em declarações de delegado. No entanto, os atributos de informações do chamador específico têm restrições sobre os tipos dos parâmetros, que eles podem atributo, para que sempre haverá uma conversão implícita de um valor substituído para o tipo de parâmetro.

É um erro ter o mesmo atributo de informações do chamador em um parâmetro de tanto a definição e implementação de parte de uma declaração de método parcial. Somente atributos de informações do chamador na parte definição são aplicados, enquanto os atributos de informações do chamador ocorrendo apenas na parte de implementação são ignorados.

Informações do chamador não afetam a resolução de sobrecarga. Como os parâmetros opcionais são atribuídos ainda são omitidos do código-fonte do chamador, a resolução de sobrecarga ignora esses parâmetros da mesma forma que ela ignora outros parâmetros opcionais omitidos ([resolução de sobrecarga](expressions.md#overload-resolution)).

Informações do chamador são substituídas somente quando uma função é invocada explicitamente no código-fonte. Invocações implícitas, tais como chamadas de construtor implícito pai não tem um local de origem e não substituirá as informações do chamador. Além disso, as chamadas que são vinculadas dinamicamente não substituirá as informações do chamador. Quando uma parâmetro atribuído é omitido nesses casos, informações do chamador, o valor padrão especificado do parâmetro é usado em vez disso.

Uma exceção é expressões de consulta. Esses são considerados expansões sintáticas e se as chamadas elas se expandem para omitir parâmetros opcionais com atributos de informações do chamador, informações do chamador serão substituídas. O local usado é o local da cláusula de consulta que a chamada foi gerada a partir.

Se mais de um atributo de informações do chamador é especificado em um determinado parâmetro, elas são preferenciais na seguinte ordem: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.

#### <a name="the-callerlinenumber-attribute"></a>O atributo CallerLineNumber

O `System.Runtime.CompilerServices.CallerLineNumberAttribute` é permitido em parâmetros opcionais quando há uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) do valor de constante `int.MaxValue` para o tipo do parâmetro. Isso garante que qualquer número de linha de não negativo até esse valor pode ser passado sem erro.

Se uma invocação de função de um local no código-fonte omite um parâmetro opcional com a `CallerLineNumberAttribute`, em seguida, um literal numérico que representa o número da linha do local é usado como um argumento para a invocação em vez do valor de parâmetro padrão.

Se a invocação abrange várias linhas, a linha escolhida é dependente de implementação.

Observe que o número de linha pode ser afetado pelas `#line` diretivas ([diretivas de linha](lexical-structure.md#line-directives)).

#### <a name="the-callerfilepath-attribute"></a>O atributo CallerFilePath

O `System.Runtime.CompilerServices.CallerFilePathAttribute` é permitido em parâmetros opcionais quando há uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) de `string` para o tipo do parâmetro.

Se uma invocação de função de um local no código-fonte omite um parâmetro opcional com a `CallerFilePathAttribute`, em seguida, um literal de cadeia de caracteres que representa o caminho do arquivo do local é usado como um argumento para a invocação em vez do valor de parâmetro padrão.

O formato do caminho do arquivo é dependente da implementação.

Observe que o caminho do arquivo pode ser afetado pelas `#line` diretivas ([diretivas de linha](lexical-structure.md#line-directives)).

#### <a name="the-callermembername-attribute"></a>O atributo CallerMemberName

O `System.Runtime.CompilerServices.CallerMemberNameAttribute` é permitido em parâmetros opcionais quando há uma conversão implícita padrão ([padrão conversões implícitas](conversions.md#standard-implicit-conversions)) de `string` para o tipo do parâmetro.

Se uma invocação de função de um local dentro do corpo de um membro da função ou dentro de um atributo aplicado ao membro da função em si ou seu tipo de retorno, parâmetros ou parâmetros de tipo no código-fonte omite um parâmetro opcional com a `CallerMemberNameAttribute`, em seguida, um literal de cadeia de caracteres que representa o nome desse membro é usado como um argumento para a invocação em vez do valor de parâmetro padrão.

Para invocações que ocorrem dentro de métodos genéricos, apenas o nome do método em si é usado, sem a lista de parâmetros de tipo.

Para invocações que ocorrem dentro de implementações de membros de interface explícita, apenas o nome do método em si é usado, sem a qualificação de interface anterior.

Para invocações que ocorrem dentro de acessadores de propriedade ou evento, o nome do membro usado é de propriedade ou evento em si.

Para invocações que ocorrem dentro de acessadores de indexador, o nome do membro usado é que o fornecido por uma `IndexerNameAttribute` ([atributo IndexerName The](attributes.md#the-indexername-attribute)) sobre o membro de indexador, se presente, ou o nome padrão `Item` caso contrário.

Para invocações que ocorrem dentro de declarações de construtores de instância, construtores estáticos, destruidores e operadores, o membro nome usado é dependente da implementação.

## <a name="attributes-for-interoperation"></a>Atributos para interoperação

Observação: Esta seção é aplicável somente para a implementação do Microsoft .NET do C#.

### <a name="interoperation-with-com-and-win32-components"></a>Interoperação com componentes COM e Win32

O tempo de execução do .NET fornece um grande número de atributos que permitem que programas em c# interoperar com componentes escritos usando COM e DLLs do Win32. Por exemplo, o `DllImport` atributo pode ser usado em um `static extern` método para indicar que a implementação do método deve ser encontrado em uma DLL Win32. Esses atributos são encontrados no `System.Runtime.InteropServices` namespace e a documentação detalhada para esses atributos for encontrado na documentação de tempo de execução do .NET.

### <a name="interoperation-with-other-net-languages"></a>Interoperação com outras linguagens .NET

#### <a name="the-indexername-attribute"></a>O atributo IndexerName

Indexadores são implementados no .NET usando as propriedades indexadas e tem um nome nos metadados do .NET. Se nenhum `IndexerName` atributo estiver presente para um indexador e, em seguida, o nome `Item` é usado por padrão. O `IndexerName` atributo permite que um desenvolvedor substituir esse padrão e especifique um nome diferente.

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
