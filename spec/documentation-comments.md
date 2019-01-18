---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54272040"
---
# <a name="documentation-comments"></a><span data-ttu-id="9542a-101">Comentários de documentação</span><span class="sxs-lookup"><span data-stu-id="9542a-101">Documentation comments</span></span>

<span data-ttu-id="9542a-102">O c# fornece um mecanismo para os programadores a documentar seu código usando uma sintaxe de comentário especial que contém o texto XML.</span><span class="sxs-lookup"><span data-stu-id="9542a-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="9542a-103">Em arquivos de código-fonte, ter uma determinada forma de comentários podem ser usados para direcionar uma ferramenta para produzir o XML desses comentários e os elementos de código do código-fonte, eles precedem.</span><span class="sxs-lookup"><span data-stu-id="9542a-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="9542a-104">Comentários usando essa sintaxe são chamados ***comentários de documentação***.</span><span class="sxs-lookup"><span data-stu-id="9542a-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="9542a-105">Eles devem preceder imediatamente um tipo definido pelo usuário (por exemplo, uma classe, delegado ou interface) ou um membro (por exemplo, um campo, evento, propriedade ou método).</span><span class="sxs-lookup"><span data-stu-id="9542a-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="9542a-106">A ferramenta de geração de XML é chamada de ***gerador de documentação***.</span><span class="sxs-lookup"><span data-stu-id="9542a-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="9542a-107">(Esse gerador poderia ser, mas não precisa ser, o compilador do c#.) A saída produzida pelo gerador de documentação é chamada de ***arquivo de documentação***.</span><span class="sxs-lookup"><span data-stu-id="9542a-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="9542a-108">Um arquivo de documentação é usado como entrada para um ***Visualizador de documentação***; uma ferramenta desenvolvida para produzir algum tipo de exibição visual de informações de tipo e a documentação associada.</span><span class="sxs-lookup"><span data-stu-id="9542a-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="9542a-109">Essa especificação sugere um conjunto de marcas a ser usado em comentários de documentação, mas o uso dessas marcas não é necessário e outras marcas podem ser usadas se desejado, como tempo as regras de XML bem formado são seguidas.</span><span class="sxs-lookup"><span data-stu-id="9542a-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="9542a-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="9542a-110">Introduction</span></span>

<span data-ttu-id="9542a-111">Ter uma forma especial de comentários podem ser usados para direcionar uma ferramenta para produzir o XML desses comentários e os elementos de código do código-fonte, eles precedem.</span><span class="sxs-lookup"><span data-stu-id="9542a-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="9542a-112">Esses comentários são os comentários de linha única que começam com três barras (`///`), ou delimitado por comentários que começam com uma barra e duas estrelas (`/**`).</span><span class="sxs-lookup"><span data-stu-id="9542a-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="9542a-113">Eles devem preceder imediatamente um tipo definido pelo usuário (por exemplo, uma classe, delegado ou interface) ou um membro (por exemplo, um campo, evento, propriedade ou método) que eles anotar.</span><span class="sxs-lookup"><span data-stu-id="9542a-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="9542a-114">Seções de atributo ([especificação do atributo](attributes.md#attribute-specification)) são considerados parte das declarações, portanto, os comentários de documentação devem preceder os atributos aplicados a um tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="9542a-115">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="9542a-116">Em um *single_line_doc_comment*, se houver um *espaço em branco* seguinte de caractere a `///` caracteres em cada um do *single_line_doc_comment*s adjacentes ao atual *single_line_doc_comment*, em seguida, em que *espaço em branco* caractere não está incluído na saída XML.</span><span class="sxs-lookup"><span data-stu-id="9542a-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="9542a-117">Em um delimitados-comentário de documento, se o primeiro caractere não espaço em branco na segunda linha for um asterisco e o mesmo padrão de caracteres de espaço em branco opcional e um caractere de asterisco é repetido no início de cada linha de dentro a delimitados--comentário da documentação, em seguida, os caracteres do padrão repetido não estão incluídos na saída XML.</span><span class="sxs-lookup"><span data-stu-id="9542a-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="9542a-118">O padrão pode incluir caracteres de espaço em branco, depois, bem como antes, o caractere de asterisco.</span><span class="sxs-lookup"><span data-stu-id="9542a-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="9542a-119">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-119">__Example:__</span></span>

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

