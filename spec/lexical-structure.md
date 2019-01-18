---
ms.openlocfilehash: f797692cbd6aeb6035aa7c8ed3139740466c6e42
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229504"
---
# <a name="lexical-structure"></a><span data-ttu-id="3411e-101">Estrutura lexical</span><span class="sxs-lookup"><span data-stu-id="3411e-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="3411e-102">Programas</span><span class="sxs-lookup"><span data-stu-id="3411e-102">Programs</span></span>

<span data-ttu-id="3411e-103">C# ***programa*** consiste em um ou mais ***arquivos de origem***, conhecido anteriormente como ***unidades de compilação*** ([unidades de compilação](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="3411e-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="3411e-104">Um arquivo de origem é uma sequência ordenada de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="3411e-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="3411e-105">Arquivos de origem normalmente têm uma correspondência com arquivos em um sistema de arquivos, mas essa correspondência não é necessária.</span><span class="sxs-lookup"><span data-stu-id="3411e-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="3411e-106">Para fins de portabilidade máxima, recomenda-se que os arquivos em um sistema de arquivos ser codificado com UTF-8 codificação.</span><span class="sxs-lookup"><span data-stu-id="3411e-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="3411e-107">Conceitualmente, um programa é compilado em três etapas:</span><span class="sxs-lookup"><span data-stu-id="3411e-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="3411e-108">Transformação, que converte um arquivo de um esquema de codifica e o repertório de caracteres específica em uma sequência de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="3411e-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="3411e-109">Análise lexical, que converte um fluxo de caracteres de entrada de Unicode em um fluxo de tokens.</span><span class="sxs-lookup"><span data-stu-id="3411e-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="3411e-110">Análise sintática, que converte o fluxo de tokens em código executável.</span><span class="sxs-lookup"><span data-stu-id="3411e-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="3411e-111">Gramáticas</span><span class="sxs-lookup"><span data-stu-id="3411e-111">Grammars</span></span>

<span data-ttu-id="3411e-112">Essa especificação apresenta a sintaxe da linguagem c# linguagem de programação usando duas gramáticas.</span><span class="sxs-lookup"><span data-stu-id="3411e-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="3411e-113">O ***gramática lexical*** ([gramática Lexical](lexical-structure.md#lexical-grammar)) define como os caracteres Unicode são combinadas para terminadores de linha de formulário, o espaço em branco, comentários, tokens e diretivas de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="3411e-114">O ***gramática sintática*** ([gramática sintática](lexical-structure.md#syntactic-grammar)) define como os tokens resultantes da gramática lexical são combinados para programas de formulário em C#.</span><span class="sxs-lookup"><span data-stu-id="3411e-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="3411e-115">Notação de gramática</span><span class="sxs-lookup"><span data-stu-id="3411e-115">Grammar notation</span></span>

<span data-ttu-id="3411e-116">As gramáticas lexicais e sintáticas são apresentadas na forma de Backus-Naur usando a notação da ferramenta ANTLR gramática.</span><span class="sxs-lookup"><span data-stu-id="3411e-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="3411e-117">Gramática lexical</span><span class="sxs-lookup"><span data-stu-id="3411e-117">Lexical grammar</span></span>

<span data-ttu-id="3411e-118">A gramática lexical da linguagem c# é apresentada na [análise léxica](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), e [diretivas de pré-processamento](lexical-structure.md#pre-processing-directives).</span><span class="sxs-lookup"><span data-stu-id="3411e-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="3411e-119">Os símbolos terminais da gramática lexical são os caracteres do conjunto de caracteres Unicode e a gramática lexical Especifica como os caracteres são combinadas para tokens de formulário ([Tokens](lexical-structure.md#tokens)), espaço em branco ([espaço em branco](lexical-structure.md#white-space)), comentários ([comentários](lexical-structure.md#comments)) e em diretivas de pré-processamento ([diretivas de pré-processamento](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="3411e-120">Cada arquivo de origem em um programa c# deve estar de acordo com o *entrada* produção da gramática lexical ([análise léxica](lexical-structure.md#lexical-analysis)).</span><span class="sxs-lookup"><span data-stu-id="3411e-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="3411e-121">Gramática sintática</span><span class="sxs-lookup"><span data-stu-id="3411e-121">Syntactic grammar</span></span>

<span data-ttu-id="3411e-122">A gramática sintática do c# é apresentada em capítulos e apêndices seguir este capítulo.</span><span class="sxs-lookup"><span data-stu-id="3411e-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="3411e-123">Os símbolos de terminal da gramática sintático são os tokens definidos pela gramática lexical e sintática gramática Especifica como os tokens são combinados para programas em c# formulário.</span><span class="sxs-lookup"><span data-stu-id="3411e-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="3411e-124">Cada arquivo de origem em um C# programa deve estar em conformidade com a *compilation_unit* produção da gramática sintática ([unidades de compilação](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="3411e-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="3411e-125">Análise léxica</span><span class="sxs-lookup"><span data-stu-id="3411e-125">Lexical analysis</span></span>

<span data-ttu-id="3411e-126">O *entrada* produção define a estrutura lexical de um arquivo de origem c#.</span><span class="sxs-lookup"><span data-stu-id="3411e-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="3411e-127">Cada arquivo de origem em um programa c# deve estar de acordo com essa produção gramática lexical.</span><span class="sxs-lookup"><span data-stu-id="3411e-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

<span data-ttu-id="3411e-128">Cinco elementos básicos compõem a estrutura lexical de um C# arquivo de origem: Terminadores de linha ([terminadores de linha](lexical-structure.md#line-terminators)), espaço em branco ([espaço em branco](lexical-structure.md#white-space)), comentários ([comentários](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), e diretivas de pré-processamento ([diretivas de pré-processamento](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="3411e-129">Esses elementos básicos, somente os tokens são significativos na gramática sintática de um programa em c# ([gramática sintática](lexical-structure.md#syntactic-grammar)).</span><span class="sxs-lookup"><span data-stu-id="3411e-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="3411e-130">O processamento léxico de um arquivo de origem c# consiste em reduzir o arquivo em uma sequência de tokens que se torna a entrada para a análise sintática.</span><span class="sxs-lookup"><span data-stu-id="3411e-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="3411e-131">Terminadores de linha, espaço em branco e comentários podem ser usado para separar os tokens e diretivas de pré-processamento podem causar a seções do arquivo de origem a serem ignoradas, mas caso contrário, esses elementos léxicos não têm impacto sobre a estrutura sintática de um programa c#.</span><span class="sxs-lookup"><span data-stu-id="3411e-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="3411e-132">No caso de literais de cadeia de caracteres interpolada ([interpoladas literais de cadeia de caracteres](lexical-structure.md#interpolated-string-literals)) um único token inicialmente é produzido pela análise lexical, mas é dividido em vários elementos de entrada que repetidamente estão sujeitos a análise léxica até que todos os literais de cadeia de caracteres interpolada foram resolvidas.</span><span class="sxs-lookup"><span data-stu-id="3411e-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="3411e-133">Os tokens resultantes, em seguida, servem como entrada para a análise sintática.</span><span class="sxs-lookup"><span data-stu-id="3411e-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="3411e-134">Quando várias produções gramática lexical corresponderem a uma sequência de caracteres em um arquivo de origem, o processamento lexical sempre forma o elemento de léxico mais longo possível.</span><span class="sxs-lookup"><span data-stu-id="3411e-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="3411e-135">Por exemplo, a sequência de caracteres `//` é processado como o início de um comentário de linha única, porque esse elemento lexical tem mais de um único `/` token.</span><span class="sxs-lookup"><span data-stu-id="3411e-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="3411e-136">Terminadores de linha</span><span class="sxs-lookup"><span data-stu-id="3411e-136">Line terminators</span></span>

