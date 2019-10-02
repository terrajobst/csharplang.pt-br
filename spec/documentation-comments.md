---
ms.openlocfilehash: 2026fc1bf9d3576b967cbc2e9a670aa44b7eab3a
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704019"
---
# <a name="documentation-comments"></a>Comentários de documentação

C#fornece um mecanismo para os programadores documentarem seu código usando uma sintaxe de comentário especial que contém texto XML. Em arquivos de código-fonte, os comentários com um determinado formulário podem ser usados para direcionar uma ferramenta para produzir XML a partir desses comentários e dos elementos de código-fonte, que eles precedem. Os comentários que usam essa sintaxe são chamados de ***comentários de documentação***. Eles devem preceder imediatamente um tipo definido pelo usuário (como uma classe, um delegado ou uma interface) ou um membro (como um campo, evento, propriedade ou método). A ferramenta de geração de XML é chamada de ***gerador de documentação***. (Esse gerador pode ser, mas não precisa ser, o C# próprio compilador.) A saída produzida pelo gerador de documentação é chamada de ***arquivo de documentação***. Um arquivo de documentação é usado como entrada para um ***Visualizador de documentação***; uma ferramenta destinada a produzir algum tipo de exibição visual de informações de tipo e sua documentação associada.

Essa especificação sugere um conjunto de marcas a serem usadas em comentários de documentação, mas o uso dessas marcas não é necessário e outras marcas podem ser usadas, se desejado, desde que as regras de XML bem formadas sejam seguidas.

## <a name="introduction"></a>Introdução

