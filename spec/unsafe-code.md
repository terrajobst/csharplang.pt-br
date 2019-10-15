---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310366"
---
# <a name="unsafe-code"></a>Código não seguro

A linguagem C# principal, conforme definido nos capítulos anteriores, difere notavelmente de C e C++ em sua omissão de ponteiros como um tipo de dados. Em vez C# disso, o fornece referências e a capacidade de criar objetos gerenciados por um coletor de lixo. Esse design, juntamente com outros recursos, torna C# uma linguagem muito mais segura do que C ou C++. No idioma principal C# , simplesmente não é possível ter uma variável não inicializada, um ponteiro "pendente" ou uma expressão que indexe uma matriz além de seus limites. Categorias inteiras de bugs que, rotineiramente, C++ C e programas são eliminados.

Embora praticamente todos os tipos de ponteiro constructem em C ou C++ tenham um C#tipo de referência equivalente em, no entanto, há situações em que o acesso a tipos de ponteiro se torna uma necessidade. Por exemplo, a interface com o sistema operacional subjacente, o acesso a um dispositivo mapeado por memória ou a implementação de um algoritmo de tempo crítico pode não ser possível ou prática sem acesso a ponteiros. Para atender a essa necessidade C# , o fornece a capacidade de escrever ***código não seguro***.

Em código não seguro, é possível declarar e operar em ponteiros, executar conversões entre ponteiros e tipos integrais, para obter o endereço de variáveis e assim por diante. De certa forma, escrever código inseguro é muito parecido com escrever código C dentro C# de um programa.

O código não seguro é, de fato, um recurso "seguro" da perspectiva de desenvolvedores e usuários. O código não seguro deve ser claramente marcado com o modificador `unsafe`, de modo que os desenvolvedores não podem possivelmente usar recursos não seguros acidentalmente, e o mecanismo de execução funciona para garantir que o código não seguro não possa ser executado em um ambiente não confiável.

## <a name="unsafe-contexts"></a>Contextos não seguros

Os recursos não seguros do C# estão disponíveis somente em contextos não seguros. Um contexto não seguro é introduzido incluindo um modificador `unsafe` na declaração de um tipo ou membro ou empregando um *unsafe_statement*:

*  Uma declaração de Class, struct, interface ou delegate pode incluir um modificador `unsafe` e, nesse caso, toda a extensão textual dessa declaração de tipo (incluindo o corpo da classe, struct ou interface) é considerada um contexto não seguro.
*  Uma declaração de um campo, método, propriedade, evento, indexador, operador, Construtor de instância, destruidor ou construtor estático pode incluir um modificador `unsafe`, caso em que a extensão textual inteira dessa declaração de membro é considerada um contexto não seguro.
*  Um *unsafe_statement* permite o uso de um contexto sem segurança dentro de um *bloco*. Toda a extensão textual do *bloco* associado é considerada um contexto não seguro.

As produções gramaticais associadas são mostradas abaixo.

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

o modificador `unsafe` especificado na declaração struct faz com que toda a extensão textual da declaração struct se torne um contexto não seguro. Portanto, é possível declarar os campos `Left` e `Right` como sendo de um tipo de ponteiro. O exemplo acima também poderia ser escrito

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Aqui, os modificadores `unsafe` nas declarações de campo fazem com que essas declarações sejam consideradas contextos não seguros.

Além de estabelecer um contexto não seguro, permitindo, portanto, o uso de tipos de ponteiro, o modificador `unsafe` não tem nenhum efeito sobre um tipo ou um membro. No exemplo

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

o modificador `unsafe` no método `F` em `A` simplesmente faz com que a extensão textual de `F` se torne um contexto não seguro no qual os recursos não seguros do idioma podem ser usados. Na substituição de `F` em `B`, não é necessário especificar novamente o modificador `unsafe`--a menos que, é claro, o método `F` no `B` em si precisa acessar recursos não seguros.

A situação é um pouco diferente quando um tipo de ponteiro faz parte da assinatura do método

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

Aqui, como a assinatura do `F` inclui um tipo de ponteiro, ela só pode ser gravada em um contexto sem segurança. No entanto, o contexto não seguro pode ser introduzido, tornando a classe inteira insegura, como é o caso no `A`, ou incluindo um modificador `unsafe` na declaração do método, como é o caso no `B`.

## <a name="pointer-types"></a>Tipos de ponteiro

Em um contexto não seguro, um *tipo* ([tipos](types.md)) pode ser um *pointer_type* , bem como um *value_type* ou um *reference_type*. No entanto, um *pointer_type* também pode ser usado em uma expressão `typeof` ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) fora de um contexto sem segurança, pois tal uso não é seguro.

```antlr
type_unsafe
    : pointer_type
    ;
```

Um *pointer_type* é escrito como um *unmanaged_type* ou a palavra-chave `void`, seguido por um token `*`:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

O tipo especificado antes do `*` em um tipo de ponteiro é chamado de ***tipo Referent*** do tipo de ponteiro. Representa o tipo da variável para a qual um valor do tipo de ponteiro aponta.

Ao contrário das referências (valores de tipos de referência), os ponteiros não são rastreados pelo coletor de lixo – o coletor de lixo não tem conhecimento de ponteiros e dos dados para os quais eles apontam. Por esse motivo, um ponteiro não tem permissão para apontar para uma referência ou para uma struct que contém referências, e o tipo Referent de um ponteiro deve ser um *unmanaged_type*.