<span data-ttu-id="3411e-137">Terminadores de linha dividem os caracteres de um arquivo de código-fonte c# em linhas.</span><span class="sxs-lookup"><span data-stu-id="3411e-137">Line terminators divide the characters of a C# source file into lines.</span></span>

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

<span data-ttu-id="3411e-138">Para ferramentas de edição que adicionem marcadores de fim-de-arquivo de código de compatibilidade com o código-fonte, e para habilitar uma fonte de arquivo para ser exibido como uma sequência de corretamente encerrada linhas, as transformações a seguir são aplicadas em ordem, para cada arquivo de origem em um programa c#:</span><span class="sxs-lookup"><span data-stu-id="3411e-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="3411e-139">Se o último caractere do arquivo de origem for um caractere de controle-Z (`U+001A`), esse caractere é excluído.</span><span class="sxs-lookup"><span data-stu-id="3411e-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="3411e-140">Um caractere de retorno de carro (`U+000D`) é adicionado ao final do arquivo de origem se esse arquivo de origem estiver vazio e se o último caractere do arquivo de origem não é um retorno de carro (`U+000D`), uma alimentação de linha (`U+000A`), um separador de linha (`U+2028`), ou um separador de parágrafo (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="3411e-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="3411e-141">Comentários</span><span class="sxs-lookup"><span data-stu-id="3411e-141">Comments</span></span>

<span data-ttu-id="3411e-142">Duas formas de comentários são suportadas: comentários de linha única e comentários delimitados.</span><span class="sxs-lookup"><span data-stu-id="3411e-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="3411e-143">***Comentários de linha única*** começar com os caracteres `//` e estender ao final da linha de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="3411e-144">***Delimitado por comentários*** começar com os caracteres `/*` e terminam com os caracteres `*/`.</span><span class="sxs-lookup"><span data-stu-id="3411e-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="3411e-145">Comentários delimitados podem abranger várias linhas.</span><span class="sxs-lookup"><span data-stu-id="3411e-145">Delimited comments may span multiple lines.</span></span>

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk* '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

<span data-ttu-id="3411e-146">Não se aninham comentários.</span><span class="sxs-lookup"><span data-stu-id="3411e-146">Comments do not nest.</span></span> <span data-ttu-id="3411e-147">As sequências de caracteres `/*` e `*/` não têm significado especial em um `//` comentário e as sequências de caracteres `//` e `/*` não têm significado especial dentro de um comentário delimitado.</span><span class="sxs-lookup"><span data-stu-id="3411e-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="3411e-148">Comentários não são processados dentro de cadeia de caracteres literais.</span><span class="sxs-lookup"><span data-stu-id="3411e-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="3411e-149">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3411e-149">The example</span></span>
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="3411e-150">inclui um comentário delimitado.</span><span class="sxs-lookup"><span data-stu-id="3411e-150">includes a delimited comment.</span></span>

<span data-ttu-id="3411e-151">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3411e-151">The example</span></span>
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="3411e-152">mostra vários comentários de linha única.</span><span class="sxs-lookup"><span data-stu-id="3411e-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="3411e-153">Espaço em branco</span><span class="sxs-lookup"><span data-stu-id="3411e-153">White space</span></span>

<span data-ttu-id="3411e-154">Espaço em branco é definido como o caractere de alimentação de qualquer caractere com Unicode classe Zs (que inclui o caractere de espaço), bem como o caractere de tabulação horizontal, o caractere de tabulação vertical e o formulário.</span><span class="sxs-lookup"><span data-stu-id="3411e-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="3411e-155">Tokens</span><span class="sxs-lookup"><span data-stu-id="3411e-155">Tokens</span></span>

<span data-ttu-id="3411e-156">Há vários tipos de tokens: identificadores, palavras-chave, literais, operadores e pontuadores.</span><span class="sxs-lookup"><span data-stu-id="3411e-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="3411e-157">Espaço em branco e os comentários não são tokens, embora eles atuam como separadores para tokens.</span><span class="sxs-lookup"><span data-stu-id="3411e-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="3411e-158">Sequências de escape de caractere Unicode</span><span class="sxs-lookup"><span data-stu-id="3411e-158">Unicode character escape sequences</span></span>

<span data-ttu-id="3411e-159">Uma sequência de escape de caractere Unicode representa um caractere Unicode.</span><span class="sxs-lookup"><span data-stu-id="3411e-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="3411e-160">Sequências de escape de caractere Unicode são processadas em identificadores ([identificadores](lexical-structure.md#identifiers)), literais de caracteres ([literais de caracteres](lexical-structure.md#character-literals)) e literais de cadeia de caracteres regulares ([deliteraisdecadeiadecaracteres](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="3411e-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="3411e-161">Um caractere de escape Unicode não é processado em qualquer outro local (por exemplo, para formar um operador, punctuator ou palavra-chave).</span><span class="sxs-lookup"><span data-stu-id="3411e-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="3411e-162">Uma sequência de escape Unicode representa o caractere Unicode formado pelos seguintes número hexadecimais de "`\u`"ou"`\U`" caracteres.</span><span class="sxs-lookup"><span data-stu-id="3411e-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="3411e-163">Como o c# usa a codificação de 16 bits dos pontos de código Unicode em caracteres e valores de cadeia de caracteres, um caractere Unicode no intervalo de u+10000 a U + 10FFFF não é permitido em um literal de caractere e é representado usando um par de substitutos de Unicode em uma cadeia de caracteres literal.</span><span class="sxs-lookup"><span data-stu-id="3411e-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="3411e-164">Não há suporte para caracteres Unicode com pontos de código acima 0x10FFFF.</span><span class="sxs-lookup"><span data-stu-id="3411e-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="3411e-165">Não são realizadas várias traduções.</span><span class="sxs-lookup"><span data-stu-id="3411e-165">Multiple translations are not performed.</span></span> <span data-ttu-id="3411e-166">Por exemplo, a cadeia de caracteres literal "`\u005Cu005C`"é equivalente a"`\u005C`"em vez de"`\`".</span><span class="sxs-lookup"><span data-stu-id="3411e-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="3411e-167">O valor Unicode `\u005C` é o caractere "`\`".</span><span class="sxs-lookup"><span data-stu-id="3411e-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="3411e-168">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3411e-168">The example</span></span>
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
<span data-ttu-id="3411e-169">mostra vários usos de `\u0066`, que é a sequência de escape para a letra "`f`".</span><span class="sxs-lookup"><span data-stu-id="3411e-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="3411e-170">O programa é equivalente a</span><span class="sxs-lookup"><span data-stu-id="3411e-170">The program is equivalent to</span></span>
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a><span data-ttu-id="3411e-171">Identificadores</span><span class="sxs-lookup"><span data-stu-id="3411e-171">Identifiers</span></span>

<span data-ttu-id="3411e-172">As regras para identificadores fornecidas nesta seção correspondem exatamente ao recomendada pelo Unicode Standard Annex 31, exceto que o sublinhado é permitido como um caractere inicial (por exemplo, é tradicional na linguagem de programação C), são sequências de escape de Unicode permitido em identificadores e o "`@`" caractere é permitido como um prefixo para habilitar palavras-chave para ser usadas como identificadores.</span><span class="sxs-lookup"><span data-stu-id="3411e-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