<span data-ttu-id="9542a-120">O texto dentro de comentários de documentação deve estar bem formado de acordo com as regras do XML (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="9542a-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="9542a-121">Se o XML está mal formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que foi encontrado um erro.</span><span class="sxs-lookup"><span data-stu-id="9542a-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="9542a-122">Embora os desenvolvedores são livres para criar seu próprio conjunto de marcas, um conjunto recomendado é definido em [marcações recomendadas](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="9542a-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="9542a-123">Algumas das marcas recomendadas têm significado especial:</span><span class="sxs-lookup"><span data-stu-id="9542a-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="9542a-124">O `<param>` marca é usada para descrever parâmetros.</span><span class="sxs-lookup"><span data-stu-id="9542a-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="9542a-125">Se tal uma marca é usada, o gerador de documentação deve verificar se o parâmetro especificado existe e que todos os parâmetros estão descritos nos comentários da documentação.</span><span class="sxs-lookup"><span data-stu-id="9542a-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="9542a-126">Se essa verificação falhar, o gerador de documentação emite um aviso.</span><span class="sxs-lookup"><span data-stu-id="9542a-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="9542a-127">O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código.</span><span class="sxs-lookup"><span data-stu-id="9542a-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="9542a-128">O gerador de documentação deve verificar se este elemento de código existe.</span><span class="sxs-lookup"><span data-stu-id="9542a-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="9542a-129">Se a verificação falhar, o gerador de documentação emite um aviso.</span><span class="sxs-lookup"><span data-stu-id="9542a-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="9542a-130">Ao procurar por um nome descritas em uma `cref` atributo, o gerador de documentação deve respeitar a visibilidade de namespace de acordo com a `using` instruções que aparecem no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="9542a-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="9542a-131">Para elementos de código que são genéricos, a sintaxe genérica normal (ou seja, "`List<T>`") não pode ser usado porque ele produz um XML inválido.</span><span class="sxs-lookup"><span data-stu-id="9542a-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="9542a-132">As chaves podem ser usadas em vez de colchetes (ou seja, "`List{T}`"), ou a sintaxe de escape XML pode ser usada (ou seja, "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="9542a-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="9542a-133">O `<summary>` marca se destina a ser usado por um visualizador de documentação para exibir informações adicionais sobre um tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="9542a-134">O `<include>` tag inclui informações de um arquivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="9542a-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="9542a-135">Observe atentamente que o arquivo de documentação não fornece informações completas sobre o tipo e membros (por exemplo, ele não contém nenhuma informação de tipo).</span><span class="sxs-lookup"><span data-stu-id="9542a-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="9542a-136">Para obter essas informações sobre um tipo ou membro, o arquivo de documentação deve ser usado em conjunto com a reflexão no membro ou tipo real.</span><span class="sxs-lookup"><span data-stu-id="9542a-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="9542a-137">Marcações recomendadas</span><span class="sxs-lookup"><span data-stu-id="9542a-137">Recommended tags</span></span>

<span data-ttu-id="9542a-138">O gerador de documentação deve aceitar e processar qualquer marca que seja válida de acordo com as regras do XML.</span><span class="sxs-lookup"><span data-stu-id="9542a-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="9542a-139">As seguintes marcas fornecem funcionalidades comumente usadas na documentação do usuário.</span><span class="sxs-lookup"><span data-stu-id="9542a-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="9542a-140">(É claro, outras marcas são possíveis.)</span><span class="sxs-lookup"><span data-stu-id="9542a-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="9542a-141">__Tag__</span><span class="sxs-lookup"><span data-stu-id="9542a-141">__Tag__</span></span>          | <span data-ttu-id="9542a-142">__Section__</span><span class="sxs-lookup"><span data-stu-id="9542a-142">__Section__</span></span>                                            | <span data-ttu-id="9542a-143">__Finalidade__</span><span class="sxs-lookup"><span data-stu-id="9542a-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="9542a-144">Definir o texto em uma fonte de código</span><span class="sxs-lookup"><span data-stu-id="9542a-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="9542a-145">Defina uma ou mais linhas de saída de programa ou código de origem</span><span class="sxs-lookup"><span data-stu-id="9542a-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="9542a-146">Indicar um exemplo</span><span class="sxs-lookup"><span data-stu-id="9542a-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="9542a-147">Identifica um método pode lançar exceções</span><span class="sxs-lookup"><span data-stu-id="9542a-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="9542a-148">Inclui o XML de um arquivo externo</span><span class="sxs-lookup"><span data-stu-id="9542a-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="9542a-149">Criar uma lista ou tabela</span><span class="sxs-lookup"><span data-stu-id="9542a-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="9542a-150">Permitir que a estrutura a ser adicionado ao texto</span><span class="sxs-lookup"><span data-stu-id="9542a-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="9542a-151">Descrever um parâmetro para um método ou construtor</span><span class="sxs-lookup"><span data-stu-id="9542a-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="9542a-152">Identificar que uma palavra é um nome de parâmetro</span><span class="sxs-lookup"><span data-stu-id="9542a-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="9542a-153">A acessibilidade de segurança de um membro do documento</span><span class="sxs-lookup"><span data-stu-id="9542a-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="9542a-154">Descrever informações adicionais sobre um tipo</span><span class="sxs-lookup"><span data-stu-id="9542a-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="9542a-155">Descrever o valor de retorno de um método</span><span class="sxs-lookup"><span data-stu-id="9542a-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="9542a-156">Especifique um link</span><span class="sxs-lookup"><span data-stu-id="9542a-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="9542a-157">Gerar uma entrada Consulte também</span><span class="sxs-lookup"><span data-stu-id="9542a-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="9542a-158">Descrever um tipo ou membro de um tipo</span><span class="sxs-lookup"><span data-stu-id="9542a-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="9542a-159">Descrever uma propriedade</span><span class="sxs-lookup"><span data-stu-id="9542a-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="9542a-160">Descrever um parâmetro de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="9542a-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="9542a-161">Identificar que uma palavra é um nome de parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="9542a-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="9542a-162">Essa marca fornece um mecanismo para indicar que um fragmento de texto dentro uma descrição deve ser definido em uma fonte especial, como o usado para um bloco de código.</span><span class="sxs-lookup"><span data-stu-id="9542a-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="9542a-163">Para linhas de código real, use `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="9542a-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="9542a-164">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="9542a-165">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="9542a-166">Essa marca é usada para definir uma ou mais linhas de saída de programa ou código de origem em alguma fonte especial.</span><span class="sxs-lookup"><span data-stu-id="9542a-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="9542a-167">Para fragmentos de código pequeno em narrativa, use `<c>` ([`<c>`](documentation-comments.md#c)).</span><span class="sxs-lookup"><span data-stu-id="9542a-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="9542a-168">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="9542a-169">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-169">__Example:__</span></span>

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

<span data-ttu-id="9542a-170">Essa marca permite que o código de exemplo de um comentário, para especificar como um método ou outro membro da biblioteca pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="9542a-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="9542a-171">Normalmente, isso também envolveria uso da marca `<code>` ([`<code>`](documentation-comments.md#code)) também.</span><span class="sxs-lookup"><span data-stu-id="9542a-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="9542a-172">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="9542a-173">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-173">__Example:__</span></span>

<span data-ttu-id="9542a-174">Ver `<code>` ([`<code>`](documentation-comments.md#code)) para obter um exemplo.</span><span class="sxs-lookup"><span data-stu-id="9542a-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="9542a-175">Essa marca fornece uma maneira para documentar um método pode lançar exceções.</span><span class="sxs-lookup"><span data-stu-id="9542a-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="9542a-176">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="9542a-177">onde</span><span class="sxs-lookup"><span data-stu-id="9542a-177">where</span></span>

* <span data-ttu-id="9542a-178">`member` é o nome de um membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-178">`member` is the name of a member.</span></span> <span data-ttu-id="9542a-179">O gerador de documentação verifica se o membro fornecido existe e converte `member` para o nome de elemento canônico no arquivo de documentação.</span><span class="sxs-lookup"><span data-stu-id="9542a-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="9542a-180">`description` é uma descrição das circunstâncias em que a exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="9542a-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="9542a-181">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-181">__Example:__</span></span>

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

<span data-ttu-id="9542a-182">Permite que essa marca, incluindo informações de um documento XML que é externo ao arquivo de código de origem.</span><span class="sxs-lookup"><span data-stu-id="9542a-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="9542a-183">O arquivo externo deve ser um documento XML bem formado, e uma expressão XPath é aplicada a esse documento para especificar quais XML desse documento para incluir.</span><span class="sxs-lookup"><span data-stu-id="9542a-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="9542a-184">O `<include>` marca, em seguida, é substituída pelo XML selecionado do documento externo.</span><span class="sxs-lookup"><span data-stu-id="9542a-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="9542a-185">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="9542a-186">onde</span><span class="sxs-lookup"><span data-stu-id="9542a-186">where</span></span>

* <span data-ttu-id="9542a-187">`filename` é o nome do arquivo de um arquivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="9542a-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="9542a-188">O nome do arquivo é interpretado em relação ao arquivo que contém a marca include.</span><span class="sxs-lookup"><span data-stu-id="9542a-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="9542a-189">`xpath` é uma expressão XPath que seleciona alguns do XML no arquivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="9542a-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="9542a-190">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-190">__Example:__</span></span>

<span data-ttu-id="9542a-191">Se o código-fonte contiver uma declaração como:</span><span class="sxs-lookup"><span data-stu-id="9542a-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="9542a-192">e o arquivo externo "docs.xml" tinha o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="9542a-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="9542a-193">em seguida, a documentação do mesma é a saída como se o código-fonte contido:</span><span class="sxs-lookup"><span data-stu-id="9542a-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="9542a-194">Essa marca é usada para criar uma lista ou tabela de itens.</span><span class="sxs-lookup"><span data-stu-id="9542a-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="9542a-195">Ele pode conter um `<listheader>` bloco para definir a linha de cabeçalho de uma tabela ou uma definição de lista.</span><span class="sxs-lookup"><span data-stu-id="9542a-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="9542a-196">(Durante a definição de uma tabela, apenas uma entrada para `term` no título precisa ser fornecido.)</span><span class="sxs-lookup"><span data-stu-id="9542a-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="9542a-197">Cada item na lista é especificado com um `<item>` bloco.</span><span class="sxs-lookup"><span data-stu-id="9542a-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="9542a-198">Ao criar uma lista de definições, ambos `term` e `description` deve ser especificado.</span><span class="sxs-lookup"><span data-stu-id="9542a-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="9542a-199">No entanto, para uma tabela, lista com marcadores ou lista numerada, apenas `description` precisa ser especificado.</span><span class="sxs-lookup"><span data-stu-id="9542a-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="9542a-200">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-200">__Syntax:__</span></span>

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

<span data-ttu-id="9542a-201">onde</span><span class="sxs-lookup"><span data-stu-id="9542a-201">where</span></span>

* <span data-ttu-id="9542a-202">`term` é o termo para definir, cuja definição está em `description`.</span><span class="sxs-lookup"><span data-stu-id="9542a-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="9542a-203">`description` é um item em um marcador ou lista numerada ou a definição de um `term`.</span><span class="sxs-lookup"><span data-stu-id="9542a-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="9542a-204">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-204">__Example:__</span></span>

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

<span data-ttu-id="9542a-205">Essa marca é para uso dentro de outras marcas, como `<summary>` ([`<remark>`](documentation-comments.md#remark)) ou `<returns>` ([`<returns>`](documentation-comments.md#returns)) e permite que a estrutura a ser adicionado ao texto.</span><span class="sxs-lookup"><span data-stu-id="9542a-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="9542a-206">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="9542a-207">onde `content` é o texto do parágrafo.</span><span class="sxs-lookup"><span data-stu-id="9542a-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="9542a-208">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-208">__Example:__</span></span>

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

<span data-ttu-id="9542a-209">Essa marca é usada para descrever um parâmetro para um método, construtor ou indexador.</span><span class="sxs-lookup"><span data-stu-id="9542a-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="9542a-210">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="9542a-211">onde</span><span class="sxs-lookup"><span data-stu-id="9542a-211">where</span></span>

* <span data-ttu-id="9542a-212">`name` É o nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9542a-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="9542a-213">`description` é uma descrição do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9542a-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="9542a-214">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-214">__Example:__</span></span>

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

<span data-ttu-id="9542a-215">Essa marca é usada para indicar que uma palavra é um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9542a-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="9542a-216">O arquivo de documentação pode ser processado para formatar esse parâmetro de alguma forma distinta.</span><span class="sxs-lookup"><span data-stu-id="9542a-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="9542a-217">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="9542a-218">onde `name` é o nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9542a-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="9542a-219">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-219">__Example:__</span></span>

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

<span data-ttu-id="9542a-220">Essa marca permite que a acessibilidade de segurança de um membro a ser documentada.</span><span class="sxs-lookup"><span data-stu-id="9542a-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="9542a-221">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="9542a-222">onde</span><span class="sxs-lookup"><span data-stu-id="9542a-222">where</span></span>

* <span data-ttu-id="9542a-223">`member` é o nome de um membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-223">`member` is the name of a member.</span></span> <span data-ttu-id="9542a-224">O gerador de documentação verifica se o elemento de código fornecido existe e converte *membro* para o nome de elemento canônico no arquivo de documentação.</span><span class="sxs-lookup"><span data-stu-id="9542a-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="9542a-225">`description` é uma descrição do acesso ao membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="9542a-226">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="9542a-227">Essa marca é usada para especificar informações adicionais sobre um tipo.</span><span class="sxs-lookup"><span data-stu-id="9542a-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="9542a-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para descrever o próprio tipo e os membros de um tipo.)</span><span class="sxs-lookup"><span data-stu-id="9542a-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="9542a-229">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="9542a-230">onde `description` é o texto do comentário.</span><span class="sxs-lookup"><span data-stu-id="9542a-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="9542a-231">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-231">__Example:__</span></span>

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

<span data-ttu-id="9542a-232">Essa marca é usada para descrever o valor de retorno de um método.</span><span class="sxs-lookup"><span data-stu-id="9542a-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="9542a-233">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="9542a-234">onde `description` é uma descrição do valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="9542a-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="9542a-235">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="9542a-236">Essa marca permite que um link para ser especificado dentro do texto.</span><span class="sxs-lookup"><span data-stu-id="9542a-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="9542a-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) para indicar o texto que deve aparecer em uma seção Consulte também.</span><span class="sxs-lookup"><span data-stu-id="9542a-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="9542a-238">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="9542a-239">onde `member` é o nome do membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-239">where `member` is the name of a member.</span></span> <span data-ttu-id="9542a-240">O gerador de documentação verifica se o elemento de código fornecido existe e se altera *membro* para o nome do elemento no arquivo de documentação gerada.</span><span class="sxs-lookup"><span data-stu-id="9542a-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="9542a-241">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-241">__Example:__</span></span>

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

<span data-ttu-id="9542a-242">Essa marca permite que uma entrada a ser gerado para a seção Consulte também.</span><span class="sxs-lookup"><span data-stu-id="9542a-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="9542a-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) para especificar um link de dentro do texto.</span><span class="sxs-lookup"><span data-stu-id="9542a-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="9542a-244">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="9542a-245">onde `member` é o nome do membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-245">where `member` is the name of a member.</span></span> <span data-ttu-id="9542a-246">O gerador de documentação verifica se o elemento de código fornecido existe e se altera *membro* para o nome do elemento no arquivo de documentação gerada.</span><span class="sxs-lookup"><span data-stu-id="9542a-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="9542a-247">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-247">__Example:__</span></span>

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

Essa marca pode ser usada para descrever um tipo ou membro de um tipo. <span data-ttu-id="9542a-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) para descrever o tipo em si.</span><span class="sxs-lookup"><span data-stu-id="9542a-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="9542a-250">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="9542a-251">onde `description` é um resumo do tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="9542a-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="9542a-252">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="9542a-253">Essa marca permite que uma propriedade a ser descrito.</span><span class="sxs-lookup"><span data-stu-id="9542a-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="9542a-254">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="9542a-255">onde `property description` é uma descrição para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="9542a-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="9542a-256">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="9542a-257">Essa marca é usada para descrever um parâmetro de tipo genérico de classe, struct, interface, delegado ou método.</span><span class="sxs-lookup"><span data-stu-id="9542a-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="9542a-258">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="9542a-259">em que `name` é o nome do parâmetro de tipo, e `description` é sua descrição.</span><span class="sxs-lookup"><span data-stu-id="9542a-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="9542a-260">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="9542a-261">Essa marca é usada para indicar que uma palavra é um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="9542a-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="9542a-262">O arquivo de documentação pode ser processado para formatar esse tipo de parâmetro de alguma forma distinta.</span><span class="sxs-lookup"><span data-stu-id="9542a-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="9542a-263">__Sintaxe:__</span><span class="sxs-lookup"><span data-stu-id="9542a-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="9542a-264">onde `name` é o nome do parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="9542a-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="9542a-265">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="9542a-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="9542a-266">Processando o arquivo de documentação</span><span class="sxs-lookup"><span data-stu-id="9542a-266">Processing the documentation file</span></span>

<span data-ttu-id="9542a-267">O gerador de documentação gera uma cadeia de caracteres de ID para cada elemento no código-fonte que é marcado com um comentário de documentação.</span><span class="sxs-lookup"><span data-stu-id="9542a-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="9542a-268">Essa cadeia de caracteres de identificação identifica exclusivamente um elemento de origem.</span><span class="sxs-lookup"><span data-stu-id="9542a-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="9542a-269">Um visualizador de documentação pode usar uma cadeia de caracteres de ID para identificar o item de metadados/reflexão correspondente ao qual se aplica a documentação.</span><span class="sxs-lookup"><span data-stu-id="9542a-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="9542a-270">O arquivo de documentação não é uma representação hierárquica do código-fonte; em vez disso, é uma lista simples com uma cadeia de caracteres de ID gerada para cada elemento.</span><span class="sxs-lookup"><span data-stu-id="9542a-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="9542a-271">Formato de cadeia de caracteres de ID</span><span class="sxs-lookup"><span data-stu-id="9542a-271">ID string format</span></span>

<span data-ttu-id="9542a-272">O gerador de documentação segue as seguintes regras ao gerar as cadeias de caracteres de ID:</span><span class="sxs-lookup"><span data-stu-id="9542a-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="9542a-273">Nenhum espaço em branco é colocado na cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="9542a-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="9542a-274">A primeira parte da cadeia de caracteres identifica o tipo de membro documentado, por meio de um único caractere seguido por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="9542a-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="9542a-275">Os seguintes tipos de membros são definidos:</span><span class="sxs-lookup"><span data-stu-id="9542a-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="9542a-276">__Caractere__</span><span class="sxs-lookup"><span data-stu-id="9542a-276">__Character__</span></span> | <span data-ttu-id="9542a-277">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="9542a-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="9542a-278">E</span><span class="sxs-lookup"><span data-stu-id="9542a-278">E</span></span>             | <span data-ttu-id="9542a-279">evento</span><span class="sxs-lookup"><span data-stu-id="9542a-279">Event</span></span>                                                       |
   | <span data-ttu-id="9542a-280">F</span><span class="sxs-lookup"><span data-stu-id="9542a-280">F</span></span>             | <span data-ttu-id="9542a-281">Campo</span><span class="sxs-lookup"><span data-stu-id="9542a-281">Field</span></span>                                                       |
   | <span data-ttu-id="9542a-282">M</span><span class="sxs-lookup"><span data-stu-id="9542a-282">M</span></span>             | <span data-ttu-id="9542a-283">Método (incluindo construtores, destruidores e operadores)</span><span class="sxs-lookup"><span data-stu-id="9542a-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="9542a-284">N</span><span class="sxs-lookup"><span data-stu-id="9542a-284">N</span></span>             | <span data-ttu-id="9542a-285">Namespace</span><span class="sxs-lookup"><span data-stu-id="9542a-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="9542a-286">P</span><span class="sxs-lookup"><span data-stu-id="9542a-286">P</span></span>             | <span data-ttu-id="9542a-287">Propriedade (incluindo indexadores)</span><span class="sxs-lookup"><span data-stu-id="9542a-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="9542a-288">T</span><span class="sxs-lookup"><span data-stu-id="9542a-288">T</span></span>             | <span data-ttu-id="9542a-289">Tipo (como a classe delegate, enum, interface e struct)</span><span class="sxs-lookup"><span data-stu-id="9542a-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="9542a-290">!</span><span class="sxs-lookup"><span data-stu-id="9542a-290">!</span></span>             | <span data-ttu-id="9542a-291">Cadeia de caracteres de erro; o restante da cadeia de caracteres fornece informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="9542a-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="9542a-292">Por exemplo, o gerador de documentação gera informações de erro para links que não pode ser resolvido.</span><span class="sxs-lookup"><span data-stu-id="9542a-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="9542a-293">A segunda parte da cadeia de caracteres é o nome totalmente qualificado do elemento, começando na raiz do namespace.</span><span class="sxs-lookup"><span data-stu-id="9542a-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="9542a-294">O nome do elemento, seu tipo (s) delimitador e o namespace são separados por pontos.</span><span class="sxs-lookup"><span data-stu-id="9542a-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="9542a-295">Se o nome do próprio item tiver pontos, elas serão substituídas por `#(U+0023)` caracteres.</span><span class="sxs-lookup"><span data-stu-id="9542a-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="9542a-296">(Ele é considerado que o elemento não tem esse caractere em seu nome.)</span><span class="sxs-lookup"><span data-stu-id="9542a-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="9542a-297">Para métodos e propriedades com argumentos, da seguinte maneira lista de argumentos entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="9542a-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="9542a-298">Para aqueles sem argumentos, os parênteses serão omitidos.</span><span class="sxs-lookup"><span data-stu-id="9542a-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="9542a-299">Os argumentos são separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="9542a-299">The arguments are separated by commas.</span></span> <span data-ttu-id="9542a-300">A codificação de cada argumento é o mesmo que uma assinatura da CLI, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9542a-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="9542a-301">Argumentos são representados por seu nome de documentação, que se baseia em seu nome totalmente qualificado, modificada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9542a-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="9542a-302">Argumentos que representam tipos genéricos possuem um acrescentadas `` ` `` caractere (acento grave) seguido pelo número de parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="9542a-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="9542a-303">Argumentos com o `out` ou `ref` modificador têm um `@` seu nome de tipo a seguir.</span><span class="sxs-lookup"><span data-stu-id="9542a-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="9542a-304">Argumentos passados por valor ou por meio de `params` não ter nenhuma anotação especial.</span><span class="sxs-lookup"><span data-stu-id="9542a-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="9542a-305">Os argumentos que são matrizes são representados como `[lowerbound:size, ... , lowerbound:size]` em que o número de vírgulas é a classificação menos um, e os limites inferior e o tamanho de cada dimensão, se conhecidos, são representados no formato decimal.</span><span class="sxs-lookup"><span data-stu-id="9542a-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="9542a-306">Se um limite inferior ou o tamanho não for especificado, ele é omitido.</span><span class="sxs-lookup"><span data-stu-id="9542a-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="9542a-307">Se o limite inferior e o tamanho de uma determinada dimensão forem omitidos, o `:` será omitido também.</span><span class="sxs-lookup"><span data-stu-id="9542a-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="9542a-308">Matrizes denteadas são representadas por um `[]` por nível.</span><span class="sxs-lookup"><span data-stu-id="9542a-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="9542a-309">Os argumentos que têm tipos de ponteiro que não seja nulo são representados usando um `*` seguindo o nome do tipo.</span><span class="sxs-lookup"><span data-stu-id="9542a-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="9542a-310">Um ponteiro nulo é representado usando um nome de tipo de `System.Void`.</span><span class="sxs-lookup"><span data-stu-id="9542a-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="9542a-311">Argumentos que se referem a definidos nos tipos de parâmetros de tipo genérico são codificados usando o `` ` `` caractere (acento grave) seguido pelo índice baseado em zero do parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="9542a-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="9542a-312">Argumentos que usam parâmetros de tipo genérico definidos em métodos usam um double-acento grave ``` `` ``` em vez do `` ` `` usado para tipos.</span><span class="sxs-lookup"><span data-stu-id="9542a-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="9542a-313">Argumentos que se referem a tipos genéricos construídos são codificados usando o tipo genérico, seguido por `{`, seguido por uma lista separada por vírgulas de argumentos de tipo, seguido por `}`.</span><span class="sxs-lookup"><span data-stu-id="9542a-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="9542a-314">Exemplos de cadeia de caracteres de ID</span><span class="sxs-lookup"><span data-stu-id="9542a-314">ID string examples</span></span>

<span data-ttu-id="9542a-315">Os exemplos seguintes mostram um fragmento de código c#, juntamente com a ID de cadeia de caracteres produzido a partir de cada elemento de origem pode ocupar um comentário de documentação:</span><span class="sxs-lookup"><span data-stu-id="9542a-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="9542a-316">Tipos são representados usando seu nome totalmente qualificado, aumentada com informações genéricas:</span><span class="sxs-lookup"><span data-stu-id="9542a-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="9542a-317">Campos são representados por seu nome totalmente qualificado:</span><span class="sxs-lookup"><span data-stu-id="9542a-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="9542a-318">Construtores.</span><span class="sxs-lookup"><span data-stu-id="9542a-318">Constructors.</span></span>

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

*  <span data-ttu-id="9542a-319">Destruidores.</span><span class="sxs-lookup"><span data-stu-id="9542a-319">Destructors.</span></span>

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

*  <span data-ttu-id="9542a-320">Métodos.</span><span class="sxs-lookup"><span data-stu-id="9542a-320">Methods.</span></span>

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

*  <span data-ttu-id="9542a-321">Propriedades e indexadores.</span><span class="sxs-lookup"><span data-stu-id="9542a-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="9542a-322">eventos.</span><span class="sxs-lookup"><span data-stu-id="9542a-322">Events.</span></span>

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

*  <span data-ttu-id="9542a-323">Operadores unários.</span><span class="sxs-lookup"><span data-stu-id="9542a-323">Unary operators.</span></span>

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

   <span data-ttu-id="9542a-324">O conjunto completo de nomes de função de operador unário usado é o seguinte: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, e `op_False`.</span><span class="sxs-lookup"><span data-stu-id="9542a-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="9542a-325">Operadores binários.</span><span class="sxs-lookup"><span data-stu-id="9542a-325">Binary operators.</span></span>

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

   <span data-ttu-id="9542a-326">O conjunto completo de nomes de função de operador binário usado é o seguinte: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, e `op_GreaterThanOrEqual`.</span><span class="sxs-lookup"><span data-stu-id="9542a-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="9542a-327">Operadores de conversão têm à direita "`~`" seguido pelo tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="9542a-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="9542a-328">Um exemplo</span><span class="sxs-lookup"><span data-stu-id="9542a-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="9542a-329">Código-fonte c#</span><span class="sxs-lookup"><span data-stu-id="9542a-329">C# source code</span></span>

<span data-ttu-id="9542a-330">O exemplo a seguir mostra o código-fonte de um `Point` classe:</span><span class="sxs-lookup"><span data-stu-id="9542a-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="9542a-331">XML resultante</span><span class="sxs-lookup"><span data-stu-id="9542a-331">Resulting XML</span></span>

<span data-ttu-id="9542a-332">Aqui está a saída produzida por um gerador de documentação quando é fornecido o código-fonte para a classe `Point`, conforme mostrado acima:</span><span class="sxs-lookup"><span data-stu-id="9542a-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
