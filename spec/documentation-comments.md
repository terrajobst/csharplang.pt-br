---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488911"
---
# <a name="documentation-comments"></a>Comentários de documentação

O c# fornece um mecanismo para os programadores a documentar seu código usando uma sintaxe de comentário especial que contém o texto XML. Em arquivos de código-fonte, ter uma determinada forma de comentários podem ser usados para direcionar uma ferramenta para produzir o XML desses comentários e os elementos de código do código-fonte, eles precedem. Comentários usando essa sintaxe são chamados ***comentários de documentação***. Eles devem preceder imediatamente um tipo definido pelo usuário (por exemplo, uma classe, delegado ou interface) ou um membro (por exemplo, um campo, evento, propriedade ou método). A ferramenta de geração de XML é chamada de ***gerador de documentação***. (Esse gerador poderia ser, mas não precisa ser, o compilador do c#.) A saída produzida pelo gerador de documentação é chamada de ***arquivo de documentação***. Um arquivo de documentação é usado como entrada para um ***Visualizador de documentação***; uma ferramenta desenvolvida para produzir algum tipo de exibição visual de informações de tipo e a documentação associada.

Essa especificação sugere um conjunto de marcas a ser usado em comentários de documentação, mas o uso dessas marcas não é necessário e outras marcas podem ser usadas se desejado, como tempo as regras de XML bem formado são seguidas.

## <a name="introduction"></a>Introdução