<span data-ttu-id="3411e-173">Para obter informações sobre as classes de caractere Unicode mencionados acima, consulte o padrão Unicode, versão 3.0, seção 4.5.</span><span class="sxs-lookup"><span data-stu-id="3411e-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="3411e-174">Exemplos de identificadores válidos incluem "`identifier1`","`_identifier2`", e "`@if`".</span><span class="sxs-lookup"><span data-stu-id="3411e-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="3411e-175">Um identificador em um programa com conformidade deve ser no formato canônico definido pelo formulário de normalização Unicode C, conforme definido pelo Unicode Standard Annex 15.</span><span class="sxs-lookup"><span data-stu-id="3411e-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="3411e-176">O comportamento quando se deparar com um identificador não está no formulário C de normalização é definido pela implementação; No entanto, um diagnóstico não é necessário.</span><span class="sxs-lookup"><span data-stu-id="3411e-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="3411e-177">O prefixo "`@`" permite o uso de palavras-chave como identificadores, que é útil ao fazer interface com outras linguagens de programação.</span><span class="sxs-lookup"><span data-stu-id="3411e-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="3411e-178">O caractere `@` não é parte do identificador, portanto, o identificador pode ser visto em outros idiomas como um identificador normal, sem o prefixo.</span><span class="sxs-lookup"><span data-stu-id="3411e-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="3411e-179">Um identificador com um `@` prefixo é chamado um ***identificador textual***.</span><span class="sxs-lookup"><span data-stu-id="3411e-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="3411e-180">Usar o `@` prefixo para os identificadores que não são palavras-chave é permitido, mas altamente recomendado como uma questão de estilo.</span><span class="sxs-lookup"><span data-stu-id="3411e-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="3411e-181">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="3411e-181">The example:</span></span>
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
<span data-ttu-id="3411e-182">define uma classe chamada "`class`"com um método estático denominado"`static`"que leva um parâmetro chamado"`bool`".</span><span class="sxs-lookup"><span data-stu-id="3411e-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="3411e-183">Observe que uma vez que faz o escape Unicode não são permitidos em palavras-chave, o token "`cl\u0061ss`"é um identificador, e é o mesmo identificador que"`@class`".</span><span class="sxs-lookup"><span data-stu-id="3411e-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="3411e-184">Dois identificadores são considerados iguais se eles forem idênticos, depois que as transformações a seguir são aplicadas na ordem:</span><span class="sxs-lookup"><span data-stu-id="3411e-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="3411e-185">O prefixo "`@`", se usado, é removido.</span><span class="sxs-lookup"><span data-stu-id="3411e-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="3411e-186">Cada *unicode_escape_sequence* é transformado em seu caractere Unicode correspondente.</span><span class="sxs-lookup"><span data-stu-id="3411e-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="3411e-187">Qualquer *formatting_character*s são removidos.</span><span class="sxs-lookup"><span data-stu-id="3411e-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="3411e-188">Identificadores que contém dois consecutivas caracteres de sublinhado (`U+005F`) são reservados para uso pela implementação.</span><span class="sxs-lookup"><span data-stu-id="3411e-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="3411e-189">Por exemplo, uma implementação pode fornecer palavras-chave estendidas que começam com dois sublinhados.</span><span class="sxs-lookup"><span data-stu-id="3411e-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="3411e-190">Palavras-chave</span><span class="sxs-lookup"><span data-stu-id="3411e-190">Keywords</span></span>

<span data-ttu-id="3411e-191">Um ***palavra-chave*** é uma sequência de tipo de identificador de caracteres que é reservado e não pode ser usada como um identificador, exceto quando precedida pela `@` caracteres.</span><span class="sxs-lookup"><span data-stu-id="3411e-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

