---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912433"
---
# <a name="documentation-comments"></a><span data-ttu-id="6901f-101">Comentários de documentação</span><span class="sxs-lookup"><span data-stu-id="6901f-101">Documentation comments</span></span>

<span data-ttu-id="6901f-102">C#fornece um mecanismo para os programadores documentarem seu código usando uma sintaxe de comentário especial que contém texto XML.</span><span class="sxs-lookup"><span data-stu-id="6901f-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="6901f-103">Em arquivos de código-fonte, os comentários com um determinado formulário podem ser usados para direcionar uma ferramenta para produzir XML a partir desses comentários e dos elementos de código-fonte, que eles precedem.</span><span class="sxs-lookup"><span data-stu-id="6901f-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="6901f-104">Os comentários que usam essa sintaxe são chamados de ***comentários de documentação***.</span><span class="sxs-lookup"><span data-stu-id="6901f-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="6901f-105">Eles devem preceder imediatamente um tipo definido pelo usuário (como uma classe, um delegado ou uma interface) ou um membro (como um campo, evento, propriedade ou método).</span><span class="sxs-lookup"><span data-stu-id="6901f-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="6901f-106">A ferramenta de geração de XML é chamada de ***gerador de documentação***.</span><span class="sxs-lookup"><span data-stu-id="6901f-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="6901f-107">(Esse gerador pode ser, mas não precisa ser, o C# próprio compilador.) A saída produzida pelo gerador de documentação é chamada de ***arquivo de documentação***.</span><span class="sxs-lookup"><span data-stu-id="6901f-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="6901f-108">Um arquivo de documentação é usado como entrada para um ***Visualizador de documentação***; uma ferramenta destinada a produzir algum tipo de exibição visual de informações de tipo e sua documentação associada.</span><span class="sxs-lookup"><span data-stu-id="6901f-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="6901f-109">Essa especificação sugere um conjunto de marcas a serem usadas em comentários de documentação, mas o uso dessas marcas não é necessário e outras marcas podem ser usadas, se desejado, desde que as regras de XML bem formadas sejam seguidas.</span><span class="sxs-lookup"><span data-stu-id="6901f-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="6901f-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="6901f-110">Introduction</span></span>

<span data-ttu-id="6901f-111">Os comentários com um formulário especial podem ser usados para direcionar uma ferramenta para produzir XML a partir desses comentários e dos elementos de código-fonte, que eles precedem.</span><span class="sxs-lookup"><span data-stu-id="6901f-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="6901f-112">Esses comentários são comentários de linha única que começam com três barras (`///`) ou comentários delimitados que começam com uma barra e duas estrelas (`/**`).</span><span class="sxs-lookup"><span data-stu-id="6901f-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="6901f-113">Eles devem preceder imediatamente um tipo definido pelo usuário (como uma classe, um delegado ou uma interface) ou um membro (como um campo, evento, propriedade ou método) que eles anotam.</span><span class="sxs-lookup"><span data-stu-id="6901f-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="6901f-114">As seções de atributo ([especificação de atributo](attributes.md#attribute-specification)) são consideradas parte das declarações; portanto, os comentários de documentação devem preceder os atributos aplicados a um tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="6901f-115">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="6901f-116">Em um *single_line_doc_comment*, se houver um caractere de *espaço* em branco `///` após os caracteres em cada um dos *single_line_doc_comments*adjacentes ao *single_line_doc_comment*atual, issoo caractere de espaço em branco não está incluído na saída XML.</span><span class="sxs-lookup"><span data-stu-id="6901f-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="6901f-117">Em um delimitado-doc-Comment, se o primeiro caractere que não seja espaço em branco na segunda linha for um asterisco e o mesmo padrão de caracteres de espaço em branco opcionais e um caractere de asterisco for repetido no início de cada linha dentro do comentário delimitado-doc-Comment, em seguida, os caracteres do padrão repetido não são incluídos na saída XML.</span><span class="sxs-lookup"><span data-stu-id="6901f-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="6901f-118">O padrão pode incluir caracteres de espaço em branco após, bem como antes, o caractere de asterisco.</span><span class="sxs-lookup"><span data-stu-id="6901f-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="6901f-119">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-119">__Example:__</span></span>

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