Um *unmanaged_type* é qualquer tipo que não seja um tipo *reference_type* ou construído e não contém *reference_type* ou campos de tipo construído em qualquer nível de aninhamento. Em outras palavras, um *unmanaged_type* é um dos seguintes:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 ou 2.
*  Qualquer *enum_type*.
*  Qualquer *pointer_type*.
*  Qualquer *struct_type* definida pelo usuário que não seja um tipo construído e contém campos de *unmanaged_type*s.

A regra intuitiva para a combinação de ponteiros e referências é que referents de referências (objetos) têm permissão para conter ponteiros, mas referents de ponteiros não tem permissão para conter referências.

Alguns exemplos de tipos de ponteiro são fornecidos na tabela a seguir:

| __Exemplo__ | __Descrição__                               |
|-------------|-----------------------------------------------|
| `byte*`     | Ponteiro para `byte`                             |
| `char*`     | Ponteiro para `char`                             |
| `int**`     | Ponteiro para ponteiro para `int`                   |
| `int*[]`    | Matriz unidimensional de ponteiros para `int` |
| `void*`     | Ponteiro para tipo desconhecido                       |

Para uma determinada implementação, todos os tipos de ponteiro devem ter o mesmo tamanho e representação.

Ao contrário de C++C e, quando vários ponteiros são declarados na C# mesma declaração, no `*` é gravado juntamente com o tipo subjacente, não como um pontuador de prefixo em cada nome de ponteiro. Por exemplo

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

