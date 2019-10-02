---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704065"
---
# <a name="lexical-structure"></a>Estrutura lexical

## <a name="programs"></a>Programas

Um C# ***programa*** consiste em um ou mais ***arquivos de origem***, conhecidos formalmente como ***unidades de compilação*** (unidades de[compilação](namespaces.md#compilation-units)). Um arquivo de origem é uma sequência ordenada de caracteres Unicode. Normalmente, os arquivos de origem têm uma correspondência um-para-um com arquivos em um sistema de arquivos, mas essa correspondência não é necessária. Para portabilidade máxima, é recomendável que os arquivos em um sistema de arquivos sejam codificados com a codificação UTF-8.

Em termos conceituais, um programa é compilado usando três etapas:

1. Transformação, que converte um arquivo de um repertório de caractere específico e esquema de codificação em uma sequência de caracteres Unicode.
2. Análise lexical, que traduz um fluxo de caracteres de entrada Unicode em um fluxo de tokens.
3. Análise sintática, que traduz o fluxo de tokens no código executável.

## <a name="grammars"></a>Gramáticas

Essa especificação apresenta a sintaxe da linguagem C# de programação usando duas gramáticas. A ***gramática léxica*** ([gramática lexical](lexical-structure.md#lexical-grammar)) define como os caracteres Unicode são combinados aos terminadores de linha de formulário, espaço em branco, comentários, tokens e diretivas de pré-processamento. A ***gramática sintática*** ([gramática sintática](lexical-structure.md#syntactic-grammar)) define como os tokens resultantes da gramática léxica são combinados para C# programas de formulário.

### <a name="grammar-notation"></a>Notação gramatical

As gramáticas lexical e sintática são apresentadas no formulário Backus-Naur usando a notação da ferramenta gramática do ANTLR.

### <a name="lexical-grammar"></a>Gramática lexical

A gramática léxica do C# é apresentada em diretivas de [análise lexical](lexical-structure.md#lexical-analysis), [tokens](lexical-structure.md#tokens)e [pré-processamento](lexical-structure.md#pre-processing-directives). Os símbolos de terminal da gramática lexical são os caracteres do conjunto de caracteres Unicode e a gramática lexical especifica como os caracteres são combinados para tokens de formulário ([tokens](lexical-structure.md#tokens)), espaço em branco ([espaço em branco](lexical-structure.md#white-space)), comentários ([comentários](lexical-structure.md#comments)) e pré-processando diretivas ([pré-processando diretivas](lexical-structure.md#pre-processing-directives)).

Cada arquivo de origem em C# um programa deve estar de acordo com a produção de *entrada* da gramática léxica ([análise léxica](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Gramática sintática

A gramática sintática do C# é apresentada nos capítulos e apêndices que seguem este capítulo. Os símbolos de terminal da gramática sintática são os tokens definidos pela gramática lexical e a gramática sintática especifica como os tokens são combinados para C# os programas de formulário.

Cada arquivo de origem em C# um programa deve estar de acordo com a produção *compilation_unit* da gramática sintática ([unidades de compilação](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Análise léxica

A produção de *entrada* define a estrutura lexical de C# um arquivo de origem. Cada arquivo de origem em C# um programa deve estar de acordo com esta produção de gramática lexical.

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

Cinco elementos básicos compõem a estrutura lexical de um C# arquivo de origem: Terminadores de linha ([terminadores de linha](lexical-structure.md#line-terminators)), espaço em branco ([espaço em branco](lexical-structure.md#white-space)), comentários ([comentários](lexical-structure.md#comments)), tokens ([tokens](lexical-structure.md#tokens)) e diretivas de pré-processamento ([diretivas de pré-processamento](lexical-structure.md#pre-processing-directives)). Desses elementos básicos, somente os tokens são significativos na gramática sintática de um C# programa ([gramática sintática](lexical-structure.md#syntactic-grammar)).

O processamento lexical de um C# arquivo de origem consiste em reduzir o arquivo em uma sequência de tokens que se torna a entrada para a análise sintática. Terminadores de linha, espaços em branco e comentários podem servir para tokens separados, e diretivas de pré-processamento podem fazer com que as seções do arquivo de origem sejam ignoradas, mas, caso contrário, esses elementos léxicos não têm C# impacto sobre a estrutura sintática de um programa.