<span data-ttu-id="6901f-120">O texto nos comentários da documentação deve estar bem formado de acordo com as regras de https://www.w3.org/TR/REC-xml) XML (.</span><span class="sxs-lookup"><span data-stu-id="6901f-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="6901f-121">Se o XML estiver mal formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que um erro foi encontrado.</span><span class="sxs-lookup"><span data-stu-id="6901f-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="6901f-122">Embora os desenvolvedores estejam livres para criar seu próprio conjunto de marcas, um conjunto recomendado é definido em [marcas recomendadas](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="6901f-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="6901f-123">Algumas das marcas recomendadas têm significado especial:</span><span class="sxs-lookup"><span data-stu-id="6901f-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="6901f-124">A `<param>` marca é usada para descrever os parâmetros.</span><span class="sxs-lookup"><span data-stu-id="6901f-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="6901f-125">Se tal marca for usada, o gerador de documentação deverá verificar se o parâmetro especificado existe e se todos os parâmetros estão descritos nos comentários da documentação.</span><span class="sxs-lookup"><span data-stu-id="6901f-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="6901f-126">Se essa verificação falhar, o gerador de documentação emitirá um aviso.</span><span class="sxs-lookup"><span data-stu-id="6901f-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="6901f-127">O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código.</span><span class="sxs-lookup"><span data-stu-id="6901f-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="6901f-128">O gerador de documentação deve verificar se esse elemento de código existe.</span><span class="sxs-lookup"><span data-stu-id="6901f-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="6901f-129">Se a verificação falhar, o gerador de documentação emitirá um aviso.</span><span class="sxs-lookup"><span data-stu-id="6901f-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="6901f-130">Ao procurar um nome descrito em um `cref` atributo, o gerador de documentação deve respeitar a visibilidade do namespace de acordo com as instruções que `using` aparecem no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="6901f-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="6901f-131">Para elementos de código genéricos, a sintaxe genérica normal (ou seja, "`List<T>`") não pode ser usada porque produz XML inválido.</span><span class="sxs-lookup"><span data-stu-id="6901f-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="6901f-132">Chaves podem ser usadas em vez de colchetes (ou seja, "`List{T}`") ou a sintaxe de escape XML pode ser usada (ou seja, "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="6901f-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="6901f-133">A `<summary>` marca destina-se a ser usada por um visualizador de documentação para exibir informações adicionais sobre um tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="6901f-134">A `<include>` marca inclui informações de um arquivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="6901f-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="6901f-135">Observe atentamente que o arquivo de documentação não fornece informações completas sobre o tipo e os membros (por exemplo, ele não contém nenhuma informação de tipo).</span><span class="sxs-lookup"><span data-stu-id="6901f-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="6901f-136">Para obter informações sobre um tipo ou membro, o arquivo de documentação deve ser usado em conjunto com reflexão no tipo ou membro real.</span><span class="sxs-lookup"><span data-stu-id="6901f-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="6901f-137">Marcas recomendadas</span><span class="sxs-lookup"><span data-stu-id="6901f-137">Recommended tags</span></span>