<span data-ttu-id="3411e-192">Em alguns locais na gramática, identificadores específicos têm significado especial, mas não são palavras-chave.</span><span class="sxs-lookup"><span data-stu-id="3411e-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="3411e-193">Esses identificadores às vezes são chamados de "palavras-chave contextuais".</span><span class="sxs-lookup"><span data-stu-id="3411e-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="3411e-194">Por exemplo, dentro de uma declaração de propriedade, o "`get`"e"`set`" identificadores têm significado especial ([acessadores](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="3411e-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="3411e-195">Um identificador diferente de `get` ou `set` nunca é permitida nesses locais, portanto, esse uso não está em conflito com um uso destas palavras como identificadores.</span><span class="sxs-lookup"><span data-stu-id="3411e-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="3411e-196">Em outros casos, assim como acontece com o identificador "`var`" nas declarações de variável locais digitadas implicitamente ([declarações de variável Local](statements.md#local-variable-declarations)), uma palavra-chave contextual pode entrar em conflito com nomes declarados.</span><span class="sxs-lookup"><span data-stu-id="3411e-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="3411e-197">Nesses casos, o nome declarado prevalece sobre o uso do identificador como uma palavra-chave contextual.</span><span class="sxs-lookup"><span data-stu-id="3411e-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="3411e-198">Literais</span><span class="sxs-lookup"><span data-stu-id="3411e-198">Literals</span></span>

<span data-ttu-id="3411e-199">Um ***literal*** é uma representação de código de origem de um valor.</span><span class="sxs-lookup"><span data-stu-id="3411e-199">A ***literal*** is a source code representation of a value.</span></span>

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a><span data-ttu-id="3411e-200">Literais boolianos</span><span class="sxs-lookup"><span data-stu-id="3411e-200">Boolean literals</span></span>

<span data-ttu-id="3411e-201">Há dois valores literais boolianos: `true` e `false`.</span><span class="sxs-lookup"><span data-stu-id="3411e-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="3411e-202">O tipo de um *boolean_literal* é `bool`.</span><span class="sxs-lookup"><span data-stu-id="3411e-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="3411e-203">Literais inteiros</span><span class="sxs-lookup"><span data-stu-id="3411e-203">Integer literals</span></span>

<span data-ttu-id="3411e-204">Literais inteiros são usados para gravar valores de tipos `int`, `uint`, `long`, e `ulong`.</span><span class="sxs-lookup"><span data-stu-id="3411e-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="3411e-205">Literais inteiros tem duas formas possíveis: decimal e hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="3411e-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

<span data-ttu-id="3411e-206">O tipo de um literal inteiro é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3411e-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="3411e-207">Se o literal não tem sufixo, ele tem o primeiro desses tipos em que seu valor pode ser representado: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="3411e-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="3411e-208">Se o literal é um sufixo formado por `U` ou `u`, ele tem o primeiro desses tipos em que seu valor pode ser representado: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="3411e-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="3411e-209">Se o literal é um sufixo formado por `L` ou `l`, ele tem o primeiro desses tipos em que seu valor pode ser representado: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="3411e-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="3411e-210">Se o literal é um sufixo formado por `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, ou `lu`, ela é do tipo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="3411e-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="3411e-211">Se o valor representado por um literal inteiro estiver fora do intervalo da `ulong` digitar, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3411e-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="3411e-212">Como uma questão de estilo, é aconselhável que "`L`"ser usada em vez de"`l`" ao escrever literais do tipo `long`, uma vez que ele é fácil confundir a letra "`l`"com o dígito"`1`".</span><span class="sxs-lookup"><span data-stu-id="3411e-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="3411e-213">Para permitir que o menor valor possível `int` e `long` valores a serem gravados como literais inteiro decimal, existem duas regras a seguir:</span><span class="sxs-lookup"><span data-stu-id="3411e-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="3411e-214">Quando um *decimal_integer_literal* com o valor 2147483648 (2 ^ 31) e não há *integer_type_suffix* aparece como o token imediatamente após um menos o token de operador unário ([menos unário operador](expressions.md#unary-minus-operator)), o resultado é uma constante do tipo `int` com o valor de -2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="3411e-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="3411e-215">Em outras situações, tal uma *decimal_integer_literal* é do tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="3411e-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="3411e-216">Quando um *decimal_integer_literal* com o valor 9223372036854775808 (2 ^ 63) e nenhuma *integer_type_suffix* ou o *integer_type_suffix* `L` ou `l` aparece como o token imediatamente após um menos o token de operador unário ([operador de subtração unário](expressions.md#unary-minus-operator)), o resultado é uma constante do tipo `long` com o valor de -9223372036854775808 (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="3411e-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="3411e-217">Em outras situações, tal uma *decimal_integer_literal* é do tipo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="3411e-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="3411e-218">Literais reais</span><span class="sxs-lookup"><span data-stu-id="3411e-218">Real literals</span></span>

<span data-ttu-id="3411e-219">Literais reais são usados para gravar valores de tipos `float`, `double`, e `decimal`.</span><span class="sxs-lookup"><span data-stu-id="3411e-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

<span data-ttu-id="3411e-220">Se nenhum *real_type_suffix* for especificado, o tipo do literal real é `double`.</span><span class="sxs-lookup"><span data-stu-id="3411e-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="3411e-221">Caso contrário, o sufixo de tipo real determina o tipo do literal real, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3411e-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="3411e-222">Um literal real tendo como sufixado `F` ou `f` é do tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="3411e-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="3411e-223">Por exemplo, os literais `1f`, `1.5f`, `1e10f`, e `123.456F` são todos do tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="3411e-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="3411e-224">Um literal real tendo como sufixado `D` ou `d` é do tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="3411e-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="3411e-225">Por exemplo, os literais `1d`, `1.5d`, `1e10d`, e `123.456D` são todos do tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="3411e-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="3411e-226">Um literal real tendo como sufixado `M` ou `m` é do tipo `decimal`.</span><span class="sxs-lookup"><span data-stu-id="3411e-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="3411e-227">Por exemplo, os literais `1m`, `1.5m`, `1e10m`, e `123.456M` são todos do tipo `decimal`.</span><span class="sxs-lookup"><span data-stu-id="3411e-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="3411e-228">Este literal é convertido em um `decimal` valor, considerando o valor exato e, se necessário, arredondando para o próximo valor representável usando arredondamento bancário ([o tipo decimal](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="3411e-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="3411e-229">Qualquer escala aparente no literal é preservada, a menos que o valor é arredondado ou o valor é zero (no último caso, a entrada e a escala será 0).</span><span class="sxs-lookup"><span data-stu-id="3411e-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="3411e-230">Portanto, o literal `2.900m` será analisado para formar o decimal com sinal `0`, coeficiente `2900`e a escala `3`.</span><span class="sxs-lookup"><span data-stu-id="3411e-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="3411e-231">Se o literal especificado não pode ser representado no tipo indicado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3411e-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="3411e-232">O valor de um literal real do tipo `float` ou `double` é determinado usando o padrão IEEE modo "arredondamento mais próximo".</span><span class="sxs-lookup"><span data-stu-id="3411e-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="3411e-233">Observe que um literal real, os dígitos decimais sempre são necessários após a vírgula decimal.</span><span class="sxs-lookup"><span data-stu-id="3411e-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="3411e-234">Por exemplo, `1.3F` é um literal real mas `1.F` não é.</span><span class="sxs-lookup"><span data-stu-id="3411e-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="3411e-235">Literais de caracteres</span><span class="sxs-lookup"><span data-stu-id="3411e-235">Character literals</span></span>

<span data-ttu-id="3411e-236">Um literal de caractere representa um único caractere e geralmente consiste em um caractere entre aspas, como em `'a'`.</span><span class="sxs-lookup"><span data-stu-id="3411e-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="3411e-237">Observação: A notação de gramática ANTLR faz o seguinte confuso!</span><span class="sxs-lookup"><span data-stu-id="3411e-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="3411e-238">No ANTLR, quando você escreve `\'` ela significa uma aspa simples `'`.</span><span class="sxs-lookup"><span data-stu-id="3411e-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="3411e-239">E quando você escreve `\\` significa de uma única barra invertida `\`.</span><span class="sxs-lookup"><span data-stu-id="3411e-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="3411e-240">Portanto a primeira regra de um caractere literal significa que ele começa com uma aspa, seguido de um caractere e uma aspa simples.</span><span class="sxs-lookup"><span data-stu-id="3411e-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="3411e-241">E são sequências de escape simples possíveis onze `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span><span class="sxs-lookup"><span data-stu-id="3411e-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

<span data-ttu-id="3411e-242">Um caractere que segue um caractere de barra invertida (`\`) em um *caractere* deve ser um dos seguintes caracteres: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="3411e-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="3411e-243">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3411e-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="3411e-244">Uma sequência de escape hexadecimal representa um único caractere Unicode, com o valor formado pelos seguintes número hexadecimais "`\x`".</span><span class="sxs-lookup"><span data-stu-id="3411e-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="3411e-245">Se o valor representado por um literal de caractere é maior que `U+FFFF`, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3411e-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="3411e-246">Uma sequência de escape de caractere Unicode ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)) em um caractere literal deve estar no intervalo `U+0000` para `U+FFFF`.</span><span class="sxs-lookup"><span data-stu-id="3411e-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="3411e-247">Uma sequência de escape simples representa uma codificação de caracteres de Unicode, conforme descrito na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="3411e-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="3411e-248">__Sequência de escape__</span><span class="sxs-lookup"><span data-stu-id="3411e-248">__Escape sequence__</span></span> | <span data-ttu-id="3411e-249">__Nome de caractere__</span><span class="sxs-lookup"><span data-stu-id="3411e-249">__Character name__</span></span> | <span data-ttu-id="3411e-250">__Codificação Unicode__</span><span class="sxs-lookup"><span data-stu-id="3411e-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="3411e-251">Aspas simples</span><span class="sxs-lookup"><span data-stu-id="3411e-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="3411e-252">Aspas duplas</span><span class="sxs-lookup"><span data-stu-id="3411e-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="3411e-253">Barra invertida</span><span class="sxs-lookup"><span data-stu-id="3411e-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="3411e-254">Nulo</span><span class="sxs-lookup"><span data-stu-id="3411e-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="3411e-255">Alerta</span><span class="sxs-lookup"><span data-stu-id="3411e-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="3411e-256">Backspace</span><span class="sxs-lookup"><span data-stu-id="3411e-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="3411e-257">Avanço de página</span><span class="sxs-lookup"><span data-stu-id="3411e-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="3411e-258">Nova linha</span><span class="sxs-lookup"><span data-stu-id="3411e-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="3411e-259">Retorno de carro</span><span class="sxs-lookup"><span data-stu-id="3411e-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="3411e-260">Tabulação horizontal</span><span class="sxs-lookup"><span data-stu-id="3411e-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="3411e-261">Tabulação vertical</span><span class="sxs-lookup"><span data-stu-id="3411e-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="3411e-262">O tipo de um *character_literal* é `char`.</span><span class="sxs-lookup"><span data-stu-id="3411e-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="3411e-263">Literais de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="3411e-263">String literals</span></span>

<span data-ttu-id="3411e-264">C# oferece suporte a dois formatos de literais de cadeia de caracteres: ***literais de cadeia de caracteres regulares*** e ***literais de cadeia de caracteres textuais***.</span><span class="sxs-lookup"><span data-stu-id="3411e-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="3411e-265">Uma cadeia de caracteres regular literal consiste em zero ou mais caracteres entre aspas duplas, como em `"hello"`e pode incluir ambas as sequências de escape simples (como `\t` para o caractere de tabulação) e hexadecimal e sequências de escape Unicode.</span><span class="sxs-lookup"><span data-stu-id="3411e-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="3411e-266">Uma cadeia de caracteres textual literal consiste em um `@` caractere seguido por um caractere de aspas duplas, zero ou mais caracteres e um caractere de aspas duplas de fechamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="3411e-267">Um exemplo simples é `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="3411e-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="3411e-268">Em uma cadeia de caracteres textual literal, os caracteres entre os delimitadores são interpretados literalmente, a única exceção é um *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="3411e-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="3411e-269">Em particular, sequências de escape simples e hexadecimal e sequências de escape Unicode não são processadas em literais de cadeia de caracteres textuais.</span><span class="sxs-lookup"><span data-stu-id="3411e-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="3411e-270">Uma cadeia de caracteres textual literal pode abranger várias linhas.</span><span class="sxs-lookup"><span data-stu-id="3411e-270">A verbatim string literal may span multiple lines.</span></span>

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

<span data-ttu-id="3411e-271">Um caractere que segue um caractere de barra invertida (`\`) em um *regular_string_literal_character* deve ser um dos seguintes caracteres: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="3411e-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="3411e-272">Caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3411e-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="3411e-273">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3411e-273">The example</span></span>
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
<span data-ttu-id="3411e-274">mostra uma variedade de literais de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3411e-274">shows a variety of string literals.</span></span> <span data-ttu-id="3411e-275">A última cadeia de caracteres literal, `j`, é uma cadeia de caracteres textual literal que abrange várias linhas.</span><span class="sxs-lookup"><span data-stu-id="3411e-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="3411e-276">Os caracteres entre aspas, incluindo espaço em branco, como caracteres de nova linha são preservados textuais.</span><span class="sxs-lookup"><span data-stu-id="3411e-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="3411e-277">Como uma sequência de escape hexadecimal pode ter um número variável de dígitos hexadecimais, a cadeia de caracteres literal `"\x123"` contém um único caractere com um valor hexadecimal 123.</span><span class="sxs-lookup"><span data-stu-id="3411e-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="3411e-278">Para criar uma cadeia de caracteres que contém o caractere com o valor hexadecimal 12 seguido pelo caractere 3, um poderia escrever `"\x00123"` ou `"\x12" + "3"` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="3411e-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="3411e-279">O tipo de um *string_literal* é `string`.</span><span class="sxs-lookup"><span data-stu-id="3411e-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="3411e-280">Cada cadeia de caracteres literal não resulta necessariamente em uma nova instância de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3411e-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="3411e-281">Quando dois ou mais literais de cadeia de caracteres que são equivalentes de acordo com o operador de igualdade de cadeia de caracteres ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)) são exibidos no mesmo programa, esses literais de cadeia de caracteres se referem à mesma instância de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3411e-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="3411e-282">Por exemplo, a saída é produzido por</span><span class="sxs-lookup"><span data-stu-id="3411e-282">For instance, the output produced by</span></span>
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
<span data-ttu-id="3411e-283">é `True` porque os dois literais se referem à mesma instância de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3411e-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="3411e-284">Literais de cadeia de caracteres interpolada</span><span class="sxs-lookup"><span data-stu-id="3411e-284">Interpolated string literals</span></span>

<span data-ttu-id="3411e-285">Literais de cadeia de caracteres interpolada são semelhantes às literais de cadeia de caracteres, mas conter orifícios delimitados por `{` e `}`, no qual as expressões podem ocorrer.</span><span class="sxs-lookup"><span data-stu-id="3411e-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="3411e-286">Em tempo de execução, as expressões são avaliadas com a finalidade de ter suas formas textuais substituídas na cadeia de caracteres no local onde ocorre a falha.</span><span class="sxs-lookup"><span data-stu-id="3411e-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="3411e-287">A sintaxe e semântica de interpolação de cadeia de caracteres é descrita na seção ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="3411e-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="3411e-288">Como literais de cadeia de caracteres literais de cadeia de caracteres interpolada podem ser regular ou textual.</span><span class="sxs-lookup"><span data-stu-id="3411e-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="3411e-289">Literais de cadeia de caracteres interpolada regulares são delimitadas por `$"` e `"`, e literais de cadeia de caracteres textual interpolada são delimitadas por `$@"` e `"`.</span><span class="sxs-lookup"><span data-stu-id="3411e-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="3411e-290">Como outros literais, a análise léxica de uma cadeia de caracteres interpolada literal inicialmente resulta em um único token, de acordo com a gramática abaixo.</span><span class="sxs-lookup"><span data-stu-id="3411e-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="3411e-291">No entanto, antes da análise sintática, o token único de uma cadeia de caracteres interpolada literal é dividido em vários tokens para as partes da cadeia de caracteres, colocando os buracos e os elementos de entrada que ocorrem os buracos são lexicalmente analisados novamente.</span><span class="sxs-lookup"><span data-stu-id="3411e-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="3411e-292">Por sua vez, isso pode produzir mais interpolada literais de cadeia de caracteres para ser processada, mas, se lexicalmente corrigir, acabará levando a uma sequência de tokens para análise sintática processar.</span><span class="sxs-lookup"><span data-stu-id="3411e-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

    close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

<span data-ttu-id="3411e-293">Uma *interpolated_string_literal* token é reinterpretado como vários tokens e outros elementos de entrada da seguinte maneira, em ordem de ocorrência a *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="3411e-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="3411e-294">Ocorrências do seguinte são reinterpretadas como tokens individuais separados: o entrelinhamento `$` sinal *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* e *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="3411e-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="3411e-295">Ocorrências de *regular_balanced_text* e *verbatim_balanced_text* entre essas são reprocessados como um *input_section* ([análise léxica ](lexical-structure.md#lexical-analysis)) e são reinterpretado como a sequência resultante dos elementos de entrada.</span><span class="sxs-lookup"><span data-stu-id="3411e-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="3411e-296">Por sua vez, eles podem incluir tokens literais de cadeia de caracteres interpolada sejam reinterpretados.</span><span class="sxs-lookup"><span data-stu-id="3411e-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="3411e-297">Análise sintática será recombinar os tokens em um *interpolated_string_expression* ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="3411e-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="3411e-298">Exemplos de tarefas Pendentes</span><span class="sxs-lookup"><span data-stu-id="3411e-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="3411e-299">O literal nulo</span><span class="sxs-lookup"><span data-stu-id="3411e-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="3411e-300">O *null_literal* pode ser convertido implicitamente em um tipo de referência ou tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="3411e-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="3411e-301">Pontuadores e operadores</span><span class="sxs-lookup"><span data-stu-id="3411e-301">Operators and punctuators</span></span>

<span data-ttu-id="3411e-302">Há vários tipos de operadores e pontuadores.</span><span class="sxs-lookup"><span data-stu-id="3411e-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="3411e-303">Operadores são usados em expressões para descrever as operações que envolvem um ou mais operandos.</span><span class="sxs-lookup"><span data-stu-id="3411e-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="3411e-304">Por exemplo, a expressão `a + b` usa o `+` para adicionar os dois operandos `a` e `b`.</span><span class="sxs-lookup"><span data-stu-id="3411e-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="3411e-305">Pontuadores são para agrupamento e a separação.</span><span class="sxs-lookup"><span data-stu-id="3411e-305">Punctuators are for grouping and separating.</span></span>

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

<span data-ttu-id="3411e-306">A barra vertical na *right_shift* e *right_shift_assignment* produções são usadas para indicar que, ao contrário de outras produções na gramática sintática, nenhum caractere de qualquer tipo (nem mesmo espaço em branco) são permitidos entre os tokens.</span><span class="sxs-lookup"><span data-stu-id="3411e-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="3411e-307">Esses produções são tratadas especialmente para permitir a manipulação correta de *type_parameter_list*s ([parâmetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="3411e-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="3411e-308">Diretivas de pré-processamento</span><span class="sxs-lookup"><span data-stu-id="3411e-308">Pre-processing directives</span></span>

<span data-ttu-id="3411e-309">As diretivas de pré-processamento fornecem a capacidade de ignorar condicionalmente seções dos arquivos de origem, com erro de relatório e as condições de aviso e para delinear regiões distintas do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="3411e-310">O termo "Pré-processando diretivas" é usado apenas para manter a consistência com as linguagens de programação C e C++.</span><span class="sxs-lookup"><span data-stu-id="3411e-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="3411e-311">No c#, não há nenhuma etapa de pré-processamento separada; Pré-processando diretivas são processadas como parte da fase de análise léxica.</span><span class="sxs-lookup"><span data-stu-id="3411e-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

<span data-ttu-id="3411e-312">As seguintes diretivas de pré-processamento estão disponíveis:</span><span class="sxs-lookup"><span data-stu-id="3411e-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="3411e-313">`#define` e `#undef`, que são usados para definir e remover, respectivamente, símbolos de compilação condicional ([diretivas de declaração](lexical-structure.md#declaration-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="3411e-314">`#if`, `#elif`, `#else`, e `#endif`, que são usados para condicionalmente ignorar seções do código-fonte ([diretivas de compilação condicional](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="3411e-315">`#line`, que é usado para controlar os números de linha emitidos para erros e avisos ([diretivas de linha](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="3411e-316">`#error` e `#warning`, que são usados para emitir os erros e avisos, respectivamente ([diretivas diagnóstico](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="3411e-317">`#region` e `#endregion`, que são usados para marcar explicitamente seções do código-fonte ([diretivas de região](lexical-structure.md#region-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="3411e-318">`#pragma`, que é usado para especificar as informações contextuais para o compilador opcionais ([diretivas Pragma](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="3411e-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="3411e-319">Uma diretiva de pré-processamento sempre ocupa uma linha separada do código-fonte e sempre começa com um `#` caractere e um nome de diretiva de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="3411e-320">Espaço em branco podem ocorrer antes do `#` caractere e entre o `#` caractere e o nome da diretiva.</span><span class="sxs-lookup"><span data-stu-id="3411e-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="3411e-321">Uma linha de código-fonte que contém um `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, ou `#endregion` diretiva pode terminar com um comentário de linha única.</span><span class="sxs-lookup"><span data-stu-id="3411e-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="3411e-322">Delimitado por comentários (o `/* */` estilo de comentários) não são permitidos em linhas de código-fonte que contém as diretivas de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="3411e-323">Pré-processando diretivas não são tokens e não fazem parte da gramática da linguagem c# sintática.</span><span class="sxs-lookup"><span data-stu-id="3411e-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="3411e-324">No entanto, as diretivas de pré-processamento podem ser usadas para incluir ou excluir as sequências de tokens e podem dessa forma afetar o significado de um programa c#.</span><span class="sxs-lookup"><span data-stu-id="3411e-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="3411e-325">Por exemplo, quando o programa compilado:</span><span class="sxs-lookup"><span data-stu-id="3411e-325">For example, when compiled, the program:</span></span>
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
<span data-ttu-id="3411e-326">resultados na mesma sequência exata dos tokens que o programa:</span><span class="sxs-lookup"><span data-stu-id="3411e-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="3411e-327">Portanto, ao passo que lexicalmente, os dois programas são bastante diferentes, sintaticamente, eles são idênticos.</span><span class="sxs-lookup"><span data-stu-id="3411e-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="3411e-328">Símbolos de compilação condicional</span><span class="sxs-lookup"><span data-stu-id="3411e-328">Conditional compilation symbols</span></span>

<span data-ttu-id="3411e-329">A funcionalidade de compilação condicional fornecida pelo `#if`, `#elif`, `#else`, e `#endif` diretivas é controlado por meio de expressões de pré-processamento ([pré-processamento de expressões](lexical-structure.md#pre-processing-expressions)) e símbolos de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="3411e-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="3411e-330">Um símbolo de compilação condicional tem dois estados possíveis: ***definidos*** ou ***indefinido***.</span><span class="sxs-lookup"><span data-stu-id="3411e-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="3411e-331">No início do processamento léxico de um arquivo de origem, um símbolo de compilação condicional é indefinido, a menos que ele foi explicitamente definido por um mecanismo externo (como uma opção de compilador de linha de comando).</span><span class="sxs-lookup"><span data-stu-id="3411e-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="3411e-332">Quando um `#define` diretiva é processada, o símbolo de compilação condicional nomeado nessa diretiva se torna definido no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="3411e-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="3411e-333">O símbolo permanece definido até que um `#undef` diretiva desse símbolo mesmo é processado, ou até o final do arquivo de origem for atingido.</span><span class="sxs-lookup"><span data-stu-id="3411e-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="3411e-334">Uma implicação disso é que `#define` e `#undef` diretivas em um arquivo de origem não têm nenhum efeito em outros arquivos de origem no mesmo programa.</span><span class="sxs-lookup"><span data-stu-id="3411e-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="3411e-335">Quando referenciado em uma expressão de pré-processamento, um símbolo de compilação condicional definido tem o valor booliano `true`, e um símbolo de compilação condicional indefinido tem o valor booliano `false`.</span><span class="sxs-lookup"><span data-stu-id="3411e-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="3411e-336">Não há nenhum requisito de que os símbolos de compilação condicional ser explicitamente declarada antes que eles são referenciados em expressões de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="3411e-337">Em vez disso, os símbolos não declarados são simplesmente indefinidos e, portanto, têm o valor `false`.</span><span class="sxs-lookup"><span data-stu-id="3411e-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="3411e-338">O espaço de nome para símbolos de compilação condicional é distinto e separado de todas as outras entidades nomeadas em um programa c#.</span><span class="sxs-lookup"><span data-stu-id="3411e-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="3411e-339">Símbolos de compilação condicional só podem ser referenciados em `#define` e `#undef` diretivas e em expressões de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="3411e-340">Pré-processamento de expressões</span><span class="sxs-lookup"><span data-stu-id="3411e-340">Pre-processing expressions</span></span>

<span data-ttu-id="3411e-341">Expressões de pré-processamento pode ocorrer em `#if` e `#elif` diretivas.</span><span class="sxs-lookup"><span data-stu-id="3411e-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="3411e-342">Os operadores `!`, `==`, `!=`, `&&` e `||` são permitidos em expressões, o pré-processamento e os parênteses podem ser usados para agrupamento.</span><span class="sxs-lookup"><span data-stu-id="3411e-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

<span data-ttu-id="3411e-343">Quando referenciado em uma expressão de pré-processamento, um símbolo de compilação condicional definido tem o valor booliano `true`, e um símbolo de compilação condicional indefinido tem o valor booliano `false`.</span><span class="sxs-lookup"><span data-stu-id="3411e-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="3411e-344">Avaliação de uma expressão de pré-processamento sempre gera um valor booliano.</span><span class="sxs-lookup"><span data-stu-id="3411e-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="3411e-345">As regras de avaliação de uma expressão de pré-processamento são os mesmos para uma expressão de constante ([expressões constantes](expressions.md#constant-expressions)), exceto que as únicas entidades definidas pelo usuário que podem ser referenciadas são símbolos de compilação condicional .</span><span class="sxs-lookup"><span data-stu-id="3411e-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="3411e-346">Diretivas de declaração</span><span class="sxs-lookup"><span data-stu-id="3411e-346">Declaration directives</span></span>

<span data-ttu-id="3411e-347">As diretivas de declaração são usadas para definir ou excluir definições de símbolos de compilação condicional.</span><span class="sxs-lookup"><span data-stu-id="3411e-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="3411e-348">O processamento de um `#define` diretiva faz com que o símbolo de compilação condicional determinado tornam-se definido, começando com a linha de código-fonte que segue a diretiva.</span><span class="sxs-lookup"><span data-stu-id="3411e-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="3411e-349">Da mesma forma, o processamento de um `#undef` diretiva faz com que o símbolo de compilação condicional determinado se torne indefinido, começando com a linha de código-fonte que segue a diretiva.</span><span class="sxs-lookup"><span data-stu-id="3411e-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="3411e-350">Qualquer `#define` e `#undef` diretivas em um arquivo de origem devem ocorrer antes da primeira *token* ([Tokens](lexical-structure.md#tokens)) no arquivo de origem; caso contrário, um erro de tempo de compilação ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="3411e-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="3411e-351">Em termos de intuitivos `#define` e `#undef` diretivas devem preceder qualquer "código real" no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="3411e-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="3411e-352">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="3411e-352">The example:</span></span>
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
<span data-ttu-id="3411e-353">é válido porque o `#define` diretivas precedem o primeiro token (o `namespace` palavra-chave) no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="3411e-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="3411e-354">O exemplo a seguir resulta em um erro de tempo de compilação porque uma `#define` segue o código real:</span><span class="sxs-lookup"><span data-stu-id="3411e-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

<span data-ttu-id="3411e-355">Um `#define` podem definir um símbolo de compilação condicional que já esteja definido, sem a existência de qualquer intervenção `#undef` para esse símbolo.</span><span class="sxs-lookup"><span data-stu-id="3411e-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="3411e-356">O exemplo a seguir define um símbolo de compilação condicional `A` e, em seguida, define-o novamente.</span><span class="sxs-lookup"><span data-stu-id="3411e-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="3411e-357">Um `#undef` pode "indefina" um símbolo de compilação condicional não está definido.</span><span class="sxs-lookup"><span data-stu-id="3411e-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="3411e-358">O exemplo a seguir define um símbolo de compilação condicional `A` e, em seguida, exclusões de definição-lo duas vezes; Embora a segunda `#undef` não tem nenhum efeito, ela ainda é válida.</span><span class="sxs-lookup"><span data-stu-id="3411e-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="3411e-359">Diretivas de compilação condicional</span><span class="sxs-lookup"><span data-stu-id="3411e-359">Conditional compilation directives</span></span>

<span data-ttu-id="3411e-360">As diretivas de compilação condicional são usadas para incluir ou excluir partes de um arquivo de origem condicionalmente.</span><span class="sxs-lookup"><span data-stu-id="3411e-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

<span data-ttu-id="3411e-361">Conforme indicado pela sintaxe, diretivas de compilação condicional devem ser escritas como conjuntos composto, em ordem, uma `#if` diretiva, zero ou mais `#elif` diretivas, zero ou uma `#else` diretiva e um `#endif` diretiva.</span><span class="sxs-lookup"><span data-stu-id="3411e-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="3411e-362">Entre as diretivas são seções condicionais do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="3411e-363">Cada seção é controlada pela diretiva imediatamente anterior.</span><span class="sxs-lookup"><span data-stu-id="3411e-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="3411e-364">Uma seção condicional pode conter diretivas de compilação condicional aninhada desde que essas diretivas formam conjuntos completos.</span><span class="sxs-lookup"><span data-stu-id="3411e-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="3411e-365">Um *pp_conditional* seleciona no máximo uma independente *conditional_section*s para o processamento normal de léxico:</span><span class="sxs-lookup"><span data-stu-id="3411e-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="3411e-366">O *pp_expression*s da `#if` e `#elif` diretivas são avaliadas na ordem até que um produz `true`.</span><span class="sxs-lookup"><span data-stu-id="3411e-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="3411e-367">Se uma expressão gera `true`, o *conditional_section* da diretiva correspondente é selecionado.</span><span class="sxs-lookup"><span data-stu-id="3411e-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="3411e-368">Se todos os *pp_expression*yield s `false`e se um `#else` diretiva estiver presente, o *conditional_section* da `#else` diretiva é selecionada.</span><span class="sxs-lookup"><span data-stu-id="3411e-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="3411e-369">Caso contrário, não *conditional_section* está selecionado.</span><span class="sxs-lookup"><span data-stu-id="3411e-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="3411e-370">Selecionado *conditional_section*, se houver, é processado como um normal *input_section*: o código-fonte contido na seção deve aderir a gramática lexical; tokens são gerados a partir do código-fonte código na seção; e pré-processando diretivas na seção tem os efeitos prescritos.</span><span class="sxs-lookup"><span data-stu-id="3411e-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="3411e-371">O restante *conditional_section*s, se houver, são processados como *skipped_section*s: exceto para as diretivas de pré-processamento, o código-fonte na seção precisa estiver de acordo com o léxico gramática; não tokens são gerados a partir do código-fonte na seção; e pré-processando diretivas na seção devem ser lexicalmente corretas, mas caso contrário, não são processadas.</span><span class="sxs-lookup"><span data-stu-id="3411e-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="3411e-372">Dentro de um *conditional_section* que está sendo processada como uma *skipped_section*qualquer aninhada *conditional_section*s (contidos no aninhadas `#if`... `#endif` e `#region`... `#endregion` constrói) também são processados como *skipped_section*s.</span><span class="sxs-lookup"><span data-stu-id="3411e-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="3411e-373">O exemplo a seguir ilustra a compilação condicional como aninham diretivas:</span><span class="sxs-lookup"><span data-stu-id="3411e-373">The following example illustrates how conditional compilation directives can nest:</span></span>
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

<span data-ttu-id="3411e-374">Exceto para pré-processando diretivas, código-fonte foi ignorada não está sujeito à análise lexical.</span><span class="sxs-lookup"><span data-stu-id="3411e-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="3411e-375">Por exemplo, o seguinte é válido, apesar do comentário não finalizado no `#else` seção:</span><span class="sxs-lookup"><span data-stu-id="3411e-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

<span data-ttu-id="3411e-376">No entanto, observe que as diretivas de pré-processamento são necessárias para ser lexicalmente correto, mesmo nas seções ignoradas do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="3411e-377">Pré-processando diretivas não são processadas quando elas aparecerem dentro de elementos de entrada multilinhas.</span><span class="sxs-lookup"><span data-stu-id="3411e-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="3411e-378">Por exemplo, o programa:</span><span class="sxs-lookup"><span data-stu-id="3411e-378">For example, the program:</span></span>
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
<span data-ttu-id="3411e-379">resultados na saída:</span><span class="sxs-lookup"><span data-stu-id="3411e-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="3411e-380">Em casos peculiares, o conjunto de diretivas de pré-processamento que é processado pode depender da avaliação do *pp_expression*.</span><span class="sxs-lookup"><span data-stu-id="3411e-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="3411e-381">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="3411e-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="3411e-382">sempre produz o mesmo fluxo de token (`class` `Q` `{` `}`), independentemente de se deseja ou não `X` está definido.</span><span class="sxs-lookup"><span data-stu-id="3411e-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="3411e-383">Se `X` é definido, as diretivas de processados somente são `#if` e `#endif`, devido ao comentário de várias linha.</span><span class="sxs-lookup"><span data-stu-id="3411e-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="3411e-384">Se `X` é indefinida, em seguida, três diretivas (`#if`, `#else`, `#endif`) são parte do conjunto de diretiva.</span><span class="sxs-lookup"><span data-stu-id="3411e-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="3411e-385">Diretivas de diagnóstico</span><span class="sxs-lookup"><span data-stu-id="3411e-385">Diagnostic directives</span></span>

<span data-ttu-id="3411e-386">As diretivas de diagnóstico são usadas para gerar mensagens de erro e aviso que são relatadas da mesma maneira que outros erros de tempo de compilação e avisos.</span><span class="sxs-lookup"><span data-stu-id="3411e-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

<span data-ttu-id="3411e-387">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="3411e-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="3411e-388">sempre produz um aviso ("revisão de código necessário antes do check-in") e produz um erro de tempo de compilação ("uma compilação não pode ser depuração e varejo") se os símbolos condicionais `Debug` e `Retail` forem definidos.</span><span class="sxs-lookup"><span data-stu-id="3411e-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="3411e-389">Observe que um *pp_message* pode conter texto arbitrário; especificamente, ele não precisa conter tokens bem formados, conforme mostrado pela aspa simples na palavra `can't`.</span><span class="sxs-lookup"><span data-stu-id="3411e-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="3411e-390">Diretivas de região</span><span class="sxs-lookup"><span data-stu-id="3411e-390">Region directives</span></span>

<span data-ttu-id="3411e-391">As diretivas de região são usadas para marcar explicitamente as regiões do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-391">The region directives are used to explicitly mark regions of source code.</span></span>

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

<span data-ttu-id="3411e-392">Nenhum significado semântico é anexado a uma região; regiões destinam para uso pelo programador ou por ferramentas automatizadas para marcar uma seção do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="3411e-393">A mensagem especificada em uma `#region` ou `#endregion` diretiva da mesma forma não tem nenhum significado semântico; ele simplesmente serve para identificar a região.</span><span class="sxs-lookup"><span data-stu-id="3411e-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="3411e-394">Correspondência `#region` e `#endregion` diretivas podem ter diferentes *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="3411e-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="3411e-395">O processamento de léxico de uma região:</span><span class="sxs-lookup"><span data-stu-id="3411e-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="3411e-396">corresponde exatamente ao processamento de léxico de uma diretiva de compilação condicional do formulário:</span><span class="sxs-lookup"><span data-stu-id="3411e-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="3411e-397">Diretivas de linha</span><span class="sxs-lookup"><span data-stu-id="3411e-397">Line directives</span></span>

<span data-ttu-id="3411e-398">Diretivas de linha podem ser usadas para alterar os números de linha e nomes de arquivo de origem que são relatados pelo compilador na saída, como erros e avisos, e que são usados por atributos de informações do chamador ([atributos de informações do chamador](attributes.md#caller-info-attributes)).</span><span class="sxs-lookup"><span data-stu-id="3411e-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="3411e-399">Diretivas de linha são mais comumente usadas nas ferramentas de metaprogramação que geram o código-fonte c# de algum outro texto de entrada.</span><span class="sxs-lookup"><span data-stu-id="3411e-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

<span data-ttu-id="3411e-400">Quando nenhum `#line` diretivas estiverem presentes, o compilador relatará os números de linha true e nomes de arquivo de origem na sua saída.</span><span class="sxs-lookup"><span data-stu-id="3411e-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="3411e-401">Durante o processamento de uma `#line` diretiva inclui um *line_indicator* que não é `default`, o compilador trata a linha após a diretiva como tendo o número de linha especificado (e o nome do arquivo, se especificado).</span><span class="sxs-lookup"><span data-stu-id="3411e-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="3411e-402">Um `#line default` diretiva inverte o efeito de todas as diretivas #line anterior.</span><span class="sxs-lookup"><span data-stu-id="3411e-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="3411e-403">O compilador relata informações de linha de true para linhas subsequentes, exatamente como se não `#line` diretivas tinham sido processadas.</span><span class="sxs-lookup"><span data-stu-id="3411e-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="3411e-404">Um `#line hidden` diretiva não tem nenhum efeito sobre o arquivo e os números de linha relatados em erro, mensagem, mas afetam a depuração no nível do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3411e-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="3411e-405">Durante a depuração, todas as linhas entre um `#line hidden` diretiva e as subsequentes `#line` diretiva (que não é `#line hidden`) não ter nenhuma informação de número de linha.</span><span class="sxs-lookup"><span data-stu-id="3411e-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="3411e-406">Ao percorrer o código no depurador, essas linhas serão ignoradas inteiramente.</span><span class="sxs-lookup"><span data-stu-id="3411e-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="3411e-407">Observe que um *file_name* difere de um literal de cadeia regular caracteres de escape não são processadas; a "`\`" caractere simplesmente designa um caractere de barra invertida comum dentro de um *file_name*.</span><span class="sxs-lookup"><span data-stu-id="3411e-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="3411e-408">Diretivas pragma</span><span class="sxs-lookup"><span data-stu-id="3411e-408">Pragma directives</span></span>

<span data-ttu-id="3411e-409">O `#pragma` diretiva de pré-processamento é usada para especificar as informações contextuais opcionais para o compilador.</span><span class="sxs-lookup"><span data-stu-id="3411e-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="3411e-410">As informações fornecidas em um `#pragma` diretiva nunca será alterado para a semântica do programa.</span><span class="sxs-lookup"><span data-stu-id="3411e-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="3411e-411">O c# fornece `#pragma` diretivas para controlar avisos do compilador.</span><span class="sxs-lookup"><span data-stu-id="3411e-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="3411e-412">Futuras versões da linguagem podem incluir adicionais `#pragma` diretivas.</span><span class="sxs-lookup"><span data-stu-id="3411e-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="3411e-413">Para garantir a interoperabilidade com outros compiladores do c#, o compilador Microsoft c# não emite erros de compilação para o desconhecido `#pragma` diretivas; tal não de diretivas porém gerar avisos.</span><span class="sxs-lookup"><span data-stu-id="3411e-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="3411e-414">Aviso de pragma</span><span class="sxs-lookup"><span data-stu-id="3411e-414">Pragma warning</span></span>

<span data-ttu-id="3411e-415">O `#pragma warning` diretiva é usada para desabilitar ou restaurar todos os ou mensagens de um conjunto específico de aviso durante a compilação de texto do programa subsequentes.</span><span class="sxs-lookup"><span data-stu-id="3411e-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

<span data-ttu-id="3411e-416">Um `#pragma warning` diretiva que omite a lista de avisos afeta todos os avisos.</span><span class="sxs-lookup"><span data-stu-id="3411e-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="3411e-417">Um `#pragma warning` diretiva o inclui uma lista de avisos afeta apenas os avisos que são especificados na lista.</span><span class="sxs-lookup"><span data-stu-id="3411e-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="3411e-418">Um `#pragma warning disable` diretiva desabilita todos os ou o conjunto determinado de avisos.</span><span class="sxs-lookup"><span data-stu-id="3411e-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="3411e-419">Um `#pragma warning restore` diretiva restaurará todos os ou o conjunto determinado de avisos para o estado que estava em vigor no início da unidade de compilação.</span><span class="sxs-lookup"><span data-stu-id="3411e-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="3411e-420">Observe que, se um aviso específico foi desabilitado externamente, um `#pragma warning restore` (se é para todos os ou o aviso específico) não habilitará esse aviso novamente.</span><span class="sxs-lookup"><span data-stu-id="3411e-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="3411e-421">O exemplo a seguir mostra o uso de `#pragma warning` desabilitar temporariamente o aviso relatado quando obsoleto membros são referenciados, usando o número de aviso do compilador do Microsoft c#.</span><span class="sxs-lookup"><span data-stu-id="3411e-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
