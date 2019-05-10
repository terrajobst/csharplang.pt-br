---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488796"
---
# <a name="unsafe-code"></a>Código não seguro

O núcleo de linguagem c#, conforme definido nos capítulos anteriores, difere da C e C++ em sua omissão de ponteiros como um tipo de dados. Em vez disso, o c# fornece referências e a capacidade de criar objetos que são gerenciados por um coletor de lixo. Esse design, juntamente com outros recursos, torna c# uma linguagem mais segura que C ou C++. Na linguagem principal c# simplesmente não é possível ter uma variável não inicializada, um ponteiro "pendentes" ou uma expressão que indexa uma matriz, além de seus limites. Categorias inteiras de bugs que rotineiramente assolam C e programas do C++, portanto, são eliminados.

Enquanto praticamente cada constructo de tipo de ponteiro em C ou C++ tem um equivalente de tipo de referência em c#, no entanto, há situações em que o acesso aos tipos de ponteiro se torna uma necessidade. Por exemplo, fazer interface com o sistema operacional subjacente, acessar um dispositivo de memória mapeada ou implementação de um algoritmo de tempo crítico pode não ser possível ou prático sem acesso a ponteiros. Para resolver essa necessidade, c# fornece a capacidade de gravar ***código não seguro***.

Em código não seguro, é possível declarar e operar em ponteiros para realizar conversões entre os ponteiros e tipos integrais, para obter o endereço de variáveis e assim por diante. De certa forma, escrever o código não seguro é semelhante a escrever código C dentro de um programa c#.

Na verdade, o código não seguro é um recurso de "seguro" da perspectiva de desenvolvedores e usuários. O código não seguro deve ser claramente marcado com o modificador `unsafe`, portanto, os desenvolvedores, possivelmente, não é possível usar os recursos não seguros acidentalmente e o mecanismo de execução trabalha para garantir que o código não seguro não pode ser executado em um ambiente não confiável.

## <a name="unsafe-contexts"></a>Contextos não seguros

Os recursos que não é seguros do c# estão disponíveis apenas em contextos que não é seguros. Um contexto não seguro é introduzido com a inclusão de um `unsafe` modificador na declaração de um tipo ou membro, ou empregando uma *unsafe_statement*:

*  Uma declaração de uma classe, struct, interface ou delegado pode incluir um `unsafe` modificador, na qual o caso de toda a extensão textual dessa declaração de tipo (incluindo o corpo da classe, struct ou interface) é considerado um contexto não seguro.
*  Uma declaração de um campo, método, propriedade, evento, indexador, operador, construtor de instância, destruidor ou construtor estático pode incluir um `unsafe` modificador, nesse caso toda a extensão textual da declaração de membro que é considerado um inseguro contexto.
*  Uma *unsafe_statement* permite o uso de um contexto não seguro dentro de uma *bloco*. Toda a extensão textual do associado *bloco* é considerado um contexto não seguro.

As produções gramática associados são mostradas abaixo.

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

No exemplo

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

o `unsafe` modificador especificado na declaração de struct faz com que toda a extensão textual da declaração de struct para se tornar um contexto não seguro. Portanto, é possível declarar o `Left` e `Right` campos para um tipo de ponteiro. O exemplo anterior também poderia ser escrito

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Aqui, o `unsafe` modificadores nas declarações de campo fazem com que essas declarações a serem considerados contextos não seguros.

Diferente de estabelecer um contexto inseguro, permitindo assim o uso de tipos de ponteiro, o `unsafe` modificador não tem nenhum efeito em um tipo ou membro. No exemplo

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

o `unsafe` modificador em de `F` método na `A` simplesmente faz com que a extensão textual do `F` para se tornar um contexto não seguro no qual os recursos que não é seguros da linguagem podem ser usados. Na substituição do `F` na `B`, não é necessário especificar novamente as `unsafe` modificador –, a menos que, é claro, o `F` método na `B` em si precisa ter acesso aos recursos que não é seguros.

A situação é ligeiramente diferente quando um tipo de ponteiro é parte da assinatura do método

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

Aqui, pois `F`da assinatura inclui um tipo de ponteiro, ele só pode ser gravado em um contexto inseguro. No entanto, o contexto não seguro pode ser introduzido, ambos fazendo a classe inteira não seguro, como é o caso na `A`, ou com a inclusão de um `unsafe` modificador na declaração de método, como é o caso no `B`.

## <a name="pointer-types"></a>Tipos de ponteiro

Em um contexto inseguro, um *tipo* ([tipos](types.md)) pode ser um *pointer_type* , bem como a *value_type* ou uma *reference_type* . No entanto, uma *pointer_type* também pode ser usado em uma `typeof` expressão ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) fora de um contexto não seguro como tal uso não é não seguro.

```antlr
type_unsafe
    : pointer_type
    ;
```

Um *pointer_type* é gravado como um *unmanaged_type* ou a palavra-chave `void`, seguido por um `*` token:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

O tipo especificado antes do `*` em um ponteiro de tipo é chamado de ***tipo referentes*** do tipo ponteiro. Ele representa o tipo da variável para o qual um valor do tipo ponteiro aponta.

Ao contrário de referências (valores de tipos de referência), ponteiros não são rastreados pelo coletor de lixo, pois o coletor de lixo não tem conhecimento dos ponteiros e os dados aos quais eles apontam. Por esse motivo, um ponteiro não é permitido para apontar para uma referência ou para um struct que contém referências e o tipo de referentes a de um ponteiro deve ser um *unmanaged_type*.