O valor de um ponteiro com o tipo `T*` representa o endereço de uma variável do tipo `T`. O operador de indireção de ponteiro `*` ([indireção de ponteiro](unsafe-code.md#pointer-indirection)) pode ser usado para acessar essa variável. Por exemplo, dada uma variável `P` do tipo `int*`, a expressão `*P` denota que a variável `int` foi encontrada no endereço contido em `P`.

Como uma referência de objeto, um ponteiro pode ser `null`. A aplicação do operador de indireção a um ponteiro `null` resulta em comportamento definido pela implementação. Um ponteiro com o valor `null` é representado por todos os bits-zero.

O tipo `void*` representa um ponteiro para um tipo desconhecido. Como o tipo Referent é desconhecido, o operador de indireção não pode ser aplicado a um ponteiro do tipo `void*`, nem qualquer aritmética pode ser executada nesse ponteiro. No entanto, um ponteiro do tipo `void*` pode ser convertido em qualquer outro tipo de ponteiro (e vice-versa).

Os tipos de ponteiro são uma categoria separada de tipos. Diferentemente dos tipos de referência e tipos de valor, os tipos de ponteiro não herdam de `object` e nenhuma conversões existe entre os tipos de ponteiro e `object`. Em particular, boxing e unboxing ([boxing e unboxing](types.md#boxing-and-unboxing)) não têm suporte para ponteiros. No entanto, as conversões são permitidas entre diferentes tipos de ponteiro e entre os tipos de ponteiro e os tipos integrais. Isso é descrito em [conversões de ponteiro](unsafe-code.md#pointer-conversions).

Um *pointer_type* não pode ser usado como um argumento de tipo ([tipos construídos](types.md#constructed-types)), e a inferência de tipos ([inferência](expressions.md#type-inference)de tipos) falha em chamadas de método genérico que teriam inferido um argumento de tipo para ser um tipo de ponteiro.

Um *pointer_type* pode ser usado como o tipo de um campo volátil ([campos voláteis](classes.md#volatile-fields)).

Embora os ponteiros possam ser passados como parâmetros `ref` ou `out`, fazer isso pode causar um comportamento indefinido, já que o ponteiro pode ser bem definido para apontar para uma variável local que não existe mais quando o método chamado retorna ou o objeto fixo ao qual ele costumava apontar , não é mais fixo. Por exemplo:

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

Um método pode retornar um valor de algum tipo, e esse tipo pode ser um ponteiro. Por exemplo, quando um ponteiro é fornecido para uma sequência contígua de `int`s, a contagem de elementos dessa sequência e algum outro valor de `int`, o método a seguir retorna o endereço desse valor nessa sequência, se ocorrer uma correspondência; caso contrário, ele retornará `null`:

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

Em um contexto não seguro, várias construções estão disponíveis para operar em ponteiros:

*  O operador `*` pode ser usado para executar o indireção de ponteiro ([indireção de ponteiro](unsafe-code.md#pointer-indirection)).
*  O operador `->` pode ser usado para acessar um membro de uma struct por meio de um ponteiro ([acesso de membro de ponteiro](unsafe-code.md#pointer-member-access)).
*  O operador `[]` pode ser usado para indexar um ponteiro ([acesso de elemento de ponteiro](unsafe-code.md#pointer-element-access)).
*  O operador `&` pode ser usado para obter o endereço de uma variável ([o operador address-of](unsafe-code.md#the-address-of-operator)).
*  Os operadores `++` e `--` podem ser usados para incrementar e decrementar ponteiros ([incremento de ponteiro e decréscimo](unsafe-code.md#pointer-increment-and-decrement)).
*  Os operadores `+` e `-` podem ser usados para executar aritmética de ponteiro ([aritmética de ponteiro](unsafe-code.md#pointer-arithmetic)).
*  Os operadores `==`, `!=`, `<`, `>`, `<=` e `=>` podem ser usados para comparar ponteiros ([comparação de ponteiros](unsafe-code.md#pointer-comparison)).
*  O operador `stackalloc` pode ser usado para alocar memória da pilha de chamadas ([buffers de tamanho fixo](unsafe-code.md#fixed-size-buffers)).
*  A instrução `fixed` pode ser usada para corrigir temporariamente uma variável para que seu endereço possa ser obtido ([a instrução Fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Variáveis fixas e móveis

O operador address-of ([o operador address-of](unsafe-code.md#the-address-of-operator)) e a instrução `fixed` ([a instrução Fixed](unsafe-code.md#the-fixed-statement)) dividem variáveis em duas categorias: ***Variáveis fixas*** e ***variáveis móveis***.

As variáveis fixas residem em locais de armazenamento que não são afetados pela operação do coletor de lixo. (Exemplos de variáveis fixas incluem variáveis locais, parâmetros de valor e variáveis criadas pela desreferenciação de ponteiros.) Por outro lado, as variáveis móveis residem em locais de armazenamento que estão sujeitos a realocação ou descarte pelo coletor de lixo. (Exemplos de variáveis móveis incluem campos em objetos e elementos de matrizes.)

O operador `&` ([o operador address-of](unsafe-code.md#the-address-of-operator)) permite que o endereço de uma variável fixa seja obtido sem restrições. No entanto, como uma variável móvel está sujeita a realocação ou alienação pelo coletor de lixo, o endereço de uma variável móvel só pode ser obtido usando uma instrução `fixed` ([a instrução Fixed](unsafe-code.md#the-fixed-statement)) e esse endereço permanece válido somente para o duração da instrução `fixed`.

Em termos precisos, uma variável fixa é uma das seguintes:

*  Uma variável resultante de um *Simple_name* ([nomes simples](expressions.md#simple-names)) que se refere a uma variável local ou a um parâmetro de valor, a menos que a variável seja capturada por uma função anônima.
*  Uma variável resultante de um *member_access* ([acesso de membro](expressions.md#member-access)) do formulário `V.I`, em que `V` é uma variável fixa de um *struct_type*.
*  Uma variável resultante de um *pointer_indirection_expression* ([indireção de ponteiro](unsafe-code.md#pointer-indirection)) do formulário `*P`, um *pointer_member_access* (acesso de[membro de ponteiro](unsafe-code.md#pointer-member-access)) do formato `P->I` ou um *pointer_element_access* ( [Acesso de elemento de ponteiro](unsafe-code.md#pointer-element-access)) do formulário `P[E]`.

Todas as outras variáveis são classificadas como variáveis móveis.

Observe que um campo estático é classificado como uma variável móvel. Observe também que um parâmetro `ref` ou `out` é classificado como uma variável móvel, mesmo que o argumento fornecido para o parâmetro seja uma variável fixa. Por fim, observe que uma variável produzida por desreferenciar um ponteiro é sempre classificada como uma variável fixa.

## <a name="pointer-conversions"></a>Conversões de ponteiro

Em um contexto sem segurança, o conjunto de conversões implícitas disponíveis ([conversões implícitas](conversions.md#implicit-conversions)) é estendido para incluir as seguintes conversões de ponteiro implícitas:

*  De qualquer *pointer_type* para o tipo `void*`.
*  Do `null` literal para qualquer *pointer_type*.

Além disso, em um contexto não seguro, o conjunto de conversões explícitas disponíveis ([conversões explícitas](conversions.md#explicit-conversions)) é estendido para incluir as seguintes conversões de ponteiro explícitas:

*  De qualquer *pointer_type* para qualquer outro *pointer_type*.
*  De `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong` para qualquer *pointer_type*.
*  De qualquer *pointer_type* para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` ou `ulong`.

Por fim, em um contexto não seguro, o conjunto de conversões implícitas padrão ([conversões implícitas padrão](conversions.md#standard-implicit-conversions)) inclui a seguinte conversão de ponteiro:

*  De qualquer *pointer_type* para o tipo `void*`.

As conversões entre dois tipos de ponteiro nunca alteram o valor real do ponteiro. Em outras palavras, uma conversão de um tipo de ponteiro para outro não tem nenhum efeito sobre o endereço subjacente fornecido pelo ponteiro.

Quando um tipo de ponteiro é convertido em outro, se o ponteiro resultante não estiver alinhado corretamente para o tipo apontado, o comportamento será indefinido se o resultado for de referência. Em geral, o conceito "alinhado corretamente" é transitiva: se um ponteiro para digitar `A` estiver corretamente alinhado para um ponteiro para digitar `B`, que, por sua vez, está alinhado corretamente para um ponteiro para digitar `C`, então um ponteiro para digitar `A` está alinhado corretamente para um Ponteiro para o tipo `C`.

Considere o seguinte caso em que uma variável com um tipo é acessada por meio de um ponteiro para um tipo diferente:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Quando um tipo de ponteiro é convertido em um ponteiro para byte, o resultado aponta para o byte de endereçamento mais baixo da variável. Incrementos sucessivos do resultado, até o tamanho da variável, geram ponteiros para os bytes restantes dessa variável. Por exemplo, o método a seguir exibe cada um dos oito bytes em um duplo como um valor hexadecimal:

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

É claro que a saída produzida depende da ordenação.

Os mapeamentos entre ponteiros e inteiros são definidos pela implementação. No entanto, em arquiteturas de CPU de 32 * e 64 bits com um espaço de endereço linear, conversões de ponteiros para ou de tipos integrais normalmente se comportam exatamente como conversões de valores `uint` ou `ulong`, respectivamente, de ou para esses tipos integral.

### <a name="pointer-arrays"></a>Matrizes de ponteiro

Em um contexto não seguro, matrizes de ponteiros podem ser construídas. Somente algumas das conversões que se aplicam a outros tipos de matriz são permitidas em matrizes de ponteiro:

*  A conversão de referência implícita ([conversões de referência implícitas](conversions.md#implicit-reference-conversions)) de qualquer *array_type* para `System.Array` e as interfaces que ele implementa também se aplica a matrizes de ponteiro. No entanto, qualquer tentativa de acessar os elementos da matriz por meio de `System.Array` ou das interfaces que ele implementa resultará em uma exceção em tempo de execução, pois os tipos de ponteiro não são conversíveis para `object`.
*  As conversões de referência implícita e explícita (conversões de[referência implícita](conversions.md#implicit-reference-conversions), [conversões de referência explícitas](conversions.md#explicit-reference-conversions)) de um tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` e suas interfaces base genéricas nunca se aplicam a matrizes de ponteiros, como os tipos de ponteiro não podem ser usados como argumentos de tipo, e não há conversões de tipos de ponteiro para tipos que não sejam ponteiros.
*  A conversão de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) do `System.Array` e das interfaces que ele implementa para qualquer *array_type* se aplica a matrizes de ponteiro.
*  As conversões de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) de `System.Collections.Generic.IList<S>` e suas interfaces base para um tipo de matriz unidimensional `T[]` nunca se aplicam a matrizes de ponteiro, pois tipos de ponteiro não podem ser usados como argumentos de tipo, e há Não há conversões de tipos de ponteiro para tipos sem ponteiro.

Essas restrições significam que a expansão para a instrução `foreach` sobre matrizes descritas na [instrução foreach](statements.md#the-foreach-statement) não pode ser aplicada a matrizes de ponteiro. Em vez disso, uma instrução foreach do formulário

```csharp
foreach (V v in x) embedded_statement
```

onde o tipo de `x` é um tipo de matriz do formulário `T[,,...,]`, `N` é o número de dimensões menos 1 e `T` ou `V` é um tipo de ponteiro, é expandido usando loops for aninhado da seguinte maneira:

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

As variáveis `a`, `i0`, `i1`,..., `iN` não são visíveis ou acessíveis para `x` ou *embedded_statement* ou qualquer outro código-fonte do programa. A variável `v` é somente leitura na instrução inserida. Se não houver uma conversão explícita ([conversões de ponteiro](unsafe-code.md#pointer-conversions)) de `T` (o tipo de elemento) para `V`, um erro será produzido e nenhuma etapa adicional será executada. Se `x` tiver o valor `null`, um `System.NullReferenceException` será lançado em tempo de execução.

## <a name="pointers-in-expressions"></a>Ponteiros em expressões

Em um contexto não seguro, uma expressão pode gerar um resultado de um tipo de ponteiro, mas fora de um contexto sem segurança, é um erro de tempo de compilação para uma expressão ser de um tipo de ponteiro. Em termos precisos, fora de um contexto sem segurança, um erro de tempo de compilação ocorrerá se qualquer *Simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro](expressions.md#member-access)), *invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)) ou  *element_access* ([acesso de elemento](expressions.md#element-access)) é de um tipo de ponteiro.

Em um contexto sem segurança, as produções *primary_no_array_creation_expression* ([expressões primárias](expressions.md#primary-expressions)) e *unary_expression* ([operadores unários](expressions.md#unary-operators)) permitem as seguintes construções adicionais:

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

Essas construções são descritas nas seções a seguir. A precedência e a Associação dos operadores inseguros é implícita pela gramática.

### <a name="pointer-indirection"></a>Indireção de ponteiro

Um *pointer_indirection_expression* consiste em um asterisco (`*`) seguido por um *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

O operador unário `*` denota indireção de ponteiro e é usado para obter a variável para a qual um ponteiro aponta. O resultado da avaliação de `*P`, em que `P` é uma expressão de um tipo de ponteiro `T*`, é uma variável do tipo `T`. É um erro de tempo de compilação para aplicar o operador unário `*` a uma expressão do tipo `void*` ou a uma expressão que não é de um tipo de ponteiro.

O efeito de aplicar o operador `*` unário a um ponteiro `null` é definido pela implementação. Em particular, não há nenhuma garantia de que essa operação gere um `System.NullReferenceException`.

Se um valor inválido tiver sido atribuído ao ponteiro, o comportamento do operador unário `*` será indefinido. Entre os valores inválidos para desreferenciar um ponteiro pelo operador `*` unário são um endereço alinhado incorretamente para o tipo apontado (consulte o exemplo em [conversões de ponteiro](unsafe-code.md#pointer-conversions)) e o endereço de uma variável após o término de seu tempo de vida.

Para fins de análise de atribuição definitiva, uma variável produzida pela avaliação de uma expressão do formulário `*P` é considerada inicialmente atribuída ([variáveis inicialmente atribuídas](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Acesso de membro de ponteiro

Um *pointer_member_access* consiste em um *primary_expression*, seguido por um token "`->`", seguido por um *identificador* e um *type_argument_list*opcional.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

Em um membro de ponteiro, o acesso do formulário `P->I`, `P` deve ser uma expressão de um tipo de ponteiro diferente de `void*` e `I` deve indicar um membro acessível do tipo para o qual `P` aponta.

Um acesso de membro de ponteiro do formulário `P->I` é avaliado exatamente como `(*P).I`. Para obter uma descrição do operador de indireção de ponteiro (`*`), consulte [indireção de ponteiro](unsafe-code.md#pointer-indirection). Para obter uma descrição do operador de acesso de membro (`.`), consulte [acesso de membro](expressions.md#member-access).

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

o operador `->` é usado para acessar campos e invocar um método de uma struct por meio de um ponteiro. Como a operação `P->I` é precisamente equivalente a `(*P).I`, o método `Main` poderia igualmente ter sido escrito:

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

Um *pointer_element_access* consiste em um *primary_no_array_creation_expression* seguido por uma expressão colocada em "`[`" e "`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

Em um elemento de ponteiro, o acesso ao formulário `P[E]`, `P` deve ser uma expressão de um tipo de ponteiro diferente de `void*`, e `E` deve ser uma expressão que possa ser convertida implicitamente em `int`, `uint`, `long` ou `ulong`.

Um elemento de ponteiro acesso ao formulário `P[E]` é avaliado exatamente como `*(P + E)`. Para obter uma descrição do operador de indireção de ponteiro (`*`), consulte [indireção de ponteiro](unsafe-code.md#pointer-indirection). Para obter uma descrição do operador de adição de ponteiro (`+`), consulte [aritmética de ponteiro](unsafe-code.md#pointer-arithmetic).

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

um acesso de elemento de ponteiro é usado para inicializar o buffer de caracteres em um loop `for`. Como a operação `P[E]` é precisamente equivalente a `*(P + E)`, o exemplo poderia igualmente ter sido escrito:

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

O operador de acesso de elemento de ponteiro não verifica erros fora de limites e o comportamento ao acessar um elemento fora dos limites é indefinido. Isso é o mesmo que C e C++.

### <a name="the-address-of-operator"></a>O operador address-of

Um *addressof_expression* consiste em um e comercial (`&`) seguido por um *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Dada uma expressão `E` que é de um tipo `T` e é classificado como uma variável fixa ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)), a construção `&E` computa o endereço da variável fornecida pelo `E`. O tipo do resultado é `T*` e é classificado como um valor. Ocorrerá um erro de tempo de compilação se `E` não for classificado como uma variável, se `E` for classificado como uma variável local somente leitura ou se `E` denotar uma variável móvel. No último caso, uma instrução fixa ([a instrução Fixed](unsafe-code.md#the-fixed-statement)) pode ser usada para "corrigir" temporariamente a variável antes de obter seu endereço. Conforme indicado no [acesso de membro](expressions.md#member-access), fora de um construtor de instância ou construtor estático para uma struct ou classe que define um campo `readonly`, esse campo é considerado um valor, e não uma variável. Como tal, seu endereço não pode ser obtido. Da mesma forma, o endereço de uma constante não pode ser obtido.

O operador `&` não exige que seu argumento seja definitivamente atribuído, mas após uma operação `&`, a variável à qual o operador é aplicado é considerada definitivamente atribuída no caminho de execução no qual a operação ocorre. É de responsabilidade do programador garantir que a inicialização correta da variável realmente ocorra nessa situação.

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

`i` é considerado definitivamente atribuído após a operação de `&i` usada para inicializar `p`. A atribuição para `*p` em vigor Inicializa `i`, mas a inclusão dessa inicialização é responsabilidade do programador e nenhum erro de tempo de compilação ocorrerá se a atribuição tiver sido removida.

As regras de atribuição definitiva para o operador `&` existem, de modo que a inicialização redundante de variáveis locais pode ser evitada. Por exemplo, muitas APIs externas usam um ponteiro para uma estrutura que é preenchida pela API. Chamadas para essas APIs normalmente passam o endereço de uma variável de struct local e sem a regra, a inicialização redundante da variável de struct seria necessária.

### <a name="pointer-increment-and-decrement"></a>Incrementar e decrementar ponteiros

Em um contexto sem segurança, os operadores `++` e `--` ([incremento de sufixo e diminuir](expressions.md#postfix-increment-and-decrement-operators) os operadores e [incremento de prefixo e decrementar](expressions.md#prefix-increment-and-decrement-operators)) podem ser aplicados a variáveis de ponteiro de todos os tipos, exceto `void*`. Portanto, para cada tipo de ponteiro `T*`, os seguintes operadores são definidos implicitamente:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Os operadores produzem os mesmos resultados que `x + 1` e `x - 1`, respectivamente ([aritmética de ponteiro](unsafe-code.md#pointer-arithmetic)). Em outras palavras, para uma variável de ponteiro do tipo `T*`, o operador `++` adiciona `sizeof(T)` ao endereço contido na variável, e o operador `--` subtrai `sizeof(T)` do endereço contido na variável.

Se uma operação de incremento ou diminuição de ponteiro estourar o domínio do tipo de ponteiro, o resultado será definido pela implementação, mas nenhuma exceção será produzida.

### <a name="pointer-arithmetic"></a>Aritmética do ponteiro

Em um contexto sem segurança, os operadores `+` e `-` ([operador de adição](expressions.md#addition-operator) e [subtração](expressions.md#subtraction-operator)) podem ser aplicados a valores de todos os tipos de ponteiro, exceto `void*`. Portanto, para cada tipo de ponteiro `T*`, os seguintes operadores são definidos implicitamente:

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

Dada uma expressão `P` de um tipo de ponteiro `T*` e uma expressão `N` do tipo `int`, `uint`, `long` ou `ulong`, as expressões `P + N` e `N + P` calculam o valor de ponteiro do tipo `T*` que resulta da adição de 0 ao endereço fornecido por 1. Da mesma forma, a expressão `P - N` computa o valor do ponteiro do tipo `T*` resultante da subtração `N * sizeof(T)` do endereço fornecido pelo `P`.

Dadas duas expressões, `P` e `Q`, de um tipo de ponteiro `T*`, a expressão `P - Q` computa a diferença entre os endereços fornecidos por `P` e `Q` e, em seguida, divide essa diferença por `sizeof(T)`. O tipo do resultado é sempre `long`. Na verdade, `P - Q` é computado como `((long)(P) - (long)(Q)) / sizeof(T)`.

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

```console
p - q = -14
q - p = 14
```

Se uma operação aritmética de ponteiro estoura o domínio do tipo de ponteiro, o resultado é truncado de maneira definida pela implementação, mas nenhuma exceção é produzida.

### <a name="pointer-comparison"></a>Comparação de ponteiro

Em um contexto sem segurança, os operadores `==`, `!=`, `<`, `>`, `<=` e `=>` ([operadores relacionais e de teste de tipo](expressions.md#relational-and-type-testing-operators)) podem ser aplicados a valores de todos os tipos de ponteiro. Os operadores de comparação de ponteiro são:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Como existe uma conversão implícita de qualquer tipo de ponteiro para o tipo `void*`, os operandos de qualquer tipo de ponteiro podem ser comparados usando esses operadores. Os operadores de comparação comparam os endereços fornecidos pelos dois operandos como se fossem inteiros sem sinal.

### <a name="the-sizeof-operator"></a>O operador sizeof

O operador `sizeof` retorna o número de bytes ocupados por uma variável de um determinado tipo. O tipo especificado como um operando para `sizeof` deve ser um *unmanaged_type* ([tipos de ponteiro](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

O resultado do operador `sizeof` é um valor do tipo `int`. Para determinados tipos predefinidos, o operador `sizeof` produz um valor constante, conforme mostrado na tabela a seguir.


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

Para todos os outros tipos, o resultado do operador `sizeof` é definido pela implementação e é classificado como um valor, não uma constante.

A ordem na qual os membros são empacotados em um struct não é especificada.

Para fins de alinhamento, pode haver um preenchimento sem nome no início de uma struct, dentro de uma struct e no final da estrutura. O conteúdo dos bits usados como preenchimento é indeterminado.

Quando aplicado a um operando que tem o tipo struct, o resultado é o número total de bytes em uma variável desse tipo, incluindo qualquer preenchimento.

## <a name="the-fixed-statement"></a>A instrução Fixed

Em um contexto não seguro, a produção *embedded_statement* ([instruções](statements.md)) permite um constructo adicional, a instrução `fixed`, que é usada para "corrigir" uma variável móvel, de modo que seu endereço permaneça constante durante a instrução .

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

Cada *fixed_pointer_declarator* declara uma variável local do *pointer_type* especificado e inicializa essa variável local com o endereço computado pelo *fixed_pointer_initializer*correspondente. Uma variável local declarada em uma instrução `fixed` é acessível em qualquer *fixed_pointer_initializer*s que ocorre à direita da declaração dessa variável e no *embedded_statement* da instrução `fixed`. Uma variável local declarada por uma instrução `fixed` é considerada somente leitura. Ocorrerá um erro de tempo de compilação se a instrução inserida tentar modificar essa variável local (por meio de atribuição ou os operadores `++` e `--`) ou passá-la como um parâmetro `ref` ou `out`.

Um *fixed_pointer_initializer* pode ser um dos seguintes:

*  O token "`&`" seguido por um *variable_reference* ([regras precisas para determinar a atribuição definitiva](variables.md#precise-rules-for-determining-definite-assignment)) para uma variável móvel ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)) de um tipo não gerenciado `T`, desde que o tipo `T*` seja implicitamente conversível para o tipo de ponteiro fornecido na instrução `fixed`. Nesse caso, o inicializador computa o endereço da variável fornecida, e a variável é garantida para permanecer em um endereço fixo durante a instrução `fixed`.
*  Uma expressão de um *array_type* com elementos de um tipo não gerenciado `T`, desde que o tipo `T*` seja implicitamente conversível no tipo de ponteiro fornecido na instrução `fixed`. Nesse caso, o inicializador computa o endereço do primeiro elemento na matriz e toda a matriz é garantida para permanecer em um endereço fixo durante a instrução `fixed`. Se a expressão de matriz for nula ou se a matriz tiver zero elementos, o inicializador computará um endereço igual a zero.
*  Uma expressão do tipo `string`, desde que o tipo `char*` seja implicitamente conversível para o tipo de ponteiro fornecido na instrução `fixed`. Nesse caso, o inicializador computa o endereço do primeiro caractere na cadeia de caracteres e toda a cadeia de caracteres é garantida para permanecer em um endereço fixo durante a instrução `fixed`. O comportamento da instrução `fixed` será definido pela implementação se a expressão de cadeia de caracteres for nula.
*  Um *Simple_name* ou *member_access* que faz referência a um membro de buffer de tamanho fixo de uma variável móvel, desde que o tipo do membro de buffer de tamanho fixo seja implicitamente conversível para o tipo de ponteiro fornecido na instrução `fixed`. Nesse caso, o inicializador computa um ponteiro para o primeiro elemento do buffer de tamanho fixo ([buffers de tamanho fixo em expressões](unsafe-code.md#fixed-size-buffers-in-expressions)) e o buffer de tamanho fixo é garantido para permanecer em um endereço fixo durante a instrução `fixed`.

Para cada endereço calculado por um *fixed_pointer_initializer* , a instrução `fixed` garante que a variável referenciada pelo endereço não esteja sujeita a realocação ou descarte pelo coletor de lixo pela duração da instrução `fixed`. Por exemplo, se o endereço computado por um *fixed_pointer_initializer* referenciar um campo de um objeto ou um elemento de uma instância de matriz, a instrução `fixed` garante que a instância de objeto recipiente não seja realocada ou descartada durante o tempo de vida da instrução.

É responsabilidade do programador garantir que os ponteiros criados pelas instruções `fixed` não sobrevivem além da execução dessas instruções. Por exemplo, quando os ponteiros criados pelas instruções `fixed` são passados para APIs externas, é responsabilidade do programador garantir que as APIs não mantenham memória desses ponteiros.

Os objetos fixos podem causar a fragmentação do heap (porque eles não podem ser movidos). Por esse motivo, os objetos devem ser corrigidos somente quando absolutamente necessário e, em seguida, apenas pelo menor tempo possível.

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

demonstra vários usos da instrução `fixed`. A primeira instrução corrige e Obtém o endereço de um campo estático, a segunda instrução corrige e Obtém o endereço de um campo de instância, e a terceira instrução corrige e Obtém o endereço de um elemento de matriz. Em cada caso, seria um erro para usar o operador regular `&`, uma vez que as variáveis são todas classificadas como variáveis móveis.

A quarta instrução `fixed` no exemplo acima produz um resultado semelhante ao terceiro.

Este exemplo da instrução `fixed` usa `string`:

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

Em um contexto não seguro, os elementos de matrizes unidimensionais são armazenados em ordem de índice crescente, começando com o índice `0` e terminando com o índice `Length - 1`. Para matrizes multidimensionais, os elementos de matriz são armazenados de modo que os índices da dimensão mais à direita sejam aumentados primeiro, depois a dimensão à esquerda e assim por diante à esquerda. Dentro de uma instrução `fixed` que obtém um ponteiro `p` para uma instância de matriz `a`, os valores de ponteiro que variam de `p` a `p + a.Length - 1` representam endereços dos elementos na matriz. Da mesma forma, as variáveis que variam de `p[0]` a `p[a.Length - 1]` representam os elementos reais da matriz. Devido à maneira como as matrizes são armazenadas, podemos tratar uma matriz de qualquer dimensão como se ela fosse linear.

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

```console
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

uma instrução `fixed` é usada para corrigir uma matriz para que seu endereço possa ser passado para um método que usa um ponteiro.

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

uma instrução Fixed é usada para corrigir um buffer de tamanho fixo de uma struct para que seu endereço possa ser usado como um ponteiro.

Um valor `char*` produzido pela correção de uma instância de cadeia de caracteres sempre aponta para uma cadeia de caracteres terminada em nulo. Dentro de uma instrução fixa que obtém um ponteiro `p` a uma instância de cadeia de caracteres `s`, os valores de ponteiro que variam de `p` a `p + s.Length - 1` representam endereços dos caracteres na cadeia de caracteres e o valor do ponteiro `p + s.Length` sempre aponta para um caractere nulo (o caractere com valor `'\0'`).

Modificar objetos de tipo gerenciado por meio de ponteiros fixos pode resultar em um comportamento indefinido. Por exemplo, como as cadeias de caracteres são imutáveis, é responsabilidade do programador garantir que o caractere referenciado por um ponteiro para uma cadeia de caracteres fixa não seja modificado.

A terminação nula automática de cadeias de caracteres é particularmente conveniente ao chamar APIs externas que esperam cadeias de caracteres "estilo C". Observe, no entanto, que uma instância de cadeia de caracteres tem permissão para conter caracteres nulos. Se esses caracteres nulos estiverem presentes, a cadeia de caracteres aparecerá truncada quando tratada como um `char*` terminada em nulo.

## <a name="fixed-size-buffers"></a>Buffers de tamanho fixo

Buffers de tamanho fixo são usados para declarar matrizes em linha "C style" como membros de structs e são principalmente úteis para a interface com APIs não gerenciadas.

### <a name="fixed-size-buffer-declarations"></a>Declarações de buffer de tamanho fixo

Um ***buffer de tamanho fixo*** é um membro que representa o armazenamento de um buffer de comprimento fixo de variáveis de um determinado tipo. Uma declaração de buffer de tamanho fixo apresenta um ou mais buffers de tamanho fixo de um determinado tipo de elemento. Buffers de tamanho fixo só são permitidos em declarações struct e só podem ocorrer em contextos não seguros ([contextos não seguros](unsafe-code.md#unsafe-contexts)).

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

Uma declaração de buffer de tamanho fixo pode incluir um conjunto de atributos ([atributos](attributes.md)), um modificador `new` ([modificadores](classes.md#modifiers)), uma combinação válida dos quatro modificadores de acesso ([parâmetros de tipo e restrições](classes.md#type-parameters-and-constraints)) e um modificador de `unsafe` ([não seguro contextos](unsafe-code.md#unsafe-contexts)). Os atributos e os modificadores se aplicam a todos os membros declarados pela declaração de buffer de tamanho fixo. É um erro para que o mesmo modificador apareça várias vezes em uma declaração de buffer de tamanho fixo.

Uma declaração de buffer de tamanho fixo não tem permissão para incluir o modificador `static`.

O tipo de elemento de buffer de uma declaração de buffer de tamanho fixo especifica o tipo de elemento dos buffers introduzidos pela declaração. O tipo de elemento de buffer deve ser um dos tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 ou 1.

O tipo de elemento buffer é seguido por uma lista de declaradores de buffer de tamanho fixo, e cada um deles introduz um novo membro. Um Declarador de buffer de tamanho fixo consiste em um identificador que nomeia o membro, seguido por uma expressão constante entre os tokens `[` e `]`. A expressão constante denota o número de elementos no membro introduzidos pelo Declarador de buffer de tamanho fixo. O tipo da expressão constante deve ser conversível implicitamente no tipo `int`, e o valor deve ser um inteiro positivo diferente de zero.

Os elementos de um buffer de tamanho fixo têm garantia de serem dispostos em sequência na memória.

Uma declaração de buffer de tamanho fixo que declara vários buffers de tamanho fixo é equivalente a várias declarações de uma única declaração de buffer de tamanho fixo com os mesmos atributos e tipos de elemento. Por exemplo

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

A pesquisa de Membros ([operadores](expressions.md#operators)) de um membro de buffer de tamanho fixo prossegue exatamente como a pesquisa de membros de um campo.

Um buffer de tamanho fixo pode ser referenciado em uma expressão usando um *Simple_name* ([inferência de tipos](expressions.md#type-inference)) ou um *member_access* ([verificação de tempo de compilação de resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Quando um membro de buffer de tamanho fixo é referenciado como um nome simples, o efeito é o mesmo que um membro de acesso do formulário `this.I`, em que `I` é o membro do buffer de tamanho fixo.

Em um acesso de membro do formulário `E.I`, se `E` for de um tipo struct e uma pesquisa de membro de `I` nesse tipo struct identificar um membro de tamanho fixo, `E.I` será avaliado como classificado a seguir:

*  Se a expressão `E.I` não ocorrer em um contexto sem segurança, ocorrerá um erro em tempo de compilação.
*  Se `E` for classificado como um valor, ocorrerá um erro em tempo de compilação.
*  Caso contrário, se `E` for uma variável móvel ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)) e a expressão `E.I` não for um *fixed_pointer_initializer* ([a instrução fixa](unsafe-code.md#the-fixed-statement)), ocorrerá um erro em tempo de compilação.
*  Caso contrário, `E` faz referência a uma variável fixa e o resultado da expressão é um ponteiro para o primeiro elemento do membro de buffer de tamanho fixo `I` em `E`. O resultado é do tipo `S*`, em que `S` é o tipo de elemento de `I` e é classificado como um valor.

Os elementos subsequentes do buffer de tamanho fixo podem ser acessados usando operações de ponteiro do primeiro elemento. Ao contrário do acesso a matrizes, o acesso aos elementos de um buffer de tamanho fixo é uma operação não segura e não é verificado.

O exemplo a seguir declara e usa uma struct com um membro de buffer de tamanho fixo.

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

### <a name="definite-assignment-checking"></a>Verificação de atribuição definitiva

Os buffers de tamanho fixo não estão sujeitos à verificação de atribuição definitiva ([atribuição definitiva](variables.md#definite-assignment)) e os membros do buffer de tamanho fixo são ignorados para fins de verificação de atribuição definitiva de variáveis de tipo struct.

Quando o que contém a variável de struct mais externa de um membro de buffer de tamanho fixo é uma variável estática, uma variável de instância de uma instância de classe ou um elemento de matriz, os elementos do buffer de tamanho fixo são inicializados automaticamente para seus valores padrão ([padrão valores](variables.md#default-values)). Em todos os outros casos, o conteúdo inicial de um buffer de tamanho fixo é indefinido.

## <a name="stack-allocation"></a>Alocação de pilha

Em um contexto não seguro, uma declaração de variável local ([declarações de variável local](statements.md#local-variable-declarations)) pode incluir um inicializador de alocação de pilha que aloca memória da pilha de chamadas.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

O *unmanaged_type* indica o tipo dos itens que serão armazenados no local alocado recentemente e a *expressão* indica o número desses itens. Em conjunto, eles especificam o tamanho de alocação necessário. Como o tamanho de uma alocação de pilha não pode ser negativo, é um erro de tempo de compilação para especificar o número de itens como um *constant_expression* que é avaliado como um valor negativo.

Um inicializador de alocação de pilha do formulário `stackalloc T[E]` requer que `T` seja um tipo não gerenciado ([tipos de ponteiro](unsafe-code.md#pointer-types)) e `E` seja uma expressão do tipo `int`. A construção aloca `E * sizeof(T)` bytes da pilha de chamadas e retorna um ponteiro, do tipo `T*`, para o bloco recentemente alocado. Se `E` for um valor negativo, o comportamento será indefinido. Se `E` for zero, nenhuma alocação será feita e o ponteiro retornado será definido pela implementação. Se não houver memória suficiente disponível para alocar um bloco de determinado tamanho, um `System.StackOverflowException` será lançado.

O conteúdo da memória recém-alocada é indefinido.

Inicializadores de alocação de pilha não são permitidos em blocos `catch` ou `finally` ([a instrução try](statements.md#the-try-statement)).

Não há nenhuma maneira de liberar explicitamente a memória alocada usando `stackalloc`. Todos os blocos de memória alocada na pilha criados durante a execução de um membro de função são automaticamente descartados quando esse membro de função retorna. Isso corresponde à função `alloca`, uma extensão normalmente encontrada em C e C++ implementações.

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

um inicializador `stackalloc` é usado no método `IntToString` para alocar um buffer de 16 caracteres na pilha. O buffer é descartado automaticamente quando o método retorna.

## <a name="dynamic-memory-allocation"></a>Alocação de memória dinâmica

Exceto pelo operador `stackalloc`, C# o não fornece construções predefinidas para gerenciar memória não coletada pelo lixo. Esses serviços normalmente são fornecidos pelo suporte a bibliotecas de classes ou importados diretamente do sistema operacional subjacente. Por exemplo, a classe `Memory` abaixo ilustra como as funções de heap de um sistema operacional subjacente podem ser acessadas de C#:

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

Um exemplo que usa a classe `Memory` é fornecido abaixo:

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

O exemplo aloca 256 bytes de memória por meio de `Memory.Alloc` e inicializa o bloco de memória com valores que aumentam de 0 a 255. Em seguida, ele aloca uma matriz de bytes do elemento 256 e usa `Memory.Copy` para copiar o conteúdo do bloco de memória para a matriz de bytes. Por fim, o bloco de memória é liberado usando `Memory.Free` e o conteúdo da matriz de bytes é apresentado no console.