No caso de literais de cadeia de caracteres interpolados ([literais de cadeia de caracteres interpolados](lexical-structure.md#interpolated-string-literals)), um único token é produzido inicialmente pela análise léxica, mas é dividido em vários elementos de entrada que são sujeitos repetidamente à análise lexical até todos interpolados literais de cadeia de caracteres foram resolvidas. Os tokens resultantes servem como entrada para a análise sintática.

Quando várias produções de gramática léxicas correspondem a uma sequência de caracteres em um arquivo de origem, o processamento lexical sempre forma o mais longo possível elemento léxico. Por exemplo, a sequência `//` de caracteres é processada como o início de um comentário de linha única porque esse elemento léxico é maior do que um único `/` token.

### <a name="line-terminators"></a>Terminadores de linha

Terminadores de linha dividem os caracteres C# de um arquivo de origem em linhas.

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

Para compatibilidade com ferramentas de edição de código-fonte que adicionam marcadores de final de arquivo e para permitir que um arquivo de origem seja exibido como uma sequência de linhas corretamente encerradas, as seguintes transformações são aplicadas, em ordem, a cada arquivo C# de origem em um programa:

*  Se o último caractere do arquivo de origem for um caractere de controle Z (`U+001A`), esse caractere será excluído.
*  Um caractere de retorno de carro`U+000D`() é adicionado ao final do arquivo de origem se esse arquivo de origem não estiver vazio e se o último caractere do arquivo de origem não for um retorno de carro`U+000D`(), um feed de`U+000A`linha (), um separador de linha (`U+2028`) ou um separador de`U+2029`parágrafo ().

### <a name="comments"></a>Comentários

Há suporte para dois formulários de comentários: Comentários de linha única e comentários delimitados. Os ***comentários de linha única*** começam com os `//` caracteres e se estendem até o final da linha de origem. Os ***comentários delimitados*** começam com `/*` os caracteres e terminam `*/`com os caracteres. Comentários delimitados podem abranger várias linhas.

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
    : '/*' delimited_comment_section* asterisk+ '/'
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

Os comentários não são aninhados. As sequências `/*` de `*/` caracteres e não têm um significado `//` especial dentro de um comentário e `//` as `/*` sequências de caracteres e não têm nenhum significado especial dentro de um comentário delimitado.

Os comentários não são processados em literais de caractere e de cadeia de caracteres.

O exemplo
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
inclui um comentário delimitado.

O exemplo
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
mostra vários comentários de linha única.

### <a name="white-space"></a>Espaço em branco

O espaço em branco é definido como qualquer caractere com a classe Unicode ZS (que inclui o caractere de espaço), bem como o caractere de tabulação horizontal, o caractere de tabulação vertical e o caractere de feed de formulário.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Tokens

Há vários tipos de tokens: identificadores, palavras-chave, literais, operadores e pontuadores. Os comentários e o espaço em branco não são tokens, embora atuem como separadores para tokens.

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

### <a name="unicode-character-escape-sequences"></a>Sequências de escape de caractere Unicode

Uma sequência de escape de caractere Unicode representa um caractere Unicode. As sequências de escape de caractere Unicode são processadas em identificadores ([identificadores](lexical-structure.md#identifiers)), literais de caracteres ([literais](lexical-structure.md#character-literals)de caracteres) e literais de cadeia de caracteres regulares ([literais de cadeia de caracteres](lexical-structure.md#string-literals)). Um escape de caractere Unicode não é processado em nenhum outro local (por exemplo, para formar um operador, um pontuador ou uma palavra-chave).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Uma sequência de escape Unicode representa o caractere Unicode único formado pelo número hexadecimal após os caracteres`\u`"" ou`\U`"". Como C# o usa uma codificação de 16 bits de pontos de código Unicode em caracteres e valores de cadeia, um caractere Unicode no intervalo de u + 10000 a u + 10FFFF não é permitido em um literal de caractere e é representado usando um par substituto Unicode em um literal de cadeia de caracteres. Não há suporte para caracteres Unicode com pontos de código acima de 0x10FFFF.

Várias traduções não são executadas. Por exemplo, a cadeia de caracteres`\u005Cu005C`literal "" é equivalente`\u005C`a "" em`\`vez de "". O valor `\u005C` Unicode é o caractere "`\`".

O exemplo
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
mostra vários usos de `\u0066`, que é a sequência de escape para a letra`f`"". O programa é equivalente a
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

### <a name="identifiers"></a>Identificadores

As regras para identificadores fornecidos nesta seção correspondem exatamente às recomendadas pelo anexo 31 padrão Unicode, exceto que o sublinhado é permitido como um caractere inicial (como é tradicional na linguagem de programação C), as sequências de escape Unicode são permitido em identificadores e o caractere "`@`" é permitido como um prefixo para permitir que palavras-chave sejam usadas como identificadores.

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

Para obter informações sobre as classes de caracteres Unicode mencionadas acima, consulte o padrão Unicode, versão 3,0, seção 4,5.

Exemplos de identificadores válidos incluem "`identifier1`", "`_identifier2`" e "`@if`".

Um identificador em um programa de conformidade deve estar no formato canônico definido pelo formulário de normalização Unicode C, conforme definido pelo anexo 15 padrão Unicode. O comportamento ao encontrar um identificador que não está no formato de normalização C é definido pela implementação; no entanto, um diagnóstico não é necessário.

O prefixo "`@`" permite o uso de palavras-chave como identificadores, o que é útil ao fazer a interface com outras linguagens de programação. O caractere `@` não é realmente parte do identificador, portanto, o identificador pode ser visto em outros idiomas como um identificador normal, sem o prefixo. Um identificador com um `@` prefixo é chamado de ***identificador textual***. O `@` uso do prefixo para identificadores que não são palavras-chave é permitido, mas altamente desencorajado como uma questão de estilo.

O Exemplo:
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
define uma classe denominada`class`"" com um método estático chamado`static`"" que usa um parâmetro denominado`bool`"". Observe que, como os escapes Unicode não são permitidos em palavras-chave, o`cl\u0061ss`token "" é um identificador e é o mesmo identificador de`@class`"".

Dois identificadores são considerados iguais se forem idênticos depois que as transformações a seguir forem aplicadas, na ordem:

*  O prefixo "`@`", se usado, será removido.
*  Cada *unicode_escape_sequence* é transformado em seu caractere Unicode correspondente.
*  Quaisquer *formatting_character*s são removidos.

Os identificadores que contêm dois caracteres de sublinhado consecutivos (`U+005F`) são reservados para uso pela implementação. Por exemplo, uma implementação pode fornecer palavras-chave estendidas que começam com dois sublinhados.

### <a name="keywords"></a>Palavras-chave

Uma ***palavra-chave*** é uma sequência semelhante a identificador de caracteres que é reservada e não pode ser usada como um identificador, exceto `@` quando precedida pelo caractere.

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

Em alguns lugares da gramática, os identificadores específicos têm um significado especial, mas não são palavras-chave. Esses identificadores são, às vezes, chamados de "palavras-chave contextuais". Por exemplo, dentro de uma declaração de propriedade,`get`os identificadores`set`"" e "" têm significado especial ([acessadores](classes.md#accessors)). Um identificador diferente de `get` ou `set` nunca é permitido nesses locais, portanto, esse uso não entra em conflito com o uso dessas palavras como identificadores. Em outros casos, como com o identificador "`var`" em declarações de variáveis locais digitadas implicitamente (declarações de[variáveis locais](statements.md#local-variable-declarations)), uma palavra-chave contextual pode entrar em conflito com nomes declarados. Nesses casos, o nome declarado tem precedência sobre o uso do identificador como uma palavra-chave contextual.

### <a name="literals"></a>Literais

Um ***literal*** é uma representação de código-fonte de um valor.

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

#### <a name="boolean-literals"></a>Literais boolianos

Há dois valores literais boolianos `true` : `false`e.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

O tipo de um *boolean_literal* é `bool`.

#### <a name="integer-literals"></a>Literais inteiros

Literais inteiros são usados para gravar valores de tipos `int`, `uint`, `long`, e `ulong`. Os literais inteiros têm duas formas possíveis: decimal e hexadecimal.

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

O tipo de um literal inteiro é determinado da seguinte maneira:

*  Se o literal não tiver nenhum sufixo, ele terá o primeiro desses tipos em que seu valor pode ser representado: `int` `long`, `uint`,, `ulong`.
*  Se o literal for sufixado `U` pelo `u`ou, ele terá o primeiro desses tipos em que seu valor pode ser representado: `uint`, `ulong`.
*  Se o literal for sufixado `L` pelo `l`ou, ele terá o primeiro desses tipos em que seu valor pode ser representado: `long`, `ulong`.
*  Se o literal for sufixado `UL`por `Ul` `uL` `ul` `LU`, `Lu` `lu`,,,,, ou, será do tipo `ulong`. `lU`

Se o valor representado por um literal inteiro estiver fora do intervalo do `ulong` tipo, ocorrerá um erro em tempo de compilação.

Como uma questão de estilo, é recomendável que "`L`" seja usado em vez de`l`"" ao escrever literais do `long`tipo, já que é fácil confundir a letra`l`"" com o dígito`1`"".

Para permitir que os menores `int` valores `long` e possíveis sejam gravados como literais inteiros decimais, existem as duas seguintes regras:

* Quando um *decimal_integer_literal* com o valor 2147483648 (2 ^ 31) e nenhum *integer_type_suffix* aparece como o token imediatamente após um token do operador menos unário ([operador menos unário](expressions.md#unary-minus-operator)), o resultado é uma constante do tipo `int` com o valor-2147483648 (-2 ^ 31). Em todas as outras situações, tal *decimal_integer_literal* é do tipo `uint`.
* Quando um *decimal_integer_literal* com o valor 9.223.372.036.854.775.808 (2 ^ 63) e nenhum *integer_type_suffix* ou *integer_type_suffix* `L` ou `l` aparece como o token imediatamente após um token do operador menos unário ([ Operador de subtração unário](expressions.md#unary-minus-operator)), o resultado é uma constante do tipo `long` com o valor-9.223.372.036.854.775.808 (-2 ^ 63). Em todas as outras situações, tal *decimal_integer_literal* é do tipo `ulong`.

#### <a name="real-literals"></a>Literais reais

Os literais reais são usados para gravar valores de `float`tipos `double`, e `decimal`.

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

Se nenhum *real_type_suffix* for especificado, o tipo do literal real será `double`. Caso contrário, o sufixo de tipo real determina o tipo do literal real, da seguinte maneira:

*  Um literal real sufixado `F` por `f` ou é do `float`tipo. Por exemplo, os literais `1f`, `1.5f`, `1e10f`e `123.456F` são todos do tipo `float`.
*  Um literal real sufixado `D` por `d` ou é do `double`tipo. Por exemplo, os literais `1d`, `1.5d`, `1e10d`e `123.456D` são todos do tipo `double`.
*  Um literal real sufixado `M` por `m` ou é do `decimal`tipo. Por exemplo, os literais `1m`, `1.5m`, `1e10m`e `123.456M` são todos do tipo `decimal`. Esse literal é convertido em um `decimal` valor usando o valor exato e, se necessário, arredondando para o valor representável mais próximo usando o arredondamento do banco ([o tipo decimal](types.md#the-decimal-type)). Qualquer escala aparente no literal é preservada, a menos que o valor seja arredondado ou o valor seja zero (nesse último caso, o sinal e a escala serão 0). Portanto, o literal `2.900m` será analisado para formar o decimal com sinal `0`, coeficiente `2900`e escala `3`.

Se o literal especificado não puder ser representado no tipo indicado, ocorrerá um erro em tempo de compilação.

O valor de um literal real do tipo `float` ou `double` é determinado usando o modo IEEE "Round to mais próximo".

Observe que, em um literal real, os dígitos decimais sempre são necessários após o ponto decimal. Por exemplo, `1.3F` é um literal real, `1.F` mas não é.

#### <a name="character-literals"></a>Literais de caracteres

Um literal de caractere representa um único caractere e geralmente consiste em um caractere entre aspas, como em `'a'`.

Observação: A notação gramatical ANTLR torna o seguinte confuso! No ANTLR, quando você escreve `\'` , significa uma aspa `'`simples. E quando você escrevê `\\` -lo significa uma barra invertida `\`única. Portanto, a primeira regra para um literal de caractere significa que ela começa com uma aspa simples, depois um caractere e uma aspa simples. E as onze sequências de escape simples `\'`possíveis `\"`são `\\`, `\0` `\a` `\b`,,,,,,,,, `\t` `\f` `\n` `\r` `\v`.

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

`\`Um caractere que segue um caractere de barra invertida () em um *caractere* deve ser um dos seguintes `'` `\`caracteres `0`: `"`, `a` `b`,,,, `f` , , `n`, `r`, `t`, `u`, `U`, `x`, `v`. Caso contrário, ocorrerá um erro de tempo de compilação.

Uma sequência de escape hexadecimal representa um único caractere Unicode, com o valor formado pelo número hexadecimal após "`\x`".

Se o valor representado por um literal de caractere for maior `U+FFFF`que, ocorrerá um erro em tempo de compilação.

Uma sequência de escape de caractere Unicode ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)) em um literal de `U+0000` caractere `U+FFFF`deve estar no intervalo para.

Uma sequência de escape simples representa uma codificação de caractere Unicode, conforme descrito na tabela a seguir.


| __Sequência de escape__ | __Nome do caractere__ | __Codificação Unicode__ |
|---------------------|--------------------|----------------------|
| `\'`                | Aspas simples       | `0x0027`             | 
| `\"`                | Aspas duplas       | `0x0022`             | 
| `\\`                | Barra invertida          | `0x005C`             | 
| `\0`                | Nulo               | `0x0000`             | 
| `\a`                | Alerta              | `0x0007`             | 
| `\b`                | Backspace          | `0x0008`             | 
| `\f`                | Avanço de página          | `0x000C`             | 
| `\n`                | Nova linha           | `0x000A`             | 
| `\r`                | Retorno de carro    | `0x000D`             | 
| `\t`                | Tabulação horizontal     | `0x0009`             | 
| `\v`                | Tabulação vertical       | `0x000B`             | 

O tipo de um *character_literal* é `char`.

#### <a name="string-literals"></a>Literais de cadeia de caracteres

C#dá suporte a duas formas de literais de cadeia de caracteres: ***literais de cadeia de caracteres regulares*** e ***literais de cadeia textual***.

Um literal de cadeia de caracteres regular consiste em zero ou mais caracteres entre aspas duplas `"hello"`, como em, e pode incluir sequências de escape `\t` simples (como para o caractere de tabulação) e sequências de escape hexadecimal e Unicode.

Um literal de cadeia de caracteres textual `@` consiste em um caractere seguido por um caractere de aspas duplas, zero ou mais caracteres e um caractere de aspas duplas de fechamento. Um exemplo simples é `@"hello"`. Em um literal de cadeia de caracteres textual, os caracteres entre os delimitadores são interpretados literalmente, a única exceção é um *quote_escape_sequence*. Em particular, sequências de escape simples e sequências de escape hexadecimal e Unicode não são processadas em literais de cadeia de caracteres textuais. Um literal de cadeia de caracteres textual pode abranger várias linhas.

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

Um caractere que segue um caractere de barra invertida (`\`) em um *regular_string_literal_character* deve ser um dos seguintes caracteres: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, 0, 1, @no__ t-12, 3, 4, 5. Caso contrário, ocorrerá um erro de tempo de compilação.

O exemplo
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
mostra uma variedade de literais de cadeia de caracteres. O último literal de cadeia `j`de caracteres,, é um literal de cadeia de caracteres textual que abrange várias linhas. Os caracteres entre as aspas, incluindo espaços em branco, como novos caracteres de linha, são preservados literalmente.

Como uma sequência de escape hexadecimal pode ter um número variável de dígitos hexadecimais, a cadeia `"\x123"` de caracteres literal contém um único caractere com o valor hexadecimal 123. Para criar uma cadeia de caracteres que contém o caractere com o valor hexadecimal 12 seguido pelo caractere 3, `"\x00123"` é `"\x12" + "3"` possível escrever ou em vez disso.

O tipo de um *string_literaL* é `string`.

Cada literal de cadeia de caracteres não resulta necessariamente em uma nova instância de cadeia de caracteres. Quando dois ou mais literais de cadeia de caracteres equivalentes de acordo com o operador de igualdade de cadeia de caracteres ([operadores de igualdade de cadeia](expressions.md#string-equality-operators)de caracteres) aparecem no mesmo programa, esses literais de cadeia de caracteres fazem referência à mesma instância de cadeia de caracteres Por exemplo, a saída produzida por
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
ocorre `True` porque os dois literais se referem à mesma instância de cadeia de caracteres.

#### <a name="interpolated-string-literals"></a>Literais de cadeia de caracteres interpolados

Literais de cadeia de caracteres interpolados são semelhantes a literais de cadeia de caracteres, `{` mas `}`contêm buracos delimitados por e, onde as expressões podem ocorrer. Em tempo de execução, as expressões são avaliadas com a finalidade de ter seus formulários textuais substituídos na cadeia de caracteres no local onde o orifício ocorre. A sintaxe e a semântica da interpolação de cadeias de caracteres são descritas na seção ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)).

Como literais de cadeia de caracteres, literais de cadeia de caracteres interpolados podem ser regulares ou textuais. Os literais de cadeia de caracteres regulares interpolados `$"` são `"`delimitados por e, e os literais de cadeia `$@"` de `"`caracteres textuais interpolados são delimitados por e.

Assim como outros literais, a análise léxica de um literal de cadeia de caracteres interpolado inicialmente resulta em um único token, de acordo com a gramática abaixo. No entanto, antes da análise sintática, o único token de um literal de cadeia de caracteres interpolado é dividido em vários tokens para as partes da cadeia de caracteres que envolvem os orifícios, e os elementos de entrada que ocorrem nos orifícios são analisados lexicalmente de novo. Isso pode, por sua vez, produzir mais literais de cadeia de caracteres interpolados a serem processados, mas, se for lexicalmente correto, eventualmente levará a uma sequência de tokens para que a análise sintática seja processada.

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

Um token *interpolated_string_literal* é reinterpretado como vários tokens e outros elementos Input da seguinte maneira, em ordem de ocorrência no *interpolated_string_literal*:

* As ocorrências dos seguintes são reinterpretadas como tokens individuais separados: o sinal `$` à esquerda, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*,  *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* e *interpolated_verbatim_string_end*.
* Ocorrências de *regular_balanced_text* e *verbatim_balanced_text* entre elas são reprocessadas como um *input_section* ([análise lexical](lexical-structure.md#lexical-analysis)) e são reinterpretadas como a sequência resultante de elementos de entrada. Isso pode, por sua vez, incluir tokens literais de cadeia de caracteres interpolados a serem reinterpretados.

A análise sintática recombinará os tokens em um *interpolated_string_expression* ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)).

Exemplos de TODO


#### <a name="the-null-literal"></a>O literal nulo

```antlr
null_literal
    : 'null'
    ;
```

O *null_literal* pode ser convertido implicitamente em um tipo de referência ou um tipo anulável.

### <a name="operators-and-punctuators"></a>Operadores e pontuadores

Há vários tipos de operadores e pontuadores. Os operadores são usados em expressões para descrever as operações que envolvem um ou mais operandos. Por exemplo, a expressão `a + b` usa o `+` operador para `a` adicionar os dois operandos e `b`. Pontuadores são para Agrupamento e separação.

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

A barra vertical nas produções *right_shift* e *right_shift_assignment* são usadas para indicar que, ao contrário de outras produções na gramática sintática, nenhum caractere de qualquer tipo (nem mesmo espaço em branco) é permitido entre os tokens. Essas produções são tratadas especialmente para habilitar a manipulação correta de *type_parameter_list*s ([parâmetros de tipo](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Pré-processando diretivas

As diretivas de pré-processamento fornecem a capacidade de ignorar condicionalmente seções de arquivos de origem, relatar condições de erro e aviso e delinear regiões distintas de código-fonte. O termo "diretivas de pré-processamento" é usado apenas para consistência com as linguagens de C++ programação e C. No C#, não há uma etapa de pré-processamento separada; as diretivas de pré-processamento são processadas como parte da fase de análise lexical.

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

As seguintes diretivas de pré-processamento estão disponíveis:

*  `#define`e `#undef`, que são usados para definir e desdefinir, respectivamente, símbolos de compilação condicional ([diretivas de declaração](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, e`#endif`, que são usados para ignorar seções de código-fonte (diretivas de[compilação condicional](lexical-structure.md#conditional-compilation-directives)) condicionalmente. `#else`
*  `#line`, que é usado para controlar os números de linha emitidos para erros e avisos ([diretivas de linha](lexical-structure.md#line-directives)).
*  `#error`e `#warning`, que são usados para emitir erros e avisos, respectivamente ([diretivas de diagnóstico](lexical-structure.md#diagnostic-directives)).
*  `#region`e `#endregion`, que são usados para marcar explicitamente as seções do código-fonte ([diretivas de região](lexical-structure.md#region-directives)).
*  `#pragma`, que é usado para especificar informações contextuais opcionais para o compilador ([diretivas pragma](lexical-structure.md#pragma-directives)).

Uma diretiva de pré-processamento sempre ocupa uma linha separada do código-fonte e sempre começa com um `#` caractere e um nome de diretiva de pré-processamento. Pode ocorrer espaço em branco antes `#` do caractere e entre `#` o caractere e o nome da diretiva.

Uma linha de origem contendo `#define`uma `#undef`diretiva `#if`, `#elif` `#else` ,,`#endif`,, ou`#endregion` pode terminar com um comentário de linha única. `#line` Comentários delimitados ( `/* */` o estilo de comentários) não são permitidos em linhas de origem que contêm diretivas de pré-processamento.

As diretivas de pré-processamento não são tokens e não fazem parte da gramática sintática de C#. No entanto, as diretivas de pré-processamento podem ser usadas para incluir ou excluir sequências de tokens e podem, dessa forma, C# afetar o significado de um programa. Por exemplo, quando compilado, o programa:
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
resulta na mesma sequência de tokens que o programa:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Portanto, embora lexicalmente, os dois programas são bem diferentes, sintaticamente, são idênticos.

### <a name="conditional-compilation-symbols"></a>Símbolos de compilação condicional

A funcionalidade de compilação condicional fornecida pelas `#if`diretivas `#elif` `#else`,, e `#endif` é controlada por expressões de pré-processamento ([expressões de pré-processamento](lexical-structure.md#pre-processing-expressions)) e condicionais símbolos de compilação.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Um símbolo de compilação condicional tem dois Estados possíveis: ***definido*** ou ***indefinido***. No início do processamento lexical de um arquivo de origem, um símbolo de compilação condicional é indefinido, a menos que tenha sido explicitamente definido por um mecanismo externo (como uma opção de compilador de linha de comando). Quando uma `#define` diretiva é processada, o símbolo de compilação condicional chamado nessa diretiva é definido nesse arquivo de origem. O símbolo permanece definido até que `#undef` uma diretiva para o mesmo símbolo seja processada ou até que o final do arquivo de origem seja atingido. Uma implicação disso é que `#define` as `#undef` diretivas e em um arquivo de origem não têm nenhum efeito sobre outros arquivos de origem no mesmo programa.

Quando referenciado em uma expressão de pré-processamento, um símbolo de compilação condicional definido tem o valor `true`booliano e um símbolo de compilação condicional indefinido tem o `false`valor booliano. Não há nenhum requisito de que os símbolos de compilação condicional sejam declarados explicitamente antes de serem referenciados em expressões de pré-processamento. Em vez disso, os símbolos não declarados são simplesmente indefinidos e, portanto, têm o valor `false`.

O espaço de nome para símbolos de compilação condicional é distinto e separado de todas as outras entidades C# nomeadas em um programa. Símbolos de compilação condicional só podem ser referenciados `#undef` em `#define` diretivas e em expressões de pré-processamento.

### <a name="pre-processing-expressions"></a>Pré-processando expressões

Expressões de pré-processamento podem ocorrer em `#if` diretivas e. `#elif` Os operadores `!`, `==`, `!=`, `&&` e sãopermitidosemexpressõesdepré-processamento,eosparêntesespodemserusadosparaAgrupamento.`||`

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

Quando referenciado em uma expressão de pré-processamento, um símbolo de compilação condicional definido tem o valor `true`booliano e um símbolo de compilação condicional indefinido tem o `false`valor booliano.

A avaliação de uma expressão de pré-processamento sempre gera um valor booliano. As regras de avaliação para uma expressão de pré-processamento são as mesmas que aquelas para uma expressão constante ([expressões constantes](expressions.md#constant-expressions)), exceto que as únicas entidades definidas pelo usuário que podem ser referenciadas são símbolos de compilação condicional.

### <a name="declaration-directives"></a>Diretivas de declaração

As diretivas de declaração são usadas para definir ou não definir símbolos de compilação condicional.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

O processamento de uma `#define` diretiva faz com que o símbolo de compilação condicional fornecido se torne definido, começando com a linha de origem que segue a diretiva. Da mesma forma, o processamento `#undef` de uma diretiva faz com que o símbolo de compilação condicional fornecido se torne indefinido, começando pela linha de origem que segue a diretiva.

As `#define` diretivas `#undef` e em um arquivo de origem devem ocorrer antes do primeiro *token* ([tokens](lexical-structure.md#tokens)) no arquivo de origem; caso contrário, ocorrerá um erro em tempo de compilação. Em termos intuitivos `#define` , `#undef` as diretivas devem preceder qualquer "código real" no arquivo de origem.

O Exemplo:
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
é válido porque as `#define` diretivas precedem o primeiro token ( `namespace` a palavra-chave) no arquivo de origem.

O exemplo a seguir resulta em um erro de tempo de compilação `#define` porque um código real a seguir:
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

Um `#define` pode definir um símbolo `#undef` de compilação condicional que já está definido, sem que haja nenhum intervir para esse símbolo. O exemplo a seguir define um símbolo `A` de compilação condicional e, em seguida, define-o novamente.
```csharp
#define A
#define A
```

R `#undef` pode "desdefinir" um símbolo de compilação condicional que não está definido. O exemplo a seguir define um símbolo `A` de compilação condicional e, em seguida, a define duas vezes; embora o segundo `#undef` não tenha efeito, ele ainda é válido.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Diretivas de compilação condicional

As diretivas de compilação condicional são usadas para incluir ou excluir de forma condicional partes de um arquivo de origem.

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

Conforme indicado pela sintaxe, as diretivas de compilação condicional devem ser escritas como conjuntos que consistem em, em ordem `#if` , em uma diretiva, `#elif` zero ou mais diretivas, `#else` zero ou uma diretiva `#endif` e uma diretiva. Entre as diretivas estão seções condicionais do código-fonte. Cada seção é controlada pela diretiva imediatamente anterior. Uma seção condicional pode conter, por si só, diretivas de compilação condicional aninhadas desde que essas diretivas sejam conjuntos completos.

Um *pp_conditional* seleciona no máximo um dos *conditional_section*s independentes para processamento lexical normal:

*  As *pp_expression*s das diretivas `#if` e `#elif` são avaliadas na ordem até que uma gere `true`. Se uma expressão produz `true`, o *conditional_section* da diretiva correspondente é selecionado.
*  Se todos os *pp_expression*de s produzirem `false` e se uma diretiva `#else` estiver presente, o *conditional_section* da diretiva `#else` será selecionado.
*  Caso contrário, nenhum *conditional_section* será selecionado.

O *conditional_section*selecionado, se houver, é processado como um *input_section*normal: o código-fonte contido na seção deve aderir à gramática léxica; os tokens são gerados a partir do código-fonte na seção; e as diretivas de pré-processamento na seção têm os efeitos prescritos.

Os *conditional_section*s restantes, se houver, são processados como *skipped_section*s: exceto pelas diretivas de pré-processamento, o código-fonte na seção não precisa aderir à gramática léxica; nenhum token é gerado a partir do código-fonte na seção; e as diretivas de pré-processamento na seção devem estar lexicalmente corretas, mas não são processadas de outra forma. Em um *conditional_section* que está sendo processado como um *skipped_section*, todos os *conditional_section*aninhados (contidos nas construções aninhadas `#if`... `#endif` e `#region`... `#endregion`) também são processados como *skipped_ seção*s.

O exemplo a seguir ilustra como as diretivas de compilação condicional podem ser aninhadas:
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

Com exceção das diretivas de pré-processamento, o código-fonte ignorado não está sujeito à análise léxica. Por exemplo, o seguinte é válido apesar do comentário não finalizado na `#else` seção:
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

No entanto, observe que as diretivas de pré-processamento precisam ser lexicalmente corretas mesmo em seções ignoradas do código-fonte.

As diretivas de pré-processamento não são processadas quando aparecem dentro de elementos de entrada de várias linhas. Por exemplo, o programa:
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
resulta na saída:
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

Em casos peculiares, o conjunto de diretivas de pré-processamento processadas pode depender da avaliação do *pp_expression*. O Exemplo:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
sempre produz o mesmo fluxo de token`class` ( `}` `Q` `{` ), independentemente de ser definido `X` ou não. Se `X` for definido, as únicas diretivas processadas `#if` serão `#endif`e, devido ao comentário de várias linhas. Se `X` for indefinido, três diretivas (`#if`, `#else`, `#endif`) fazem parte do conjunto de diretivas.

### <a name="diagnostic-directives"></a>Diretivas de diagnóstico

As diretivas de diagnóstico são usadas para gerar explicitamente mensagens de erro e de aviso que são relatadas da mesma maneira que outros erros e avisos em tempo de compilação.

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

O Exemplo:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
sempre produz um aviso ("revisão de código necessária antes do check-in") e produz um erro de tempo de compilação ("uma compilação não pode ser depuração e varejo") se os `Debug` símbolos `Retail` condicionais e forem ambos definidos. Observe que um *pp_message* pode conter texto arbitrário; especificamente, ele não precisa conter tokens bem formados, conforme mostrado pelas aspas simples na palavra `can't`.

### <a name="region-directives"></a>Diretivas de região

As diretivas de região são usadas para marcar explicitamente regiões do código-fonte.

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

Nenhum significado semântico é anexado a uma região; as regiões são destinadas ao uso pelo programador ou por ferramentas automatizadas para marcar uma seção do código-fonte. A mensagem especificada em uma `#region` diretiva `#endregion` ou, da mesma forma, não tem significado semântico; ela simplesmente serve para identificar a região. As diretivas `#region` e `#endregion` correspondentes podem ter *pp_message*s diferentes.

O processamento lexical de uma região:
```csharp
#region
...
#endregion
```
corresponde exatamente ao processamento lexical de uma diretiva de compilação condicional do formulário:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Diretivas de linha

As diretivas de linha podem ser usadas para alterar os números de linha e os nomes de arquivo de origem que são relatados pelo compilador em saída, como avisos e erros, e que são usados por atributos de informações do chamador ([atributos de informações do chamador](attributes.md#caller-info-attributes)).

As diretivas de linha são usadas com mais frequência em ferramentas de metaprogramação que geram C# código-fonte de alguma outra entrada de texto.

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

Quando não `#line` há diretivas presentes, o compilador relata os números de linha verdadeiros e os nomes de arquivo de origem em sua saída. Ao processar uma diretiva `#line` que inclui um *line_indicator* que não é `default`, o compilador trata a linha após a diretiva como tendo o número de linha fornecido (e o nome do arquivo, se especificado).

Uma `#line default` diretiva reverte o efeito de todas as diretivas de #line anteriores. O compilador relata informações de linha verdadeiras para linhas subsequentes, precisamente como `#line` se nenhuma das diretivas tivesse sido processada.

Uma `#line hidden` diretiva não tem efeito sobre o arquivo e os números de linha relatados em mensagens de erro, mas afeta a depuração de nível de origem. Durante a depuração, todas as linhas `#line hidden` entre uma diretiva e `#line` a diretiva subsequente (que `#line hidden`não é) não têm nenhuma informação de número de linha. Ao percorrer o código no depurador, essas linhas serão ignoradas inteiramente.

Observe que um *file_name* é diferente de um literal de cadeia de caracteres regular, pois os caracteres de escape não são processados; o caractere "`\`" simplesmente designa um caractere de barra invertida comum em um *nome_do_arquivo*.

### <a name="pragma-directives"></a>Diretivas pragma

A `#pragma` diretiva de pré-processamento é usada para especificar informações contextuais opcionais para o compilador. As informações fornecidas em uma `#pragma` diretiva nunca alterarão a semântica do programa.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#fornece `#pragma` diretivas para controlar os avisos do compilador. As versões futuras do idioma podem incluir diretivas `#pragma` adicionais. Para garantir a interoperabilidade C# com outros compiladores, C# o compilador da Microsoft não emite erros de `#pragma` compilação para diretivas desconhecidas; no entanto, essas diretivas geram avisos.

#### <a name="pragma-warning"></a>aviso de pragma

A `#pragma warning` diretiva é usada para desabilitar ou restaurar todos ou um determinado conjunto de mensagens de aviso durante a compilação do texto do programa subsequente.

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

Uma `#pragma warning` diretiva que omite a lista de avisos afeta todos os avisos. Uma `#pragma warning` diretiva que inclui uma lista de avisos afeta apenas os avisos especificados na lista.

Uma `#pragma warning disable` diretiva desabilita todos ou o conjunto de avisos fornecido.

Uma `#pragma warning restore` diretiva restaura todos ou o conjunto determinado de avisos para o estado que estava em vigor no início da unidade de compilação. Observe que, se um aviso específico tiver sido desabilitado externamente, um `#pragma warning restore` (seja para todos ou o aviso específico) não reabilitará esse aviso.

O exemplo a seguir mostra o `#pragma warning` uso de para desabilitar temporariamente o aviso relatado quando membros obsoletos são referenciados, usando o número de C# aviso do compilador da Microsoft.
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
