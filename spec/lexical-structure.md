# <a name="lexical-structure"></a>Estrutura lexical

## <a name="programs"></a>Programas

C# ***programa*** consiste em um ou mais ***arquivos de origem***, conhecido anteriormente como ***unidades de compilação*** ([unidades de compilação](namespaces.md#compilation-units)). Um arquivo de origem é uma sequência ordenada de caracteres Unicode. Arquivos de origem normalmente têm uma correspondência com arquivos em um sistema de arquivos, mas essa correspondência não é necessária. Para fins de portabilidade máxima, recomenda-se que os arquivos em um sistema de arquivos ser codificado com UTF-8 codificação.

Conceitualmente, um programa é compilado em três etapas:

1. Transformação, que converte um arquivo de um esquema de codifica e o repertório de caracteres específica em uma sequência de caracteres Unicode.
2. Análise lexical, que converte um fluxo de caracteres de entrada de Unicode em um fluxo de tokens.
3. Análise sintática, que converte o fluxo de tokens em código executável.

## <a name="grammars"></a>Gramáticas

Essa especificação apresenta a sintaxe da linguagem c# linguagem de programação usando duas gramáticas. O ***gramática lexical*** ([gramática Lexical](lexical-structure.md#lexical-grammar)) define como os caracteres Unicode são combinadas para terminadores de linha de formulário, o espaço em branco, comentários, tokens e diretivas de pré-processamento. O ***gramática sintática*** ([gramática sintática](lexical-structure.md#syntactic-grammar)) define como os tokens resultantes da gramática lexical são combinados para programas de formulário em C#.

### <a name="grammar-notation"></a>Notação de gramática

As gramáticas lexicais e sintáticas são apresentadas na forma de Backus-Naur usando a notação da ferramenta ANTLR gramática.

### <a name="lexical-grammar"></a>Gramática lexical

A gramática lexical da linguagem c# é apresentada na [análise léxica](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), e [diretivas de pré-processamento](lexical-structure.md#pre-processing-directives). Os símbolos terminais da gramática lexical são os caracteres do conjunto de caracteres Unicode e a gramática lexical Especifica como os caracteres são combinadas para tokens de formulário ([Tokens](lexical-structure.md#tokens)), espaço em branco ([espaço em branco](lexical-structure.md#white-space)), comentários ([comentários](lexical-structure.md#comments)) e em diretivas de pré-processamento ([diretivas de pré-processamento](lexical-structure.md#pre-processing-directives)).

Cada arquivo de origem em um programa c# deve estar de acordo com o *entrada* produção da gramática lexical ([análise léxica](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Gramática sintática

A gramática sintática do c# é apresentada em capítulos e apêndices seguir este capítulo. Os símbolos de terminal da gramática sintático são os tokens definidos pela gramática lexical e sintática gramática Especifica como os tokens são combinados para programas em c# formulário.

Cada arquivo de origem em um programa c# deve estar de acordo com o *compilation_unit* produção da gramática sintática ([unidades de compilação](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Análise léxica

O *entrada* produção define a estrutura lexical de um arquivo de origem c#. Cada arquivo de origem em um programa c# deve estar de acordo com essa produção gramática lexical.

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

Cinco elementos básicos compõem a estrutura lexical de um arquivo de origem c#: terminadores de linha ([terminadores de linha](lexical-structure.md#line-terminators)), espaço em branco ([espaço em branco](lexical-structure.md#white-space)), comentários ([comentários](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)) e em diretivas de pré-processamento ([diretivas de pré-processamento](lexical-structure.md#pre-processing-directives)). Esses elementos básicos, somente os tokens são significativos na gramática sintática de um programa em c# ([gramática sintática](lexical-structure.md#syntactic-grammar)).

O processamento léxico de um arquivo de origem c# consiste em reduzir o arquivo em uma sequência de tokens que se torna a entrada para a análise sintática. Terminadores de linha, espaço em branco e comentários podem ser usado para separar os tokens e diretivas de pré-processamento podem causar a seções do arquivo de origem a serem ignoradas, mas caso contrário, esses elementos léxicos não têm impacto sobre a estrutura sintática de um programa c#.