Uma *unmanaged_type* é qualquer tipo que não é um *reference_type* ou o tipo construído e não contém *reference_type* ou construído de campos de tipo em qualquer nível de aninhamento. Em outras palavras, uma *unmanaged_type* é um dos seguintes:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, ou `bool`.
*  Qualquer *enum_type*.
*  Qualquer *pointer_type*.
*  Qualquer definidos pelo usuário *struct_type* que não é um tipo construído e contém campos de *unmanaged_type*somente s.

A regra intuitiva para a mixagem de ponteiros e referências é que referents de referências (objetos) são permitidos para conter os ponteiros, mas referents de ponteiros não são permitidos para conter referências.

Alguns exemplos de tipos de ponteiro são fornecidos na tabela a seguir:

| __Exemplo__ | __Descrição__                               |
|-------------|-----------------------------------------------|
| `byte*`     | ponteiro para `byte`                             |
| `char*`     | ponteiro para `char`                             |
| `int**`     | Ponteiro para ponteiro `int`                   |
| `int*[]`    | Matriz unidimensional de ponteiros para `int` |
| `void*`     | Ponteiro para tipo desconhecido                       |

Para uma determinada implementação, todos os tipos de ponteiro devem ter o mesmo tamanho e representação.

Ao contrário do C e C++, quando vários ponteiros são declarados na mesma declaração, em c# a `*` é gravada junto com o tipo subjacente apenas, não como um pontuador do prefixo no nome de cada ponteiro. Por exemplo

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