<span data-ttu-id="6901f-138">O gerador de documentação deve aceitar e processar qualquer marca que seja válida de acordo com as regras de XML.</span><span class="sxs-lookup"><span data-stu-id="6901f-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="6901f-139">As marcas a seguir fornecem funcionalidade comumente usada na documentação do usuário.</span><span class="sxs-lookup"><span data-stu-id="6901f-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="6901f-140">(É claro que outras marcas são possíveis.)</span><span class="sxs-lookup"><span data-stu-id="6901f-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="6901f-141">__Tags__</span><span class="sxs-lookup"><span data-stu-id="6901f-141">__Tag__</span></span>          | <span data-ttu-id="6901f-142">__Section__</span><span class="sxs-lookup"><span data-stu-id="6901f-142">__Section__</span></span>                                            | <span data-ttu-id="6901f-143">__Finalidade__</span><span class="sxs-lookup"><span data-stu-id="6901f-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="6901f-144">Definir texto em uma fonte do tipo código</span><span class="sxs-lookup"><span data-stu-id="6901f-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="6901f-145">Definir uma ou mais linhas do código-fonte ou saída do programa</span><span class="sxs-lookup"><span data-stu-id="6901f-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="6901f-146">Indicar um exemplo</span><span class="sxs-lookup"><span data-stu-id="6901f-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="6901f-147">Identifica as exceções que um método pode gerar</span><span class="sxs-lookup"><span data-stu-id="6901f-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="6901f-148">Inclui XML de um arquivo externo</span><span class="sxs-lookup"><span data-stu-id="6901f-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="6901f-149">Criar uma lista ou tabela</span><span class="sxs-lookup"><span data-stu-id="6901f-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="6901f-150">Permitir que a estrutura seja adicionada ao texto</span><span class="sxs-lookup"><span data-stu-id="6901f-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="6901f-151">Descrever um parâmetro para um método ou Construtor</span><span class="sxs-lookup"><span data-stu-id="6901f-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="6901f-152">Identificar que uma palavra é um nome de parâmetro</span><span class="sxs-lookup"><span data-stu-id="6901f-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="6901f-153">Documentar a acessibilidade de segurança de um membro</span><span class="sxs-lookup"><span data-stu-id="6901f-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="6901f-154">Descrever informações adicionais sobre um tipo</span><span class="sxs-lookup"><span data-stu-id="6901f-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="6901f-155">Descrever o valor de retorno de um método</span><span class="sxs-lookup"><span data-stu-id="6901f-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="6901f-156">Especificar um link</span><span class="sxs-lookup"><span data-stu-id="6901f-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="6901f-157">Gerar uma entrada Consulte também</span><span class="sxs-lookup"><span data-stu-id="6901f-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="6901f-158">Descrever um tipo ou um membro de um tipo</span><span class="sxs-lookup"><span data-stu-id="6901f-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="6901f-159">Descrever uma propriedade</span><span class="sxs-lookup"><span data-stu-id="6901f-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="6901f-160">Descrever um parâmetro de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="6901f-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="6901f-161">Identificar que uma palavra é um nome de parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="6901f-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="6901f-162">Essa marca fornece um mecanismo para indicar que um fragmento de texto dentro de uma descrição deve ser definido em uma fonte especial, como a usada para um bloco de código.</span><span class="sxs-lookup"><span data-stu-id="6901f-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="6901f-163">Para linhas de código real, use `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="6901f-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="6901f-164">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="6901f-165">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="6901f-166">Essa marca é usada para definir uma ou mais linhas de código-fonte ou saída do programa em alguma fonte especial.</span><span class="sxs-lookup"><span data-stu-id="6901f-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="6901f-167">Para fragmentos de código pequenos em narrativas `<c>` ,[`<c>`](documentation-comments.md#c)use ().</span><span class="sxs-lookup"><span data-stu-id="6901f-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="6901f-168">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="6901f-169">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-169">__Example:__</span></span>

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

<span data-ttu-id="6901f-170">Essa marca permite código de exemplo dentro de um comentário, para especificar como um método ou outro membro da biblioteca pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="6901f-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="6901f-171">Normalmente, isso também envolveria o uso da marca `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="6901f-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="6901f-172">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="6901f-173">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-173">__Example:__</span></span>

