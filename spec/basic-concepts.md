---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704000"
---
# <a name="basic-concepts"></a>Conceitos básicos

## <a name="application-startup"></a>Inicialização do aplicativo

Um assembly que tem um ***ponto de entrada*** é chamado de ***aplicativo***. Quando um aplicativo é executado, um novo ***domínio de aplicativo*** é criado. Várias instanciações diferentes de um aplicativo podem existir no mesmo computador ao mesmo tempo, e cada uma tem seu próprio domínio de aplicativo.

Um domínio de aplicativo habilita o isolamento de aplicativos agindo como um contêiner para o estado do aplicativo. Um domínio de aplicativo atua como um contêiner e limite para os tipos definidos no aplicativo e as bibliotecas de classes que ele usa. Os tipos carregados em um domínio de aplicativo são diferentes do mesmo tipo carregado em outro domínio de aplicativo, e as instâncias de objetos não são diretamente compartilhadas entre domínios de aplicativo. Por exemplo, cada domínio de aplicativo tem sua própria cópia de variáveis estáticas para esses tipos, e um construtor estático para um tipo é executado no máximo uma vez por domínio de aplicativo. As implementações são gratuitas para fornecer políticas ou mecanismos específicos de implementação para a criação e destruição de domínios de aplicativo.

A ***inicialização do aplicativo*** ocorre quando o ambiente de execução chama um método designado, que é chamado de ponto de entrada do aplicativo. Esse método de ponto de entrada sempre é denominado `Main` e pode ter uma das seguintes assinaturas:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Como mostrado, o ponto de entrada pode opcionalmente retornar um valor `int`. Esse valor de retorno é usado na terminação do aplicativo ([terminação do aplicativo](basic-concepts.md#application-termination)).

O ponto de entrada pode, opcionalmente, ter um parâmetro formal. O parâmetro pode ter qualquer nome, mas o tipo do parâmetro deve ser `string[]`. Se o parâmetro formal estiver presente, o ambiente de execução criará e passará um argumento `string[]` contendo os argumentos de linha de comando que foram especificados quando o aplicativo foi iniciado. O argumento `string[]` nunca é nulo, mas pode ter um comprimento zero se nenhum argumento de linha de comando foi especificado.

Como C# o dá suporte à sobrecarga de método, uma classe ou struct pode conter várias definições de algum método, desde que cada uma tenha uma assinatura diferente. No entanto, em um único programa, nenhuma classe ou estrutura pode conter mais de um método chamado `Main` cuja definição o qualifica para ser usado como um ponto de entrada do aplicativo. Outras versões sobrecarregadas do `Main` são permitidas, no entanto, desde que tenham mais de um parâmetro, ou seu único parâmetro seja diferente do tipo `string[]`.

Um aplicativo pode ser composto por várias classes ou structs. É possível que mais de uma dessas classes ou structs contenham um método chamado `Main` cuja definição a qualifica para ser usada como um ponto de entrada de aplicativo. Nesses casos, um mecanismo externo (como uma opção de compilador de linha de comando) deve ser usado para selecionar um desses métodos `Main` como o ponto de entrada.

No C#, cada método deve ser definido como um membro de uma classe ou estrutura. Normalmente, a acessibilidade declarada (a[acessibilidade declarada](basic-concepts.md#declared-accessibility)) de um método é determinada pelos modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) especificados em sua declaração e, da mesma forma, a acessibilidade declarada de um tipo é determinada pelo modificadores de acesso especificados em sua declaração. Para que um determinado método de um determinado tipo seja chamado, o tipo e o membro devem estar acessíveis. No entanto, o ponto de entrada do aplicativo é um caso especial. Especificamente, o ambiente de execução pode acessar o ponto de entrada do aplicativo independentemente de sua acessibilidade declarada e independentemente da acessibilidade declarada de suas declarações de tipo delimitador.

O método de ponto de entrada de aplicativo pode não estar em uma declaração de classe genérica.

Em todos os outros aspectos, os métodos de ponto de entrada se comportam como aqueles que não são pontos de entrada.

## <a name="application-termination"></a>Encerramento do aplicativo

A ***terminação de aplicativo*** retorna o controle para o ambiente de execução.

Se o tipo de retorno do método de ***ponto de entrada*** do aplicativo for `int`, o valor retornado servirá como o ***código de status de encerramento***do aplicativo. A finalidade desse código é permitir a comunicação de êxito ou falha no ambiente de execução.

Se o tipo de retorno do método de ponto de entrada for `void`, atingir a chave direita (`}`) que encerra esse método, ou executar uma instrução `return` que não tenha nenhuma expressão, resultará em um código de status de encerramento de `0`.

Antes do encerramento de um aplicativo, os destruidores para todos os seus objetos que ainda não foram coletados pelo lixo são chamados, a menos que essa limpeza tenha sido suprimida (por uma chamada para o método de biblioteca `GC.SuppressFinalize`, por exemplo).

## <a name="declarations"></a>Declarations

As declarações em C# um programa definem os elementos constituintes do programa. C#os programas são organizados usando Namespaces ([namespaces](namespaces.md)), que podem conter declarações de tipo e declarações de namespace aninhadas. Declarações de tipo ([declarações de tipo](namespaces.md#type-declarations)) são usadas para definir classes ([classes](classes.md)), estruturas[(structs](structs.md)), interfaces ([interfaces](interfaces.md)), enumerações ([enums](enums.md)) e delegados ([delegados](delegates.md)). Os tipos de membros permitidos em uma declaração de tipo dependem da forma da declaração de tipo. Por exemplo, declarações de classe podem conter declarações para constantes ([constantes](classes.md#constants)), campos ([campos](classes.md#fields)), métodos ([métodos](classes.md#methods)), Propriedades ([Propriedades](classes.md#properties)), eventos ([eventos](classes.md#events)), indexadores ([indexadores](classes.md#indexers)), operadores ([operadores](classes.md#operators)), construtores de instância ([construtores de instância](classes.md#instance-constructors)), construtores estáticos ([construtores estáticos](classes.md#static-constructors)), destruidores ([destruidores](classes.md#destructors)) e tipos aninhados ([tipos aninhados](classes.md#nested-types)).

Uma declaração define um nome no ***espaço de declaração*** ao qual a declaração pertence. Exceto para Membros sobrecarregados ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)), é um erro de tempo de compilação ter duas ou mais declarações que apresentam Membros com o mesmo nome em um espaço de declaração. Nunca é possível que um espaço de declaração contenha diferentes tipos de membros com o mesmo nome. Por exemplo, um espaço de declaração nunca pode conter um campo e um método com o mesmo nome.

Há vários tipos diferentes de espaços de declaração, conforme descrito a seguir.

