# <a name="namespaces"></a>Namespaces

Programas em c# são organizados com o uso de namespaces. Namespaces são usados como um sistema de organização "interno" para um programa e como um sistema de organização "externa" — uma maneira de apresentar os elementos do programa que são expostos a outros programas.

Diretivas de uso ([diretivas Using](namespaces.md#using-directives)) são fornecidos para facilitar o uso de namespaces.

## <a name="compilation-units"></a>Unidades de compilação

Um *compilation_unit* define a estrutura geral de um arquivo de origem. Uma unidade de compilação consiste de zero ou mais *using_directive*s seguido por zero ou mais *global_attributes* seguido por zero ou mais *namespace_member_declaration*s .

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

Um programa c# consiste em uma ou mais unidades de compilação, cada um contido em um arquivo de origem separado. Quando um programa c# é compilado, todas as unidades de compilação são processadas em conjunto. Assim, unidades de compilação podem depender entre si, possivelmente de forma circular.

O *using_directive*s de um efeito de unidade de compilação a *global_attributes* e *namespace_member_declaration*s dessa unidade de compilação, mas não têm nenhum efeito outras unidades de compilação.

O *global_attributes* ([atributos](attributes.md)) de uma unidade de compilação permitem a especificação de atributos para o assembly de destino e o módulo. Módulos e assemblies atuam como contêineres físicos para tipos. Um assembly pode conter vários módulos separados fisicamente.

O *namespace_member_declaration*s de cada unidade de compilação de um programa de colaborar com membros para um espaço de única declaração chamado namespace global. Por exemplo:

Arquivo `A.cs`:
```csharp
class A {}
```

Arquivo `B.cs`:
```csharp
class B {}
```

As unidades de dois compilação contribuem para o namespace global único, nesse caso, declarando duas classes com nomes totalmente qualificados `A` e `B`. Porque as unidades de dois compilação contribuir com o mesmo espaço de declaração, teria sido um erro se cada continha uma declaração de um membro com o mesmo nome.

## <a name="namespace-declarations"></a>Declarações de namespace

Um *namespace_declaration* consiste na palavra-chave `namespace`, seguido por um nome de namespace e o corpo, opcionalmente seguido por um ponto e vírgula.

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

Um *namespace_declaration* pode ocorrer como uma declaração de nível superior em um *compilation_unit* ou como uma declaração de membro dentro de outra *namespace_declaration*. Quando um *namespace_declaration* ocorre como uma declaração de nível superior em um *compilation_unit*, o namespace se torna um membro do namespace global. Quando um *namespace_declaration* ocorre dentro de outra *namespace_declaration*, o namespace interno se torna membro o namespace externo. Em ambos os casos, o nome de um namespace deve ser exclusivo dentro do namespace que contém.

Namespaces são implicitamente `public` e a declaração de um namespace não pode incluir qualquer modificador de acesso.

Dentro de um *namespace_body*, opcional *using_directive*s importar os nomes de outros namespaces, tipos e membros, permitindo que elas sejam referenciadas diretamente em vez de por meio de nomes qualificados. Opcional *namespace_member_declaration*s contribuir com os membros do espaço de declaração do namespace. Observe que todos os *using_directive*s deve aparecer antes de qualquer declaração de membro.

O *qualified_identifier* de uma *namespace_declaration* pode ser um identificador único ou uma sequência de identificadores separados por "`.`" tokens. A segunda forma permite que um programa para definir um namespace aninhado sem aninhamento lexicalmente várias declarações de namespace. Por exemplo,

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
é semanticamente equivalente a
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

Namespaces são abertos e contribuir com duas declarações de namespace com o mesmo nome totalmente qualificado para o mesmo espaço de declaração ([declarações](basic-concepts.md#declarations)). No exemplo
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
As duas declarações de namespace acima contribuem para o mesmo espaço de declaração, nesse caso, declarando duas classes com nomes totalmente qualificados `N1.N2.A` e `N1.N2.B`. Porque as duas declarações a contribuir com o mesmo espaço de declaração, teria sido um erro se cada continha uma declaração de um membro com o mesmo nome.

## <a name="extern-aliases"></a>Aliases extern

Uma *extern_alias_directive* apresenta um identificador que serve como um alias para um namespace. A especificação do namespace com alias é externa ao código-fonte do programa e também se aplica a namespaces aninhados do namespace com alias.

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

O escopo de um *extern_alias_directive* se estende pela *using_directive*s, *global_attributes* e *namespace_member_declaration*s de seu corpo de namespace ou unidade de compilação imediatamente contido.

Dentro de um corpo de namespace ou unidade de compilação que contém um *extern_alias_directive*, o identificador introduzido pelo *extern_alias_directive* pode ser usado para referenciar o namespace com alias. É um erro de tempo de compilação para o *identificador* para ser a palavra `global`.

Uma *extern_alias_directive* disponibiliza um alias dentro de um corpo de namespace ou unidade de compilação específica, mas não contribui com os novos membros para o espaço de declaração subjacente. Em outras palavras, uma *extern_alias_directive* não é transitiva, mas, em vez disso, afeta apenas o compilação unidade namespace corpo ou nas quais ele ocorre.

O programa a seguir declara e usa dois alias extern, `X` e `Y`, cada do que representam a raiz de uma hierarquia de namespace distinto:
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

O programa declara a existência da extern aliases `X` e `Y`, mas as reais definições de aliases são externas ao programa. O nome idêntico `N.B` classes agora podem ser referenciadas como `X.N.B` e `Y.N.B`, ou, usando o qualificador alias de namespace `X::N.B` e `Y::N.B`. Um erro ocorrerá se um programa declara um alias externo para o qual é fornecida nenhuma definição de externa.

## <a name="using-directives"></a>usando diretivas

***Usando diretivas*** facilitar o uso de namespaces e tipos definidos em outros namespaces. Usando o processo de resolução de nome do impacto de diretivas *namespace_or_type_name*s ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) e *simple_name*s ([nomes simples ](expressions.md#simple-names)), mas, ao contrário de declarações, diretivas de uso não contribuem novos membros para os espaços de declaração subjacente do unidades de compilação ou namespaces dentro do qual eles são usados.

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

Um *using_alias_directive* ([diretivas de alias Using](namespaces.md#using-alias-directives)) apresenta um alias para um namespace ou tipo.

Um *using_namespace_directive* ([usando diretivas de namespace](namespaces.md#using-namespace-directives)) importa os membros de tipo de um namespace.

Um *using_static_directive* ([diretivas estáticas de uso](namespaces.md#using-static-directives)) importa os tipos aninhados e membros estáticos de um tipo.

O escopo de um *using_directive* se estende pela *namespace_member_declaration*s de seu corpo de namespace ou unidade de compilação imediatamente contido. O escopo de um *using_directive* especificamente não inclui seu par *using_directive*s. Assim, emparelhar *using_directive*s não afetam uns aos outros e a ordem na qual elas são gravadas é insignificante.

### <a name="using-alias-directives"></a>Usando diretivas de alias

Um *using_alias_directive* apresenta um identificador que serve como um alias para um namespace ou tipo dentro do corpo de namespace ou unidade de compilação imediatamente delimitador.

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

Dentro de declarações de membro em um corpo de namespace ou unidade de compilação que contém um *using_alias_directive*, o identificador introduzido pelo *using_alias_directive* pode ser usado para referência a determinado o namespace ou tipo. Por exemplo:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

Acima, dentro de declarações de membro na `N3` namespace, `A` é um alias para `N1.N2.A`e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`. O mesmo efeito pode ser obtido pela criação de um alias `R` para `N1.N2` e, em seguida, referenciar `R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

O *identificador* de uma *using_alias_directive* deve ser exclusivo dentro do espaço de declaração da unidade de compilação ou namespace que contém imediatamente o *using_alias_directive* . Por exemplo:
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

Acima, `N3` já contém um membro `A`, portanto, é um erro de tempo de compilação para um *using_alias_directive* para usar esse identificador. Da mesma forma, ele é um erro de tempo de compilação para dois ou mais *using_alias_directive*s na mesma compilação unidade ou namespace corpo declarar aliases com o mesmo nome.

Um *using_alias_directive* disponibiliza um alias dentro de um corpo de namespace ou unidade de compilação específica, mas não contribui com os novos membros para o espaço de declaração subjacente. Em outras palavras, uma *using_alias_directive* não é transitiva, mas em vez disso, afeta apenas o compilação unidade namespace corpo ou nas quais ele ocorre. No exemplo
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
o escopo do *using_alias_directive* que apresenta `R` só se estende às declarações de membro no corpo do namespace no qual ele está contido, portanto, `R` é desconhecido na segunda declaração de namespace. No entanto, colocar o *using_alias_directive* na compilação do recipiente unidade faz com que o alias para se tornar disponível em ambas as declarações de namespace:
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

Assim como os membros regulares, os nomes introduzidos por *using_alias_directive*s ficam ocultos por membros nomeados da mesma forma em escopos aninhados. No exemplo
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
a referência ao `R.A` na declaração de `B` causa um erro de tempo de compilação porque `R` refere-se ao `N3.R`, e não `N1.N2`.

A ordem na qual *using_alias_directive*s são gravados não tem nenhum significado e a resolução da *namespace_or_type_name* referenciado por um *using_alias_directive*não é afetado pela *using_alias_directive* em si ou por outras *using_directive*s em que o corpo de namespace ou unidade de compilação imediatamente contido. Em outras palavras, o *namespace_or_type_name* de uma *using_alias_directive* for resolvida como se o corpo de namespace ou unidade de compilação imediatamente contido tivesse não *using_directive*s. Um *using_alias_directive* Entretanto pode ser afetado pela *extern_alias_directive*s em que o corpo de namespace ou unidade de compilação imediatamente contido. No exemplo
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
a última *using_alias_directive* resulta em um erro de tempo de compilação porque ela não é afetada pela primeira *using_alias_directive*. A primeira *using_alias_directive* não resulta em um erro desde o escopo do alias extern `E` inclui as *using_alias_directive*.

Um *using_alias_directive* pode criar um alias para qualquer namespace ou tipo, incluindo o namespace no qual ele é exibido e qualquer namespace ou tipo aninhado dentro desse namespace.

Acessando um namespace ou tipo por meio de um alias produz exatamente o mesmo resultado e o acesso a esse namespace ou tipo por meio de seu nome declarado. Por exemplo, dada
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
os nomes `N1.N2.A`, `R1.N2.A`, e `R2.A` são equivalentes e todos se referir à classe cujo nome totalmente qualificado é `N1.N2.A`.

Usando aliases pode nome de um tipo construído fechado, mas não o nome de uma declaração de tipo genérico não associado sem fornecer argumentos de tipo. Por exemplo:
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>Usando diretivas de namespace

Um *using_namespace_directive* importa os tipos contidos em um namespace para o fechamento imediato compilação unidade ou namespace corpo, permitindo que o identificador de cada tipo a ser usado sem qualificação.

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

Dentro de declarações de membro em um corpo de namespace ou unidade de compilação que contém um *using_namespace_directive*, os tipos contidos no namespace específico podem ser referenciados diretamente. Por exemplo:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

Acima, dentro de declarações de membro na `N3` namespace, os membros do tipo `N1.N2` estão diretamente disponíveis e, portanto, a classe `N3.B` deriva da classe `N1.N2.A`.

Um *using_namespace_directive* importa os tipos contidos no namespace específico, mas especificamente não importa namespaces aninhados. No exemplo
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
o *using_namespace_directive* importa os tipos contidos em `N1`, mas não os namespaces aninhados em `N1`. Portanto, a referência ao `N2.A` na declaração de `B` resulta em um erro de tempo de compilação porque nenhum membro denominado `N2` estão no escopo.

Ao contrário de um *using_alias_directive*, um *using_namespace_directive* pode importar os tipos cujos identificadores já estão definidos dentro do corpo de namespace ou unidade de compilação delimitador. Na verdade, os nomes são importados por um *using_namespace_directive* ficam ocultos por membros nomeados da mesma forma no corpo de namespace ou unidade de compilação delimitador. Por exemplo:
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

Aqui, dentro de declarações de membro na `N3` namespace, `A` refere-se ao `N3.A` em vez de `N1.N2.A`.

Quando mais de um namespace ou tipo importado pelo *using_namespace_directive*s ou *using_static_directive*s em que o corpo de namespace ou unidade de compilação mesmo conter tipos com o mesmo nome, as referências a Esse nome como um *type_name* são considerados ambíguos. No exemplo
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
ambos `N1` e `N2` conter um membro `A`e porque `N3` importa ambos, referenciando `A` em `N3` é um erro de tempo de compilação. Nessa situação, o conflito pode ser resolvido por meio da qualificação de referências a `A`, ou com a introdução de um *using_alias_directive* que seleciona um determinado `A`. Por exemplo:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

Além disso, quando mais de um namespace ou tipo importado pelo *using_namespace_directive*s ou *using_static_directive*s em que o corpo de namespace ou unidade de compilação mesmo conter tipos ou membros pela mesmo nome, as referências a esse nome como um *simple_name* são considerados ambíguos. No exemplo
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` contém um membro de tipo `A`, e `C` contém um método estático `A`e porque `N2` importa ambos, referenciando `A` como uma *simple_name* é ambígua e um tempo de compilação erro. 

Como uma *using_alias_directive*, um *using_namespace_directive* não contribui com os novos membros para o espaço de declaração subjacente da unidade de compilação ou namespace, mas em vez disso, só afeta o compilação de unidade ou namespace corpo na qual ele aparece.

O *namespace_name* referenciado por uma *using_namespace_directive* for resolvida da mesma maneira como o *namespace_or_type_name* referenciado por um  *using_alias_directive*. Portanto, *using_namespace_directive*s em que o mesmo corpo de namespace ou unidade de compilação não afetam uns aos outros e podem ser escritos em qualquer ordem.

### <a name="using-static-directives"></a>Diretivas estáticas de uso

Um *using_static_directive* importa os tipos aninhados e membros estáticos contidos diretamente em uma declaração de tipo para o fechamento imediato compilação unidade ou namespace corpo, permitindo que o identificador de cada membro e o tipo para ser usados sem qualificação.

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

Dentro de declarações de membro em um corpo de namespace ou unidade de compilação que contém um *using_static_directive*, aninhado a acessível, tipos e membros estáticos (exceto os métodos de extensão) contidos diretamente na declaração da tipo fornecido pode ser referenciado diretamente. Por exemplo:

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

Acima, dentro de declarações de membro na `N2` namespace, os membros estáticos e tipos aninhados de `N1.A` estão diretamente disponíveis e, portanto, o método `N` é capaz de fazer referência a ambos os `B` e `M` membros `N1.A`.

Um *using_static_directive* especificamente não importa os métodos de extensão diretamente como métodos estáticos, mas torna-os disponíveis para invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)). No exemplo

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
o *using_static_directive* importa o método de extensão `M` contidos no `N1.A`, mas somente como um método de extensão. Portanto, a primeira referência às `M` no corpo da `B.N` resulta em um erro de tempo de compilação porque nenhum membro denominado `M` estão no escopo.

Um *using_static_directive* importa somente os membros e tipos declarados diretamente no tipo especificado, não os tipos e membros declarados em classes base.

TODO: Exemplo

Ambiguidades entre vários *using_namespace_directives* e *using_static_directives* são discutidos na [usando diretivas de namespace](namespaces.md#using-namespace-directives).

## <a name="namespace-members"></a>Membros do Namespace

Um *namespace_member_declaration* é um *namespace_declaration* ([declarações de Namespace](namespaces.md#namespace-declarations)) ou um *type_declaration* ( [As declarações de tipo](namespaces.md#type-declarations)).

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

Uma unidade de compilação ou um corpo de namespace pode conter *namespace_member_declaration*s e essas declarações contribuir com novos membros para o espaço de declaração subjacente do corpo que contém compilação unidade ou namespace.

## <a name="type-declarations"></a>Declarações de tipo

Um *type_declaration* é um *class_declaration* ([declarações de classe](classes.md#class-declarations)), um *struct_declaration* ([Struct declarações](structs.md#struct-declarations)), uma *interface_declaration* ([declarações de Interface](interfaces.md#interface-declarations)), uma *enum_declaration* ([Enum declarações](enums.md#enum-declarations)), ou um *delegate_declaration* ([declarações de delegado](delegates.md#delegate-declarations)).

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

Um *type_declaration* pode ocorrer como uma declaração de nível superior em uma unidade de compilação ou como uma declaração de membro dentro de um namespace, classe ou struct.

Quando uma declaração de tipo para um tipo `T` ocorre como uma declaração de nível superior em uma unidade de compilação, o nome totalmente qualificado do tipo declarado recentemente é simplesmente `T`. Quando uma declaração de tipo para um tipo `T` ocorre dentro de um namespace, classe ou struct, o nome totalmente qualificado do tipo declarado recentemente é `N.T`, onde `N` é o nome totalmente qualificado do namespace, classe ou struct recipiente.

Um tipo declarado dentro de uma classe ou struct é chamado um tipo aninhado ([tipos aninhados](classes.md#nested-types)).

Os modificadores de acesso permitido e o acesso padrão para uma declaração de tipo dependem do contexto no qual a declaração ocorre ([declarado acessibilidade](basic-concepts.md#declared-accessibility)):

*  Tipos declarados em unidades de compilação ou namespaces podem ter `public` ou `internal` acesso. O padrão é `internal` acesso.
*  Tipos declarados em classes podem ter `public`, `protected internal`, `protected`, `internal`, ou `private` acesso. O padrão é `private` acesso.
*  Tipos declarados em estruturas podem ter `public`, `internal`, ou `private` acesso. O padrão é `private` acesso.

## <a name="namespace-alias-qualifiers"></a>Qualificadores alias de Namespace

O ***qualificador alias de namespace*** `::` torna possível garantir que as pesquisas de nome de tipo não são afetadas pela introdução de novos tipos e membros. O qualificador alias de namespace sempre aparece entre dois identificadores, conhecidos como os identificadores do lado esquerdos e direito. Ao contrário de normal `.` qualificador, o identificador à esquerda do `::` qualificador é pesquisado up apenas como um externo ou usando o alias.

Um *qualified_alias_member* é definido da seguinte maneira:

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

Um *qualified_alias_member* pode ser usado como um *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)) ou como o operando esquerdo em uma *member_access* ([Acesso de membro](expressions.md#member-access)).

Um *qualified_alias_member* tem uma das duas formas:

*  `N::I<A1, ..., Ak>`, onde `N` e `I` representam identificadores, e `<A1, ..., Ak>` é uma lista de argumentos de tipo. (`K` é sempre pelo menos um.)
*  `N::I`, onde `N` e `I` representam identificadores. (Nesse caso, `K` é considerado como zero.)

Usando essa notação, o significado de um *qualified_alias_member* é determinado da seguinte maneira:

*  Se `N` é o identificador `global`, em seguida, o namespace global é pesquisado `I`:
   * Se o namespace global contém um namespace chamado `I` e `K` for zero, em seguida, a *qualified_alias_member* refere-se ao namespace.
   * Caso contrário, se o namespace global contém um tipo não genérico chamado `I` e `K` for zero, em seguida, a *qualified_alias_member* refere-se a esse tipo.
   * Caso contrário, se o namespace global contém um tipo chamado `I` que tem `K`  parâmetros de tipo, o *qualified_alias_member* refere-se ao tipo construído com os argumentos de tipo em questão.
   * Caso contrário, o *qualified_alias_member* é indefinido e ocorrer um erro de tempo de compilação.

*  Caso contrário, começando com a declaração de namespace ([declarações de Namespace](namespaces.md#namespace-declarations)) imediatamente que contém o *qualified_alias_member* (se houver), continuando com cada declaração de namespace delimitador (se houver) e terminando com a unidade de compilação que contém o *qualified_alias_member*, as etapas a seguir são avaliadas até que uma entidade está localizada:

   * Se a unidade de compilação ou de declaração de namespace contém uma *using_alias_directive* que associa `N` com um tipo, em seguida, o *qualified_alias_member* é indefinido e um tempo de compilação erro ocorre.
   * Caso contrário, se a unidade de compilação ou de declaração de namespace contém um *extern_alias_directive* ou *using_alias_directive* que associa `N` com um namespace, então:
     * Se o namespace associado `N` contém um namespace chamado `I` e `K` for zero, o *qualified_alias_member* refere-se ao namespace.
     * Caso contrário, se o namespace associado `N` contém um tipo não genérico chamado `I` e `K` for zero, o *qualified_alias_member* refere-se a esse tipo.
     * Caso contrário, se o namespace associado `N` contém um tipo denominado `I` que tem `K`  parâmetros de tipo, o *qualified_alias_member* refere-se ao que o tipo construído com os argumentos de tipo em questão.
     * Caso contrário, o *qualified_alias_member* é indefinido e ocorrer um erro de tempo de compilação.
*  Caso contrário, o *qualified_alias_member* é indefinido e ocorrer um erro de tempo de compilação.

Observe que usando o qualificador alias de namespace com um alias que faz referência a um tipo causa um erro de tempo de compilação. Observe também que, se o identificador `N` é `global`, em seguida, a pesquisa é executada no namespace global, mesmo se houver um alias associando usando `global` com um tipo ou namespace.

### <a name="uniqueness-of-aliases"></a>Exclusividade de aliases

Cada corpo de unidade e o namespace de compilação tem um espaço de declaração separada para aliases extern e uso de aliases. Assim, enquanto o nome de um alias externo ou usar o alias deve ser exclusivo dentro do conjunto de aliases extern e aliases using declaradas no corpo imediatamente contido compilação unidade ou namespace, um alias é permitido ter o mesmo nome que um tipo ou namespace, desde que eu t é usado apenas com o `::` qualificador.

No exemplo
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
o nome `A` tem dois significados possíveis no corpo de namespace da segunda porque os dois é a classe `A` e o uso de alias `A` estão no escopo. Por esse motivo, uso de `A` no nome qualificado `A.Stream` é ambíguo e faz com que ocorra um erro de tempo de compilação. No entanto, usar `A` com o `::` qualificador não é um erro porque `A` é pesquisado apenas como um alias de namespace.