<span data-ttu-id="6901f-174">Consulte `<code>` [(`<code>`](documentation-comments.md#code)) para obter um exemplo.</span><span class="sxs-lookup"><span data-stu-id="6901f-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="6901f-175">Essa marca fornece uma maneira de documentar as exceções que um método pode gerar.</span><span class="sxs-lookup"><span data-stu-id="6901f-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="6901f-176">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="6901f-177">onde</span><span class="sxs-lookup"><span data-stu-id="6901f-177">where</span></span>

* <span data-ttu-id="6901f-178">`member`é o nome de um membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-178">`member` is the name of a member.</span></span> <span data-ttu-id="6901f-179">O gerador de documentação verifica se o membro fornecido existe e `member` se traduz no nome do elemento canônico no arquivo de documentação.</span><span class="sxs-lookup"><span data-stu-id="6901f-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="6901f-180">`description`é uma descrição das circunstâncias em que a exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="6901f-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="6901f-181">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-181">__Example:__</span></span>

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

<span data-ttu-id="6901f-182">Essa marca permite incluir informações de um documento XML externo ao arquivo de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="6901f-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="6901f-183">O arquivo externo deve ser um documento XML bem formado e uma expressão XPath é aplicada a esse documento para especificar o XML desse documento a ser incluído.</span><span class="sxs-lookup"><span data-stu-id="6901f-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="6901f-184">Em `<include>` seguida, a marca é substituída pelo XML selecionado do documento externo.</span><span class="sxs-lookup"><span data-stu-id="6901f-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="6901f-185">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="6901f-186">onde</span><span class="sxs-lookup"><span data-stu-id="6901f-186">where</span></span>

* <span data-ttu-id="6901f-187">`filename`é o nome de arquivo de um arquivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="6901f-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="6901f-188">O nome do arquivo é interpretado em relação ao arquivo que contém a marca include.</span><span class="sxs-lookup"><span data-stu-id="6901f-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="6901f-189">`xpath`é uma expressão XPath que seleciona parte do XML no arquivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="6901f-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="6901f-190">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-190">__Example:__</span></span>

<span data-ttu-id="6901f-191">Se o código-fonte contiver uma declaração como:</span><span class="sxs-lookup"><span data-stu-id="6901f-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="6901f-192">e o arquivo externo "docs. xml" tinha o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="6901f-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="6901f-193">em seguida, a mesma documentação é saída como se o código-fonte contivesse:</span><span class="sxs-lookup"><span data-stu-id="6901f-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="6901f-194">Essa marca é usada para criar uma lista ou tabela de itens.</span><span class="sxs-lookup"><span data-stu-id="6901f-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="6901f-195">Ele pode conter um `<listheader>` bloco para definir a linha de cabeçalho de uma tabela ou lista de definições.</span><span class="sxs-lookup"><span data-stu-id="6901f-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="6901f-196">(Ao definir uma tabela, apenas uma entrada para `term` no cabeçalho precisa ser fornecida.)</span><span class="sxs-lookup"><span data-stu-id="6901f-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="6901f-197">Cada item na lista é especificado com um `<item>` bloco.</span><span class="sxs-lookup"><span data-stu-id="6901f-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="6901f-198">Ao criar uma lista de definições, `term` e `description` deve ser especificado.</span><span class="sxs-lookup"><span data-stu-id="6901f-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="6901f-199">No entanto, para uma tabela, lista com marcadores ou lista numerada, `description` só é necessário especificar.</span><span class="sxs-lookup"><span data-stu-id="6901f-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="6901f-200">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-200">__Syntax:__</span></span>

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

<span data-ttu-id="6901f-201">onde</span><span class="sxs-lookup"><span data-stu-id="6901f-201">where</span></span>

* <span data-ttu-id="6901f-202">`term`é o termo a ser definido, cuja definição está `description`.</span><span class="sxs-lookup"><span data-stu-id="6901f-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="6901f-203">`description`é um item em uma lista com marcadores ou numerada ou a definição de um `term`.</span><span class="sxs-lookup"><span data-stu-id="6901f-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="6901f-204">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-204">__Example:__</span></span>

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

<span data-ttu-id="6901f-205">Essa marca é para uso dentro de `<summary>` outras marcas, como ([`<remarks>`](documentation-comments.md#remarks)) ou `<returns>` ([`<returns>`](documentation-comments.md#returns)), e permite que a estrutura seja adicionada ao texto.</span><span class="sxs-lookup"><span data-stu-id="6901f-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="6901f-206">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="6901f-207">em `content` que é o texto do parágrafo.</span><span class="sxs-lookup"><span data-stu-id="6901f-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="6901f-208">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-208">__Example:__</span></span>

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

<span data-ttu-id="6901f-209">Essa marca é usada para descrever um parâmetro para um método, Construtor ou indexador.</span><span class="sxs-lookup"><span data-stu-id="6901f-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="6901f-210">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="6901f-211">onde</span><span class="sxs-lookup"><span data-stu-id="6901f-211">where</span></span>

* <span data-ttu-id="6901f-212">`name`é o nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6901f-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="6901f-213">`description`é uma descrição do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6901f-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="6901f-214">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-214">__Example:__</span></span>

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

<span data-ttu-id="6901f-215">Essa marca é usada para indicar que uma palavra é um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6901f-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="6901f-216">O arquivo de documentação pode ser processado para formatar esse parâmetro de uma maneira distinta.</span><span class="sxs-lookup"><span data-stu-id="6901f-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="6901f-217">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="6901f-218">em `name` que é o nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6901f-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="6901f-219">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-219">__Example:__</span></span>

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

<span data-ttu-id="6901f-220">Essa marca permite que a acessibilidade de segurança de um membro seja documentada.</span><span class="sxs-lookup"><span data-stu-id="6901f-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="6901f-221">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="6901f-222">onde</span><span class="sxs-lookup"><span data-stu-id="6901f-222">where</span></span>

* <span data-ttu-id="6901f-223">`member`é o nome de um membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-223">`member` is the name of a member.</span></span> <span data-ttu-id="6901f-224">O gerador de documentação verifica se o elemento de código fornecido existe e traduz o *membro* para o nome do elemento canônico no arquivo de documentação.</span><span class="sxs-lookup"><span data-stu-id="6901f-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="6901f-225">`description`é uma descrição do acesso ao membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="6901f-226">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="6901f-227">Essa marca é usada para especificar informações adicionais sobre um tipo.</span><span class="sxs-lookup"><span data-stu-id="6901f-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="6901f-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para descrever o próprio tipo e os membros de um tipo.)</span><span class="sxs-lookup"><span data-stu-id="6901f-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="6901f-229">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="6901f-230">em `description` que é o texto do comentário.</span><span class="sxs-lookup"><span data-stu-id="6901f-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="6901f-231">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-231">__Example:__</span></span>

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

<span data-ttu-id="6901f-232">Essa marca é usada para descrever o valor de retorno de um método.</span><span class="sxs-lookup"><span data-stu-id="6901f-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="6901f-233">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="6901f-234">em `description` que é uma descrição do valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="6901f-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="6901f-235">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="6901f-236">Essa marca permite que um link seja especificado dentro do texto.</span><span class="sxs-lookup"><span data-stu-id="6901f-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="6901f-237">Use `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) para indicar o texto que deve aparecer em uma seção ver também.</span><span class="sxs-lookup"><span data-stu-id="6901f-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="6901f-238">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="6901f-239">em `member` que é o nome de um membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-239">where `member` is the name of a member.</span></span> <span data-ttu-id="6901f-240">O gerador de documentação verifica se o elemento de código fornecido existe e altera o *membro* para o nome do elemento no arquivo de documentação gerado.</span><span class="sxs-lookup"><span data-stu-id="6901f-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="6901f-241">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-241">__Example:__</span></span>

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

<span data-ttu-id="6901f-242">Essa marca permite que uma entrada seja gerada para a seção Consulte também.</span><span class="sxs-lookup"><span data-stu-id="6901f-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="6901f-243">Use `<see>` [(`<see>`](documentation-comments.md#see)) para especificar um link de dentro do texto.</span><span class="sxs-lookup"><span data-stu-id="6901f-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="6901f-244">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="6901f-245">em `member` que é o nome de um membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-245">where `member` is the name of a member.</span></span> <span data-ttu-id="6901f-246">O gerador de documentação verifica se o elemento de código fornecido existe e altera o *membro* para o nome do elemento no arquivo de documentação gerado.</span><span class="sxs-lookup"><span data-stu-id="6901f-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="6901f-247">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-247">__Example:__</span></span>

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

Essa marca pode ser usada para descrever um tipo ou um membro de um tipo. <span data-ttu-id="6901f-249">Use `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)) para descrever o próprio tipo.</span><span class="sxs-lookup"><span data-stu-id="6901f-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="6901f-250">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="6901f-251">em `description` que é um resumo do tipo ou membro.</span><span class="sxs-lookup"><span data-stu-id="6901f-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="6901f-252">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="6901f-253">Essa marca permite que uma propriedade seja descrita.</span><span class="sxs-lookup"><span data-stu-id="6901f-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="6901f-254">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="6901f-255">em `property description` que é uma descrição para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="6901f-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="6901f-256">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="6901f-257">Essa marca é usada para descrever um parâmetro de tipo genérico para uma classe, struct, interface, delegado ou método.</span><span class="sxs-lookup"><span data-stu-id="6901f-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="6901f-258">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="6901f-259">em `name` que é o nome do parâmetro de tipo e `description` é sua descrição.</span><span class="sxs-lookup"><span data-stu-id="6901f-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="6901f-260">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="6901f-261">Essa marca é usada para indicar que uma palavra é um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="6901f-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="6901f-262">O arquivo de documentação pode ser processado para formatar esse parâmetro de tipo de alguma maneira distinta.</span><span class="sxs-lookup"><span data-stu-id="6901f-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="6901f-263">__Sintaxe__</span><span class="sxs-lookup"><span data-stu-id="6901f-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="6901f-264">em `name` que é o nome do parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="6901f-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="6901f-265">__Exemplo:__</span><span class="sxs-lookup"><span data-stu-id="6901f-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="6901f-266">Processando o arquivo de documentação</span><span class="sxs-lookup"><span data-stu-id="6901f-266">Processing the documentation file</span></span>

<span data-ttu-id="6901f-267">O gerador de documentação gera uma cadeia de caracteres de ID para cada elemento no código-fonte que é marcado com um comentário de documentação.</span><span class="sxs-lookup"><span data-stu-id="6901f-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="6901f-268">Essa cadeia de caracteres de ID identifica exclusivamente um elemento de origem.</span><span class="sxs-lookup"><span data-stu-id="6901f-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="6901f-269">Um visualizador de documentação pode usar uma cadeia de caracteres de ID para identificar o item de metadados/reflexão correspondente ao qual a documentação se aplica.</span><span class="sxs-lookup"><span data-stu-id="6901f-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="6901f-270">O arquivo de documentação não é uma representação hierárquica do código-fonte; em vez disso, é uma lista simples com uma cadeia de caracteres de ID gerada para cada elemento.</span><span class="sxs-lookup"><span data-stu-id="6901f-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="6901f-271">Formato da cadeia de caracteres de ID</span><span class="sxs-lookup"><span data-stu-id="6901f-271">ID string format</span></span>

<span data-ttu-id="6901f-272">O gerador de documentação observa as seguintes regras ao gerar as cadeias de caracteres de ID:</span><span class="sxs-lookup"><span data-stu-id="6901f-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="6901f-273">Nenhum espaço em branco é colocado na cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="6901f-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="6901f-274">A primeira parte da cadeia de caracteres identifica o tipo de membro que está sendo documentado, por meio de um único caractere seguido por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="6901f-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="6901f-275">Os seguintes tipos de membros são definidos:</span><span class="sxs-lookup"><span data-stu-id="6901f-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="6901f-276">__Caractere__</span><span class="sxs-lookup"><span data-stu-id="6901f-276">__Character__</span></span> | <span data-ttu-id="6901f-277">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="6901f-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="6901f-278">E</span><span class="sxs-lookup"><span data-stu-id="6901f-278">E</span></span>             | <span data-ttu-id="6901f-279">evento</span><span class="sxs-lookup"><span data-stu-id="6901f-279">Event</span></span>                                                       |
   | <span data-ttu-id="6901f-280">F</span><span class="sxs-lookup"><span data-stu-id="6901f-280">F</span></span>             | <span data-ttu-id="6901f-281">Campo</span><span class="sxs-lookup"><span data-stu-id="6901f-281">Field</span></span>                                                       |
   | <span data-ttu-id="6901f-282">M</span><span class="sxs-lookup"><span data-stu-id="6901f-282">M</span></span>             | <span data-ttu-id="6901f-283">Método (incluindo construtores, destruidores e operadores)</span><span class="sxs-lookup"><span data-stu-id="6901f-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="6901f-284">N</span><span class="sxs-lookup"><span data-stu-id="6901f-284">N</span></span>             | <span data-ttu-id="6901f-285">Namespace</span><span class="sxs-lookup"><span data-stu-id="6901f-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="6901f-286">P</span><span class="sxs-lookup"><span data-stu-id="6901f-286">P</span></span>             | <span data-ttu-id="6901f-287">Propriedade (incluindo indexadores)</span><span class="sxs-lookup"><span data-stu-id="6901f-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="6901f-288">T</span><span class="sxs-lookup"><span data-stu-id="6901f-288">T</span></span>             | <span data-ttu-id="6901f-289">Tipo (como classe, delegado, enumeração, interface e struct)</span><span class="sxs-lookup"><span data-stu-id="6901f-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="6901f-290">!</span><span class="sxs-lookup"><span data-stu-id="6901f-290">!</span></span>             | <span data-ttu-id="6901f-291">Cadeia de caracteres de erro; o restante da cadeia de caracteres fornece informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="6901f-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="6901f-292">Por exemplo, o gerador de documentação gera informações de erro para links que não podem ser resolvidos.</span><span class="sxs-lookup"><span data-stu-id="6901f-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="6901f-293">A segunda parte da cadeia de caracteres é o nome totalmente qualificado do elemento, começando na raiz do namespace.</span><span class="sxs-lookup"><span data-stu-id="6901f-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="6901f-294">O nome do elemento, seus tipos de delimitador e o namespace são separados por pontos.</span><span class="sxs-lookup"><span data-stu-id="6901f-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="6901f-295">Se o nome do próprio item tiver pontos, eles serão substituídos por `#(U+0023)` caracteres.</span><span class="sxs-lookup"><span data-stu-id="6901f-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="6901f-296">(Supõe-se que nenhum elemento tenha esse caractere em seu nome.)</span><span class="sxs-lookup"><span data-stu-id="6901f-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="6901f-297">Para métodos e propriedades com argumentos, a lista de argumentos segue, entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="6901f-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="6901f-298">Para aqueles sem argumentos, os parênteses são omitidos.</span><span class="sxs-lookup"><span data-stu-id="6901f-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="6901f-299">Os argumentos são separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="6901f-299">The arguments are separated by commas.</span></span> <span data-ttu-id="6901f-300">A codificação de cada argumento é a mesma que uma assinatura da CLI, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6901f-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="6901f-301">Os argumentos são representados pelo nome da documentação, que se baseia em seu nome totalmente qualificado, modificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6901f-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="6901f-302">Os argumentos que representam tipos genéricos têm um caractere `` ` `` anexado (de marca de seleção) seguido pelo número de parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="6901f-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="6901f-303">Os argumentos que `out` têm `ref` o modificador `@` ou têm um nome de tipo a seguir.</span><span class="sxs-lookup"><span data-stu-id="6901f-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="6901f-304">Os argumentos passados por valor ou `params` via não têm notação especial.</span><span class="sxs-lookup"><span data-stu-id="6901f-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="6901f-305">Os argumentos que são matrizes são `[lowerbound:size, ... , lowerbound:size]` representados como onde o número de vírgulas é a classificação menos uma, e os limites inferiores e o tamanho de cada dimensão, se conhecido, são representados em decimal.</span><span class="sxs-lookup"><span data-stu-id="6901f-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="6901f-306">Se um limite inferior ou tamanho não for especificado, ele será omitido.</span><span class="sxs-lookup"><span data-stu-id="6901f-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="6901f-307">Se o limite inferior e o tamanho de uma determinada dimensão forem omitidos `:` , o também será omitido.</span><span class="sxs-lookup"><span data-stu-id="6901f-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="6901f-308">As matrizes denteadas são `[]` representadas por um por nível.</span><span class="sxs-lookup"><span data-stu-id="6901f-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="6901f-309">Os argumentos que têm tipos de ponteiro diferentes de void são representados usando `*` o nome do tipo a seguir.</span><span class="sxs-lookup"><span data-stu-id="6901f-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="6901f-310">Um ponteiro void é representado usando um nome de tipo `System.Void`de.</span><span class="sxs-lookup"><span data-stu-id="6901f-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="6901f-311">Os argumentos que se referem a parâmetros de tipo genérico definidos em tipos `` ` `` são codificados usando o caractere (backtick) seguido pelo índice de base zero do parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="6901f-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="6901f-312">``` `` ``` Os`` ` `` argumentos que usam parâmetros de tipo genérico definidos em métodos usam uma marca de seleção dupla em vez de usados para tipos.</span><span class="sxs-lookup"><span data-stu-id="6901f-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="6901f-313">Os argumentos que se referem a tipos genéricos construídos são codificados usando o tipo `{`genérico, seguido por, seguido por uma lista separada por vírgulas de `}`argumentos de tipo, seguida por.</span><span class="sxs-lookup"><span data-stu-id="6901f-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="6901f-314">Exemplos de cadeia de caracteres de ID</span><span class="sxs-lookup"><span data-stu-id="6901f-314">ID string examples</span></span>

<span data-ttu-id="6901f-315">Os exemplos a seguir mostram um fragmento de C# código, juntamente com a cadeia de caracteres de ID produzida de cada elemento de origem capaz de ter um comentário de documentação:</span><span class="sxs-lookup"><span data-stu-id="6901f-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="6901f-316">Os tipos são representados usando seu nome totalmente qualificado, aumentados com informações genéricas:</span><span class="sxs-lookup"><span data-stu-id="6901f-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="6901f-317">Os campos são representados pelo nome totalmente qualificado:</span><span class="sxs-lookup"><span data-stu-id="6901f-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="6901f-318">Construtores.</span><span class="sxs-lookup"><span data-stu-id="6901f-318">Constructors.</span></span>

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

*  <span data-ttu-id="6901f-319">Destruidores.</span><span class="sxs-lookup"><span data-stu-id="6901f-319">Destructors.</span></span>

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

*  <span data-ttu-id="6901f-320">Maneiras.</span><span class="sxs-lookup"><span data-stu-id="6901f-320">Methods.</span></span>

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

*  <span data-ttu-id="6901f-321">Propriedades e indexadores.</span><span class="sxs-lookup"><span data-stu-id="6901f-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="6901f-322">LostFocus.</span><span class="sxs-lookup"><span data-stu-id="6901f-322">Events.</span></span>

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

*  <span data-ttu-id="6901f-323">Operadores unários.</span><span class="sxs-lookup"><span data-stu-id="6901f-323">Unary operators.</span></span>

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

   <span data-ttu-id="6901f-324">O conjunto completo de nomes de funções de operador unários usado `op_UnaryPlus`é o seguinte: `op_Increment` `op_LogicalNot`, `op_Decrement` `op_UnaryNegation` `op_OnesComplement`, `op_True`,, `op_False`,, e.</span><span class="sxs-lookup"><span data-stu-id="6901f-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="6901f-325">Operadores binários.</span><span class="sxs-lookup"><span data-stu-id="6901f-325">Binary operators.</span></span>

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

   <span data-ttu-id="6901f-326">O conjunto completo de nomes de funções de operador binários usado é `op_Addition`o `op_Subtraction`seguinte `op_Multiply`: `op_Division`, `op_Modulus` `op_LeftShift` `op_ExclusiveOr` `op_BitwiseAnd` `op_BitwiseOr`,,,,, `op_RightShift`,,,, ,,,,`op_Equality`e .`op_GreaterThanOrEqual` `op_Inequality` `op_LessThan` `op_LessThanOrEqual` `op_GreaterThan`</span><span class="sxs-lookup"><span data-stu-id="6901f-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="6901f-327">Os operadores de conversão têm um "`~`" à direita seguido pelo tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="6901f-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="6901f-328">Um exemplo</span><span class="sxs-lookup"><span data-stu-id="6901f-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="6901f-329">C#código-fonte</span><span class="sxs-lookup"><span data-stu-id="6901f-329">C# source code</span></span>

<span data-ttu-id="6901f-330">O exemplo a seguir mostra o código-fonte `Point` de uma classe:</span><span class="sxs-lookup"><span data-stu-id="6901f-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="6901f-331">XML resultante</span><span class="sxs-lookup"><span data-stu-id="6901f-331">Resulting XML</span></span>

<span data-ttu-id="6901f-332">Aqui está a saída produzida por um gerador de documentação quando é fornecido o código- `Point`fonte para a classe, mostrado acima:</span><span class="sxs-lookup"><span data-stu-id="6901f-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