Os comentários com um formulário especial podem ser usados para direcionar uma ferramenta para produzir XML a partir desses comentários e dos elementos de código-fonte, que eles precedem. Esses comentários são comentários de linha única que começam com três barras (`///`) ou comentários delimitados que começam com uma barra e duas estrelas (`/**`). Eles devem preceder imediatamente um tipo definido pelo usuário (como uma classe, um delegado ou uma interface) ou um membro (como um campo, evento, propriedade ou método) que eles anotam. As seções de atributo ([especificação de atributo](attributes.md#attribute-specification)) são consideradas parte das declarações; portanto, os comentários de documentação devem preceder os atributos aplicados a um tipo ou membro.

__Sintaxe__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

Em um *single_line_doc_comment*, se houver um caractere de *espaço em branco* após o `///` caracteres em cada um dos *single_line_doc_comments*adjacentes ao *single_line_doc_comment*atual, esse espaço em *branco* o caractere não está incluído na saída XML.

Em um delimitado-doc-Comment, se o primeiro caractere que não seja espaço em branco na segunda linha for um asterisco e o mesmo padrão de caracteres de espaço em branco opcionais e um caractere de asterisco for repetido no início de cada linha dentro do comentário delimitado-doc-Comment, em seguida, os caracteres do padrão repetido não são incluídos na saída XML. O padrão pode incluir caracteres de espaço em branco após, bem como antes, o caractere de asterisco.

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

O texto nos comentários da documentação deve estar bem formado de acordo com as regras de https://www.w3.org/TR/REC-xml) XML (. Se o XML estiver mal formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que um erro foi encontrado.

Embora os desenvolvedores estejam livres para criar seu próprio conjunto de marcas, um conjunto recomendado é definido em [marcas recomendadas](documentation-comments.md#recommended-tags). Algumas das marcas recomendadas têm significado especial:

*  A `<param>` marca é usada para descrever os parâmetros. Se tal marca for usada, o gerador de documentação deverá verificar se o parâmetro especificado existe e se todos os parâmetros estão descritos nos comentários da documentação. Se essa verificação falhar, o gerador de documentação emitirá um aviso.
*  O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código. O gerador de documentação deve verificar se esse elemento de código existe. Se a verificação falhar, o gerador de documentação emitirá um aviso. Ao procurar um nome descrito em um `cref` atributo, o gerador de documentação deve respeitar a visibilidade do namespace de acordo com as instruções que `using` aparecem no código-fonte. Para elementos de código genéricos, a sintaxe genérica normal (ou seja, "`List<T>`") não pode ser usada porque produz XML inválido. Chaves podem ser usadas em vez de colchetes (ou seja, "`List{T}`") ou a sintaxe de escape XML pode ser usada (ou seja, "`List&lt;T&gt;`").
*  A `<summary>` marca destina-se a ser usada por um visualizador de documentação para exibir informações adicionais sobre um tipo ou membro.
*  A `<include>` marca inclui informações de um arquivo XML externo.

Observe atentamente que o arquivo de documentação não fornece informações completas sobre o tipo e os membros (por exemplo, ele não contém nenhuma informação de tipo). Para obter informações sobre um tipo ou membro, o arquivo de documentação deve ser usado em conjunto com reflexão no tipo ou membro real.

## <a name="recommended-tags"></a>Marcas recomendadas

O gerador de documentação deve aceitar e processar qualquer marca que seja válida de acordo com as regras de XML. As marcas a seguir fornecem funcionalidade comumente usada na documentação do usuário. (É claro que outras marcas são possíveis.)


| __Tags__          | __Section__                                            | __Finalidade__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Definir texto em uma fonte do tipo código                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Definir uma ou mais linhas do código-fonte ou saída do programa |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Indicar um exemplo                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifica as exceções que um método pode gerar           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Inclui XML de um arquivo externo                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Criar uma lista ou tabela                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Permitir que a estrutura seja adicionada ao texto                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Descrever um parâmetro para um método ou Construtor       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Identificar que uma palavra é um nome de parâmetro               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Documentar a acessibilidade de segurança de um membro        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | Descrever informações adicionais sobre um tipo           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Descrever o valor de retorno de um método                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Especificar um link                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Gerar uma entrada Consulte também                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Descrever um tipo ou um membro de um tipo                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Descrever uma propriedade                                    |
| `<typeparam>`    |                                                        | Descrever um parâmetro de tipo genérico                      |
| `<typeparamref>` |                                                        | Identificar que uma palavra é um nome de parâmetro de tipo          |

### `<c>`

Essa marca fornece um mecanismo para indicar que um fragmento de texto dentro de uma descrição deve ser definido em uma fonte especial, como a usada para um bloco de código. Para linhas de código real, use `<code>` ([`<code>`](documentation-comments.md#code)).

__Sintaxe__

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

Essa marca é usada para definir uma ou mais linhas de código-fonte ou saída do programa em alguma fonte especial. Para fragmentos de código pequenos em narrativas `<c>` ,[`<c>`](documentation-comments.md#c)use ().

__Sintaxe__

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

Essa marca permite código de exemplo dentro de um comentário, para especificar como um método ou outro membro da biblioteca pode ser usado. Normalmente, isso também envolveria o uso da marca `<code>` ([`<code>`](documentation-comments.md#code)).

__Sintaxe__

```xml
<example>description</example>
```

__Exemplo:__

Consulte `<code>` [(`<code>`](documentation-comments.md#code)) para obter um exemplo.

### `<exception>`

Essa marca fornece uma maneira de documentar as exceções que um método pode gerar.

__Sintaxe__

```xml
<exception cref="member">description</exception>
```

onde

* `member`é o nome de um membro. O gerador de documentação verifica se o membro fornecido existe e `member` se traduz no nome do elemento canônico no arquivo de documentação.
* `description`é uma descrição das circunstâncias em que a exceção é lançada.

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

Essa marca permite incluir informações de um documento XML externo ao arquivo de código-fonte. O arquivo externo deve ser um documento XML bem formado e uma expressão XPath é aplicada a esse documento para especificar o XML desse documento a ser incluído. Em `<include>` seguida, a marca é substituída pelo XML selecionado do documento externo.

__Sintaxe__

```xml
<include file="filename" path="xpath" />
```

onde

* `filename`é o nome de arquivo de um arquivo XML externo. O nome do arquivo é interpretado em relação ao arquivo que contém a marca include.
* `xpath`é uma expressão XPath que seleciona parte do XML no arquivo XML externo.

__Exemplo:__

Se o código-fonte contiver uma declaração como:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

e o arquivo externo "docs. xml" tinha o seguinte conteúdo:

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

em seguida, a mesma documentação é saída como se o código-fonte contivesse:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Essa marca é usada para criar uma lista ou tabela de itens. Ele pode conter um `<listheader>` bloco para definir a linha de cabeçalho de uma tabela ou lista de definições. (Ao definir uma tabela, apenas uma entrada para `term` no cabeçalho precisa ser fornecida.)

Cada item na lista é especificado com um `<item>` bloco. Ao criar uma lista de definições, `term` e `description` deve ser especificado. No entanto, para uma tabela, lista com marcadores ou lista numerada, `description` só é necessário especificar.

__Sintaxe__

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

* `term`é o termo a ser definido, cuja definição está `description`.
* `description`é um item em uma lista com marcadores ou numerada ou a definição de um `term`.

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

Essa marca é para uso dentro de `<summary>` outras marcas, como ([`<remarks>`](documentation-comments.md#remarks)) ou `<returns>` ([`<returns>`](documentation-comments.md#returns)), e permite que a estrutura seja adicionada ao texto.

__Sintaxe__

```xml
<para>content</para>
```

em `content` que é o texto do parágrafo.

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

Essa marca é usada para descrever um parâmetro para um método, Construtor ou indexador.

__Sintaxe__

```xml
<param name="name">description</param>
```

onde

* `name`é o nome do parâmetro.
* `description`é uma descrição do parâmetro.

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

Essa marca é usada para indicar que uma palavra é um parâmetro. O arquivo de documentação pode ser processado para formatar esse parâmetro de uma maneira distinta.

__Sintaxe__

```xml
<paramref name="name"/>
```

em `name` que é o nome do parâmetro.

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

Essa marca permite que a acessibilidade de segurança de um membro seja documentada.

__Sintaxe__

```xml
<permission cref="member">description</permission>
```

onde

* `member`é o nome de um membro. O gerador de documentação verifica se o elemento de código fornecido existe e traduz o *membro* para o nome do elemento canônico no arquivo de documentação.
* `description`é uma descrição do acesso ao membro.

__Exemplo:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

Essa marca é usada para especificar informações adicionais sobre um tipo. (Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para descrever o próprio tipo e os membros de um tipo.)

__Sintaxe__

```xml
<remarks>description</remarks>
```

em `description` que é o texto do comentário.

__Exemplo:__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

Essa marca é usada para descrever o valor de retorno de um método.

__Sintaxe__

```xml
<returns>description</returns>
```

em `description` que é uma descrição do valor de retorno.

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

Essa marca permite que um link seja especificado dentro do texto. Use `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) para indicar o texto que deve aparecer em uma seção ver também.

__Sintaxe__

```xml
<see cref="member"/>
```

em `member` que é o nome de um membro. O gerador de documentação verifica se o elemento de código fornecido existe e altera o *membro* para o nome do elemento no arquivo de documentação gerado.

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

Essa marca permite que uma entrada seja gerada para a seção Consulte também. Use `<see>` [(`<see>`](documentation-comments.md#see)) para especificar um link de dentro do texto.

__Sintaxe__

```xml
<seealso cref="member"/>
```

em `member` que é o nome de um membro. O gerador de documentação verifica se o elemento de código fornecido existe e altera o *membro* para o nome do elemento no arquivo de documentação gerado.

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

Essa marca pode ser usada para descrever um tipo ou um membro de um tipo. Use `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)) para descrever o próprio tipo.

__Sintaxe__

```xml
<summary>description</summary>
```

em `description` que é um resumo do tipo ou membro.

__Exemplo:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Essa marca permite que uma propriedade seja descrita.

__Sintaxe__

```xml
<value>property description</value>
```

em `property description` que é uma descrição para a propriedade.

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

Essa marca é usada para descrever um parâmetro de tipo genérico para uma classe, struct, interface, delegado ou método.

__Sintaxe__

```xml
<typeparam name="name">description</typeparam>
```

em `name` que é o nome do parâmetro de tipo e `description` é sua descrição.

__Exemplo:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Essa marca é usada para indicar que uma palavra é um parâmetro de tipo. O arquivo de documentação pode ser processado para formatar esse parâmetro de tipo de alguma maneira distinta.

__Sintaxe__

```xml
<typeparamref name="name"/>
```

em `name` que é o nome do parâmetro de tipo.

__Exemplo:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Processando o arquivo de documentação

O gerador de documentação gera uma cadeia de caracteres de ID para cada elemento no código-fonte que é marcado com um comentário de documentação. Essa cadeia de caracteres de ID identifica exclusivamente um elemento de origem. Um visualizador de documentação pode usar uma cadeia de caracteres de ID para identificar o item de metadados/reflexão correspondente ao qual a documentação se aplica.

O arquivo de documentação não é uma representação hierárquica do código-fonte; em vez disso, é uma lista simples com uma cadeia de caracteres de ID gerada para cada elemento.

### <a name="id-string-format"></a>Formato da cadeia de caracteres de ID

O gerador de documentação observa as seguintes regras ao gerar as cadeias de caracteres de ID:

*  Nenhum espaço em branco é colocado na cadeia de caracteres.

*  A primeira parte da cadeia de caracteres identifica o tipo de membro que está sendo documentado, por meio de um único caractere seguido por dois-pontos. Os seguintes tipos de membros são definidos:

   | __Caractere__ | __Descrição__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | evento                                                       |
   | F             | Campo                                                       |
   | M             | Método (incluindo construtores, destruidores e operadores) |
   | N             | Namespace                                                   |
   | P             | Propriedade (incluindo indexadores)                               |
   | T             | Tipo (como classe, delegado, enumeração, interface e struct) |
   | !             | Cadeia de caracteres de erro; o restante da cadeia de caracteres fornece informações sobre o erro. Por exemplo, o gerador de documentação gera informações de erro para links que não podem ser resolvidos. |

*  A segunda parte da cadeia de caracteres é o nome totalmente qualificado do elemento, começando na raiz do namespace. O nome do elemento, seus tipos de delimitador e o namespace são separados por pontos. Se o nome do próprio item tiver pontos, eles serão substituídos por `#(U+0023)` caracteres. (Supõe-se que nenhum elemento tenha esse caractere em seu nome.)
*  Para métodos e propriedades com argumentos, a lista de argumentos segue, entre parênteses. Para aqueles sem argumentos, os parênteses são omitidos. Os argumentos são separados por vírgulas. A codificação de cada argumento é a mesma que uma assinatura da CLI, da seguinte maneira:
   *  Os argumentos são representados pelo nome da documentação, que se baseia em seu nome totalmente qualificado, modificado da seguinte maneira:
      * Os argumentos que representam tipos genéricos têm um caractere `` ` `` anexado (de marca de seleção) seguido pelo número de parâmetros de tipo
      * Os argumentos que `out` têm `ref` o modificador `@` ou têm um nome de tipo a seguir. Os argumentos passados por valor ou `params` via não têm notação especial.
      * Os argumentos que são matrizes são `[lowerbound:size, ... , lowerbound:size]` representados como onde o número de vírgulas é a classificação menos uma, e os limites inferiores e o tamanho de cada dimensão, se conhecido, são representados em decimal. Se um limite inferior ou tamanho não for especificado, ele será omitido. Se o limite inferior e o tamanho de uma determinada dimensão forem omitidos `:` , o também será omitido. As matrizes denteadas são `[]` representadas por um por nível.
      * Os argumentos que têm tipos de ponteiro diferentes de void são representados usando `*` o nome do tipo a seguir. Um ponteiro void é representado usando um nome de tipo `System.Void`de.
      * Os argumentos que se referem a parâmetros de tipo genérico definidos em tipos `` ` `` são codificados usando o caractere (backtick) seguido pelo índice de base zero do parâmetro de tipo.
      * ``` `` ``` Os`` ` `` argumentos que usam parâmetros de tipo genérico definidos em métodos usam uma marca de seleção dupla em vez de usados para tipos.
      * Os argumentos que se referem a tipos genéricos construídos são codificados usando o tipo `{`genérico, seguido por, seguido por uma lista separada por vírgulas de `}`argumentos de tipo, seguida por.

### <a name="id-string-examples"></a>Exemplos de cadeia de caracteres de ID

Os exemplos a seguir mostram um fragmento de C# código, juntamente com a cadeia de caracteres de ID produzida de cada elemento de origem capaz de ter um comentário de documentação:

*  Os tipos são representados usando seu nome totalmente qualificado, aumentados com informações genéricas:

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

*  Os campos são representados pelo nome totalmente qualificado:

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

*  Maneiras.

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

*  LostFocus.

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

   O conjunto completo de nomes de funções de operador unários usado `op_UnaryPlus`é o seguinte: `op_Increment` `op_LogicalNot`, `op_Decrement` `op_UnaryNegation` `op_OnesComplement`, `op_True`,, `op_False`,, e.

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

   O conjunto completo de nomes de funções de operador binários usado é `op_Addition`o `op_Subtraction`seguinte `op_Multiply`: `op_Division`, `op_Modulus` `op_LeftShift` `op_ExclusiveOr` `op_BitwiseAnd` `op_BitwiseOr`,,,,, `op_RightShift`,,,, ,,,,`op_Equality`e .`op_GreaterThanOrEqual` `op_Inequality` `op_LessThan` `op_LessThanOrEqual` `op_GreaterThan`

*  Os operadores de conversão têm um "`~`" à direita seguido pelo tipo de retorno.

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

### <a name="c-source-code"></a>C#código-fonte

O exemplo a seguir mostra o código-fonte `Point` de uma classe:

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

Aqui está a saída produzida por um gerador de documentação quando é fornecido o código- `Point`fonte para a classe, mostrado acima:

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