Ter uma forma especial de comentários podem ser usados para direcionar uma ferramenta para produzir o XML desses comentários e os elementos de código do código-fonte, eles precedem. Esses comentários são os comentários de linha única que começam com três barras (`///`), ou delimitado por comentários que começam com uma barra e duas estrelas (`/**`). Eles devem preceder imediatamente um tipo definido pelo usuário (por exemplo, uma classe, delegado ou interface) ou um membro (por exemplo, um campo, evento, propriedade ou método) que eles anotar. Seções de atributo ([especificação do atributo](attributes.md#attribute-specification)) são considerados parte das declarações, portanto, os comentários de documentação devem preceder os atributos aplicados a um tipo ou membro.

__Sintaxe:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

Em um *single_line_doc_comment*, se houver um *espaço em branco* seguinte de caractere a `///` caracteres em cada um do *single_line_doc_comment*s adjacentes ao atual *single_line_doc_comment*, em seguida, em que *espaço em branco* caractere não está incluído na saída XML.

Em um delimitados-comentário de documento, se o primeiro caractere não espaço em branco na segunda linha for um asterisco e o mesmo padrão de caracteres de espaço em branco opcional e um caractere de asterisco é repetido no início de cada linha de dentro a delimitados--comentário da documentação, em seguida, os caracteres do padrão repetido não estão incluídos na saída XML. O padrão pode incluir caracteres de espaço em branco, depois, bem como antes, o caractere de asterisco.

__Exemplo:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

O texto dentro de comentários de documentação deve estar bem formado de acordo com as regras do XML (https://www.w3.org/TR/REC-xml). Se o XML está mal formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que foi encontrado um erro.

Embora os desenvolvedores são livres para criar seu próprio conjunto de marcas, um conjunto recomendado é definido em [marcações recomendadas](documentation-comments.md#recommended-tags). Algumas das marcas recomendadas têm significado especial:

*  O `<param>` marca é usada para descrever parâmetros. Se tal uma marca é usada, o gerador de documentação deve verificar se o parâmetro especificado existe e que todos os parâmetros estão descritos nos comentários da documentação. Se essa verificação falhar, o gerador de documentação emite um aviso.
*  O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código. O gerador de documentação deve verificar se este elemento de código existe. Se a verificação falhar, o gerador de documentação emite um aviso. Ao procurar por um nome descritas em uma `cref` atributo, o gerador de documentação deve respeitar a visibilidade de namespace de acordo com a `using` instruções que aparecem no código-fonte. Para elementos de código que são genéricos, a sintaxe genérica normal (ou seja, "`List<T>`") não pode ser usado porque ele produz um XML inválido. As chaves podem ser usadas em vez de colchetes (ou seja, "`List{T}`"), ou a sintaxe de escape XML pode ser usada (ou seja, "`List&lt;T&gt;`").
*  O `<summary>` marca se destina a ser usado por um visualizador de documentação para exibir informações adicionais sobre um tipo ou membro.
*  O `<include>` tag inclui informações de um arquivo XML externo.

Observe atentamente que o arquivo de documentação não fornece informações completas sobre o tipo e membros (por exemplo, ele não contém nenhuma informação de tipo). Para obter essas informações sobre um tipo ou membro, o arquivo de documentação deve ser usado em conjunto com a reflexão no membro ou tipo real.

## <a name="recommended-tags"></a>Marcações recomendadas

O gerador de documentação deve aceitar e processar qualquer marca que seja válida de acordo com as regras do XML. As seguintes marcas fornecem funcionalidades comumente usadas na documentação do usuário. (É claro, outras marcas são possíveis.)


| __Tag__          | __Section__                                            | __Finalidade__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Definir o texto em uma fonte de código                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Defina uma ou mais linhas de saída de programa ou código de origem |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Indicar um exemplo                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifica um método pode lançar exceções           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Inclui o XML de um arquivo externo                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Criar uma lista ou tabela                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Permitir que a estrutura a ser adicionado ao texto                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Descrever um parâmetro para um método ou construtor       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Identificar que uma palavra é um nome de parâmetro               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | A acessibilidade de segurança de um membro do documento        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | Descrever informações adicionais sobre um tipo           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Descrever o valor de retorno de um método                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Especifique um link                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Gerar uma entrada Consulte também                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Descrever um tipo ou membro de um tipo                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Descrever uma propriedade                                    |
| `<typeparam>`    |                                                        | Descrever um parâmetro de tipo genérico                      |
| `<typeparamref>` |                                                        | Identificar que uma palavra é um nome de parâmetro de tipo          |

### `<c>`

Essa marca fornece um mecanismo para indicar que um fragmento de texto dentro uma descrição deve ser definido em uma fonte especial, como o usado para um bloco de código. Para linhas de código real, use `<code>` ([`<code>`](documentation-comments.md#code)).

__Sintaxe:__

```xml
<c>text</c>
```

__Exemplo:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

Essa marca é usada para definir uma ou mais linhas de saída de programa ou código de origem em alguma fonte especial. Para fragmentos de código pequeno em narrativa, use `<c>` ([`<c>`](documentation-comments.md#c)).

__Sintaxe:__

```xml
<code>source code or program output</code>
```

__Exemplo:__

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

Essa marca permite que o código de exemplo de um comentário, para especificar como um método ou outro membro da biblioteca pode ser usado. Normalmente, isso também envolveria uso da marca `<code>` ([`<code>`](documentation-comments.md#code)) também.

__Sintaxe:__

```xml
<example>description</example>
```

__Exemplo:__

Ver `<code>` ([`<code>`](documentation-comments.md#code)) para obter um exemplo.

### `<exception>`

Essa marca fornece uma maneira para documentar um método pode lançar exceções.

__Sintaxe:__

```xml
<exception cref="member">description</exception>
```

onde

* `member` é o nome de um membro. O gerador de documentação verifica se o membro fornecido existe e converte `member` para o nome de elemento canônico no arquivo de documentação.
* `description` é uma descrição das circunstâncias em que a exceção é lançada.

__Exemplo:__

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

Permite que essa marca, incluindo informações de um documento XML que é externo ao arquivo de código de origem. O arquivo externo deve ser um documento XML bem formado, e uma expressão XPath é aplicada a esse documento para especificar quais XML desse documento para incluir. O `<include>` marca, em seguida, é substituída pelo XML selecionado do documento externo.

__Sintaxe:__

```
<include file="filename" path="xpath" />
```

onde

* `filename` é o nome do arquivo de um arquivo XML externo. O nome do arquivo é interpretado em relação ao arquivo que contém a marca include.
* `xpath` é uma expressão XPath que seleciona alguns do XML no arquivo XML externo.

__Exemplo:__

Se o código-fonte contiver uma declaração como:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

e o arquivo externo "docs.xml" tinha o seguinte conteúdo:

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

em seguida, a documentação do mesma é a saída como se o código-fonte contido:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Essa marca é usada para criar uma lista ou tabela de itens. Ele pode conter um `<listheader>` bloco para definir a linha de cabeçalho de uma tabela ou uma definição de lista. (Durante a definição de uma tabela, apenas uma entrada para `term` no título precisa ser fornecido.)

Cada item na lista é especificado com um `<item>` bloco. Ao criar uma lista de definições, ambos `term` e `description` deve ser especificado. No entanto, para uma tabela, lista com marcadores ou lista numerada, apenas `description` precisa ser especificado.

__Sintaxe:__

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

onde

* `term` é o termo para definir, cuja definição está em `description`.
* `description` é um item em um marcador ou lista numerada ou a definição de um `term`.

__Exemplo:__

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

Essa marca é para uso dentro de outras marcas, como `<summary>` ([`<remark>`](documentation-comments.md#remark)) ou `<returns>` ([`<returns>`](documentation-comments.md#returns)) e permite que a estrutura a ser adicionado ao texto.

__Sintaxe:__

```xml
<para>content</para>
```

onde `content` é o texto do parágrafo.

__Exemplo:__

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

Essa marca é usada para descrever um parâmetro para um método, construtor ou indexador.

__Sintaxe:__

```xml
<param name="name">description</param>
```

onde

* `name` É o nome do parâmetro.
* `description` é uma descrição do parâmetro.

__Exemplo:__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

Essa marca é usada para indicar que uma palavra é um parâmetro. O arquivo de documentação pode ser processado para formatar esse parâmetro de alguma forma distinta.

__Sintaxe:__

```xml
<paramref name="name"/>
```

onde `name` é o nome do parâmetro.

__Exemplo:__

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

Essa marca permite que a acessibilidade de segurança de um membro a ser documentada.

__Sintaxe:__

```xml
<permission cref="member">description</permission>
```

onde

* `member` é o nome de um membro. O gerador de documentação verifica se o elemento de código fornecido existe e converte *membro* para o nome de elemento canônico no arquivo de documentação.
* `description` é uma descrição do acesso ao membro.

__Exemplo:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

Essa marca é usada para especificar informações adicionais sobre um tipo. (Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para descrever o próprio tipo e os membros de um tipo.)

__Sintaxe:__

```xml
<remark>description</remark>
```

onde `description` é o texto do comentário.

__Exemplo:__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

Essa marca é usada para descrever o valor de retorno de um método.

__Sintaxe:__

```xml
<returns>description</returns>
```

onde `description` é uma descrição do valor de retorno.

__Exemplo:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

Essa marca permite que um link para ser especificado dentro do texto. Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) para indicar o texto que deve aparecer em uma seção Consulte também.

__Sintaxe:__

```xml
<see cref="member"/>
```

onde `member` é o nome do membro. O gerador de documentação verifica se o elemento de código fornecido existe e se altera *membro* para o nome do elemento no arquivo de documentação gerada.

__Exemplo:__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

Essa marca permite que uma entrada a ser gerado para a seção Consulte também. Use `<see>` ([`<see>`](documentation-comments.md#see)) para especificar um link de dentro do texto.

__Sintaxe:__

```xml
<seealso cref="member"/>
```

onde `member` é o nome do membro. O gerador de documentação verifica se o elemento de código fornecido existe e se altera *membro* para o nome do elemento no arquivo de documentação gerada.

__Exemplo:__

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

Essa marca pode ser usada para descrever um tipo ou membro de um tipo. Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) para descrever o tipo em si.

__Sintaxe:__

```xml
<summary>description</summary>
```

onde `description` é um resumo do tipo ou membro.

__Exemplo:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Essa marca permite que uma propriedade a ser descrito.

__Sintaxe:__

```xml
<value>property description</value>
```

onde `property description` é uma descrição para a propriedade.

__Exemplo:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

Essa marca é usada para descrever um parâmetro de tipo genérico de classe, struct, interface, delegado ou método.

__Sintaxe:__

```xml
<typeparam name="name">description</typeparam>
```

em que `name` é o nome do parâmetro de tipo, e `description` é sua descrição.

__Exemplo:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Essa marca é usada para indicar que uma palavra é um parâmetro de tipo. O arquivo de documentação pode ser processado para formatar esse tipo de parâmetro de alguma forma distinta.

__Sintaxe:__

```xml
<typeparamref name="name"/>
```

onde `name` é o nome do parâmetro de tipo.

__Exemplo:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Processando o arquivo de documentação

O gerador de documentação gera uma cadeia de caracteres de ID para cada elemento no código-fonte que é marcado com um comentário de documentação. Essa cadeia de caracteres de identificação identifica exclusivamente um elemento de origem. Um visualizador de documentação pode usar uma cadeia de caracteres de ID para identificar o item de metadados/reflexão correspondente ao qual se aplica a documentação.

O arquivo de documentação não é uma representação hierárquica do código-fonte; em vez disso, é uma lista simples com uma cadeia de caracteres de ID gerada para cada elemento.

### <a name="id-string-format"></a>Formato de cadeia de caracteres de ID

O gerador de documentação segue as seguintes regras ao gerar as cadeias de caracteres de ID:

*  Nenhum espaço em branco é colocado na cadeia de caracteres.

*  A primeira parte da cadeia de caracteres identifica o tipo de membro documentado, por meio de um único caractere seguido por dois-pontos. Os seguintes tipos de membros são definidos:

   | __Caractere__ | __Descrição__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | evento                                                       |
   | F             | Campo                                                       |
   | M             | Método (incluindo construtores, destruidores e operadores) |
   | N             | Namespace                                                   |
   | P             | Propriedade (incluindo indexadores)                               |
   | T             | Tipo (como a classe delegate, enum, interface e struct) |
   | !             | Cadeia de caracteres de erro; o restante da cadeia de caracteres fornece informações sobre o erro. Por exemplo, o gerador de documentação gera informações de erro para links que não pode ser resolvido. |

*  A segunda parte da cadeia de caracteres é o nome totalmente qualificado do elemento, começando na raiz do namespace. O nome do elemento, seu tipo (s) delimitador e o namespace são separados por pontos. Se o nome do próprio item tiver pontos, elas serão substituídas por `#(U+0023)` caracteres. (Ele é considerado que o elemento não tem esse caractere em seu nome.)
*  Para métodos e propriedades com argumentos, da seguinte maneira lista de argumentos entre parênteses. Para aqueles sem argumentos, os parênteses serão omitidos. Os argumentos são separados por vírgulas. A codificação de cada argumento é o mesmo que uma assinatura da CLI, da seguinte maneira:
   *  Argumentos são representados por seu nome de documentação, que se baseia em seu nome totalmente qualificado, modificada da seguinte maneira:
      * Argumentos que representam tipos genéricos possuem um acrescentadas `` ` `` caractere (acento grave) seguido pelo número de parâmetros de tipo
      * Argumentos com o `out` ou `ref` modificador têm um `@` seu nome de tipo a seguir. Argumentos passados por valor ou por meio de `params` não ter nenhuma anotação especial.
      * Os argumentos que são matrizes são representados como `[lowerbound:size, ... , lowerbound:size]` em que o número de vírgulas é a classificação menos um, e os limites inferior e o tamanho de cada dimensão, se conhecidos, são representados no formato decimal. Se um limite inferior ou o tamanho não for especificado, ele é omitido. Se o limite inferior e o tamanho de uma determinada dimensão forem omitidos, o `:` será omitido também. Matrizes denteadas são representadas por um `[]` por nível.
      * Os argumentos que têm tipos de ponteiro que não seja nulo são representados usando um `*` seguindo o nome do tipo. Um ponteiro nulo é representado usando um nome de tipo de `System.Void`.
      * Argumentos que se referem a definidos nos tipos de parâmetros de tipo genérico são codificados usando o `` ` `` caractere (acento grave) seguido pelo índice baseado em zero do parâmetro de tipo.
      * Argumentos que usam parâmetros de tipo genérico definidos em métodos usam um double-acento grave ``` `` ``` em vez do `` ` `` usado para tipos.
      * Argumentos que se referem a tipos genéricos construídos são codificados usando o tipo genérico, seguido por `{`, seguido por uma lista separada por vírgulas de argumentos de tipo, seguido por `}`.

### <a name="id-string-examples"></a>Exemplos de cadeia de caracteres de ID

Os exemplos seguintes mostram um fragmento de código c#, juntamente com a ID de cadeia de caracteres produzido a partir de cada elemento de origem pode ocupar um comentário de documentação:

*  Tipos são representados usando seu nome totalmente qualificado, aumentada com informações genéricas:

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  Campos são representados por seu nome totalmente qualificado:

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  Construtores.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  Destruidores.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  Métodos.

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  Propriedades e indexadores.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  eventos.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  Operadores unários.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   O conjunto completo de nomes de função de operador unário usado é o seguinte: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, e `op_False`.

*  Operadores binários.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   O conjunto completo de nomes de função de operador binário usado é o seguinte: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, e `op_GreaterThanOrEqual`.

*  Operadores de conversão têm à direita "`~`" seguido pelo tipo de retorno.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a>Um exemplo

### <a name="c-source-code"></a>Código-fonte c#

O exemplo a seguir mostra o código-fonte de um `Point` classe:

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a>XML resultante

Aqui está a saída produzida por um gerador de documentação quando é fornecido o código-fonte para a classe `Point`, conforme mostrado acima:

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