*  Em todos os arquivos de origem de um programa, *namespace_member_declaration*s sem *namespace_declaration* delimitadores são membros de um único espaço de declaração combinado chamado de ***espaço de declaração global***.
*  Em todos os arquivos de origem de um programa, *namespace_member_declaration*s em *namespace_declaration*s que têm o mesmo nome de namespace totalmente qualificado são membros de um único espaço de declaração combinado.
*  Cada declaração de classe, struct ou interface cria um novo espaço de declaração. Os nomes são introduzidos nesse espaço de declaração por meio de *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s ou *type_parameter*s. Exceto para declarações de construtor de instância sobrecarregadas e declarações de Construtor estáticos, uma classe ou struct não pode conter uma declaração de membro com o mesmo nome que a classe ou struct. Uma classe, estrutura ou interface permite a declaração de métodos e indexadores sobrecarregados. Além disso, uma classe ou struct permite a declaração de operadores e construtores de instância sobrecarregados. Por exemplo, uma classe, struct ou interface pode conter várias declarações de método com o mesmo nome, desde que essas declarações de método difiram em sua assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)). Observe que as classes base não contribuem para o espaço de declaração de uma classe, e as interfaces base não contribuem para o espaço de declaração de uma interface. Assim, uma classe ou interface derivada tem permissão para declarar um membro com o mesmo nome de um membro herdado. Esse membro é dito para ***ocultar*** o membro herdado.
*  Cada declaração de delegado cria um novo espaço de declaração. Os nomes são introduzidos nesse espaço de declaração por meio de parâmetros formais (*fixed_parameter*s e *parameter_array*s) e *type_parameter*s.
*  Cada declaração de enumeração cria um novo espaço de declaração. Os nomes são introduzidos nesse espaço de declaração por meio de *enum_member_declarations*.
*  Cada declaração de método, declaração de indexador, declaração de operador, declaração de construtor de instância e função anônima cria um novo espaço de declaração chamado de ***espaço de declaração de variável local***. Os nomes são introduzidos nesse espaço de declaração por meio de parâmetros formais (*fixed_parameter*s e *parameter_array*s) e *type_parameter*s. O corpo do membro da função ou da função anônima, se houver, será considerado aninhado dentro do espaço de declaração da variável local. É um erro para um espaço de declaração de variável local e um espaço de declaração de variável local aninhada para conter elementos com o mesmo nome. Portanto, dentro de um espaço de declaração aninhada não é possível declarar uma variável local ou constante com o mesmo nome de uma variável local ou constante em um espaço de declaração de delimitador. É possível que dois espaços de declaração contenham elementos com o mesmo nome, desde que nenhum espaço de declaração contenha o outro.
*  Cada *bloco* ou *switch_block* , bem como uma instrução *for*, *foreach* e *using* , cria um espaço de declaração de variável local para variáveis locais e constantes locais. Os nomes são introduzidos nesse espaço de declaração por meio de *local_variable_declaration*s e *local_constant_declaration*s. Observe que os blocos que ocorrem como ou dentro do corpo de um membro de função ou função anônima são aninhados dentro do espaço de declaração de variável local declarado por essas funções para seus parâmetros. Portanto, é um erro ter, por exemplo, um método com uma variável local e um parâmetro de mesmo nome.
*  Cada *bloco* ou *switch_block* cria um espaço de declaração separado para rótulos. Os nomes são introduzidos nesse espaço de declaração por meio de *labeled_statement*s e os nomes são referenciados por meio de *goto_statement*s. O ***espaço de declaração de rótulo*** de um bloco inclui todos os blocos aninhados. Portanto, em um bloco aninhado não é possível declarar um rótulo com o mesmo nome de um rótulo em um bloco delimitador.

Em geral, a ordem textual na qual os nomes são declarados não é significado. Em particular, a ordem textual não é significativa para a declaração e o uso de namespaces, constantes, métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores, construtores estáticos e tipos. A ordem de declaração é significativa das seguintes maneiras:

*  A ordem de declaração para declarações de campo e declarações de variáveis locais determina a ordem na qual seus inicializadores (se houver) são executados.
*  As variáveis locais devem ser definidas antes de serem usadas ([escopos](basic-concepts.md#scopes)).
*  A ordem de declaração para declarações de membro de enumeração ([membros de enum](enums.md#enum-members)) é significativa quando os valores de *constant_expression* são omitidos.

O espaço de declaração de um namespace é "Open finalizado" e duas declarações de namespace com o mesmo nome totalmente qualificado contribuem para o mesmo espaço de declaração. Por exemplo
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

As duas declarações de namespace acima contribuem para o mesmo espaço de declaração, neste caso, declarando duas classes com os nomes totalmente qualificados `Megacorp.Data.Customer` e `Megacorp.Data.Order`. Como as duas declarações contribuem para o mesmo espaço de declaração, ela causaria um erro de tempo de compilação se cada uma continha uma declaração de uma classe com o mesmo nome.

Conforme especificado acima, o espaço de declaração de um bloco inclui todos os blocos aninhados. Portanto, no exemplo a seguir, os métodos `F` e `G` resultam em um erro de tempo de compilação porque o nome `i` é declarado no bloco externo e não pode ser declarado novamente no bloco interno. No entanto, os métodos `H` e `I` são válidos, pois os dois `i` são declarados em blocos não aninhados separados.

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>Membros

Namespaces e tipos têm ***Membros***. Os membros de uma entidade estão geralmente disponíveis por meio do uso de um nome qualificado que começa com uma referência à entidade, seguida por um token "`.`", seguido pelo nome do membro.

Os membros de um tipo são declarados na declaração de tipo ou ***herdados*** da classe base do tipo. Quando um tipo é herdado de uma classe base, todos os membros da classe base, exceto construtores de instância, destruidores e construtores estáticos, se tornam membros do tipo derivado. A acessibilidade declarada de um membro de classe base não controla se o membro é herdado — a herança se estende a qualquer membro que não seja um construtor de instância, construtor estático ou destruidor. No entanto, um membro herdado pode não estar acessível em um tipo derivado, devido à sua acessibilidade declarada ([acessibilidade declarada](basic-concepts.md#declared-accessibility)) ou porque está ocultada por uma declaração no próprio tipo ([ocultando por herança](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Membros do namespace

Namespaces e tipos que não têm namespace delimitador são membros do ***namespace global***. Isso corresponde diretamente aos nomes declarados no espaço de declaração global.

Namespaces e tipos declarados em um namespace são membros desse namespace. Isso corresponde diretamente aos nomes declarados no espaço de declaração do namespace.

Namespaces não têm nenhuma restrição de acesso. Não é possível declarar namespaces privados, protegidos ou internos, e os nomes de namespace são sempre acessíveis publicamente.

### <a name="struct-members"></a>Membros de struct

Os membros de uma struct são os membros declarados na struct e os membros herdados da classe base direta da struct `System.ValueType` e a classe base indireta `object`.

Os membros de um tipo simples correspondem diretamente aos membros do tipo struct com o alias do tipo simples:

*  Os membros de `sbyte` são os membros da struct `System.SByte`.
*  Os membros de `byte` são os membros da struct `System.Byte`.
*  Os membros de `short` são os membros da struct `System.Int16`.
*  Os membros de `ushort` são os membros da struct `System.UInt16`.
*  Os membros de `int` são os membros da struct `System.Int32`.
*  Os membros de `uint` são os membros da struct `System.UInt32`.
*  Os membros de `long` são os membros da struct `System.Int64`.
*  Os membros de `ulong` são os membros da struct `System.UInt64`.
*  Os membros de `char` são os membros da struct `System.Char`.
*  Os membros de `float` são os membros da struct `System.Single`.
*  Os membros de `double` são os membros da struct `System.Double`.
*  Os membros de `decimal` são os membros da struct `System.Decimal`.
*  Os membros de `bool` são os membros da struct `System.Boolean`.

### <a name="enumeration-members"></a>Membros de enumeração

Os membros de uma enumeração são as constantes declaradas na enumeração e os membros herdados da classe base direta da enumeração `System.Enum` e as classes base indiretas `System.ValueType` e `object`.

### <a name="class-members"></a>Membros de classe

Os membros de uma classe são os membros declarados na classe e os membros herdados da classe base (exceto para a classe `object` que não tem nenhuma classe base). Os membros herdados da classe base incluem as constantes, os campos, os métodos, as propriedades, os eventos, os indexadores, os operadores e os tipos da classe base, mas não os construtores de instância, destruidores e construtores estáticos da classe base. Os membros da classe base são herdados sem considerar sua acessibilidade.

Uma declaração de classe pode conter declarações de constantes, campos, métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores, construtores e tipos estáticos.

Os membros de `object` e `string` correspondem diretamente aos membros dos tipos de classe aos quais eles se alias:

*  Os membros de `object` são os membros da classe `System.Object`.
*  Os membros de `string` são os membros da classe `System.String`.

### <a name="interface-members"></a>Membros da interface

Os membros de uma interface são os membros declarados na interface e em todas as interfaces base da interface. Os membros na classe `object` não são, estritamente falando, membros de qualquer interface ([membros da interface](interfaces.md#interface-members)). No entanto, os membros na classe `object` estão disponíveis por meio de pesquisa de membro em qualquer tipo de interface ([pesquisa de membros](expressions.md#member-lookup)).

### <a name="array-members"></a>Membros da matriz

Os membros de uma matriz são os membros herdados da classe `System.Array`.

### <a name="delegate-members"></a>Delegar Membros

Os membros de um delegado são os membros herdados da classe `System.Delegate`.

## <a name="member-access"></a>Acesso de membros

Declarações de Membros permitem o controle sobre o acesso de membro. A acessibilidade de um membro é estabelecida pela acessibilidade declarada ([acessibilidade declarada](basic-concepts.md#declared-accessibility)) do membro combinado com a acessibilidade do tipo que o contém imediatamente, se houver.

Quando o acesso a um membro específico é permitido, o membro é dito como ***acessível***. Por outro lado, quando o acesso a um determinado membro não é permitido, o membro é dito como ***inacessível***. O acesso a um membro é permitido quando o local textual no qual o acesso ocorre está incluído no domínio de acessibilidade ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)) do membro.

### <a name="declared-accessibility"></a>Acessibilidade declarada

A ***acessibilidade declarada*** de um membro pode ser uma das seguintes:

*  Public, que é selecionado incluindo um modificador `public` na declaração de membro. O significado intuitivo de `public` é "acesso não limitado".
*  Protegido, que é selecionado incluindo um modificador `protected` na declaração de membro. O significado intuitivo de `protected` é "acesso limitado à classe que a contém ou tipos derivados da classe que a contém".
*  Interno, que é selecionado incluindo um modificador `internal` na declaração de membro. O significado intuitivo de `internal` é "acesso limitado a este programa".
*  Protegido interno (ou seja, protegido ou interno), que é selecionado incluindo um `protected` e um modificador `internal` na declaração de membro. O significado intuitivo de `protected internal` é "acesso limitado a este programa ou tipos derivados da classe que a contém".
*  Privado, que é selecionado incluindo um modificador `private` na declaração de membro. O significado intuitivo de `private` é "acesso limitado ao tipo recipiente".

Dependendo do contexto no qual uma declaração de membro ocorre, somente determinados tipos de acessibilidade declarada são permitidos. Além disso, quando uma declaração de membro não inclui nenhum modificador de acesso, o contexto no qual a declaração ocorre determina a acessibilidade declarada padrão.

*  Os namespaces implicitamente têm `public` declaração de acessibilidade. Nenhum modificador de acesso é permitido em declarações de namespace.
*  Tipos declarados em unidades de compilação ou namespaces podem ter `public` ou `internal` declarado como acessibilidade e o padrão para `internal` a acessibilidade declarada.
*  Os membros de classe podem ter qualquer um dos cinco tipos de acessibilidade declarada e o padrão para `private` declarou a acessibilidade. (Observe que um tipo declarado como membro de uma classe pode ter qualquer um dos cinco tipos de acessibilidade declarada, enquanto um tipo declarado como membro de um namespace pode ter apenas `public` ou `internal`, a acessibilidade foi declarada.)
*  Os membros de struct podem ter a acessibilidade `public`, `internal` ou `private` declarada e padrão para `private` a acessibilidade declarada porque as estruturas são seladas implicitamente. Membros de struct introduzidos em uma struct (ou seja, não herdados por essa struct) não podem ter `protected` ou `protected internal` declarou acessibilidade. (Observe que um tipo declarado como membro de um struct pode ter `public`, `internal` ou `private` declarados como acessibilidade, enquanto um tipo declarado como membro de um namespace pode ter apenas `public` ou `internal` com acessibilidade declarada.)
*  Os membros da interface têm implicitamente `public` com acessibilidade declarada. Nenhum modificador de acesso é permitido em declarações de membro de interface.
*  Os membros de enumeração têm implicitamente `public` declarou acessibilidade. Nenhum modificador de acesso é permitido em declarações de membro de enumeração.

### <a name="accessibility-domains"></a>Domínios de acessibilidade

O ***domínio de acessibilidade*** de um membro consiste nas seções (possivelmente disjunção) do texto do programa no qual o acesso ao membro é permitido. Para fins de definição do domínio de acessibilidade de um membro, um membro será considerado de ***nível superior*** se ele não for declarado dentro de um tipo, e um membro será considerado ***aninhado*** se for declarado dentro de outro tipo. Além disso, o ***texto do programa*** de um programa é definido como todo o texto do programa contido em todos os arquivos de origem do programa, e o texto do programa de um tipo é definido como todo o texto do programa contido no *type_declaration*s desse tipo (incluindo, possivelmente, tipos que são aninhados dentro do tipo).

O domínio de acessibilidade de um tipo predefinido (como `object`, `int` ou `double`) é ilimitado.

O domínio de acessibilidade de um tipo não associado de nível superior `T` ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)) que é declarado em um programa `P` é definido da seguinte maneira:

*  Se a acessibilidade declarada de `T` for `public`, o domínio de acessibilidade de `T` será o texto do programa de `P` e qualquer programa que faça referência a `P`.
*  Se a acessibilidade declarada de `T` for `internal`, o domínio de acessibilidade de `T` será o texto de programa de `P`.

A partir dessas definições, a seguir, o domínio de acessibilidade de um tipo não associado de nível superior é sempre pelo menos o texto do programa do programa no qual esse tipo é declarado.

O domínio de acessibilidade para um tipo construído `T<A1, ..., An>` é a interseção do domínio de acessibilidade do tipo genérico não associado `T` e os domínios de acessibilidade dos argumentos de tipo `A1, ..., An`.

O domínio de acessibilidade de um membro aninhado `M` declarado em um tipo `T` em um programa `P` é definido da seguinte maneira (observar que `M` em si pode ser um tipo):

*  Se a acessibilidade declarada de `M` for `public`, o domínio de acessibilidade de `M` será o domínio de acessibilidade de `T`.
*  Se a acessibilidade declarada de `M` for `protected internal`, permita que `D` seja a União do texto do programa de `P` e o texto do programa de qualquer tipo derivado de `T`, que é declarado fora de `P`. O domínio de acessibilidade de `M` é a interseção do domínio de acessibilidade de `T` com `D`.
*  Se a acessibilidade declarada de `M` for `protected`, permita que `D` seja a União do texto do programa de `T` e o texto do programa de qualquer tipo derivado de `T`. O domínio de acessibilidade de `M` é a interseção do domínio de acessibilidade de `T` com `D`.
*  Se a acessibilidade declarada de `M` for `internal`, o domínio de acessibilidade de `M` será a interseção do domínio de acessibilidade de `T` com o texto de programa de `P`.
*  Se a acessibilidade declarada de `M` for `private`, o domínio de acessibilidade de `M` será o texto de programa de `T`.

A partir dessas definições, a seguir, o domínio de acessibilidade de um membro aninhado é sempre pelo menos o texto do programa do tipo no qual o membro é declarado. Além disso, ele segue que o domínio de acessibilidade de um membro nunca é mais inclusivo do que o domínio de acessibilidade do tipo no qual o membro é declarado.

Em termos intuitivos, quando um tipo ou membro `M` é acessado, as etapas a seguir são avaliadas para garantir que o acesso seja permitido:

*  Primeiro, se `M` for declarado dentro de um tipo (em oposição a uma unidade de compilação ou um namespace), ocorrerá um erro em tempo de compilação se esse tipo não estiver acessível.
*  Em seguida, se `M` for `public`, o acesso será permitido.
*  Caso contrário, se `M` for `protected internal`, o acesso será permitido se ocorrer dentro do programa no qual `M` é declarado ou se ocorrer dentro de uma classe derivada da classe na qual o `M` é declarado e ocorre por meio do tipo de classe derivada ([protegido acesso para membros da instância](basic-concepts.md#protected-access-for-instance-members)).
*  Caso contrário, se `M` for `protected`, o acesso será permitido se ocorrer dentro da classe na qual `M` é declarado, ou se ocorrer dentro de uma classe derivada da classe na qual `M` é declarado e ocorre por meio do tipo de classe derivada ([protegido acesso para membros da instância](basic-concepts.md#protected-access-for-instance-members)).
*  Caso contrário, se `M` for `internal`, o acesso será permitido se ocorrer dentro do programa no qual `M` é declarado.
*  Caso contrário, se `M` for `private`, o acesso será permitido se ocorrer dentro do tipo no qual `M` é declarado.
*  Caso contrário, o tipo ou o membro será inacessível e ocorrerá um erro em tempo de compilação.

No exemplo
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
as classes e os membros têm os seguintes domínios de acessibilidade:

*  O domínio de acessibilidade de `A` e `A.X` é ilimitado.
*  O domínio de acessibilidade de `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X` e `B.C.Y` é o texto do programa que contém o programa.
*  O domínio de acessibilidade de `A.Z` é o texto do programa do `A`.
*  O domínio de acessibilidade de `B.Z` e `B.D` é o texto do programa de `B`, incluindo o texto do programa de `B.C` e `B.D`.
*  O domínio de acessibilidade de `B.C.Z` é o texto do programa do `B.C`.
*  O domínio de acessibilidade de `B.D.X` e `B.D.Y` é o texto do programa de `B`, incluindo o texto do programa de `B.C` e `B.D`.
*  O domínio de acessibilidade de `B.D.Z` é o texto do programa do `B.D`.

Como ilustra o exemplo, o domínio de acessibilidade de um membro nunca é maior que o de um tipo recipiente. Por exemplo, embora todos os membros `X` tenham acessibilidade declarada pública, tudo, mas `A.X`, têm domínios de acessibilidade restritos por um tipo recipiente.

Conforme descrito em [Membros](basic-concepts.md#members), todos os membros de uma classe base, exceto para construtores de instância, destruidores e construtores estáticos, são herdados por tipos derivados. Isso inclui até membros privados de uma classe base. No entanto, o domínio de acessibilidade de um membro privado inclui apenas o texto do programa do tipo no qual o membro é declarado. No exemplo
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
a classe `B` herda o membro privado `x` da classe `A`. Como o membro é privado, ele só é acessível dentro do *class_body* de `A`. Assim, o acesso a `b.x` é bem-sucedida no método `A.F`, mas falha no método `B.F`.

### <a name="protected-access-for-instance-members"></a>Acesso protegido para membros de instância

Quando um membro da instância `protected` é acessado fora do texto do programa da classe na qual ele é declarado e quando um membro da instância `protected internal` é acessado fora do texto do programa no qual ele é declarado, o acesso deve ocorrer dentro de uma classe declaração que deriva da classe na qual ela é declarada. Além disso, o acesso é necessário para ocorrer por meio de uma instância desse tipo de classe derivada ou um tipo de classe construído a partir dele. Essa restrição impede que uma classe derivada acesse membros protegidos de outras classes derivadas, mesmo quando os membros são herdados da mesma classe base.

Permita que `B` seja uma classe base que declare um membro de instância protegida `M` e permita que `D` seja uma classe derivada de `B`. Dentro do *class_body* de `D`, o acesso a `M` pode ter uma das seguintes formas:

*  Um *type_name* ou *primary_expression* não qualificado do formulário `M`.
*  Um *primary_expression* do formulário `E.M`, desde que o tipo de `E` seja `T` ou uma classe derivada de `T`, em que `T` é o tipo de classe `D` ou um tipo de classe construído a partir de `D`
*  Um *primary_expression* do formulário `base.M`.

Além dessas formas de acesso, uma classe derivada pode acessar um construtor de instância protegida de uma classe base em um *constructor_initializer* ([inicializadores de Construtor](classes.md#constructor-initializers)).

No exemplo
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
dentro de `A`, é possível acessar `x` por meio de instâncias de `A` e `B`, já que, em ambos os casos, o acesso ocorre por meio de uma instância de `A` ou uma classe derivada de `A`. No entanto, dentro de `B`, não é possível acessar `x` por meio de uma instância de `A`, já que `A` não deriva de `B`.

No exemplo
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
as três atribuições para `x` são permitidas porque todas elas ocorrem por meio de instâncias de tipos de classe construídos a partir do tipo genérico.

### <a name="accessibility-constraints"></a>Restrições de acessibilidade

Várias construções no C# idioma exigem que um tipo seja ***pelo menos como acessível como*** um membro ou outro tipo. Um tipo `T` é considerado pelo menos como acessível como um membro ou tipo `M` se o domínio de acessibilidade de `T` for um superconjunto do domínio de acessibilidade de `M`. Em outras palavras, `T` é pelo menos como acessível como `M` se `T` estiver acessível em todos os contextos nos quais `M` está acessível.

Existem as seguintes restrições de acessibilidade:

*  A classe base direta de um tipo de classe deve ser, pelo menos, tão acessível quanto o próprio tipo de classe.
*  As interfaces base explícitas de um tipo de interface devem ser, pelo menos, tão acessíveis quanto o próprio tipo de interface.
*  O tipo de retorno e os tipos de parâmetro de um tipo delegado devem ser, pelo menos, tão acessíveis quanto o próprio tipo delegado.
*  O tipo de uma constante deve ser, pelo menos, tão acessível quanto a própria constante.
*  O tipo de um campo deve ser, pelo menos, tão acessível quanto o próprio campo.
*  O tipo de retorno e os tipos de parâmetro de um método devem ser, pelo menos, tão acessíveis quanto o próprio método.
*  O tipo de uma propriedade deve ser, pelo menos, tão acessível quanto a propriedade em si.
*  O tipo de um evento deve ser, pelo menos, tão acessível quanto o próprio evento.
*  O tipo e os tipos de parâmetro de um indexador devem ser, pelo menos, tão acessíveis quanto o próprio indexador.
*  O tipo de retorno e os tipos de parâmetro de um operador devem ser, pelo menos, tão acessíveis quanto o próprio operador.
*  Os tipos de parâmetro de um construtor de instância devem ser pelo menos tão acessíveis quanto o próprio construtor de instância.

No exemplo
```csharp
class A {...}

public class B: A {...}
```
a classe `B` resulta em um erro de tempo de compilação porque `A` não é pelo menos tão acessível quanto `B`.

Da mesma forma, no exemplo
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
o método `H` no `B` resulta em um erro de tempo de compilação porque o tipo de retorno `A` não é pelo menos tão acessível quanto o método.

## <a name="signatures-and-overloading"></a>Assinaturas e sobrecargas

Os métodos, construtores de instância, indexadores e operadores são caracterizados por suas ***assinaturas***:

*  A assinatura de um método consiste no nome do método, no número de parâmetros de tipo e no tipo e tipo (valor, referência ou saída) de cada um de seus parâmetros formais, considerados na ordem da esquerda para a direita. Para essas finalidades, qualquer parâmetro de tipo do método que ocorre no tipo de um parâmetro formal é identificado não por seu nome, mas por sua posição ordinal na lista de argumentos de tipo do método. A assinatura de um método especificamente não inclui o tipo de retorno, o modificador `params` que pode ser especificado para o parâmetro mais à direita, nem as restrições de parâmetro de tipo opcionais.
*  A assinatura de um construtor de instância consiste no tipo e tipo (valor, referência ou saída) de cada um de seus parâmetros formais, considerados na ordem da esquerda para a direita. A assinatura de um construtor de instância especificamente não inclui o modificador `params` que pode ser especificado para o parâmetro mais à direita.
*  A assinatura de um indexador consiste no tipo de cada um de seus parâmetros formais, considerados na ordem da esquerda para a direita. A assinatura de um indexador especificamente não inclui o tipo de elemento, nem inclui o modificador `params` que pode ser especificado para o parâmetro mais à direita.
*  A assinatura de um operador consiste no nome do operador e no tipo de cada um de seus parâmetros formais, considerados na ordem da esquerda para a direita. A assinatura de um operador especificamente não inclui o tipo de resultado.

As assinaturas são o mecanismo de habilitação para ***sobrecarga*** de membros em classes, estruturas e interfaces:

*  A sobrecarga de métodos permite que uma classe, estrutura ou interface declare vários métodos com o mesmo nome, desde que suas assinaturas sejam exclusivas nessa classe, struct ou interface.
*  A sobrecarga de construtores de instância permite que uma classe ou estrutura declare vários construtores de instância, desde que suas assinaturas sejam exclusivas dentro dessa classe ou estrutura.
*  A sobrecarga de indexadores permite que uma classe, estrutura ou interface declare vários indexadores, desde que suas assinaturas sejam exclusivas nessa classe, struct ou interface.
*  A sobrecarga de operadores permite que uma classe ou estrutura declare vários operadores com o mesmo nome, desde que suas assinaturas sejam exclusivas dentro dessa classe ou estrutura.

Embora os modificadores de parâmetro `out` e `ref` sejam considerados parte de uma assinatura, os membros declarados em um único tipo não podem diferir na assinatura exclusivamente por `ref` e `out`. Ocorrerá um erro de tempo de compilação se dois membros forem declarados no mesmo tipo com assinaturas que seriam as mesmas se todos os parâmetros em ambos os métodos com modificadores `out` foram alterados para os modificadores `ref`. Para outras finalidades de correspondência de assinatura (por exemplo, ocultar ou substituir), `ref` e `out` são considerados parte da assinatura e não coincidem. (Essa restrição é permitir que C# os programas sejam facilmente convertidos para serem executados no Common Language Infrastructure (CLI), que não fornece uma maneira de definir métodos que diferem somente em `ref` e `out`.)

Para fins de assinatura, os tipos `object` e `dynamic` são considerados iguais. Os membros declarados em um único tipo podem, portanto, não ser diferentes na assinatura exclusivamente por `object` e `dynamic`.

O exemplo a seguir mostra um conjunto de declarações de método sobrecarregadas junto com suas assinaturas.
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

Observe que quaisquer modificadores de parâmetro `ref` e `out` ([parâmetros de método](classes.md#method-parameters)) fazem parte de uma assinatura. Assim, `F(int)` e `F(ref int)` são assinaturas exclusivas. No entanto, `F(ref int)` e `F(out int)` não podem ser declarados na mesma interface porque suas assinaturas diferem exclusivamente por `ref` e `out`. Além disso, observe que o tipo de retorno e o modificador `params` não fazem parte de uma assinatura, portanto não é possível sobrecarregar unicamente com base no tipo de retorno ou na inclusão ou exclusão do modificador `params`. Como tal, as declarações dos métodos `F(int)` e `F(params string[])` identificados acima resultam em um erro de tempo de compilação.

## <a name="scopes"></a>Escopos

O ***escopo*** de um nome é a região do texto do programa no qual é possível fazer referência à entidade declarada pelo nome sem qualificação do nome. Os escopos podem ser ***aninhados***, e um escopo interno pode redeclarar o significado de um nome a partir de um escopo externo (isso não, no entanto, remover a restrição imposta por [declarações](basic-concepts.md#declarations) que dentro de um bloco aninhado não é possível declarar uma variável local com o mesmo nome que uma variável local em um bloco delimitador). O nome do escopo externo, em seguida, é dito como ***oculto*** na região do texto do programa coberto pelo escopo interno, e o acesso ao nome externo só é possível qualificando o nome.

*  O escopo de um membro de namespace declarado por um *namespace_member_declaration* ([membros de namespace](namespaces.md#namespace-members)) sem delimitador *namespace_declaration* é o texto completo do programa.
*  O escopo de um membro de namespace declarado por um *namespace_member_declaration* em um *namespace_declaration* cujo nome totalmente qualificado é `N` é o *namespace_body* de cada *namespace_declaration* , cujo total o nome qualificado é `N` ou começa com `N`, seguido de um ponto.
*  O escopo do nome definido por um *extern_alias_directive* estende-se pelos *using_directive*s, *global_attributes* e *namespace_member_declaration*s de seu logo contendo a unidade de compilação ou o corpo do namespace. Um *extern_alias_directive* não contribui com nenhum novo membro para o espaço de declaração subjacente. Em outras palavras, um *extern_alias_directive* não é transitiva, mas, em vez disso, afeta apenas a unidade de compilação ou o corpo do namespace no qual ele ocorre.
*  O escopo de um nome definido ou importado por um *using_directive* ([usando as diretivas](namespaces.md#using-directives)) estende-se pelos *namespace_member_declaration*s do *compilation_unit* ou *namespace_body* no qual o *using_directive* ocorre. Um *using_directive* pode tornar zero ou mais nomes de namespace, tipo ou membro disponíveis em um *compilation_unit* ou *namespace_body*específico, mas não contribui com novos membros para o espaço de declaração subjacente. Em outras palavras, um *using_directive* não é transitiva, mas afeta apenas o *compilation_unit* ou *namespace_body* em que ele ocorre.
*  O escopo de um parâmetro de tipo declarado por um *type_parameter_list* em um *class_declaration* ([declarações de classe](classes.md#class-declarations)) é o *class_base*, *type_parameter_constraints_clause*s e *class_body* de que  *class_declaration*.
*  O escopo de um parâmetro de tipo declarado por um *type_parameter_list* em *um struct_declaration* ([declarações struct](structs.md#struct-declarations)) é *struct_interfaces*, *type_parameter_constraints_clause*s e *struct_body* de Esse *struct_declaration*.
*  O escopo de um parâmetro de tipo declarado por um *type_parameter_list* em um *interface_declaration* ([declarações de interface](interfaces.md#interface-declarations)) é *interface_base*, *type_parameter_constraints_clause*s e *interface_body* desse *interface_declaration*.
*  O escopo de um parâmetro de tipo declarado por um *type_parameter_list* em um *delegate_declaration* ([declarações de representante](delegates.md#delegate-declarations)) é *return_type*, *formal_parameter_list*e *type_parameter_constraints_clause* s desse *delegate_declaration*.
*  O escopo de um membro declarado por um *class_member_declaration* ([corpo de classe](classes.md#class-body)) é o *class_body* no qual a declaração ocorre. Além disso, o escopo de um membro de classe se estende para o *class_body* dessas classes derivadas que são incluídas no domínio de acessibilidade ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)) do membro.
*  O escopo de um membro declarado por um *struct_member_declaration* ([membros de struct](structs.md#struct-members)) é o *struct_body* no qual a declaração ocorre.
*  O escopo de um membro declarado por um *enum_member_declaration* ([membros de enum](enums.md#enum-members)) é o *enum_body* no qual a declaração ocorre.
*  O escopo de um parâmetro declarado em um *method_declaration* ([métodos](classes.md#methods)) é o *method_body* desse *method_declaration*.
*  O escopo de um parâmetro declarado em um *indexer_declaration* ([indexadores](classes.md#indexers)) é o *accessor_declarations* desse *indexer_declaration*.
*  O escopo de um parâmetro declarado em um *operator_declaration* ([Operators](classes.md#operators)) é o *bloco* desse *operator_declaration*.
*  O escopo de um parâmetro declarado em um *constructor_declaration* ([construtores de instância](classes.md#instance-constructors)) é o *constructor_initializer* e o *bloco* desse *constructor_declaration*.
*  O escopo de um parâmetro declarado em um *lambda_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) é o *anonymous_function_body* desse *lambda_expression*
*  O escopo de um parâmetro declarado em um *anonymous_method_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) é o *bloco* desse *anonymous_method_expression*.
*  O escopo de um rótulo declarado em um *labeled_statement* ([instruções rotuladas](statements.md#labeled-statements)) é o *bloco* no qual a declaração ocorre.
*  O escopo de uma variável local declarada em um *local_variable_declaration* ([declarações de variável local](statements.md#local-variable-declarations)) é o bloco no qual a declaração ocorre.
*  O escopo de uma variável local declarada em um *switch_block* de uma instrução `switch` ([a instrução switch](statements.md#the-switch-statement)) é o *switch_block*.
*  O escopo de uma variável local declarada em um *for_initializer* de uma instrução `for` ([a instrução for](statements.md#the-for-statement)) *é for_initializer*, *for_condition*, *for_iterator*e a *instrução* contida do instrução `for`.
*  O escopo de uma constante local declarada em uma *local_constant_declaration* ([declarações de constantes locais](statements.md#local-constant-declarations)) é o bloco no qual a declaração ocorre. É um erro de tempo de compilação para se referir a uma constante local em uma posição textual que precede seu *constant_declarator*.
*  O escopo de uma variável declarada como parte de um *foreach_statement*, *using_statement*, *lock_statement* ou *query_expression* é determinado pela expansão da construção fornecida.

Dentro do escopo de um namespace, classe, struct ou membro de enumeração, é possível fazer referência ao membro em uma posição textual que precede a declaração do membro. Por exemplo
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Aqui, é válido para `F` fazer referência a `i` antes de ser declarado.

Dentro do escopo de uma variável local, é um erro de tempo de compilação para se referir à variável local em uma posição textual que precede o *local_variable_declarator* da variável local. Por exemplo
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

No método `F` acima, a primeira atribuição para `i` especificamente não se refere ao campo declarado no escopo externo. Em vez disso, ele se refere à variável local e resulta em um erro de tempo de compilação porque ele precede textualmente a declaração da variável. No método `G`, o uso de `j` no inicializador para a declaração de `j` é válido porque o uso não precede o *local_variable_declarator*. No método `H`, um *local_variable_declarator* subsequente se refere corretamente a uma variável local declarada em um *local_variable_declarator* anterior no mesmo *local_variable_declaration*.

As regras de escopo para variáveis locais são projetadas para garantir que o significado de um nome usado em um contexto de expressão seja sempre o mesmo dentro de um bloco. Se o escopo de uma variável local fosse estender apenas de sua declaração para o final do bloco, no exemplo acima, a primeira atribuição seria atribuída à variável de instância e a segunda atribuição seria atribuída à variável local, possivelmente levando a erros de tempo de compilação se as instruções do bloco foram posteriormente reorganizadas.

O significado de um nome dentro de um bloco pode ser diferente com base no contexto no qual o nome é usado. No exemplo
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
o nome `A` é usado em um contexto de expressão para se referir à variável local `A` e em um contexto de tipo para se referir à classe `A`.

### <a name="name-hiding"></a>Ocultação de nomes

O escopo de uma entidade normalmente abrange mais texto de programa do que o espaço de declaração da entidade. Em particular, o escopo de uma entidade pode incluir declarações que apresentam novos espaços de declaração contendo entidades de mesmo nome. Tais declarações fazem com que a entidade original se torne ***oculta***. Por outro lado, uma entidade é considerada ***visível*** quando não está oculta.

A ocultação de nome ocorre quando os escopos se sobrepõem no aninhamento e quando os escopos se sobrepõem pela herança As características dos dois tipos de ocultação são descritas nas seções a seguir.

#### <a name="hiding-through-nesting"></a>Ocultando por meio de aninhamento

O nome que oculta o aninhamento pode ocorrer como resultado de aninhar namespaces ou tipos dentro de namespaces, como resultado de aninhar tipos dentro de classes ou estruturas, e como resultado de declarações de parâmetro e variável local.

No exemplo
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
dentro do método `F`, a variável de instância `i` é ocultada pela variável local `i`, mas dentro do método `G`, `i` ainda se refere à variável de instância.

Quando um nome em um escopo interno oculta um nome em um escopo externo, ele oculta todas as ocorrências sobrecarregadas desse nome. No exemplo
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
a chamada `F(1)` invoca o `F` declarado em `Inner` porque todas as ocorrências externas de `F` são ocultadas pela declaração interna. Pelo mesmo motivo, a chamada `F("Hello")` resulta em um erro de tempo de compilação.

#### <a name="hiding-through-inheritance"></a>Ocultando por meio de herança

O nome que se oculta pela herança ocorre quando as classes ou estruturas redeclaram nomes que foram herdados de classes base. Esse tipo de ocultação de nome usa uma das seguintes formas:

*  Uma constante, campo, propriedade, evento ou tipo introduzido em uma classe ou struct oculta todos os membros da classe base com o mesmo nome.
*  Um método introduzido em uma classe ou struct oculta todos os membros da classe base que não são de método com o mesmo nome e todos os métodos da classe base com a mesma assinatura (nome do método e contagem de parâmetros, modificadores e tipos).
*  Um indexador introduzido em uma classe ou estrutura oculta todos os indexadores de classe base com a mesma assinatura (contagem de parâmetros e tipos).

As regras que regem as declarações de operador ([operadores](classes.md#operators)) tornam impossível para uma classe derivada declarar um operador com a mesma assinatura de um operador em uma classe base. Portanto, os operadores nunca ocultam um ao outro.

Ao contrário de ocultar um nome de um escopo externo, ocultar um nome acessível de um escopo herdado faz com que um aviso seja relatado. No exemplo
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
a declaração de `F` em `Derived` faz com que um aviso seja relatado. Ocultar um nome herdado não é especificamente um erro, já que isso impediria uma evolução separada das classes base. Por exemplo, a situação acima pode ter surgido porque uma versão mais recente do `Base` introduziu um método `F` que não estava presente em uma versão anterior da classe. Se houvesse um erro na situação acima, qualquer alteração feita em uma classe base em uma biblioteca de classes com controle de versão separada poderia potencialmente fazer com que as classes derivadas se tornassem inválidas.

O aviso causado pela ocultação de um nome herdado pode ser eliminado por meio do uso do modificador `new`:
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

O modificador `new` indica que o `F` em `Derived` é "novo" e que, na verdade, é destinado a ocultar o membro herdado.

Uma declaração de um novo membro oculta um membro herdado somente dentro do escopo do novo membro.

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

No exemplo acima, a declaração de `F` em `Derived` oculta o `F` que foi herdado de `Base`, mas como o novo `F` em `Derived` tem acesso privado, seu escopo não se estende para `MoreDerived`. Assim, a chamada `F()` em `MoreDerived.G` é válida e invocará `Base.F`.

## <a name="namespace-and-type-names"></a>Namespace e nomes de tipo

Vários contextos em um C# programa exigem que um *Namespace_Name* ou um *type_name* seja especificado.

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

Um *Namespace_Name* é um *namespace_or_type_name* que se refere a um namespace. Após a resolução, conforme descrito abaixo, o *namespace_or_type_name* de um *Namespace_Name* deve se referir a um namespace ou, caso contrário, ocorrerá um erro de tempo de compilação. Nenhum argumento de tipo ([argumentos de tipo](types.md#type-arguments)) pode estar presente em um *Namespace_Name* (somente tipos podem ter argumentos de tipo).

Um *type_name* é um *namespace_or_type_name* que se refere a um tipo. Após a resolução, conforme descrito abaixo, o *namespace_or_type_name* de um *type_name* deve se referir a um tipo ou, caso contrário, ocorrerá um erro de tempo de compilação.

Se *namespace_or_type_name* for um membro de alias qualificado, seu significado será conforme descrito em [qualificadores de alias de namespace](namespaces.md#namespace-alias-qualifiers). Caso contrário, um *namespace_or_type_name* terá uma das quatro formas:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

onde `I` é um único identificador, `N` é um *namespace_or_type_name* e `<A1, ..., Ak>` é um *type_argument_list*opcional. Quando nenhum *type_argument_list* for especificado, considere `k` como zero.

O significado de um *namespace_or_type_name* é determinado da seguinte maneira:

*   Se *namespace_or_type_name* estiver no formato `I` ou no formato `I<A1, ..., Ak>`:
    * Se `K` for zero e o *namespace_or_type_name* aparecer dentro de uma declaração de método genérico ([métodos](classes.md#methods)) e se essa declaração incluir um parâmetro de tipo ([parâmetros de tipo](classes.md#type-parameters)) com o nome @ no__t-4, o *namespace_or_type_ nome* refere-se a esse parâmetro de tipo.
    * Caso contrário, se o *namespace_or_type_name* aparecer dentro de uma declaração de tipo, para cada tipo de instância @ no__t-1 ([o tipo de instância](classes.md#the-instance-type)), começando com o tipo de instância dessa declaração de tipo e continuando com o tipo de instância de cada declaração de classe ou struct de circunscrição (se houver):
        * Se `K` for zero e a declaração de `T` incluir um parâmetro de tipo com o nome @ no__t-2, o *namespace_or_type_name* se referirá a esse parâmetro de tipo.
        * Caso contrário, se o *namespace_or_type_name* aparecer no corpo da declaração de tipo e `T` ou qualquer um de seus tipos base contiver um tipo acessível aninhado com os parâmetros Name @ no__t-2 e `K` @ no__t-4type, então o *namespace_or_type _name* refere-se a esse tipo construído com os argumentos de tipo fornecidos. Se houver mais de um tipo, o tipo declarado dentro do tipo mais derivado será selecionado. Observe que os membros sem tipo (constantes, campos, métodos, propriedades, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e membros de tipo com um número diferente de parâmetros de tipo são ignorados ao determinar o significado do *namespace_or_type_name*.
    * Se as etapas anteriores não tiverem sido bem-sucedidas, para cada namespace @ no__t-0, começando com o namespace no qual o *namespace_or_type_name* ocorre, continuando com cada namespace delimitador (se houver) e terminando com o namespace global, o seguinte as etapas são avaliadas até que uma entidade esteja localizada:
        * Se `K` for zero e `I` for o nome de um namespace em @ no__t-2, então:
            * Se o local onde o *namespace_or_type_name* ocorrer é incluído por uma declaração de namespace para `N` e a declaração de namespace contém um *extern_alias_directive* ou *using_alias_directive* que associa o nome @ no __t-4 com um namespace ou tipo, o *namespace_or_type_name* é ambíguo e ocorre um erro de tempo de compilação.
            * Caso contrário, o *namespace_or_type_name* refere-se ao namespace chamado `I` em `N`.
        * Caso contrário, se `N` contiver um tipo acessível com os parâmetros Name @ no__t-1 e `K` @ no__t-3type, então:
            * Se `K` for zero e o local em que o *namespace_or_type_name* ocorrer for incluído por uma declaração de namespace para `N` e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome @ no__t-5 a um namespace ou tipo, então, o *namespace_or_type_name* é ambíguo e ocorre um erro de tempo de compilação.
            * Caso contrário, o *namespace_or_type_name* se refere ao tipo construído com os argumentos de tipo fornecidos.
        * Caso contrário, se o local onde o *namespace_or_type_name* ocorrer for incluído por uma declaração de namespace para `N`:
            * Se `K` for zero e a declaração de namespace contiver um *extern_alias_directive* ou um *using_alias_directive* que associa o nome @ no__t-3 a um namespace ou tipo importado, o *namespace_or_type_name* se referirá a isso namespace ou tipo.
            * Caso contrário, se os namespaces e as declarações de tipo importados pelos *using_namespace_directive*s e *using_alias_directive*s da declaração de namespace contiverem exatamente um tipo acessível com o nome @ no__t-2 e `K` @ no__t-4type os parâmetros, em seguida, o *namespace_or_type_name* refere-se a esse tipo construído com os argumentos de tipo fornecidos.
            * Caso contrário, se os namespaces e as declarações de tipo importados pelos *using_namespace_directive*s e *using_alias_directive*s da declaração de namespace contiverem mais de um tipo acessível com o nome @ no__t-2 e `K` @ no__t-4type parâmetros, o *namespace_or_type_name* é ambíguo e ocorre um erro.
    * Caso contrário, o *namespace_or_type_name* será indefinido e ocorrerá um erro em tempo de compilação.
*  Caso contrário, o *namespace_or_type_name* será do formato `N.I` ou do formulário `N.I<A1, ..., Ak>`. `N` é resolvido primeiro como um *namespace_or_type_name*. Se a resolução de `N` não for bem-sucedida, ocorrerá um erro em tempo de compilação. Caso contrário, `N.I` ou `N.I<A1, ..., Ak>` será resolvido da seguinte maneira:
    * Se `K` for zero e `N` se referir a um namespace e `N` contiver um namespace aninhado com o nome `I`, o *namespace_or_type_name* se referirá a esse namespace aninhado.
    * Caso contrário, se `N` se referir a um namespace e `N` contiver um tipo acessível com os parâmetros Name @ no__t-2 e `K` @ no__t-4type, o *namespace_or_type_name* se referirá a esse tipo construído com os argumentos de tipo fornecidos.
    * Caso contrário, se `N` se referir a uma classe (possivelmente construída) ou tipo struct e `N` ou qualquer uma de suas classes base contiverem um tipo acessível aninhado com os parâmetros Name @ no__t-2 e `K` @ no__t-4type, o *namespace_or_type_name* se referirá a Esse tipo construído com os argumentos de tipo fornecidos. Se houver mais de um tipo, o tipo declarado dentro do tipo mais derivado será selecionado. Observe que, se o significado de `N.I` estiver sendo determinado como parte da resolução da especificação de classe base de `N`, a classe base direta de `N` será considerada objeto ([classes base](classes.md#base-classes)).
    * Caso contrário, `N.I` é um *namespace_or_type_name*inválido e ocorre um erro de tempo de compilação.

Um *namespace_or_type_name* tem permissão para fazer referência a uma classe estática ([classes estáticas](classes.md#static-classes)) somente se

*  O *namespace_or_type_name* é o `T` em um *namespace_or_type_name* do formulário `T.I` ou
*  O *namespace_or_type_name* é o `T` em um *typeof_expression* ([lista de argumentos](expressions.md#argument-lists)1) do formato `typeof(T)`.

### <a name="fully-qualified-names"></a>Nomes totalmente qualificados

Cada namespace e tipo tem um ***nome totalmente qualificado***, que identifica exclusivamente o namespace ou o tipo entre todos os outros. O nome totalmente qualificado de um namespace ou tipo `N` é determinado da seguinte maneira:

*  Se `N` for um membro do namespace global, seu nome totalmente qualificado será `N`.
*  Caso contrário, seu nome totalmente qualificado é `S.N`, em que `S` é o nome totalmente qualificado do namespace ou tipo no qual `N` é declarado.

Em outras palavras, o nome totalmente qualificado de `N` é o caminho hierárquico completo dos identificadores que levam a `N`, a partir do namespace global. Como cada membro de um namespace ou tipo deve ter um nome exclusivo, ele segue que o nome totalmente qualificado de um namespace ou tipo é sempre exclusivo.

O exemplo a seguir mostra várias declarações de namespace e tipo junto com seus nomes totalmente qualificados associados.
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>Gerenciamento automático de memória

C#o emprega o gerenciamento automático de memória, que libera os desenvolvedores de alocar e liberar manualmente a memória ocupada por objetos. As políticas de gerenciamento automático de memória são implementadas por um ***coletor de lixo***. O ciclo de vida de gerenciamento de memória de um objeto é o seguinte:

1. Quando o objeto é criado, a memória é alocada para ele, o construtor é executado e o objeto é considerado dinâmico.
2. Se o objeto, ou qualquer parte dele, não puder ser acessado por nenhuma possível continuação de execução, além da execução de destruidores, o objeto será considerado não em uso e se tornará elegível para destruição. O C# compilador e o coletor de lixo podem optar por analisar o código para determinar quais referências a um objeto podem ser usadas no futuro. Por exemplo, se uma variável local que está no escopo for a única referência existente a um objeto, mas essa variável local nunca for mencionada em qualquer continuação possível de execução do ponto de execução atual no procedimento, o coletor de lixo poderá (mas não é necessário para) tratar o objeto como não está mais em uso.
3. Depois que o objeto estiver qualificado para destruição, em algum momento não especificado, o destruidor ([destruidores](classes.md#destructors)) (se houver) para o objeto será executado. Em circunstâncias normais, o destruidor para o objeto é executado apenas uma vez, embora APIs específicas de implementação possam permitir que esse comportamento seja substituído.
4. Depois que o destruidor de um objeto for executado, se esse objeto ou qualquer parte dele, não puder ser acessado por qualquer continuação possível de execução, incluindo a execução de destruidores, o objeto será considerado inacessível e o objeto se tornará elegível para a coleta.
5. Finalmente, em algum momento depois que o objeto se tornar qualificado para a coleta, o coletor de lixo liberará a memória associada a esse objeto.

O coletor de lixo mantém informações sobre o uso do objeto e usa essas informações para tomar decisões de gerenciamento de memória, como o local na memória para localizar um objeto recém-criado, quando realocar um objeto e quando um objeto não está mais em uso ou inacessível.

Assim como outras linguagens que assumem a existência de um C# coletor de lixo, é projetada para que o coletor de lixo possa implementar uma ampla gama de políticas de gerenciamento de memória. Por exemplo, C# o não exige que os destruidores sejam executados ou que os objetos sejam coletados assim que estiverem qualificados ou que os destruidores sejam executados em qualquer ordem específica ou em qualquer thread específico.

O comportamento do coletor de lixo pode ser controlado, em algum grau, por meio de métodos estáticos na classe `System.GC`. Essa classe pode ser usada para solicitar que uma coleção ocorra, destruidores sejam executados (ou não executados) e assim por diante.

Como o coletor de lixo tem permissão de uma ampla latitude ao decidir quando coletar objetos e executar destruidores, uma implementação de conformidade pode produzir uma saída diferente da mostrada pelo código a seguir. O programa
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
Cria uma instância da classe `A` e uma instância da classe `B`. Esses objetos se tornam qualificados para coleta de lixo quando a variável `b` recebe o valor `null`, já que, após esse tempo, é impossível para qualquer código escrito pelo usuário acessá-los. A saída pode ser

```console
Destruct instance of A
Destruct instance of B
```
ou
```console
Destruct instance of B
Destruct instance of A
```
Porque o idioma impõe nenhuma restrição na ordem em que os objetos são coletados como lixo.

Em casos sutis, a distinção entre "qualificado para destruição" e "elegível para coleta" pode ser importante. Por exemplo,
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

No programa acima, se o coletor de lixo optar por executar o destruidor de `A` antes do destruidor de `B`, a saída desse programa poderá ser:
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Observe que, embora a instância de `A` não esteja em uso e o destruidor de `A` fosse executado, ainda é possível que os métodos de `A` (nesse caso, `F`) sejam chamados de outro destruidor. Além disso, observe que a execução de um destruidor pode fazer com que um objeto se torne utilizável do programa principal novamente. Nesse caso, a execução do destruidor de `B` causou uma instância de `A` que não estava em uso anteriormente para tornar-se acessível da referência dinâmica `Test.RefA`. Após a chamada para `WaitForPendingFinalizers`, a instância de `B` é elegível para a coleção, mas a instância de `A` não é, devido à referência `Test.RefA`.

Para evitar confusão e comportamento inesperado, geralmente é uma boa ideia para destruidores apenas realizar a limpeza nos dados armazenados nos próprios campos de seu objeto e não executar ações em objetos referenciados ou em campos estáticos.

Uma alternativa ao uso de destruidores é permitir que uma classe implemente a interface `System.IDisposable`. Isso permite que o cliente do objeto determine quando liberar os recursos do objeto, normalmente acessando o objeto como um recurso em uma instrução `using` ([a instrução using](statements.md#the-using-statement)).

## <a name="execution-order"></a>Ordem de execução

A execução de C# um programa prossegue de forma que os efeitos colaterais de cada thread em execução sejam preservados em pontos de execução críticos. Um ***efeito colateral*** é definido como uma leitura ou gravação de um campo volátil, uma gravação em uma variável não volátil, uma gravação em um recurso externo e o lançamento de uma exceção. Os pontos de execução críticos em que a ordem desses efeitos colaterais devem ser preservados são referências a campos voláteis ([campos voláteis](classes.md#volatile-fields)), instruções `lock` ([a instrução Lock](statements.md#the-lock-statement)) e a criação e o encerramento de threads. O ambiente de execução é livre para alterar a ordem de execução de C# um programa, sujeito às seguintes restrições:

*  A dependência de dados é preservada dentro de um thread de execução. Ou seja, o valor de cada variável é calculado como se todas as instruções no thread fossem executadas na ordem do programa original.
*  As regras de ordenação de inicialização são preservadas ([inicialização de campo](classes.md#field-initialization) e [inicializadores de variável](classes.md#variable-initializers)).
*  A ordenação de efeitos colaterais é preservada em relação às leituras e gravações voláteis ([campos voláteis](classes.md#volatile-fields)). Além disso, o ambiente de execução não precisa avaliar parte de uma expressão se puder deduzir que o valor da expressão não é usado e que nenhum efeito colateral necessário é produzido (incluindo qualquer causado pela chamada de um método ou acesso a um campo volátil). Quando a execução do programa é interrompida por um evento assíncrono (como uma exceção lançada por outro thread), não há garantia de que os efeitos laterais observáveis estão visíveis na ordem do programa original.