O valor de um ponteiro que têm o tipo `T*` representa o endereço de uma variável do tipo `T`. O operador de indireção de ponteiro `*` ([indireção de ponteiro](unsafe-code.md#pointer-indirection)) pode ser usado para acessar essa variável. Por exemplo, dada uma variável `P` do tipo `int*`, a expressão `*P` denota a `int` variável encontrada no endereço contido na `P`.

Como uma referência de objeto, um ponteiro pode estar `null`. Aplicando o operador de indireção a um `null` ponteiro resulta no comportamento definido pela implementação. Um ponteiro com o valor `null` é representado por zeros bits.

O `void*` tipo representa um ponteiro para um tipo desconhecido. Como o tipo referentes é desconhecido, o operador de indireção não pode ser aplicado a um ponteiro de tipo `void*`, nem qualquer aritmética pode ser realizada em tal um ponteiro. No entanto, um ponteiro de tipo `void*` pode ser convertido em qualquer outro tipo de ponteiro (e vice-versa).

Tipos de ponteiro são uma categoria separada de tipos. Diferentemente dos tipos de referência e tipos de valor, tipos de ponteiro não herdam `object` e não há nenhuma conversão entre tipos de ponteiro e `object`. Em particular, conversões boxing e unboxing ([conversões Boxing e unboxing](types.md#boxing-and-unboxing)) não têm suporte para ponteiros. No entanto, as conversões são permitidas entre diferentes tipos de ponteiro e entre tipos de ponteiro e tipos integrais. Isso é descrito em [conversões de ponteiro](unsafe-code.md#pointer-conversions).

Um *pointer_type* não pode ser usado como um argumento de tipo ([construído tipos](types.md#constructed-types)) e Inferência de tipo ([inferência de tipo](expressions.md#type-inference)) Falha em chamadas de método genérico que seriam ter inferido um tipo de argumento seja um tipo de ponteiro.

Um *pointer_type* pode ser usado como o tipo de um campo volátil ([campos voláteis](classes.md#volatile-fields)).

Embora os ponteiros podem ser passados como `ref` ou `out` parâmetros, fazer isso pode causar um comportamento indefinido, uma vez que o ponteiro pode também ser definida para apontar para uma variável local que não existe mais quando é retornado do método chamado ou o objeto fixo para o qual ele usado para apontar, não é fixo. Por exemplo:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

Um método pode retornar um valor de algum tipo, e esse tipo pode ser um ponteiro. Por exemplo, ao receber um ponteiro para uma sequência contígua de `int`s, contagem de elementos da sequência que e alguns outros `int` valor, o método a seguir retorna o endereço desse valor na sequência, se ocorrer uma correspondência; caso contrário, retornará `null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

Em um contexto inseguro, várias construções estão disponíveis para a operação em ponteiros:

*  O `*` operador pode ser usado para executar indireção de ponteiro ([indireção de ponteiro](unsafe-code.md#pointer-indirection)).
*  O `->` operador pode ser usado para acessar um membro de um struct através de um ponteiro ([acesso de membro do ponteiro](unsafe-code.md#pointer-member-access)).
*  O `[]` operador pode ser usado para indexar um ponteiro ([acesso de elemento de ponteiro](unsafe-code.md#pointer-element-access)).
*  O `&` operador pode ser usado para obter o endereço de uma variável ([o operador address-of](unsafe-code.md#the-address-of-operator)).
*  O `++` e `--` operadores podem ser usados para ponteiros de incremento e decremento ([ponteiro incremento e decremento](unsafe-code.md#pointer-increment-and-decrement)).
*  O `+` e `-` operadores podem ser usados para realizar aritmética de ponteiro ([aritmética de ponteiro](unsafe-code.md#pointer-arithmetic)).
*  O `==`, `!=`, `<`, `>`, `<=`, e `=>` operadores podem ser usados para comparar ponteiros ([comparação de ponteiros](unsafe-code.md#pointer-comparison)).
*  O `stackalloc` operador pode ser usado para alocar memória da pilha de chamadas ([buffers de tamanho fixo](unsafe-code.md#fixed-size-buffers)).
*  O `fixed` instrução pode ser usada para corrigir temporariamente uma variável para que seu endereço pode ser obtido ([a instrução fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Variáveis fixas e móveis

O operador address-of ([o operador address-of](unsafe-code.md#the-address-of-operator)) e o `fixed` instrução ([a instrução fixed](unsafe-code.md#the-fixed-statement)) dividir as variáveis em duas categorias: ***Corrigido variáveis*** e ***variáveis moveable***.

Variáveis fixas residem em locais de armazenamento que não são afetados pela operação do coletor de lixo. (Exemplos de variáveis fixas incluem variáveis locais, parâmetros de valor e variáveis criadas por desreferenciar ponteiros.) Por outro lado, as variáveis moveable residem em locais de armazenamento que estão sujeitos a realocação ou a eliminação pelo coletor de lixo. (Variáveis moveable exemplos de campos em elementos de matrizes e objetos.)

O `&` operador ([o operador address-of](unsafe-code.md#the-address-of-operator)) permite que o endereço de uma variável fixa a ser obtido sem restrições. No entanto, como uma variável móvel está sujeito a realocação ou a eliminação pelo coletor de lixo, o endereço de uma variável móvel só pode ser obtido usando um `fixed` instrução ([a instrução fixed](unsafe-code.md#the-fixed-statement)) e esse endereço permanece válido apenas para a duração do que `fixed` instrução.

Em termos de precisos, uma variável fixa é um dos seguintes:

*  Uma variável resultante de uma *simple_name* ([nomes simples](expressions.md#simple-names)) que se refere a uma variável local ou um parâmetro de valor, a menos que a variável é capturada por uma função anônima.
*  Uma variável resultante de uma *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `V.I`, onde `V` é uma variável fixa de um *struct_type*.
*  Uma variável resultante de uma *pointer_indirection_expression* ([indireção de ponteiro](unsafe-code.md#pointer-indirection)) do formulário `*P`, uma *pointer_member_access* ([Acesso de membro do ponteiro](unsafe-code.md#pointer-member-access)) no formato `P->I`, ou uma *pointer_element_access* ([acesso de elemento de ponteiro](unsafe-code.md#pointer-element-access)) do formulário `P[E]`.

Todas as outras variáveis são classificados como moveable variáveis.

Observe que um campo estático é classificado como uma variável móvel. Observe também que um `ref` ou `out` parâmetro é classificado como uma variável móvel, mesmo se o argumento fornecido para o parâmetro é uma variável fixa. Por fim, observe que uma variável produzido por um ponteiro de remoção de referência sempre é classificado como uma variável fixa.

## <a name="pointer-conversions"></a>Conversões de ponteiro

Em um contexto inseguro, o conjunto de conversões implícitas disponíveis ([conversões implícitas](conversions.md#implicit-conversions)) é estendido para incluir as conversões de ponteiro implícitas a seguir:

*  De qualquer *pointer_type* para o tipo `void*`.
*  Dos `null` literal a qualquer *pointer_type*.

Além disso, em um contexto não seguro, o conjunto de conversões explícitas disponíveis ([conversões explícitas](conversions.md#explicit-conversions)) é estendido para incluir as seguintes conversões de ponteiro explícitas:

*  De qualquer *pointer_type* a qualquer outra *pointer_type*.
*  Partir `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, ou `ulong` para qualquer *pointer_type*.
*  De qualquer *pointer_type* à `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, ou `ulong`.

Por fim, em um contexto não seguro, o conjunto de conversões implícitas padrão ([conversões implícitas padrão](conversions.md#standard-implicit-conversions)) inclui a conversão de ponteiro a seguir:

*  De qualquer *pointer_type* para o tipo `void*`.

Conversões entre dois tipos de ponteiro nunca altere o valor do ponteiro real. Em outras palavras, uma conversão de tipo de um ponteiro para outro não tem efeito no endereço base fornecido pelo ponteiro.

Quando um tipo de ponteiro é convertido para outro, se o ponteiro resultante não está alinhado corretamente para o tipo apontado, o comportamento será indefinido se o resultado for desreferenciado. Em geral, o conceito de "alinhado corretamente" é transitivo: se um ponteiro para o tipo `A` esteja alinhada corretamente para um ponteiro para o tipo `B`, que, por sua vez, está alinhado corretamente com um ponteiro para o tipo `C`, em seguida, um ponteiro para tipo `A`esteja alinhada corretamente para um ponteiro para o tipo `C`.

Considere o seguinte caso em que uma variável com um tipo é acessada por meio de um ponteiro para um tipo diferente:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Quando um tipo de ponteiro é convertido em um ponteiro para um byte, os pontos de resultado para o menor byte endereçado da variável. Incrementos sucessivos do resultado, até o tamanho da variável, produzem ponteiros para os bytes restantes da variável. Por exemplo, o método a seguir exibe cada um dos oito bytes em um duplo como um valor hexadecimal:

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

Obviamente, a saída produzida depende de ordenação de bits.

Mapeamentos entre os ponteiros e inteiros são definidos pela implementação. No entanto, em 32 * e arquiteturas de CPU de 64 bits com um espaço de endereço linear, conversões de ponteiros de tipos integrais geralmente se comporta exatamente como as conversões de `uint` ou `ulong` valores, respectivamente, para ou a partir dos tipos integrais.

### <a name="pointer-arrays"></a>Matrizes de ponteiro

Em um contexto inseguro, matrizes de ponteiros podem ser construídos. Apenas algumas das conversões que se aplicam a outros tipos de matriz são permitidas em matrizes de ponteiro:

*  A conversão de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) de qualquer *array_type* para `System.Array` e as interfaces que ele implementa também se aplica a matrizes de ponteiro. No entanto, qualquer tentativa de acessar os elementos de matriz por meio `System.Array` ou as interfaces que ele implementa resultará em uma exceção em tempo de execução, como tipos de ponteiro não são conversíveis `object`.
*  Conversões de referência implícita e explícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions), [conversões de referência explícita](conversions.md#explicit-reference-conversions)) de um tipo de matriz unidimensional `S[]` para `System.Collections.Generic.IList<T>` e suas interfaces de base genéricos nunca se aplicam a matrizes de ponteiro, como tipos de ponteiro não podem ser usados como argumentos de tipo e não há nenhuma conversões de tipos de ponteiro para os tipos de não ponteiro.
*  A conversão de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)) do `System.Array` e as interfaces que ele implementa a qualquer *array_type* se aplica a matrizes de ponteiro.
*  As conversões de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)) do `System.Collections.Generic.IList<S>` e sua base de interfaces para um tipo de matriz unidimensional `T[]` nunca se aplica a matrizes de ponteiro, como tipos de ponteiro não podem ser usados como argumentos de tipo, e não há nenhuma conversões de tipos de ponteiro para os tipos de não ponteiro.

Essas restrições significam que a expansão para o `foreach` instrução em matrizes descrito [a instrução foreach](statements.md#the-foreach-statement) não pode ser aplicado às matrizes de ponteiro. Em vez disso, uma instrução foreach do formulário

```csharp
foreach (V v in x) embedded_statement
```

em que o tipo de `x` é um tipo de matriz no formato `T[,,...,]`, `N` é o número de dimensões menos 1 e `T` ou `V` é um tipo de ponteiro, é expandido com o uso de loops aninhados da seguinte maneira:

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

As variáveis `a`, `i0`, `i1`,..., `iN` não estão visíveis nem acessíveis ao `x` ou o *embedded_statement* ou qualquer outro código-fonte do programa. A variável `v` é somente leitura na instrução inserida. Se não houver uma conversão explícita ([conversões de ponteiro](unsafe-code.md#pointer-conversions)) do `T` (o tipo de elemento) para `V`, um erro é produzido e nenhuma outra etapa será efetuada. Se `x` tem o valor `null`, um `System.NullReferenceException` é gerada em tempo de execução.

## <a name="pointers-in-expressions"></a>Ponteiros em expressões

Em um contexto não seguro, uma expressão pode produzir um resultado de um tipo de ponteiro, mas fora de um contexto não seguro é um erro de tempo de compilação para uma expressão é de um tipo de ponteiro. Em termos de precisos, fora de um contexto sem segurança um erro de tempo de compilação ocorre se houver *simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro ](expressions.md#member-access)), *invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)), ou *element_access* ([acesso de elemento](expressions.md#element-access)) é de um tipo de ponteiro.

Em um contexto inseguro, a *primary_no_array_creation_expression* ([expressões primárias](expressions.md#primary-expressions)) e *unary_expression* ([osoperadoresunários](expressions.md#unary-operators)) produções permitem as seguintes construções adicionais:

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

Essas construções são descritas nas seções a seguir. Precedência e associatividade dos operadores unsafe é indicado pela gramática.

### <a name="pointer-indirection"></a>Indireção de ponteiro

Um *pointer_indirection_expression* consiste em um asterisco (`*`) seguido por um *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

O operador unário `*` operador denota indireção de ponteiro e é usado para obter a variável para o qual um ponteiro aponta. O resultado da avaliação `*P`, onde `P` é uma expressão de um tipo de ponteiro `T*`, é uma variável do tipo `T`. Ele é um erro de tempo de compilação para aplicar o operador unário `*` operador em uma expressão de tipo `void*` ou como uma expressão que não seja de um tipo de ponteiro.

O efeito de aplicar o operador unário `*` operador para um `null` ponteiro é definido pela implementação. Em particular, não há nenhuma garantia de que essa operação lançar um `System.NullReferenceException`.

Se tiver sido atribuído a um valor inválido para o ponteiro, o comportamento do operador unário `*` operador é indefinido. Entre os valores inválidos para desreferenciar um ponteiro, o operador unário `*` operadores têm um endereço alinhado de forma inadequada para o tipo apontado (consulte o exemplo [conversões de ponteiro](unsafe-code.md#pointer-conversions)) e o endereço de uma variável após o fim do seu tempo de vida.

Para fins de análise de atribuição definitiva, uma variável produzidos ao avaliar uma expressão do formulário `*P` é considerada inicialmente atribuída ([inicialmente atribuída variáveis](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Acesso de membro do ponteiro

Um *pointer_member_access* consiste em um *primary_expression*, seguido por um "`->`" token, seguido por um *identificador* e um opcional *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

Em um acesso de membro do ponteiro do formulário `P->I`, `P` deve ser uma expressão de um tipo de ponteiro diferente de `void*`, e `I` deve indicar um membro acessível do tipo ao qual `P` pontos.

Um acesso de membro do ponteiro do formulário `P->I` é avaliado exatamente como `(*P).I`. Para obter uma descrição do operador de indireção de ponteiro (`*`), consulte [indireção de ponteiro](unsafe-code.md#pointer-indirection). Para obter uma descrição do operador de acesso de membro (`.`), consulte [acesso de membro](expressions.md#member-access).

No exemplo

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

o `->` operador é usado para acessar os campos e invocar um método de um struct através de um ponteiro. Porque a operação `P->I` é precisamente equivalente a `(*P).I`, o `Main` método poderia igualmente bem ter sido escrito:

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>Acesso de elemento de ponteiro

Um *pointer_element_access* consiste em um *primary_no_array_creation_expression* seguido por uma expressão entre "`[`"e"`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

Em um acesso de elemento de ponteiro do formulário `P[E]`, `P` deve ser uma expressão de um tipo de ponteiro diferente de `void*`, e `E` deve ser uma expressão que pode ser convertida implicitamente em `int`, `uint`, `long`, ou `ulong`.

Um acesso de elemento de ponteiro do formulário `P[E]` é avaliado exatamente como `*(P + E)`. Para obter uma descrição do operador de indireção de ponteiro (`*`), consulte [indireção de ponteiro](unsafe-code.md#pointer-indirection). Para obter uma descrição do operador de adição de ponteiro (`+`), consulte [aritmética de ponteiro](unsafe-code.md#pointer-arithmetic).

No exemplo

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

um acesso de elemento de ponteiro é usado para inicializar o buffer de caracteres em um `for` loop. Porque a operação `P[E]` é precisamente equivalente a `*(P + E)`, o exemplo poderia igualmente bem ter sido escrito:

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

O operador de acesso de elemento de ponteiro não verifica se existe fora dos limites erros e o comportamento ao acessar um fora dos limites de elemento é indefinido. Isso é o mesmo que o C e C++.

### <a name="the-address-of-operator"></a>O operador address-of

Uma *addressof_expression* consiste em um e comercial (`&`) seguido por um *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Considerando uma expressão `E` que é do tipo `T` e é classificado como uma variável fixa ([fixo e variáveis moveable](unsafe-code.md#fixed-and-moveable-variables)), a construção `&E` calcula o endereço da variável fornecida pelo `E`. O tipo do resultado é `T*` e é classificado como um valor. Ocorrerá um erro de tempo de compilação se `E` não é classificado como uma variável, se `E` é classificado como uma variável local somente leitura, ou se `E` denota uma variável móvel. No último caso, uma instrução fixed ([a instrução fixed](unsafe-code.md#the-fixed-statement)) pode ser usado para "corrigir" temporariamente a variável antes de obter seu endereço. Conforme mencionado na [acesso de membro](expressions.md#member-access), fora de um construtor de instância ou um construtor estático para um struct ou classe que define um `readonly` campo, esse campo é considerado um valor, não uma variável. Como tal, seu endereço não pode ser removido. Da mesma forma, o endereço de uma constante não pode ser executado.

O `&` operador não requer seu argumento ser definitivamente atribuído, mas seguir uma `&` operação, a variável à qual o operador é aplicado é considerada definitivamente atribuída no caminho de execução em que a operação ocorre. É responsabilidade do programador garantir que a inicialização correta da variável ocorrem realmente nessa situação.

No exemplo

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` é considerado atribuído definitivamente seguindo a `&i` operação usada para inicializar `p`. A atribuição ao `*p` em vigor inicializa `i`, mas a inclusão dessa inicialização é responsabilidade do programador, e nenhum erro de tempo de compilação ocorrerá se a atribuição foi removida.

As regras de atribuição definitiva para o `&` operador existe, de modo que pode ser evitada com redundância de inicialização de variáveis locais. Por exemplo, muitas APIs externas levam um ponteiro para uma estrutura que será preenchida pela API. Chamadas para essas APIs normalmente passam o endereço de uma variável local de struct, e sem a regra, com redundância de inicialização da variável struct seria necessária.

### <a name="pointer-increment-and-decrement"></a>Decremento e incremento do ponteiro

Em um contexto inseguro, a `++` e `--` operadores ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators) e [incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)) pode ser aplicado a ponteiro variáveis de todos os tipos exceto `void*`. Assim, para cada tipo de ponteiro `T*`, os operadores a seguir são definidos implicitamente:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Os operadores produzem os mesmos resultados que `x + 1` e `x - 1`, respectivamente ([aritmética de ponteiro](unsafe-code.md#pointer-arithmetic)). Em outras palavras, para uma variável de ponteiro do tipo `T*`, o `++` operador adiciona `sizeof(T)` para o endereço contido na variável e o `--` operador subtrai `sizeof(T)` do endereço contido na variável.

Se um incremento de ponteiro ou decremento operação estoura o domínio do tipo ponteiro, o resultado é definido pela implementação, mas nenhuma exceção é produzidas.

### <a name="pointer-arithmetic"></a>Aritmética de ponteiro

Em um contexto inseguro, a `+` e `-` operadores ([operador de adição](expressions.md#addition-operator) e [operador de subtração](expressions.md#subtraction-operator)) podem ser aplicadas a valores de todos os tipos de ponteiro, exceto `void*`. Assim, para cada tipo de ponteiro `T*`, os operadores a seguir são definidos implicitamente:

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

Considerando uma expressão `P` de um tipo de ponteiro `T*` e uma expressão `N` do tipo `int`, `uint`, `long`, ou `ulong`, as expressões `P + N` e `N + P` computar o valor do ponteiro do tipo `T*` resultante da adição `N * sizeof(T)` para o endereço fornecido pelo `P`. Da mesma forma, a expressão `P - N` calcula o valor do ponteiro do tipo `T*` que resulta da subtração `N * sizeof(T)` do endereço, fornecido por `P`.

Duas expressões, dadas `P` e `Q`, de um tipo de ponteiro `T*`, a expressão `P - Q` calcula a diferença entre os endereços dados pelos `P` e `Q` e, em seguida, divide essa diferença pelo `sizeof(T)`. O tipo do resultado é sempre `long`. Na verdade, `P - Q` é computado como `((long)(P) - (long)(Q)) / sizeof(T)`.

Por exemplo:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

que produz a saída:

```
p - q = -14
q - p = 14
```

Se uma operação aritmética de ponteiro estoura o domínio do tipo de ponteiro, o resultado será truncado de maneira definida pela implementação, mas nenhuma exceção é produzidas.

### <a name="pointer-comparison"></a>Comparação de ponteiros

Em um contexto inseguro, a `==`, `!=`, `<`, `>`, `<=`, e `=>` operadores ([operadores de teste de tipo e relacional](expressions.md#relational-and-type-testing-operators)) pode ser aplicada a valores de todos os tipos de ponteiro. Os operadores de comparação de ponteiro são:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Porque existe uma conversão implícita de qualquer tipo de ponteiro para o `void*` tipo, os operandos de qualquer tipo de ponteiro pode ser comparado usando esses operadores. Os operadores de comparação comparam os endereços dados pelos dois operandos, como se fossem inteiros sem sinal.

### <a name="the-sizeof-operator"></a>O operador sizeof

O `sizeof` operador retorna o número de bytes ocupados por uma variável de um determinado tipo. O tipo especificado como um operando `sizeof` deve ser um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

O resultado do `sizeof` operador é um valor do tipo `int`. Para determinados tipos predefinidos, o `sizeof` operador produz um valor constante, como mostrado na tabela a seguir.


| __Expressão__   | __Result__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

Para todos os outros tipos, o resultado do `sizeof` operador é definido pela implementação e é classificado como um valor, não uma constante.

A ordem na qual os membros são incluídos em um struct é especificada.

Para fins de alinhamento, pode haver sem nome no início de um struct, dentro de um struct e no final da estrutura de preenchimento. O conteúdo dos bits usados como o preenchimento é indeterminado.

Quando aplicado a um operando que tem o tipo de struct, o resultado é o número total de bytes em uma variável desse tipo, incluindo qualquer preenchimento.

## <a name="the-fixed-statement"></a>A instrução fixed

Em um contexto inseguro, a *embedded_statement* ([instruções](statements.md)) produção permite que uma construção adicional, o `fixed` instrução, que é usada para "corrigir" uma variável móvel, de modo que seu endereço permanecerá constante para a duração da instrução.

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

Cada *fixed_pointer_declarator* declara uma variável local da determinado *pointer_type* e inicializa a variável local com o endereço computado pelo correspondente *fixed_ pointer_initializer*. Uma variável local declarada em um `fixed` instrução é acessível em qualquer *fixed_pointer_initializer*s ocorrendo para a direita da declaração da variável e, no *embedded_statement* da `fixed` instrução. Uma variável local declarada por um `fixed` instrução é considerada como somente leitura. Um erro de tempo de compilação ocorrerá se a instrução inserida tenta modificar essa variável local (por meio da atribuição ou o `++` e `--` operadores) ou passá-lo como um `ref` ou `out` parâmetro.

Um *fixed_pointer_initializer* pode ser uma das seguintes opções:

*  O token "`&`" seguido por um *variable_reference* ([regras precisas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) para uma variável móvel ([fixo e variáveis moveable](unsafe-code.md#fixed-and-moveable-variables)) de um tipo não gerenciado `T`, fornecido o tipo `T*` é implicitamente conversível para o tipo de ponteiro fornecido no `fixed` instrução. Nesse caso, o inicializador calcula o endereço da variável determinada, e a variável é assegurada de permanecer em um endereço fixo durante a `fixed` instrução.
*  Uma expressão de um *array_type* com os elementos de um tipo não gerenciado `T`, fornecido o tipo `T*` é implicitamente conversível para o tipo de ponteiro fornecido no `fixed` instrução. Nesse caso, o inicializador calcula o endereço do primeiro elemento na matriz, e toda a matriz é assegurada de permanecer em um endereço fixo durante a `fixed` instrução. Se a expressão de matriz é nula ou se a matriz com zero elementos, o inicializador calcula endereço igual a zero.
*  Uma expressão do tipo `string`, fornecido o tipo `char*` é implicitamente conversível para o tipo de ponteiro fornecido no `fixed` instrução. Nesse caso, o inicializador calcula o endereço do primeiro caractere na cadeia de caracteres e cadeia de caracteres inteira é garantido que permanecem em um endereço fixo durante a `fixed` instrução. O comportamento do `fixed` instrução é definido pela implementação se a expressão de cadeia de caracteres for nula.
*  Um *simple_name* ou *member_access* que faz referência a um membro de buffer de tamanho fixo de uma variável móvel, desde que o tipo do membro de buffer de tamanho fixo é implicitamente conversível para o tipo de ponteiro fornecido no `fixed` instrução. Nesse caso, o inicializador calcula um ponteiro para o primeiro elemento do buffer de tamanho fixo ([buffers de tamanho fixo em expressões](unsafe-code.md#fixed-size-buffers-in-expressions)), e o buffer de tamanho fixo é assegurado de permanecer em um endereço fixo durante a `fixed`instrução.

Para cada endereço computado por um *fixed_pointer_initializer* o `fixed` instrução garante que a variável referenciada pelo endereço não está sujeita às realocação ou a eliminação pelo coletor de lixo durante a `fixed` instrução. Por exemplo, se o endereço é calculado por uma *fixed_pointer_initializer* faz referência a um campo de um objeto ou um elemento de uma instância de matriz, o `fixed` instrução garante que a instância do objeto contentor não for realocada ou descartado durante a vida útil da instrução.

É responsabilidade do programador para garantir que os ponteiros criado pelo `fixed` instruções não sobrevivem além da execução dessas instruções. Por exemplo, quando ponteiros criada por `fixed` declarações são passadas para APIs externas, é responsabilidade do programador garantir que as APIs de mantenham nenhuma memória desses ponteiros.

Objetos fixo podem causar a fragmentação do heap (porque eles não podem ser movidos). Por esse motivo, os objetos devem ser corrigidos somente quando absolutamente necessário e, em seguida, apenas para a menor quantidade de tempo possível.

O exemplo

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

demonstra vários usos do `fixed` instrução. A primeira instrução correções e obtém o endereço de um campo estático, a segunda instrução correções e obtém o endereço de um campo de instância e a terceira instrução correções e obtém o endereço de um elemento de matriz. Em cada caso ele teria sido um erro usar regulares `&` operador, pois as variáveis são classificadas como variáveis moveable.

O quarto `fixed` instrução no exemplo acima produz um resultado semelhante ao terceiro.

Este exemplo do `fixed` instrução usa `string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

Em um contexto inseguro elementos da matriz de matrizes unidimensionais são armazenados em ordem crescente de índice, começando com o índice `0` e terminando com um índice `Length - 1`. Para matrizes multidimensionais, matriz, de modo que os índices da dimensão mais à direita são aumentados em primeiro lugar, os elementos são armazenados e em seguida, a próxima esquerda dimensão, e assim por diante para a esquerda. Dentro de um `fixed` instrução que obtém um ponteiro `p` a uma instância de matriz `a`, os valores de ponteiro que variam de `p` para `p + a.Length - 1` representam endereços dos elementos na matriz. Da mesma forma, as variáveis desde `p[0]` para `p[a.Length - 1]` representam os elementos da matriz real. Dada a maneira na qual as matrizes são armazenadas, é possível tratar uma matriz de qualquer dimensão como se fosse linear.

Por exemplo:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

que produz a saída:

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

No exemplo

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

um `fixed` instrução é usada para corrigir uma matriz, portanto, seu endereço pode ser passado para um método que usa um ponteiro.

No exemplo:

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

uma instrução fixed é usada para corrigir um buffer de tamanho fixo de um struct para que seu endereço possa ser usado como um ponteiro.

Um `char*` valor produzido por corrigir a uma instância de cadeia de caracteres sempre aponta para uma cadeia de caracteres terminada em nulo. Dentro de uma instrução fixa que obtém um ponteiro `p` a uma instância de cadeia de caracteres `s`, os valores de ponteiro desde `p` ao `p + s.Length - 1` representam endereços dos caracteres na cadeia de caracteres e o valor do ponteiro `p + s.Length` sempre aponta para um caractere nulo (o caractere com valor `'\0'`).

Modificação de objetos do tipo gerenciado por meio de ponteiros fixos pode resulta em comportamento inesperado. Por exemplo, como cadeias de caracteres são imutáveis, é responsabilidade do programador garantir que os caracteres referenciados por um ponteiro para uma cadeia de caracteres fixa não são modificados.

A terminação null automática de cadeias de caracteres é especialmente conveniente ao chamar as APIs externas que esperam cadeias de caracteres "C-style". No entanto, observe que uma instância de cadeia de caracteres pode conter caracteres nulos. Se tais caracteres nulos estão presentes, a cadeia de caracteres apareçam truncada quando tratada como uma terminação nula `char*`.

## <a name="fixed-size-buffers"></a>Buffers de tamanho fixo

Buffers de tamanho fixo são usados para declarar matrizes na linha de "Estilo C" como membros de structs e são úteis principalmente para fazer interface com APIs não gerenciadas.

### <a name="fixed-size-buffer-declarations"></a>Declarações de buffer de tamanho fixo

Um ***buffer de tamanho fixo*** é um membro que representa o armazenamento para um buffer de comprimento fixo de variáveis de um determinado tipo. Uma declaração de buffer de tamanho fixo apresenta um ou mais buffers de tamanho fixo de um tipo de elemento especificado. Buffers de tamanho fixo são permitidos somente em declarações de struct e podem ocorrer somente em contextos não seguros ([contextos não seguros](unsafe-code.md#unsafe-contexts)).

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

Uma declaração de buffer de tamanho fixo pode incluir um conjunto de atributos ([atributos](attributes.md)), um `new` modificador ([modificadores](classes.md#modifiers)), uma combinação válida de as quatro modificadores de acesso ([tipo parâmetros e restrições](classes.md#type-parameters-and-constraints)) e uma `unsafe` modificador ([contextos não seguros](unsafe-code.md#unsafe-contexts)). Os atributos e modificadores se aplicam a todos os membros declarados pela declaração de buffer de tamanho fixo. É um erro para o mesmo modificador aparecer várias vezes em uma declaração de buffer de tamanho fixo.

Uma declaração de buffer de tamanho fixo não é permitida para incluir o `static` modificador.

O tipo de elemento do buffer de uma declaração de buffer de tamanho fixo Especifica o tipo de elemento do buffer (s) introduzidas pela declaração. O tipo de elemento do buffer deve ser um dos tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, ou `bool`.

O tipo de elemento do buffer é seguido por uma lista de declaradores de buffer de tamanho fixo, cada uma delas apresenta um novo membro. Um Declarador de buffer de tamanho fixo consiste em um identificador que nomeia o membro, seguido por uma expressão constante entre `[` e `]` tokens. A expressão de constante denota o número de elementos no membro introduzidos por esse Declarador de buffer de tamanho fixo. O tipo da expressão constante deve ser implicitamente conversível para o tipo `int`, e o valor deve ser um número inteiro positivo diferente de zero.

Os elementos de um buffer de tamanho fixo têm garantia de ser dispostos sequencialmente na memória.

Uma declaração de buffer de tamanho fixo que declara vários buffers de tamanho fixo é equivalente a várias declarações de uma declaração de buffer de tamanho fixo único com os mesmos atributos e tipos de elemento. Por exemplo

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

equivale a

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>Buffers de tamanho fixo em expressões

Pesquisa de membro ([operadores](expressions.md#operators)) de tamanho fixo membro buffer continua exatamente como a pesquisa de membro de um campo.

Um buffer de tamanho fixo pode ser referenciado em uma expressão que usa um *simple_name* ([inferência](expressions.md#type-inference)) ou uma *member_access* ([verificação de tempo de compilação resolução de sobrecarga dinâmico](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Quando um membro de buffer de tamanho fixo é referenciado como um nome simples, o efeito é o mesmo que um acesso de membro do formulário `this.I`, onde `I` é o membro de buffer de tamanho fixo.

Em um acesso de membro do formulário `E.I`, se `E` é de um tipo de struct e uma pesquisa de membro de `I` em que o tipo de struct identifica um membro de tamanho fixo, em seguida, `E.I` é avaliada uma classificados da seguinte maneira:

*  Se a expressão `E.I` não ocorre em um contexto inseguro, ocorre um erro de tempo de compilação.
*  Se `E` é classificado como um valor, ocorre um erro de tempo de compilação.
*  Caso contrário, se `E` for uma variável móvel ([fixo e variáveis moveable](unsafe-code.md#fixed-and-moveable-variables)) e a expressão `E.I` não é um *fixed_pointer_initializer* ([fixa instrução](unsafe-code.md#the-fixed-statement)), ocorre um erro de tempo de compilação.
*  Caso contrário, `E` faz referência a uma variável fixa e o resultado da expressão é um ponteiro para o primeiro elemento do membro de buffer de tamanho fixo `I` em `E`. O resultado é do tipo `S*`, onde `S` é o tipo de elemento de `I`e é classificado como um valor.

Os elementos subsequentes do buffer de tamanho fixo podem ser acessados usando operações de ponteiro do primeiro elemento. Ao contrário de acesso a matrizes, o acesso aos elementos de um buffer de tamanho fixo é uma operação não segura e não é verificado de intervalo.

O exemplo a seguir declara e usa uma estrutura com um membro de buffer de tamanho fixo.

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>Verificando a atribuição definida

Buffers de tamanho fixo não estão sujeitos a verificação de atribuição definitiva ([atribuição definitiva](variables.md#definite-assignment)), e membros de buffer de tamanho fixo são ignorados para fins de verificação de variáveis do tipo struct de atribuição definitiva.

Quando a variável de estrutura que contém mais externo de um membro de buffer de tamanho fixo é uma variável estática, uma variável de instância de uma instância da classe ou um elemento de matriz, os elementos do buffer de tamanho fixo são inicializados automaticamente para seus valores padrão ([Valores padrão](variables.md#default-values)). Em todos os outros casos, o conteúdo inicial de um buffer de tamanho fixo é indefinido.

## <a name="stack-allocation"></a>Alocação da pilha

Em um contexto inseguro, uma declaração de variável local ([declarações de variável Local](statements.md#local-variable-declarations)) pode conter um inicializador de alocação de pilha que aloca memória da pilha de chamadas.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

O *unmanaged_type* indica o tipo dos itens que serão armazenados no local alocado recentemente, e o *expressão* indica o número desses itens. Juntos, eles especificam o tamanho de alocação necessários. Uma vez que o tamanho de uma alocação de pilha não pode ser negativo, ele é um erro de tempo de compilação para especificar o número de itens como um *constant_expression* que é avaliada como um valor negativo.

Um inicializador de alocação da pilha do formulário `stackalloc T[E]` requer `T` para ser um tipo não gerenciado ([tipos de ponteiro](unsafe-code.md#pointer-types)) e `E` como uma expressão do tipo `int`. A construção aloca `E * sizeof(T)` bytes da chamada de pilha e retorna um ponteiro, do tipo `T*`, para o bloco recém-alocada. Se `E` é um valor negativo, em seguida, o comportamento será indefinido. Se `E` for zero, então nenhuma alocação é feita, e o ponteiro retornado é definido pela implementação. Se não houver memória suficiente disponível para alocar um bloco de determinado tamanho, um `System.StackOverflowException` é gerada.

O conteúdo da memória recém-alocada é indefinido.

Inicializadores de alocação de pilha não são permitidos em `catch` ou `finally` blocos ([a instrução try](statements.md#the-try-statement)).

Não é possível liberar explicitamente a memória alocada usando `stackalloc`. Todos os blocos de memória alocado por pilha criados durante a execução de um membro da função são descartados automaticamente quando essa função membro retorna. Isso corresponde à `alloca` função, uma extensão geralmente encontrados em implementações de C e C++.

No exemplo

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

uma `stackalloc` inicializador é usado no `IntToString` método alocar um buffer de 16 caracteres na pilha. O buffer é descartado automaticamente quando o método retorna.

## <a name="dynamic-memory-allocation"></a>Alocação de memória dinâmica

Exceto para o `stackalloc` operador, c# não fornece construções nenhum predefinidas para gerenciamento de memória de não-lixo coletada. Esses serviços normalmente são fornecidos, oferecendo suporte a bibliotecas de classes ou importados diretamente do sistema operacional subjacente. Por exemplo, o `Memory` classe abaixo ilustra como as funções de heap de um sistema operacional subjacente podem ser acessadas do c#:

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

Um exemplo que usa o `Memory` classe é fornecido abaixo:

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

O exemplo aloca 256 bytes de memória por meio de `Memory.Alloc` e inicializa o bloco de memória com valores de aumento de 0 a 255. Em seguida, aloca uma matriz de bytes de 256 elemento e usa `Memory.Copy` para copiar o conteúdo do bloco de memória para a matriz de bytes. Por fim, o bloco de memória é liberado usando `Memory.Free` e o conteúdo da matriz de bytes é a saída no console.