No caso de literais de cadeia de caracteres interpolada ([interpoladas literais de cadeia de caracteres](lexical-structure.md#interpolated-string-literals)) um único token inicialmente é produzido pela análise lexical, mas é dividido em vários elementos de entrada que repetidamente estão sujeitos a análise léxica até que todos os literais de cadeia de caracteres interpolada foram resolvidas. Os tokens resultantes, em seguida, servem como entrada para a análise sintática.

Quando várias produções gramática lexical corresponderem a uma sequência de caracteres em um arquivo de origem, o processamento lexical sempre forma o elemento de léxico mais longo possível. Por exemplo, a sequência de caracteres `//` é processado como o início de um comentário de linha única, porque esse elemento lexical tem mais de um único `/` token.

### <a name="line-terminators"></a>Terminadores de linha

Terminadores de linha dividem os caracteres de um arquivo de código-fonte c# em linhas.

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

Para ferramentas de edição que adicionem marcadores de fim-de-arquivo de código de compatibilidade com o código-fonte, e para habilitar uma fonte de arquivo para ser exibido como uma sequência de corretamente encerrada linhas, as transformações a seguir são aplicadas em ordem, para cada arquivo de origem em um programa c#:

*  Se o último caractere do arquivo de origem for um caractere de controle-Z (`U+001A`), esse caractere é excluído.
*  Um caractere de retorno de carro (`U+000D`) é adicionado ao final do arquivo de origem se esse arquivo de origem estiver vazio e se o último caractere do arquivo de origem não é um retorno de carro (`U+000D`), uma alimentação de linha (`U+000A`), um separador de linha (`U+2028`), ou um separador de parágrafo (`U+2029`).

### <a name="comments"></a>Comentários

Duas formas de comentários são suportadas: comentários de linha única e comentários delimitados. ***Comentários de linha única*** começar com os caracteres `//` e estender ao final da linha de código-fonte. ***Delimitado por comentários*** começar com os caracteres `/*` e terminam com os caracteres `*/`. Comentários delimitados podem abranger várias linhas.

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

Não se aninham comentários. As sequências de caracteres `/*` e `*/` não têm significado especial em um `//` comentário e as sequências de caracteres `//` e `/*` não têm significado especial dentro de um comentário delimitado.

Comentários não são processados dentro de cadeia de caracteres literais.

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

Espaço em branco é definido como o caractere de alimentação de qualquer caractere com Unicode classe Zs (que inclui o caractere de espaço), bem como o caractere de tabulação horizontal, o caractere de tabulação vertical e o formulário.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Tokens

Há vários tipos de tokens: identificadores, palavras-chave, literais, operadores e pontuadores. Espaço em branco e os comentários não são tokens, embora eles atuam como separadores para tokens.

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

Uma sequência de escape de caractere Unicode representa um caractere Unicode. Sequências de escape de caractere Unicode são processadas em identificadores ([identificadores](lexical-structure.md#identifiers)), literais de caracteres ([literais de caracteres](lexical-structure.md#character-literals)) e literais de cadeia de caracteres regulares ([deliteraisdecadeiadecaracteres](lexical-structure.md#string-literals)). Um caractere de escape Unicode não é processado em qualquer outro local (por exemplo, para formar um operador, punctuator ou palavra-chave).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Uma sequência de escape Unicode representa o caractere Unicode formado pelos seguintes número hexadecimais de "`\u`"ou"`\U`" caracteres. Como o c# usa a codificação de 16 bits dos pontos de código Unicode em caracteres e valores de cadeia de caracteres, um caractere Unicode no intervalo de u+10000 a U + 10FFFF não é permitido em um literal de caractere e é representado usando um par de substitutos de Unicode em uma cadeia de caracteres literal. Não há suporte para caracteres Unicode com pontos de código acima 0x10FFFF.

Não são realizadas várias traduções. Por exemplo, a cadeia de caracteres literal "`\u005Cu005C`"é equivalente a"`\u005C`"em vez de"`\`". O valor Unicode `\u005C` é o caractere "`\`".

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
mostra vários usos de `\u0066`, que é a sequência de escape para a letra "`f`". O programa é equivalente a
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

As regras para identificadores fornecidas nesta seção correspondem exatamente ao recomendada pelo Unicode Standard Annex 31, exceto que o sublinhado é permitido como um caractere inicial (por exemplo, é tradicional na linguagem de programação C), são sequências de escape de Unicode permitido em identificadores e o "`@`" caractere é permitido como um prefixo para habilitar palavras-chave para ser usadas como identificadores.

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

Para obter informações sobre as classes de caractere Unicode mencionados acima, consulte o padrão Unicode, versão 3.0, seção 4.5.

Exemplos de identificadores válidos incluem "`identifier1`","`_identifier2`", e "`@if`".

Um identificador em um programa com conformidade deve ser no formato canônico definido pelo formulário de normalização Unicode C, conforme definido pelo Unicode Standard Annex 15. O comportamento quando se deparar com um identificador não está no formulário C de normalização é definido pela implementação; No entanto, um diagnóstico não é necessário.

O prefixo "`@`" permite o uso de palavras-chave como identificadores, que é útil ao fazer interface com outras linguagens de programação. O caractere `@` não é parte do identificador, portanto, o identificador pode ser visto em outros idiomas como um identificador normal, sem o prefixo. Um identificador com um `@` prefixo é chamado um ***identificador textual***. Usar o `@` prefixo para os identificadores que não são palavras-chave é permitido, mas altamente recomendado como uma questão de estilo.

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
define uma classe chamada "`class`"com um método estático denominado"`static`"que leva um parâmetro chamado"`bool`". Observe que uma vez que faz o escape Unicode não são permitidos em palavras-chave, o token "`cl\u0061ss`"é um identificador, e é o mesmo identificador que"`@class`".

Dois identificadores são considerados iguais se eles forem idênticos, depois que as transformações a seguir são aplicadas na ordem:

*  O prefixo "`@`", se usado, é removido.
*  Cada *unicode_escape_sequence* é transformado em seu caractere Unicode correspondente.
*  Qualquer *formatting_character*s são removidos.

Identificadores que contém dois consecutivas caracteres de sublinhado (`U+005F`) são reservados para uso pela implementação. Por exemplo, uma implementação pode fornecer palavras-chave estendidas que começam com dois sublinhados.

### <a name="keywords"></a>Palavras-chave

Um ***palavra-chave*** é uma sequência de tipo de identificador de caracteres que é reservado e não pode ser usada como um identificador, exceto quando precedida pela `@` caracteres.

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

Em alguns locais na gramática, identificadores específicos têm significado especial, mas não são palavras-chave. Esses identificadores às vezes são chamados de "palavras-chave contextuais". Por exemplo, dentro de uma declaração de propriedade, o "`get`"e"`set`" identificadores têm significado especial ([acessadores](classes.md#accessors)). Um identificador diferente de `get` ou `set` nunca é permitida nesses locais, portanto, esse uso não está em conflito com um uso destas palavras como identificadores. Em outros casos, assim como acontece com o identificador "`var`" nas declarações de variável locais digitadas implicitamente ([declarações de variável Local](statements.md#local-variable-declarations)), uma palavra-chave contextual pode entrar em conflito com nomes declarados. Nesses casos, o nome declarado prevalece sobre o uso do identificador como uma palavra-chave contextual.

### <a name="literals"></a>Literais

Um ***literal*** é uma representação de código de origem de um valor.

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

Há dois valores literais boolianos: `true` e `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

O tipo de um *boolean_literal* é `bool`.

#### <a name="integer-literals"></a>Literais inteiros

Literais inteiros são usados para gravar valores de tipos `int`, `uint`, `long`, e `ulong`. Literais inteiros tem duas formas possíveis: decimal e hexadecimal.

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

*  Se o literal não tem sufixo, ele tem o primeiro desses tipos em que seu valor pode ser representado: `int`, `uint`, `long`, `ulong`.
*  Se o literal é um sufixo formado por `U` ou `u`, ele tem o primeiro desses tipos em que seu valor pode ser representado: `uint`, `ulong`.
*  Se o literal é um sufixo formado por `L` ou `l`, ele tem o primeiro desses tipos em que seu valor pode ser representado: `long`, `ulong`.
*  Se o literal é um sufixo formado por `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, ou `lu`, ela é do tipo `ulong`.

Se o valor representado por um literal inteiro estiver fora do intervalo da `ulong` digitar, ocorre um erro de tempo de compilação.

Como uma questão de estilo, é aconselhável que "`L`"ser usada em vez de"`l`" ao escrever literais do tipo `long`, uma vez que ele é fácil confundir a letra "`l`"com o dígito"`1`".

Para permitir que o menor valor possível `int` e `long` valores a serem gravados como literais inteiro decimal, existem duas regras a seguir:

* Quando um *decimal_integer_literal* com o valor 2147483648 (2 ^ 31) e não há *integer_type_suffix* aparece como o token imediatamente após um menos o token de operador unário ([menos unário operador](expressions.md#unary-minus-operator)), o resultado é uma constante do tipo `int` com o valor de -2147483648 (-2 ^ 31). Em outras situações, tal uma *decimal_integer_literal* é do tipo `uint`.
* Quando um *decimal_integer_literal* com o valor 9223372036854775808 (2 ^ 63) e nenhuma *integer_type_suffix* ou o *integer_type_suffix* `L` ou `l` aparece como o token imediatamente após um menos o token de operador unário ([operador de subtração unário](expressions.md#unary-minus-operator)), o resultado é uma constante do tipo `long` com o valor de -9223372036854775808 (-2 ^ 63). Em outras situações, tal uma *decimal_integer_literal* é do tipo `ulong`.

#### <a name="real-literals"></a>Literais reais

Literais reais são usados para gravar valores de tipos `float`, `double`, e `decimal`.

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

Se nenhum *real_type_suffix* for especificado, o tipo do literal real é `double`. Caso contrário, o sufixo de tipo real determina o tipo do literal real, da seguinte maneira:

*  Um literal real tendo como sufixado `F` ou `f` é do tipo `float`. Por exemplo, os literais `1f`, `1.5f`, `1e10f`, e `123.456F` são todos do tipo `float`.
*  Um literal real tendo como sufixado `D` ou `d` é do tipo `double`. Por exemplo, os literais `1d`, `1.5d`, `1e10d`, e `123.456D` são todos do tipo `double`.
*  Um literal real tendo como sufixado `M` ou `m` é do tipo `decimal`. Por exemplo, os literais `1m`, `1.5m`, `1e10m`, e `123.456M` são todos do tipo `decimal`. Este literal é convertido em um `decimal` valor, considerando o valor exato e, se necessário, arredondando para o próximo valor representável usando arredondamento bancário ([o tipo decimal](types.md#the-decimal-type)). Qualquer escala aparente no literal é preservada, a menos que o valor é arredondado ou o valor é zero (no último caso, a entrada e a escala será 0). Portanto, o literal `2.900m` será analisado para formar o decimal com sinal `0`, coeficiente `2900`e a escala `3`.

Se o literal especificado não pode ser representado no tipo indicado, ocorrerá um erro de tempo de compilação.

O valor de um literal real do tipo `float` ou `double` é determinado usando o padrão IEEE modo "arredondamento mais próximo".

Observe que um literal real, os dígitos decimais sempre são necessários após a vírgula decimal. Por exemplo, `1.3F` é um literal real mas `1.F` não é.

#### <a name="character-literals"></a>Literais de caracteres

Um literal de caractere representa um único caractere e geralmente consiste em um caractere entre aspas, como em `'a'`.

Observação: A notação de gramática ANTLR faz o seguinte confuso! No ANTLR, quando você escreve `\'` ela significa uma aspa simples `'`. E quando você escreve `\\` significa de uma única barra invertida `\`. Portanto a primeira regra de um caractere literal significa que ele começa com uma aspa, seguido de um caractere e uma aspa simples. E são sequências de escape simples possíveis onze `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

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

Um caractere que segue um caractere de barra invertida (`\`) em um *caractere* deve ser um dos seguintes caracteres: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Caso contrário, ocorrerá um erro de tempo de compilação.

Uma sequência de escape hexadecimal representa um único caractere Unicode, com o valor formado pelos seguintes número hexadecimais "`\x`".

Se o valor representado por um literal de caractere é maior que `U+FFFF`, ocorre um erro de tempo de compilação.

Uma sequência de escape de caractere Unicode ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)) em um caractere literal deve estar no intervalo `U+0000` para `U+FFFF`.

Uma sequência de escape simples representa uma codificação de caracteres de Unicode, conforme descrito na tabela a seguir.


| __Sequência de escape__ | __Nome de caractere__ | __Codificação Unicode__ |
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

C# oferece suporte a dois formatos de literais de cadeia de caracteres: ***literais de cadeia de caracteres regulares*** e ***literais de cadeia de caracteres textuais***.

Uma cadeia de caracteres regular literal consiste em zero ou mais caracteres entre aspas duplas, como em `"hello"`e pode incluir ambas as sequências de escape simples (como `\t` para o caractere de tabulação) e hexadecimal e sequências de escape Unicode.

Uma cadeia de caracteres textual literal consiste em um `@` caractere seguido por um caractere de aspas duplas, zero ou mais caracteres e um caractere de aspas duplas de fechamento. Um exemplo simples é `@"hello"`. Em uma cadeia de caracteres textual literal, os caracteres entre os delimitadores são interpretados literalmente, a única exceção é um *quote_escape_sequence*. Em particular, sequências de escape simples e hexadecimal e sequências de escape Unicode não são processadas em literais de cadeia de caracteres textuais. Uma cadeia de caracteres textual literal pode abranger várias linhas.

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

Um caractere que segue um caractere de barra invertida (`\`) em um *regular_string_literal_character* deve ser um dos seguintes caracteres: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Caso contrário, ocorrerá um erro de tempo de compilação.

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
mostra uma variedade de literais de cadeia de caracteres. A última cadeia de caracteres literal, `j`, é uma cadeia de caracteres textual literal que abrange várias linhas. Os caracteres entre aspas, incluindo espaço em branco, como caracteres de nova linha são preservados textuais.

Como uma sequência de escape hexadecimal pode ter um número variável de dígitos hexadecimais, a cadeia de caracteres literal `"\x123"` contém um único caractere com um valor hexadecimal 123. Para criar uma cadeia de caracteres que contém o caractere com o valor hexadecimal 12 seguido pelo caractere 3, um poderia escrever `"\x00123"` ou `"\x12" + "3"` em vez disso.

O tipo de um *string_literal* é `string`.

Cada cadeia de caracteres literal não resulta necessariamente em uma nova instância de cadeia de caracteres. Quando dois ou mais literais de cadeia de caracteres que são equivalentes de acordo com o operador de igualdade de cadeia de caracteres ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)) são exibidos no mesmo programa, esses literais de cadeia de caracteres se referem à mesma instância de cadeia de caracteres. Por exemplo, a saída é produzido por
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
é `True` porque os dois literais se referem à mesma instância de cadeia de caracteres.

#### <a name="interpolated-string-literals"></a>Literais de cadeia de caracteres interpolada

Literais de cadeia de caracteres interpolada são semelhantes às literais de cadeia de caracteres, mas conter orifícios delimitados por `{` e `}`, no qual as expressões podem ocorrer. Em tempo de execução, as expressões são avaliadas com a finalidade de ter suas formas textuais substituídas na cadeia de caracteres no local onde ocorre a falha. A sintaxe e semântica de interpolação de cadeia de caracteres é descrita na seção ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)).

Como literais de cadeia de caracteres literais de cadeia de caracteres interpolada podem ser regular ou textual. Literais de cadeia de caracteres interpolada regulares são delimitadas por `$"` e `"`, e literais de cadeia de caracteres textual interpolada são delimitadas por `$@"` e `"`.

Como outros literais, a análise léxica de uma cadeia de caracteres interpolada literal inicialmente resulta em um único token, de acordo com a gramática abaixo. No entanto, antes da análise sintática, o token único de uma cadeia de caracteres interpolada literal é dividido em vários tokens para as partes da cadeia de caracteres, colocando os buracos e os elementos de entrada que ocorrem os buracos são lexicalmente analisados novamente. Por sua vez, isso pode produzir mais interpolada literais de cadeia de caracteres para ser processada, mas, se lexicalmente corrigir, acabará levando a uma sequência de tokens para análise sintática processar.

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

Uma *interpolated_string_literal* token é reinterpretado como vários tokens e outros elementos de entrada da seguinte maneira, em ordem de ocorrência a *interpolated_string_literal*:

* Ocorrências do seguinte são reinterpretadas como tokens individuais separados: o entrelinhamento `$` sinal *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* e *interpolated_verbatim_string_end*.
* Ocorrências de *regular_balanced_text* e *verbatim_balanced_text* entre essas são reprocessados como um *input_section* ([análise léxica ](lexical-structure.md#lexical-analysis)) e são reinterpretado como a sequência resultante dos elementos de entrada. Por sua vez, eles podem incluir tokens literais de cadeia de caracteres interpolada sejam reinterpretados.

Análise sintática será recombinar os tokens em um *interpolated_string_expression* ([cadeias de caracteres interpoladas](expressions.md#interpolated-strings)).

Exemplos de tarefas Pendentes


#### <a name="the-null-literal"></a>O literal nulo

```antlr
null_literal
    : 'null'
    ;
```

O *null_literal* pode ser convertido implicitamente em um tipo de referência ou tipo anulável.

### <a name="operators-and-punctuators"></a>Pontuadores e operadores

Há vários tipos de operadores e pontuadores. Operadores são usados em expressões para descrever as operações que envolvem um ou mais operandos. Por exemplo, a expressão `a + b` usa o `+` para adicionar os dois operandos `a` e `b`. Pontuadores são para agrupamento e a separação.

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

A barra vertical na *right_shift* e *right_shift_assignment* produções são usadas para indicar que, ao contrário de outras produções na gramática sintática, nenhum caractere de qualquer tipo (nem mesmo espaço em branco) são permitidos entre os tokens. Esses produções são tratadas especialmente para permitir a manipulação correta de *type_parameter_list*s ([parâmetros de tipo](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Diretivas de pré-processamento

As diretivas de pré-processamento fornecem a capacidade de ignorar condicionalmente seções dos arquivos de origem, com erro de relatório e as condições de aviso e para delinear regiões distintas do código-fonte. O termo "Pré-processando diretivas" é usado apenas para manter a consistência com as linguagens de programação C e C++. No c#, não há nenhuma etapa de pré-processamento separada; Pré-processando diretivas são processadas como parte da fase de análise léxica.

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

*  `#define` e `#undef`, que são usados para definir e remover, respectivamente, símbolos de compilação condicional ([diretivas de declaração](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, e `#endif`, que são usados para condicionalmente ignorar seções do código-fonte ([diretivas de compilação condicional](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, que é usado para controlar os números de linha emitidos para erros e avisos ([diretivas de linha](lexical-structure.md#line-directives)).
*  `#error` e `#warning`, que são usados para emitir os erros e avisos, respectivamente ([diretivas diagnóstico](lexical-structure.md#diagnostic-directives)).
*  `#region` e `#endregion`, que são usados para marcar explicitamente seções do código-fonte ([diretivas de região](lexical-structure.md#region-directives)).
*  `#pragma`, que é usado para especificar as informações contextuais para o compilador opcionais ([diretivas Pragma](lexical-structure.md#pragma-directives)).

Uma diretiva de pré-processamento sempre ocupa uma linha separada do código-fonte e sempre começa com um `#` caractere e um nome de diretiva de pré-processamento. Espaço em branco podem ocorrer antes do `#` caractere e entre o `#` caractere e o nome da diretiva.

Uma linha de código-fonte que contém um `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, ou `#endregion` diretiva pode terminar com um comentário de linha única. Delimitado por comentários (o `/* */` estilo de comentários) não são permitidos em linhas de código-fonte que contém as diretivas de pré-processamento.

Pré-processando diretivas não são tokens e não fazem parte da gramática da linguagem c# sintática. No entanto, as diretivas de pré-processamento podem ser usadas para incluir ou excluir as sequências de tokens e podem dessa forma afetar o significado de um programa c#. Por exemplo, quando o programa compilado:
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
resultados na mesma sequência exata dos tokens que o programa:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Portanto, ao passo que lexicalmente, os dois programas são bastante diferentes, sintaticamente, eles são idênticos.

### <a name="conditional-compilation-symbols"></a>Símbolos de compilação condicional

A funcionalidade de compilação condicional fornecida pelo `#if`, `#elif`, `#else`, e `#endif` diretivas é controlado por meio de expressões de pré-processamento ([pré-processamento de expressões](lexical-structure.md#pre-processing-expressions)) e símbolos de compilação condicional.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Um símbolo de compilação condicional tem dois estados possíveis: ***definidos*** ou ***indefinido***. No início do processamento léxico de um arquivo de origem, um símbolo de compilação condicional é indefinido, a menos que ele foi explicitamente definido por um mecanismo externo (como uma opção de compilador de linha de comando). Quando um `#define` diretiva é processada, o símbolo de compilação condicional nomeado nessa diretiva se torna definido no arquivo de origem. O símbolo permanece definido até que um `#undef` diretiva desse símbolo mesmo é processado, ou até o final do arquivo de origem for atingido. Uma implicação disso é que `#define` e `#undef` diretivas em um arquivo de origem não têm nenhum efeito em outros arquivos de origem no mesmo programa.

Quando referenciado em uma expressão de pré-processamento, um símbolo de compilação condicional definido tem o valor booliano `true`, e um símbolo de compilação condicional indefinido tem o valor booliano `false`. Não há nenhum requisito de que os símbolos de compilação condicional ser explicitamente declarada antes que eles são referenciados em expressões de pré-processamento. Em vez disso, os símbolos não declarados são simplesmente indefinidos e, portanto, têm o valor `false`.

O espaço de nome para símbolos de compilação condicional é distinto e separado de todas as outras entidades nomeadas em um programa c#. Símbolos de compilação condicional só podem ser referenciados em `#define` e `#undef` diretivas e em expressões de pré-processamento.

### <a name="pre-processing-expressions"></a>Pré-processamento de expressões

Expressões de pré-processamento pode ocorrer em `#if` e `#elif` diretivas. Os operadores `!`, `==`, `!=`, `&&` e `||` são permitidos em expressões, o pré-processamento e os parênteses podem ser usados para agrupamento.

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

Quando referenciado em uma expressão de pré-processamento, um símbolo de compilação condicional definido tem o valor booliano `true`, e um símbolo de compilação condicional indefinido tem o valor booliano `false`.

Avaliação de uma expressão de pré-processamento sempre gera um valor booliano. As regras de avaliação de uma expressão de pré-processamento são os mesmos para uma expressão de constante ([expressões constantes](expressions.md#constant-expressions)), exceto que as únicas entidades definidas pelo usuário que podem ser referenciadas são símbolos de compilação condicional .

### <a name="declaration-directives"></a>Diretivas de declaração

As diretivas de declaração são usadas para definir ou excluir definições de símbolos de compilação condicional.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

O processamento de um `#define` diretiva faz com que o símbolo de compilação condicional determinado tornam-se definido, começando com a linha de código-fonte que segue a diretiva. Da mesma forma, o processamento de um `#undef` diretiva faz com que o símbolo de compilação condicional determinado se torne indefinido, começando com a linha de código-fonte que segue a diretiva.

Qualquer `#define` e `#undef` diretivas em um arquivo de origem devem ocorrer antes da primeira *token* ([Tokens](lexical-structure.md#tokens)) no arquivo de origem; caso contrário, um erro de tempo de compilação ocorrerá. Em termos de intuitivos `#define` e `#undef` diretivas devem preceder qualquer "código real" no arquivo de origem.

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
é válido porque o `#define` diretivas precedem o primeiro token (o `namespace` palavra-chave) no arquivo de origem.

O exemplo a seguir resulta em um erro de tempo de compilação porque uma `#define` segue o código real:
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

Um `#define` podem definir um símbolo de compilação condicional que já esteja definido, sem a existência de qualquer intervenção `#undef` para esse símbolo. O exemplo a seguir define um símbolo de compilação condicional `A` e, em seguida, define-o novamente.
```csharp
#define A
#define A
```

Um `#undef` pode "indefina" um símbolo de compilação condicional não está definido. O exemplo a seguir define um símbolo de compilação condicional `A` e, em seguida, exclusões de definição-lo duas vezes; Embora a segunda `#undef` não tem nenhum efeito, ela ainda é válida.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Diretivas de compilação condicional

As diretivas de compilação condicional são usadas para incluir ou excluir partes de um arquivo de origem condicionalmente.

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

Conforme indicado pela sintaxe, diretivas de compilação condicional devem ser escritas como conjuntos composto, em ordem, uma `#if` diretiva, zero ou mais `#elif` diretivas, zero ou uma `#else` diretiva e um `#endif` diretiva. Entre as diretivas são seções condicionais do código-fonte. Cada seção é controlada pela diretiva imediatamente anterior. Uma seção condicional pode conter diretivas de compilação condicional aninhada desde que essas diretivas formam conjuntos completos.

Um *pp_conditional* seleciona no máximo uma independente *conditional_section*s para o processamento normal de léxico:

*  O *pp_expression*s da `#if` e `#elif` diretivas são avaliadas na ordem até que um produz `true`. Se uma expressão gera `true`, o *conditional_section* da diretiva correspondente é selecionado.
*  Se todos os *pp_expression*yield s `false`e se um `#else` diretiva estiver presente, o *conditional_section* da `#else` diretiva é selecionada.
*  Caso contrário, não *conditional_section* está selecionado.

Selecionado *conditional_section*, se houver, é processado como um normal *input_section*: o código-fonte contido na seção deve aderir a gramática lexical; tokens são gerados a partir do código-fonte código na seção; e pré-processando diretivas na seção tem os efeitos prescritos.

O restante *conditional_section*s, se houver, são processados como *skipped_section*s: exceto para as diretivas de pré-processamento, o código-fonte na seção precisa estiver de acordo com o léxico gramática; não tokens são gerados a partir do código-fonte na seção; e pré-processando diretivas na seção devem ser lexicalmente corretas, mas caso contrário, não são processadas. Dentro de um *conditional_section* que está sendo processada como uma *skipped_section*qualquer aninhada *conditional_section*s (contidos no aninhadas `#if`... `#endif` e `#region`... `#endregion` constrói) também são processados como *skipped_section*s.

O exemplo a seguir ilustra a compilação condicional como aninham diretivas:
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

Exceto para pré-processando diretivas, código-fonte foi ignorada não está sujeito à análise lexical. Por exemplo, o seguinte é válido, apesar do comentário não finalizado no `#else` seção:
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

No entanto, observe que as diretivas de pré-processamento são necessárias para ser lexicalmente correto, mesmo nas seções ignoradas do código-fonte.

Pré-processando diretivas não são processadas quando elas aparecerem dentro de elementos de entrada multilinhas. Por exemplo, o programa:
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
resultados na saída:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

Em casos peculiares, o conjunto de diretivas de pré-processamento que é processado pode depender da avaliação do *pp_expression*. O Exemplo:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
sempre produz o mesmo fluxo de token (`class` `Q` `{` `}`), independentemente de se deseja ou não `X` está definido. Se `X` é definido, as diretivas de processados somente são `#if` e `#endif`, devido ao comentário de várias linha. Se `X` é indefinida, em seguida, três diretivas (`#if`, `#else`, `#endif`) são parte do conjunto de diretiva.

### <a name="diagnostic-directives"></a>Diretivas de diagnóstico

As diretivas de diagnóstico são usadas para gerar mensagens de erro e aviso que são relatadas da mesma maneira que outros erros de tempo de compilação e avisos.

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
sempre produz um aviso ("revisão de código necessário antes do check-in") e produz um erro de tempo de compilação ("uma compilação não pode ser depuração e varejo") se os símbolos condicionais `Debug` e `Retail` forem definidos. Observe que um *pp_message* pode conter texto arbitrário; especificamente, ele não precisa conter tokens bem formados, conforme mostrado pela aspa simples na palavra `can't`.

### <a name="region-directives"></a>Diretivas de região

As diretivas de região são usadas para marcar explicitamente as regiões do código-fonte.

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

Nenhum significado semântico é anexado a uma região; regiões destinam para uso pelo programador ou por ferramentas automatizadas para marcar uma seção do código-fonte. A mensagem especificada em uma `#region` ou `#endregion` diretiva da mesma forma não tem nenhum significado semântico; ele simplesmente serve para identificar a região. Correspondência `#region` e `#endregion` diretivas podem ter diferentes *pp_message*s.

O processamento de léxico de uma região:
```csharp
#region
...
#endregion
```
corresponde exatamente ao processamento de léxico de uma diretiva de compilação condicional do formulário:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Diretivas de linha

Diretivas de linha podem ser usadas para alterar os números de linha e nomes de arquivo de origem que são relatados pelo compilador na saída, como erros e avisos, e que são usados por atributos de informações do chamador ([atributos de informações do chamador](attributes.md#caller-info-attributes)).

Diretivas de linha são mais comumente usadas nas ferramentas de metaprogramação que geram o código-fonte c# de algum outro texto de entrada.

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

Quando nenhum `#line` diretivas estiverem presentes, o compilador relatará os números de linha true e nomes de arquivo de origem na sua saída. Durante o processamento de uma `#line` diretiva inclui um *line_indicator* que não é `default`, o compilador trata a linha após a diretiva como tendo o número de linha especificado (e o nome do arquivo, se especificado).

Um `#line default` diretiva inverte o efeito de todas as diretivas #line anterior. O compilador relata informações de linha de true para linhas subsequentes, exatamente como se não `#line` diretivas tinham sido processadas.

Um `#line hidden` diretiva não tem nenhum efeito sobre o arquivo e os números de linha relatados em erro, mensagem, mas afetam a depuração no nível do código-fonte. Durante a depuração, todas as linhas entre um `#line hidden` diretiva e as subsequentes `#line` diretiva (que não é `#line hidden`) não ter nenhuma informação de número de linha. Ao percorrer o código no depurador, essas linhas serão ignoradas inteiramente.

Observe que um *file_name* difere de um literal de cadeia regular caracteres de escape não são processadas; a "`\`" caractere simplesmente designa um caractere de barra invertida comum dentro de um *file_name*.

### <a name="pragma-directives"></a>Diretivas pragma

O `#pragma` diretiva de pré-processamento é usada para especificar as informações contextuais opcionais para o compilador. As informações fornecidas em um `#pragma` diretiva nunca será alterado para a semântica do programa.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

O c# fornece `#pragma` diretivas para controlar avisos do compilador. Futuras versões da linguagem podem incluir adicionais `#pragma` diretivas. Para garantir a interoperabilidade com outros compiladores do c#, o compilador Microsoft c# não emite erros de compilação para o desconhecido `#pragma` diretivas; tal não de diretivas porém gerar avisos.

#### <a name="pragma-warning"></a>Aviso de pragma

O `#pragma warning` diretiva é usada para desabilitar ou restaurar todos os ou mensagens de um conjunto específico de aviso durante a compilação de texto do programa subsequentes.

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

Um `#pragma warning` diretiva que omite a lista de avisos afeta todos os avisos. Um `#pragma warning` diretiva o inclui uma lista de avisos afeta apenas os avisos que são especificados na lista.

Um `#pragma warning disable` diretiva desabilita todos os ou o conjunto determinado de avisos.

Um `#pragma warning restore` diretiva restaurará todos os ou o conjunto determinado de avisos para o estado que estava em vigor no início da unidade de compilação. Observe que, se um aviso específico foi desabilitado externamente, um `#pragma warning restore` (se é para todos os ou o aviso específico) não habilitará esse aviso novamente.

O exemplo a seguir mostra o uso de `#pragma warning` desabilitar temporariamente o aviso relatado quando obsoleto membros são referenciados, usando o número de aviso do compilador do Microsoft c#.
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
