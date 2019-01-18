---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229495"
---
# <a name="basic-concepts"></a>Conceitos básicos

## <a name="application-startup"></a>Inicialização do aplicativo

Um assembly que tem um ***ponto de entrada*** é chamado um ***aplicativo***. Quando um aplicativo é executado, uma nova ***domínio de aplicativo*** é criado. Várias instanciações diferentes de um aplicativo podem existir no mesmo computador ao mesmo tempo, e cada um tem seu próprio domínio de aplicativo.

Um domínio de aplicativo permite que o isolamento do aplicativo atuando como um contêiner para o estado do aplicativo. Um domínio de aplicativo atua como um contêiner e limites para os tipos definidos no aplicativo e as bibliotecas de classes que ele usa. Tipos carregados em um domínio de aplicativo são diferentes do mesmo tipo carregado em outro domínio de aplicativo e instâncias de objetos não são compartilhadas diretamente entre domínios de aplicativo. Por exemplo, cada domínio de aplicativo tem sua própria cópia de variáveis estáticas para esses tipos, e um construtor estático para um tipo é executado no máximo uma vez por domínio de aplicativo. Implementações são livres para fornecer a política específica de implementação ou mecanismos para a criação e destruição de domínios do aplicativo.

***Inicialização do aplicativo*** ocorre quando o ambiente de execução chama um método designado, o que é conhecido como ponto de entrada do aplicativo. Esse método de ponto de entrada é sempre denominado `Main`e pode ter uma das seguintes assinaturas:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Conforme mostrado, o ponto de entrada pode, opcionalmente, retornar um `int` valor. Isso retornará o valor é usado no encerramento do aplicativo ([terminação de aplicativos](basic-concepts.md#application-termination)).

O ponto de entrada, opcionalmente, pode ter um parâmetro formal. O parâmetro pode ter qualquer nome, mas o tipo do parâmetro deve ser `string[]`. Se o parâmetro formal estiver presente, o ambiente de execução cria e passa um `string[]` argumento que contém os argumentos de linha de comando que foram especificados quando o aplicativo foi iniciado. O `string[]` nunca o argumento for nulo, mas ele pode ter um comprimento de zero, se nenhum argumento de linha de comando foi especificado.

Uma vez que o c# dá suporte a sobrecarga de método, uma classe ou struct pode conter várias definições de algum método, desde que cada um tem uma assinatura diferente. No entanto, dentro de um único programa, nenhuma classe ou struct pode conter mais de um método chamado `Main` cuja definição é qualificado para ser usado como um ponto de entrada do aplicativo. Outras versões sobrecarregadas dos `Main` são permitidas, no entanto, desde que eles têm mais de um parâmetro ou o único parâmetro é diferente de tipo `string[]`.

Um aplicativo pode ser composto por várias classes ou estruturas. É possível que mais de uma dessas classes ou structs conter um método chamado `Main` cuja definição é qualificado para ser usado como um ponto de entrada do aplicativo. Nesses casos, um mecanismo externo (como uma opção de compilador de linha de comando) deve ser usado para selecionar uma destas opções `Main` métodos como o ponto de entrada.

No c#, cada método deve ser definido como um membro de uma classe ou struct. Normalmente, a acessibilidade declarada ([declarado acessibilidade](basic-concepts.md#declared-accessibility)) de um método é determinado pelos modificadores de acesso ([modificadores de acesso](classes.md#access-modifiers)) especificado na sua declaração e da mesma forma a declarado acessibilidade de um tipo é determinada pelos modificadores de acesso especificados na sua declaração. Para que um determinado método de um determinado tipo a ser chamado, o tipo e o membro devem ser acessíveis. No entanto, o ponto de entrada do aplicativo é um caso especial. Especificamente, o ambiente de execução pode acessar o ponto de entrada do aplicativo, independentemente de sua acessibilidade declarada e independentemente da acessibilidade declarada de suas declarações de tipo delimitador.

O método de ponto de entrada do aplicativo não pode ser em uma declaração de classe genérica.

Em todos os outros aspectos, os métodos do ponto de entrada se comportam como os que não são pontos de entrada.

## <a name="application-termination"></a>Encerramento do aplicativo

***Encerramento do aplicativo*** retorna o controle para o ambiente de execução.

Se o tipo de retorno da caixa de diálogo ***ponto de entrada*** método é `int`, o valor retornado serve como o aplicativo ***código de status do encerramento***. O objetivo desse código é permitir a comunicação de sucesso ou falha para o ambiente de execução.

Se for o tipo de retorno do método de ponto de entrada `void`, atingir a chave à direita (`}`) que encerra que método ou executar uma `return` instrução que não tem nenhuma expressão resulta em um código de status do encerramento `0`.

Antes do encerramento do aplicativo, os destruidores de todos os seus objetos que ainda não foi coletado como lixo são chamados, a menos que a limpeza foi suprimida (por uma chamada para o método de biblioteca `GC.SuppressFinalize`, por exemplo).

## <a name="declarations"></a>Declarações

Declarações em um programa c# definem os elementos constituintes do programa. Programas em c# são organizados usando namespaces ([Namespaces](namespaces.md)), que pode conter um tipo de declarações e declarações de namespace aninhado. As declarações de tipo ([as declarações de tipo](namespaces.md#type-declarations)) são usadas para definir classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md) ), enums ([Enums](enums.md)), delegados e ([delegados](delegates.md)). Os tipos de membros permitidos em uma declaração de tipo dependem da forma de declaração de tipo. Por exemplo, declarações de classe podem conter declarações de constantes ([constantes](classes.md#constants)), campos ([campos](classes.md#fields)), métodos ([métodos](classes.md#methods)), propriedades ([ As propriedades](classes.md#properties)), eventos ([eventos](classes.md#events)), os indexadores ([indexadores](classes.md#indexers)), operadores ([operadores](classes.md#operators)), construtores de instância ([ Construtores de instância](classes.md#instance-constructors)), construtores estáticos ([construtores estáticos](classes.md#static-constructors)), destruidores ([destruidores](classes.md#destructors)) e tipos aninhados ([tiposaninhados](classes.md#nested-types)).

Uma declaração define um nome na ***espaço de declaração*** ao qual pertence a declaração. Exceto para sobrecarregado membros ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)), ele é um erro de tempo de compilação para ter duas ou mais declarações que apresentam os membros com o mesmo nome em um espaço de declaração. Nunca é possível que um espaço de declaração conter tipos diferentes de membros com o mesmo nome. Por exemplo, um espaço de declaração pode nunca contêm um campo e um método com o mesmo nome.

Há vários tipos diferentes de espaços de declaração, conforme descrito a seguir.

*  Em todos os arquivos de origem de um programa *namespace_member_declaration*s com nenhum delimitador *namespace_declaration* são membros de um espaço único declaração combinado chamado o ***global espaço de declaração***.
*  Em todos os arquivos de origem de um programa *namespace_member_declaration*s dentro *namespace_declaration*s que possuem o mesmo nome totalmente qualificado do namespace são membros de uma única declaração combinado espaço.
*  Cada classe, struct ou declaração de interface cria um novo espaço de declaração. Nomes são apresentados no espaço declaração por meio *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, ou *type_parameter*s. Exceto para o construtor de instância sobrecarregada declarações e o construtor estático de declarações, uma classe ou struct não podem conter uma declaração de membro com o mesmo nome que a classe ou struct. Uma classe, struct ou interface permite que a declaração de métodos sobrecarregados e indexadores. Além disso, uma classe ou struct permite a declaração de construtores de instância sobrecarregada e operadores. Por exemplo, uma classe, struct ou interface pode conter várias declarações de método com o mesmo nome, desde que essas declarações de método diferem em sua assinatura ([assinaturas e sobrecarga](basic-concepts.md#signatures-and-overloading)). Observe que as classes base não contribuem para o espaço de declaração de uma classe e interfaces base não contribuem para o espaço de declaração de uma interface. Portanto, uma classe derivada ou interface é permitida para declarar um membro com o mesmo nome que um membro herdado. Esse membro é considerado ***ocultar*** o membro herdado.
*  Cada declaração de delegado cria um novo espaço de declaração. Nomes são apresentados no espaço declaração por meio de parâmetros formais (*fixed_parameter*s e *parameter_array*s) e *type_parameter*s.
*  Cada declaração de enumeração cria um novo espaço de declaração. Nomes são apresentados no espaço declaração por meio *enum_member_declarations*.
*  Cada declaração de método, a declaração de indexador, declaração do operador, declaração de construtor de instância e função anônima criam um novo espaço de declaração chamado um ***espaço de declaração de variável local***. Nomes são apresentados no espaço declaração por meio de parâmetros formais (*fixed_parameter*s e *parameter_array*s) e *type_parameter*s. O corpo da função membro ou uma função anônima, se houver, é considerado ser aninhado dentro do espaço de declaração de variável local. É um erro para um espaço de declaração de variável local e um espaço de declaração de variável local aninhada para conter elementos com o mesmo nome. Assim, dentro de um espaço de declaração aninhada não é possível declarar um local constante ou variável ou constante com o mesmo nome como uma variável local em um espaço de declaração de delimitador. É possível que dois espaços de declaração conter elementos com o mesmo nome, desde que nenhum espaço de declaração contém o outro.
*  Cada *bloco* ou *switch_block* , bem como um *para*, *foreach* e *usando* instrução, cria um espaço de declaração de variável local para locais constantes e variáveis locais. Nomes são apresentados no espaço declaração por meio *local_variable_declaration*s e *local_constant_declaration*s. Observe que os blocos que ocorrem como ou dentro do corpo de um membro da função ou função anônima são aninhados dentro do espaço de declaração de variável local declarado por essas funções para seus parâmetros. Portanto, é um erro, por exemplo, ter um método com uma variável local e um parâmetro de mesmo nome.
*  Cada *bloco* ou *switch_block* cria um espaço de declaração separada para rótulos. Nomes são apresentados no espaço declaração por meio *labeled_statement*s e os nomes são referenciados por meio *goto_statement*s. O ***rotular o espaço de declaração*** de um bloco inclui todos os blocos aninhados. Assim, dentro de um bloco aninhado não é possível declarar um rótulo com o mesmo nome como um rótulo em um bloco delimitador.

A ordem textual na qual os nomes são declarados geralmente é de nenhum significado. Em particular, a ordem textual não é significativo para a declaração e uso de namespaces, constantes, métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores, construtores estáticos e tipos. Ordem de declaração é significativa das seguintes maneiras:

*  Ordem de declaração para declarações de campo e declarações de variável local determina a ordem na qual seus inicializadores (se houver) são executados.
*  Variáveis locais devem ser definidas antes de serem usadas ([escopos](basic-concepts.md#scopes)).
*  Ordem de declaração para declarações de membro enum ([membros de Enum](enums.md#enum-members)) é significativo quando *constant_expression* valores são omitidos.

O espaço de declaração de um namespace é "ilimitado" e namespace duas declarações com o mesmo nome totalmente qualificado contribuem para o mesmo espaço de declaração. Por exemplo
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

As duas declarações de namespace acima contribuem para o mesmo espaço de declaração, nesse caso, declarando duas classes com nomes totalmente qualificados `Megacorp.Data.Customer` e `Megacorp.Data.Order`. Porque as duas declarações a contribuir com o mesmo espaço de declaração, ele teria causado um erro de tempo de compilação se cada continha uma declaração de uma classe com o mesmo nome.

Conforme especificado acima, o espaço de declaração de um bloco inclui todos os blocos aninhados. Assim, no exemplo a seguir, o `F` e `G` métodos resultam em um erro de tempo de compilação porque o nome `i` é declarado em um bloco externo e não pode ser declarado novamente no bloco interno. No entanto, o `H` e `I` métodos são válidos, desde os dois `i`do são declarados em blocos separados não aninhadas.

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

Namespaces e tipos têm ***membros***. Os membros de uma entidade estão disponíveis com o uso de um nome qualificado que começa com uma referência à entidade, seguido por um "`.`" token, seguido pelo nome do membro.

Membros de um tipo ou são declarados na declaração de tipo ou ***herdadas*** da classe base do tipo. Quando um tipo herda de uma classe base, todos os membros da classe base, exceto construtores de instância, destruidores e construtores estáticos, se tornam membros do tipo derivado. A acessibilidade declarada de um membro da classe base não controla se o membro é herdado — herança estende a qualquer membro que não seja um construtor de instância, um construtor estático ou um destruidor. No entanto, um membro herdado não possa ser acessado em um tipo derivado, devido à sua acessibilidade declarada ([declarado acessibilidade](basic-concepts.md#declared-accessibility)) ou porque está oculta por uma declaração no próprio tipo ([ocultando por meio de herança](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Membros do Namespace

Namespaces e tipos que têm sem namespace delimitador são membros do ***namespace global***. Isso corresponde diretamente para os nomes declarados no espaço de declaração global.

Namespaces e tipos declarados dentro de um namespace são membros desse namespace. Isso corresponde diretamente para os nomes declarados no espaço de declaração de namespace.

Namespaces não têm nenhuma restrição de acesso. Não é possível declarar namespaces privado, protegido ou interno e nomes de namespace são sempre acessíveis publicamente.

### <a name="struct-members"></a>Membros de struct

Os membros de um struct são os membros declarados na estrutura e os membros herdados da classe de base direta do struct `System.ValueType` e a classe base indireta `object`.

Os membros de um tipo simples correspondem diretamente aos membros do alias de tipo de struct pelo tipo simples:

*  Os membros da `sbyte` são os membros do `System.SByte` struct.
*  Os membros da `byte` são os membros do `System.Byte` struct.
*  Os membros da `short` são os membros do `System.Int16` struct.
*  Os membros da `ushort` são os membros do `System.UInt16` struct.
*  Os membros da `int` são os membros do `System.Int32` struct.
*  Os membros da `uint` são os membros do `System.UInt32` struct.
*  Os membros da `long` são os membros do `System.Int64` struct.
*  Os membros da `ulong` são os membros do `System.UInt64` struct.
*  Os membros da `char` são os membros do `System.Char` struct.
*  Os membros da `float` são os membros do `System.Single` struct.
*  Os membros da `double` são os membros do `System.Double` struct.
*  Os membros da `decimal` são os membros do `System.Decimal` struct.
*  Os membros da `bool` são os membros do `System.Boolean` struct.

### <a name="enumeration-members"></a>Membros de enumeração

Os membros de uma enumeração são constantes declaradas na enumeração e os membros herdados classe base direta de enumeração da `System.Enum` e as classes base indiretas `System.ValueType` e `object`.

### <a name="class-members"></a>Membros de classe

Os membros de uma classe são os membros declarados na classe e os membros herdados da classe base (com exceção de classe `object` que não tem classe base). Os membros herdados da classe base incluem constantes, campos, métodos, propriedades, eventos, indexadores, operadores e tipos de classe base, mas não os construtores de instância, destruidores e construtores estáticos da classe base. Membros de classe base são herdados sem considerar sua acessibilidade.

Uma declaração de classe pode conter declarações de constantes, campos, métodos, propriedades, eventos, indexadores, operadores, construtores de instância, destruidores, construtores estáticos e tipos.

Os membros da `object` e `string` correspondem diretamente aos membros dos tipos de classe eles alias:

*  Os membros da `object` são os membros do `System.Object` classe.
*  Os membros da `string` são os membros do `System.String` classe.

### <a name="interface-members"></a>Membros de interface

Os membros de uma interface são os membros declarados na interface e em todas as interfaces base da interface. Os membros na classe `object` não é, estritamente falando, os membros de qualquer interface ([membros da Interface](interfaces.md#interface-members)). No entanto, os membros na classe `object` estão disponíveis por meio de pesquisa de membro em qualquer tipo de interface ([pesquisa de membro](expressions.md#member-lookup)).

### <a name="array-members"></a>Membros da matriz

Os membros de uma matriz são os membros herdados da classe `System.Array`.

### <a name="delegate-members"></a>Membros de delegado

Os membros de um delegado são os membros herdados da classe `System.Delegate`.

## <a name="member-access"></a>Acesso de membros

Declarações de membros permitem o controle sobre o acesso de membro. A acessibilidade de um membro é estabelecida pela acessibilidade declarada ([declarado acessibilidade](basic-concepts.md#declared-accessibility)) do membro combinada com a acessibilidade do tipo imediatamente contido, se houver.

Quando é permitido o acesso a um determinado membro, o membro será considerado ***acessível***. Por outro lado, quando não é permitido o acesso a um determinado membro, o membro será considerado ***inacessível***. Acesso a um membro é permitido quando o textual local no qual o acesso ocorre está incluído no domínio de acessibilidade ([domínios de acessibilidade](basic-concepts.md#accessibility-domains)) do membro.

### <a name="declared-accessibility"></a>Acessibilidade declarada

O ***declarado acessibilidade*** de um membro pode ser um dos seguintes:

*  Público, que é selecionado, incluindo um `public` modificador na declaração de membro. O significado intuitivo de `public` é "não se limitando o acesso".
*  Protegido, que está selecionada, incluindo um `protected` modificador na declaração de membro. O significado intuitivo de `protected` é "acesso limitado à classe recipiente ou tipos derivado da classe que contém".
*  Interno, que é selecionado, incluindo um `internal` modificador na declaração de membro. O significado intuitivo de `internal` é "acesso limitado a esse programa".
*  Protegido interno (ou seja, protegido ou interno), que está selecionada, incluindo ambas uma `protected` e um `internal` modificador na declaração de membro. O significado intuitivo de `protected internal` é "acesso limitado a esse programa ou tipos derivados da classe recipiente".
*  Privado, que é selecionado, incluindo um `private` modificador na declaração de membro. O significado intuitivo de `private` é "acesso limitado ao tipo recipiente".

Dependendo do contexto no qual uma declaração de membro usa colocar, somente determinados tipos de acessibilidade declarada são permitidos. Além disso, quando uma declaração de membro não incluir nenhum modificador de acesso, o contexto no qual a declaração ocorre determina o padrão declarado de acessibilidade.

*  Namespaces têm implicitamente `public` declarado de acessibilidade. Nenhum modificador de acesso é permitidos em declarações de namespace.
*  Tipos declarados em unidades de compilação ou namespaces podem ter `public` ou `internal` declarados como padrão e acessibilidade `internal` declarado acessibilidade.
*  Membros de classe podem ter qualquer um dos cinco tipos de acessibilidade declarada e o padrão `private` declarado de acessibilidade. (Observe que um tipo declarado como um membro de uma classe pode ter qualquer um dos cinco tipos de acessibilidade declarada, enquanto que um tipo declarado como um membro de um namespace pode ter apenas `public` ou `internal` declarado acessibilidade.)
*  Membros de struct podem ter `public`, `internal`, ou `private` declarados como padrão e acessibilidade `private` declarados acessibilidade, pois structs são lacrados implicitamente. Membros de struct introduzidos em um struct (que não é, herdado por esse struct) não podem ter `protected` ou `protected internal` declarado de acessibilidade. (Observe que um tipo declarado como um membro de um struct pode ter `public`, `internal`, ou `private` declarado acessibilidade, enquanto que um tipo declarado como um membro de um namespace pode ter apenas `public` ou `internal` declarado acessibilidade.)
*  Membros de interface possuem implicitamente `public` declarado de acessibilidade. Nenhum modificador de acesso é permitidos em declarações de membro de interface.
*  Membros de enumeração têm implicitamente `public` declarado de acessibilidade. Nenhum modificador de acesso é permitidos em declarações de membro de enumeração.

### <a name="accessibility-domains"></a>Domínios de acessibilidade

O ***domínio de acessibilidade*** de um membro consiste nas seções (possivelmente não contíguas) de texto do programa em que é permitido o acesso ao membro. Para fins de definir o domínio de acessibilidade de um membro, um membro é considerado ***nível superior*** se ele não está declarado dentro de um tipo e membro será considerado ***aninhada*** se ela for declarada dentro de outro tipo. Além disso, o ***texto de programa*** de um programa é definido como todos os programas texto contido em todos os arquivos de origem do programa e o texto do programa de um tipo é definido como todos os programas texto contido no *type_declaration*s desse tipo (incluindo, possivelmente, tipos aninhados dentro do tipo).

Domínio de acessibilidade de um tipo predefinido (como `object`, `int`, ou `double`) é ilimitado.

Domínio de acessibilidade de um nível superior de tipo não associado `T` ([associado e não associados a tipos](types.md#bound-and-unbound-types)) que é declarado em um programa `P` é definido da seguinte maneira:

*  Se a acessibilidade declarada de `T` está `public`, o domínio de acessibilidade de `T` é o texto de programa do `P` e em qualquer programa que faz referência a `P`.
*  Se a acessibilidade declarada de `T` for `internal`, o domínio de acessibilidade de `T` será o texto de programa de `P`.

Essas definições, ele segue que o domínio de acessibilidade de um tipo não associado de nível superior é sempre pelo menos o texto de programa do programa em que esse tipo é declarado.

O domínio de acessibilidade para um tipo construído `T<A1, ..., An>` é a interseção do domínio de acessibilidade de tipo genérico não associado `T` e os domínios de acessibilidade dos argumentos de tipo `A1, ..., An`.

O domínio de acessibilidade de um membro aninhado `M` declarado em um tipo `T` dentro de um programa `P` é definido da seguinte maneira (observar que `M` possivelmente pode ser um tipo):

*  Se a acessibilidade declarada de `M` for `public`, o domínio de acessibilidade de `M` será o domínio de acessibilidade de `T`.
*  Se a acessibilidade declarada de `M` está `protected internal`, vamos `D` ser a união entre o texto do programa de `P` e o texto do programa de qualquer tipo derivado de `T`, que é declarada fora `P`. Domínio de acessibilidade de `M` é a interseção do domínio de acessibilidade de `T` com `D`.
*  Se a acessibilidade declarada de `M` está `protected`, vamos `D` ser a união do texto do programa `T` e o texto do programa de qualquer tipo derivado de `T`. Domínio de acessibilidade de `M` é a interseção do domínio de acessibilidade de `T` com `D`.
*  Se a acessibilidade declarada de `M` for `internal`, o domínio de acessibilidade de `M` será a interseção do domínio de acessibilidade de `T` com o texto de programa de `P`.
*  Se a acessibilidade declarada de `M` for `private`, o domínio de acessibilidade de `M` será o texto de programa de `T`.

Essas definições, ele segue que o domínio de acessibilidade de um membro aninhado é sempre pelo menos o texto de programa do tipo no qual o membro é declarado. Além disso, ele segue o domínio de acessibilidade de um membro nunca é mais inclusivo do domínio de acessibilidade do tipo no qual o membro é declarado.

Em termos de intuitivos, quando um tipo ou membro `M` é acessado, as etapas a seguir são avaliadas para garantir que o acesso é permitido:

*  Primeiro, se `M` é declarada dentro de um tipo (ao contrário de uma unidade de compilação ou um namespace), um erro de tempo de compilação ocorrerá se esse tipo não está acessível.
*  Então, se `M` é `public`, o acesso é permitido.
*  Caso contrário, se `M` está `protected internal`, o acesso é permitido se ele ocorrer dentro do programa no qual `M` for declarado, ou se ele ocorre dentro de uma classe derivada da classe na qual `M` é declarado e ocorre por meio de derivado tipo de classe ([Protected acessar, por exemplo, os membros](basic-concepts.md#protected-access-for-instance-members)).
*  Caso contrário, se `M` está `protected`, o acesso é permitido se ele ocorrer dentro da classe na qual `M` for declarado, ou se ele ocorre dentro de uma classe derivada da classe na qual `M` é declarado e ocorre por meio de derivado tipo de classe ([Protected acessar, por exemplo, os membros](basic-concepts.md#protected-access-for-instance-members)).
*  Caso contrário, se `M` está `internal`, o acesso é permitido se ele ocorrer dentro do programa no qual `M` é declarada.
*  Caso contrário, se `M` está `private`, o acesso é permitido se ele ocorrer dentro do tipo no qual `M` é declarada.
*  Caso contrário, o tipo ou membro está inacessível, e ocorre um erro de tempo de compilação.

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
as classes e membros têm os domínios de acessibilidade a seguir:

*  Domínio de acessibilidade de `A` e `A.X` é ilimitado.
*  Domínio de acessibilidade de `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, e `B.C.Y` é o texto de programa do programa recipiente.
*  Domínio de acessibilidade de `A.Z` é o texto de programa do `A`.
*  Domínio de acessibilidade de `B.Z` e `B.D` é o texto de programa da `B`, incluindo o texto do programa de `B.C` e `B.D`.
*  Domínio de acessibilidade de `B.C.Z` é o texto de programa do `B.C`.
*  Domínio de acessibilidade de `B.D.X` e `B.D.Y` é o texto de programa da `B`, incluindo o texto do programa de `B.C` e `B.D`.
*  Domínio de acessibilidade de `B.D.Z` é o texto de programa do `B.D`.

Como mostra o exemplo, o domínio de acessibilidade de um membro nunca é maior do que um tipo contido. Por exemplo, mesmo que todos os `X` membros terem acessibilidade declarada pública, tudo, exceto `A.X` tem domínios de acessibilidade que são restritos por um tipo de contenção.

Conforme descrito em [membros](basic-concepts.md#members), todos os membros de uma classe base, exceto por exemplo, construtores, destruidores e construtores estáticos, são herdados por tipos derivados. Isso inclui o mesmo membros privados de uma classe base. No entanto, o domínio de acessibilidade de um membro privado inclui apenas o texto de programa do tipo no qual o membro é declarado. No exemplo
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
o `B` classe herda o membro privado `x` do `A` classe. Como o membro é privado, ele é acessível somente dentro de *class_body* de `A`. Assim, o acesso aos `b.x` tiver êxito na `A.F` método, mas falha no `B.F` método.

### <a name="protected-access-for-instance-members"></a>Acesso protegido para membros de instância

Quando um `protected` membro de instância é acessado fora do texto de programa da classe na qual ela é declarada, e quando um `protected internal` membro de instância é acessado fora do texto de programa do programa no qual ela é declarada, o acesso deve ocorrer dentro de um declaração de classe que deriva da classe na qual ela é declarada. Além disso, o acesso é necessário para entrar em vigor por meio de uma instância do tipo de classe derivada ou um tipo de classe construída a partir dele. Essa restrição impede que uma classe derivada de acessar membros protegidos de outras classes derivadas, mesmo quando os membros são herdados da mesma classe base.

Deixe `B` ser uma classe base que declara um membro de instância protegida `M`e deixe `D` ser uma classe que deriva de `B`. Dentro de *class_body* de `D`, acesso a `M` pode assumir um dos seguintes formatos:

*  Um não-qualificados *type_name* ou *primary_expression* do formulário `M`.
*  Um *primary_expression* do formulário `E.M`, fornecido o tipo do `E` está `T` ou uma classe derivada de `T`, onde `T` é o tipo de classe `D`, ou um tipo de classe construído a partir de `D`
*  Um *primary_expression* do formulário `base.M`.

Esses formulários de acesso, além de uma classe derivada pode acessar um construtor de instância protegida de uma classe base em um *constructor_initializer* ([inicializadores de construtor](classes.md#constructor-initializers)).

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
dentro de `A`, é possível acessar `x` por meio de instâncias de ambos `A` e `B`, já que em ambos os casos o acesso ocorre por meio de uma instância do `A` ou uma classe derivada de `A`. No entanto, dentro `B`, não é possível acessar `x` por meio de uma instância do `A`, uma vez que `A` não é derivado de `B`.

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
as atribuições de três para `x` são permitidas porque todos eles ocorrem por meio de instâncias de tipos de classe construídas a partir do tipo genérico.

### <a name="accessibility-constraints"></a>Restrições de acessibilidade

Várias construções de linguagem c# exigem um tipo seja ***pelo menos tão acessíveis quanto*** um membro ou outro tipo. Um tipo `T` deve ser pelo menos tão acessíveis quanto um membro ou tipo `M` se o domínio de acessibilidade de `T` é um superconjunto do domínio de acessibilidade de `M`. Em outras palavras, `T` é pelo menos tão acessíveis quanto `M` se `T` é acessível em todos os contextos em que `M` está acessível.

As restrições de acessibilidade a seguir existirem:

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
*  Os tipos de parâmetro de um construtor de instância devem ser pelo menos tão acessíveis quanto o construtor de instância.

No exemplo
```csharp
class A {...}

public class B: A {...}
```
o `B` classe resulta em um erro de tempo de compilação porque `A` não é pelo menos tão acessíveis quanto `B`.

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
o `H` método no `B` resulta em um erro de tempo de compilação porque o tipo de retorno `A` não é pelo menos tão acessíveis quanto o método.

## <a name="signatures-and-overloading"></a>Assinaturas e sobrecarga

Métodos, construtores de instância, indexadores e operadores são caracterizados por seus ***assinaturas***:

*  A assinatura de um método consiste do nome do método, o número de parâmetros de tipo e o tipo e o tipo (valor, referência ou saída) de cada um dos seus parâmetros formais, considerados na ordem da esquerda para a direita. Para esses fins, qualquer parâmetro de tipo do método que ocorre no tipo de um parâmetro formal é identificado, não por seu nome, mas, por sua posição ordinal na lista de argumentos de tipo do método. A assinatura de um método especificamente não inclui o tipo de retorno, o `params` modificador que pode ser especificado para o parâmetro mais à direita, nem as restrições de parâmetro de tipo opcionais.
*  A assinatura de um construtor de instância consiste o tipo e o tipo (valor, referência ou saída) de cada um dos seus parâmetros formais, considerados na ordem da esquerda para a direita. A assinatura de um construtor de instância não inclui especificamente o `params` modificador que pode ser especificado para o parâmetro mais à direita.
*  A assinatura de um indexador consiste do tipo de cada um dos seus parâmetros formais, considerados na ordem da esquerda para a direita. A assinatura de um indexador especificamente não inclui o tipo de elemento, nem inclui o `params` modificador que pode ser especificado para o parâmetro mais à direita.
*  A assinatura de um operador consiste do nome do operador e o tipo de cada um dos seus parâmetros formais, considerados na ordem da esquerda para a direita. Especificamente, a assinatura de um operador não inclui o tipo de resultado.

As assinaturas são o mecanismo de habilitação de ***sobrecarga*** dos membros em classes, structs e interfaces:

*  Sobrecarga dos métodos permite que uma classe, struct ou interface para declarar vários métodos com o mesmo nome fornecido de suas assinaturas são exclusivos dentro dessa classe, struct ou interface.
*  Sobrecargas de construtores de instância permite que uma classe ou struct para declarar vários construtores de instância, desde que suas assinaturas sejam exclusivas dentro dessa classe ou struct.
*  Sobrecarga de indexadores permite que uma classe, struct ou interface para declarar vários indexadores, desde que suas assinaturas sejam exclusivas dentro dessa classe, struct ou interface.
*  Sobrecarga de operadores permite que uma classe ou struct para declarar vários operadores com o mesmo nome fornecido de suas assinaturas são exclusivos dentro dessa classe ou struct.

Embora `out` e `ref` modificadores de parâmetro são considerados parte de uma assinatura, os membros declarados em um único tipo não podem diferir na assinatura exclusivamente pela `ref` e `out`. Ocorrerá um erro de tempo de compilação se dois membros são declarados no mesmo tipo com assinaturas que seria o mesmo se todos os parâmetros em ambos os métodos `out` modificadores foram alterados para `ref` modificadores. Para outras finalidades de correspondência de assinatura (por exemplo, ocultar ou substituir) `ref` e `out` são considerados parte da assinatura e não correspondem uns aos outros. (Essa restrição é permitir que C# programas para ser facilmente traduzida para executar no comuns Language Infrastructure (CLI), que não fornece uma maneira de definir métodos que diferem apenas em `ref` e `out`.)

Para fins de assinaturas, os tipos `object` e `dynamic` são considerados iguais. Membros declarados em um único tipo podem, portanto, não diferente na assinatura exclusivamente pela `object` e `dynamic`.

O exemplo a seguir mostra um conjunto de declarações de método sobrecarregado, juntamente com suas assinaturas.
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

Observe que quaisquer `ref` e `out` modificadores de parâmetro ([parâmetros de método](classes.md#method-parameters)) fazem parte de uma assinatura. Portanto, `F(int)` e `F(ref int)` são assinaturas exclusivas. No entanto, `F(ref int)` e `F(out int)` não pode ser declarado dentro da mesma interface, porque suas assinaturas sejam diferentes exclusivamente pela `ref` e `out`. Além disso, observe que o tipo de retorno e o `params` modificador não fazem parte de uma assinatura, portanto, não é possível sobrecarregar exclusivamente com base no tipo de retorno ou na inclusão ou exclusão do `params` modificador. Como tal, as declarações dos métodos `F(int)` e `F(params string[])` identificados acima resultará em um erro de tempo de compilação.

## <a name="scopes"></a>Escopos

O ***escopo*** de um nome é a região de texto do programa dentro do qual é possível referir-se à entidade declarada com o nome sem qualificação do nome. Os escopos podem ser ***aninhados***, e um escopo interno pode declarar novamente o significado de um nome de um escopo externo (isso, no entanto, remove a restrição imposta por [declarações](basic-concepts.md#declarations) que dentro de um bloco aninhado não é possível declarar uma variável local com o mesmo nome que uma variável local em um bloco delimitador). O nome do escopo externo, em seguida, deve ser ***ocultos*** na região do programa de texto coberto pelo escopo interno, e acesso para o nome externo só é possível qualificar o nome.

*  O escopo de um membro do namespace declarado por um *namespace_member_declaration* ([membros do Namespace](namespaces.md#namespace-members)) com nenhum delimitador *namespace_declaration* é todo o programa texto.
*  O escopo de um membro do namespace declarado por um *namespace_member_declaration* dentro de uma *namespace_declaration* cujo nome totalmente qualificado é `N` é o *namespace_body*  de cada *namespace_declaration* cujo nome totalmente qualificado é `N` ou começa com `N`, seguido por um período.
*  O escopo de nome definido por uma *extern_alias_directive* se estende pela *using_directive*s, *global_attributes* e *namespace_member_ declaração*s de seu corpo de namespace ou unidade de compilação imediatamente contido. Uma *extern_alias_directive* não contribui com os novos membros para o espaço de declaração subjacente. Em outras palavras, uma *extern_alias_directive* não é transitiva, mas, em vez disso, afeta apenas o compilação unidade namespace corpo ou nas quais ele ocorre.
*  O escopo de um nome definido ou importado por um *using_directive* ([diretivas Using](namespaces.md#using-directives)) se estende pela *namespace_member_declaration*s do  *compilation_unit* ou *namespace_body* em que o *using_directive* ocorre. Um *using_directive* podem disponibilizar zero ou mais nomes de namespace, tipo ou membro dentro de um determinado *compilation_unit* ou *namespace_body*, mas não contribuir com os novos membros para o espaço de declaração subjacente. Em outras palavras, uma *using_directive* não é transitiva, mas em vez disso, só afeta as *compilation_unit* ou *namespace_body* no qual ele ocorre.
*  O escopo de um parâmetro de tipo declarados por um *type_parameter_list* em um *class_declaration* ([declarações de classe](classes.md#class-declarations)) é o *class_base*, *type_parameter_constraints_clause*s, e *class_body* de que *class_declaration*.
*  O escopo de um parâmetro de tipo declarados por um *type_parameter_list* em um *struct_declaration* ([declarações de Struct](structs.md#struct-declarations)) é o *struct_interfaces* , *type_parameter_constraints_clause*s, e *struct_body* isso *struct_declaration*.
*  O escopo de um parâmetro de tipo declarados por um *type_parameter_list* em um *interface_declaration* ([declarações de Interface](interfaces.md#interface-declarations)) é o *interface_base* , *type_parameter_constraints_clause*s, e *interface_body* isso *interface_declaration*.
*  O escopo de um parâmetro de tipo declarados por um *type_parameter_list* em um *delegate_declaration* ([declarações de delegado](delegates.md#delegate-declarations)) é o *return_type*, *formal_parameter_list*, e *type_parameter_constraints_clause*s isso *delegate_declaration*.
*  O escopo de um membro declarado por um *class_member_declaration* ([classe corpo](classes.md#class-body)) é o *class_body* no qual ocorre a declaração. Além disso, o escopo de um membro de classe se estende para o *class_body* das classes derivadas que são incluídas no domínio de acessibilidade ([domínios acessibilidade](basic-concepts.md#accessibility-domains)) do membro.
*  O escopo de um membro declarado por um *struct_member_declaration* ([membros de Struct](structs.md#struct-members)) é o *struct_body* no qual ocorre a declaração.
*  O escopo de um membro declarado por um *enum_member_declaration* ([membros de Enum](enums.md#enum-members)) é o *enum_body* no qual ocorre a declaração.
*  O escopo de um parâmetro declarado em uma *method_declaration* ([métodos](classes.md#methods)) é o *method_body* isso *method_declaration*.
*  O escopo de um parâmetro declarado em uma *indexer_declaration* ([indexadores](classes.md#indexers)) é o *accessor_declarations* isso *indexer_declaration*.
*  O escopo de um parâmetro declarado em uma *operator_declaration* ([operadores](classes.md#operators)) é o *bloco* isso *operator_declaration*.
*  O escopo de um parâmetro declarado em uma *constructor_declaration* ([construtores de instância](classes.md#instance-constructors)) é o *constructor_initializer* e *bloco* disso *constructor_declaration*.
*  O escopo de um parâmetro declarado em uma *lambda_expression* ([expressões de função anônima](expressions.md#anonymous-function-expressions)) é o *anonymous_function_body* isso *lambda_ expressão*
*  O escopo de um parâmetro declarado em uma *anonymous_method_expression* ([expressões de função anônima](expressions.md#anonymous-function-expressions)) é o *bloco* isso *anonymous_method _expression*.
*  O escopo de um rótulo é declarado em uma *labeled_statement* ([rotulado instruções](statements.md#labeled-statements)) é o *bloco* no qual ocorre a declaração.
*  O escopo de uma variável local declarada em um *local_variable_declaration* ([declarações de variável Local](statements.md#local-variable-declarations)) é o bloco no qual ocorre a declaração.
*  O escopo de uma variável local declarada em um *switch_block* de uma `switch` instrução ([da instrução switch](statements.md#the-switch-statement)) é o *switch_block*.
*  O escopo de uma variável local declarada em um *for_initializer* de um `for` instrução ([a instrução](statements.md#the-for-statement)) é o *for_initializer*, o  *for_condition*, o *for_iterator*e independente *instrução* do `for` instrução.
*  O escopo de uma constante local declarado em uma *local_constant_declaration* ([declarações de constante Local](statements.md#local-constant-declarations)) é o bloco no qual ocorre a declaração. Ele é um erro de tempo de compilação para fazer referência a uma constante local em uma posição textual que precede seus *constant_declarator*.
*  O escopo de uma variável declarada como parte de um *foreach_statement*, *using_statement*, *lock_statement* ou *query_expression* é determinado pela expansão da construção determinada.

Dentro do escopo de um membro de namespace, class, struct ou enumeração, é possível referir-se ao membro em uma posição textual que precede a declaração do membro. Por exemplo
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Aqui, ele é válido para `F` para se referir a `i` antes de ser declarada.

Dentro do escopo de uma variável local, ele é um erro de tempo de compilação para fazer referência à variável local em uma posição textual que precede o *local_variable_declarator* da variável local. Por exemplo
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

No `F` método acima, a primeira atribuição para `i` especificamente não se refere ao campo declarado no escopo externo. Em vez disso, ele se refere à variável local e resulta em um erro de tempo de compilação porque textualmente precede a declaração da variável. No `G` método, o uso de `j` no inicializador para a declaração de `j` é válido porque o uso não precede o *local_variable_declarator*. No `H` método, um subsequentes *local_variable_declarator* corretamente se refere a uma variável local declarada em um anteriores *local_variable_declarator* dentro do mesmo  *local_variable_declaration*.

As regras de escopo para variáveis locais são projetadas para garantir que o significado de um nome usado em um contexto de expressão é sempre o mesmo dentro de um bloco. Se o escopo de uma variável local estender somente de sua declaração até o final do bloco, no exemplo acima, a primeira atribuição atribui à variável de instância e a segunda atribuição atribui à variável local, possivelmente levando a erros de tempo de compilação se as instruções do bloco foram mais tarde para ser reorganizadas.

O significado de um nome de dentro de um bloco pode mudar com base no contexto no qual o nome é usado. No exemplo
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
o nome `A` é usado em um contexto de expressão para fazer referência à variável local `A` e em um contexto de tipo para fazer referência à classe `A`.

### <a name="name-hiding"></a>Ocultação de nome

O escopo de uma entidade normalmente abrange mais texto do programa que o espaço de declaração da entidade. Em particular, o escopo de uma entidade pode incluir declarações que introduzem novos espaços de declaração que contém entidades de mesmo nome. Essas declarações fazem com que a entidade original se torne ***oculto***. Por outro lado, uma entidade é considerada ***visíveis*** quando não estiver oculto.

Ocultação de nome ocorre quando houver sobreposição de escopos por meio de aninhamento e quando os escopos se sobrepõem por meio da herança. As características dos dois tipos de ocultação de são descritas nas seções a seguir.

#### <a name="hiding-through-nesting"></a>Ocultação por meio de aninhamento

Ocultação por meio de aninhamento de nome pode ocorrer como resultado de aninhamento namespaces ou tipos em namespaces, como resultado de aninhar tipos dentro de classes ou structs e como resultado do parâmetro e declarações de variável local.

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
dentro de `F` método, a variável de instância `i` está oculto pela variável local `i`, mas dentro a `G` método, `i` ainda se refere à variável de instância.

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
a chamada `F(1)` invoca o `F` declarado em `Inner` porque todas as ocorrências externas de `F` estão ocultos pela declaração interna. Pelo mesmo motivo, a chamada `F("Hello")` resulta em um erro de tempo de compilação.

#### <a name="hiding-through-inheritance"></a>Ocultação por meio de herança

Ocultação por meio da herança de nome ocorre quando as classes ou structs redeclarar nomes que foram herdados de classes base. Esse tipo de ocultação de nome usa uma das seguintes formas:

*  Uma constante, campo, propriedade, evento ou tipo introduzido em uma classe ou struct oculta todos os membros da classe base com o mesmo nome.
*  Um método introduzido em uma classe ou struct oculta todos os membros de classe de base não-método com o mesmo nome e todos os métodos da classe base com a mesma assinatura (nome do método e contagem de parâmetros, modificadores e tipos).
*  Um indexador introduzido em uma classe ou struct oculta todos os indexadores de classe base com a mesma assinatura (contagem de parâmetros e tipos).

As regras que regem as declarações de operador ([operadores](classes.md#operators)) tornar impossível para uma classe derivada declarar um operador com a mesma assinatura como um operador em uma classe base. Assim, operadores ocultam nunca uma da outra.

Ao contrário de ocultar um nome de um escopo externo, ocultar um nome acessível de um escopo herdado faz com que um aviso a ser relatado. No exemplo
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
a declaração de `F` em `Derived` faz com que um aviso a ser relatado. Ocultar um nome herdado especificamente não é um erro, já que seria que impeçam a evolução separada das classes base. Por exemplo, a situação acima pode ter ocorrer porque uma versão posterior do `Base` introduziu um `F` método que não estava presente em uma versão anterior da classe. A situação acima tivesse sido um erro, em seguida, qualquer alteração feita em uma classe base em uma biblioteca de classes separadamente com controle de versão poderia potencialmente causar classes derivadas para se tornar inválido.

O aviso causado por ocultar um nome herdado pode ser eliminado por meio do uso do `new` modificador:
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

O `new` modificador indica que o `F` em `Derived` é "new", e que, de fato, destina-se para ocultar o membro herdado.

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

No exemplo acima, a declaração de `F` na `Derived` oculta a `F` que foi herdada de `Base`, mas porque o novo `F` na `Derived` tem acesso privado, seu escopo não se estende para `MoreDerived` . Portanto, a chamada `F()` na `MoreDerived.G` é válida e invocará `Base.F`.

## <a name="namespace-and-type-names"></a>Nomes de Namespace e tipo

Vários contextos em um C# programa exigir uma *namespace_name* ou uma *type_name* seja especificado.

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

Um *namespace_name* é um *namespace_or_type_name* que se refere a um namespace. Após a resolução conforme descrito abaixo, o *namespace_or_type_name* de uma *namespace_name* deve se referir a um namespace, ou caso contrário, ocorrerá um erro de tempo de compilação. Nenhum argumento de tipo ([argumentos de tipo](types.md#type-arguments)) podem estar presentes em um *namespace_name* (somente tipos podem ter argumentos de tipo).

Um *type_name* é um *namespace_or_type_name* que se refere a um tipo. Após a resolução conforme descrito abaixo, o *namespace_or_type_name* de uma *type_name* deve se referir a um tipo, ou caso contrário, ocorrerá um erro de tempo de compilação.

Se o *namespace_or_type_name* é um qualificado-alias-membro de seu significado conforme descrito em [qualificadores alias de Namespace](namespaces.md#namespace-alias-qualifiers). Caso contrário, uma *namespace_or_type_name* tem uma das quatro formas:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

em que `I` é um identificador único, `N` é um *namespace_or_type_name* e `<A1, ..., Ak>` é um recurso opcional *type_argument_list*. Quando nenhum *type_argument_list* é especificado, considere `k` seja zero.

O significado de um *namespace_or_type_name* é determinado da seguinte maneira:

*   Se o *namespace_or_type_name* tem o formato `I` ou do formulário `I<A1, ..., Ak>`:
    * Se `K` é zero e o *namespace_or_type_name* aparece dentro de uma declaração de método genérico ([métodos](classes.md#methods)) e se essa declaração inclui um parâmetro de tipo ([tipo parâmetros](classes.md#type-parameters)) com o nome `I`, em seguida, a *namespace_or_type_name* refere-se a esse parâmetro de tipo.
    * Caso contrário, se o *namespace_or_type_name* aparece dentro de uma declaração de tipo, em seguida, para cada tipo de instância `T` ([o tipo de instância](classes.md#the-instance-type)), começando com o tipo de instância desse tipo declaração e continuando com o tipo de instância de cada declaração de classe ou struct delimitador (se houver):
        * Se `K` é zero e a declaração de `T` inclui um parâmetro de tipo com nome `I`, em seguida, o *namespace_or_type_name* refere-se a esse parâmetro de tipo.
        * Caso contrário, se o *namespace_or_type_name* aparecerá no corpo da declaração de tipo, e `T` ou qualquer um dos seus tipos base contém um tipo aninhado de acessível com nome `I` e `K`  parâmetros de tipo, o *namespace_or_type_name* refere-se ao tipo construído com os argumentos de tipo em questão. Se houver mais de um tal tipo, o tipo declarado no tipo mais derivado é selecionado. Observe que os membros sem tipo (constantes, campos, métodos, propriedades, indexadores, operadores, construtores de instância, destruidores e construtores estáticos) e membros de tipo com um número diferente de parâmetros de tipo são ignorados ao determinar o significado das *namespace_or_type_name*.
    * Se as etapas anteriores não foram bem-sucedidos em seguida, para cada namespace `N`, começando com o namespace no qual o *namespace_or_type_name* ocorre, continuando com cada namespace delimitador (se houver) e terminando com o namespace global, as etapas a seguir são avaliadas até que uma entidade está localizada:
        * Se `K` é zero e `I` é o nome de um namespace em `N`, então:
            * Se o local onde o *namespace_or_type_name* ocorre estiver envolvido por uma declaração de namespace `N` e a declaração de namespace contém uma *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` com um namespace ou tipo, em seguida, o *namespace_or_type_name* é ambíguo e ocorrer um erro de tempo de compilação.
            * Caso contrário, o *namespace_or_type_name* refere-se ao namespace chamado `I` em `N`.
        * Caso contrário, se `N` contém um tipo acessível com o nome `I` e `K`  , em seguida, os parâmetros de tipo:
            * Se `K` é zero e o local onde o *namespace_or_type_name* ocorre é incluído por uma declaração de namespace `N` e a declaração de namespace contém um *extern_alias_directive*  ou *using_alias_directive* que associa o nome `I` com um namespace ou tipo, em seguida, a *namespace_or_type_name* é ambígua e um tempo de compilação erro ocorre.
            * Caso contrário, o *namespace_or_type_name* refere-se para o tipo construído com os argumentos de tipo em questão.
        * Caso contrário, se o local em que o *namespace_or_type_name* ocorre é incluído por uma declaração de namespace para `N`:
            * Se `K` é zero e a declaração de namespace contém uma *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` com um namespace importado ou tipo, em seguida, a *namespace_or_type_name* refere-se a esse namespace ou tipo.
            * Caso contrário, se as declarações de namespaces e o tipo importado pela *using_namespace_directive*s e *using_alias_directive*s da declaração do namespace conter exatamente um tipo acessível ter o nome `I` e `K`  parâmetros de tipo, o *namespace_or_type_name* refere-se ao tipo construído com os argumentos de tipo em questão.
            * Caso contrário, se as declarações de namespaces e o tipo importado pela *using_namespace_directive*s e *using_alias_directive*s da declaração do namespace contêm mais de um tipo acessível ter o nome `I` e `K`  parâmetros de tipo, o *namespace_or_type_name* é ambíguo e ocorre um erro.
    * Caso contrário, o *namespace_or_type_name* é indefinido e ocorrer um erro de tempo de compilação.
*  Caso contrário, o *namespace_or_type_name* tem o formato `N.I` ou do formulário `N.I<A1, ..., Ak>`. `N` primeiro é resolvido como uma *namespace_or_type_name*. Se a resolução do `N` não for bem-sucedida, ocorre um erro de tempo de compilação. Caso contrário, `N.I` ou `N.I<A1, ..., Ak>` é resolvido da seguinte maneira:
    * Se `K` é zero e `N` refere-se a um namespace e `N` contém um namespace aninhado com nome `I`, em seguida, a *namespace_or_type_name* refere-se ao namespace aninhado.
    * Caso contrário, se `N` refere-se a um namespace e `N` contém um tipo acessível com nome `I` e `K`  parâmetros de tipo, o *namespace_or_type_name* refere-se para esse tipo construído com os argumentos de tipo em questão.
    * Caso contrário, se `N` refere-se a um tipo de classe ou struct (possivelmente construído) e `N` ou qualquer uma das suas classes base contém um tipo aninhado de acessível com nome `I` e `K`  digite parâmetros e, em seguida, o *namespace_or_type_name* refere-se ao tipo construído com os argumentos de tipo em questão. Se houver mais de um tal tipo, o tipo declarado no tipo mais derivado é selecionado. Observe que, se o significado dos `N.I` está sendo determinado como parte da solução da especificação de classe base da `N` , em seguida, a classe base direta de `N` é considerado como objeto ([classes Base](classes.md#base-classes)).
    * Caso contrário, `N.I` é inválido *namespace_or_type_name*, e ocorre um erro de tempo de compilação.

Um *namespace_or_type_name* tem permissão para fazer referência a uma classe estática ([classes estáticas](classes.md#static-classes)) somente se

*  O *namespace_or_type_name* é o `T` em uma *namespace_or_type_name* do formulário `T.I`, ou
*  O *namespace_or_type_name* é o `T` em uma *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) do formulário `typeof(T)`.

### <a name="fully-qualified-names"></a>Nomes totalmente qualificados

Cada namespace e o tipo tem um ***nome totalmente qualificado***, que identifica exclusivamente o namespace ou tipo entre todos os outros. O nome totalmente qualificado de um tipo ou namespace `N` é determinado da seguinte maneira:

*  Se `N` é um membro do namespace global, seu nome totalmente qualificado é `N`.
*  Caso contrário, será o seu nome totalmente qualificado `S.N`, onde `S` é o nome totalmente qualificado do namespace ou tipo no qual `N` é declarada.

Em outras palavras, o nome totalmente qualificado da `N` é o caminho completo hierárquico dos identificadores que levam a `N`, a partir do namespace global. Como todos os membros de um tipo ou namespace devem ter um nome exclusivo, ele segue o nome totalmente qualificado de um tipo ou namespace é sempre exclusivo.

O exemplo a seguir mostra várias declarações de namespace e o tipo junto com seus nomes totalmente qualificados associados.
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

C# emprega o gerenciamento automático de memória, que libera os desenvolvedores de manualmente alocando e liberando a memória ocupada por objetos. Políticas de gerenciamento automático de memória são implementadas por um ***coletor de lixo***. O ciclo de vida de gerenciamento de memória de um objeto é da seguinte maneira:

1. Quando o objeto é criado, de memória alocada para ele, o construtor é executado e o objeto é considerado ao vivo.
2. Se o objeto, ou qualquer parte dele, não pode ser acessada por qualquer possível continuação da execução, que não seja a execução de destruidores, o objeto é considerado não mais em uso e se tornar qualificado para destruição. O compilador do c# e o coletor de lixo podem optar por analisar o código para determinar o que faz referência a um objeto pode ser usada no futuro. Por exemplo, se uma variável local que está no escopo é a única referência existente a um objeto, mas essa variável local nunca é chamada em qualquer possível continuação da execução da execução atual do ponto no procedimento, o coletor de lixo pode (mas não é deve) tratar o objeto como não mais em uso.
3. Depois que o objeto está qualificado para destruição, alguns não for especificado posteriormente tempo o destruidor ([destruidores](classes.md#destructors)) (se houver) para o objeto é executado. Em circunstâncias normais o destruidor do objeto é executado apenas uma vez, embora as APIs específicas da implementação pode permitir que esse comportamento a ser substituído.
4. Depois que o destruidor para um objeto é executado, se esse objeto, ou qualquer parte dele, não pode ser acessado por qualquer possível continuação da execução, incluindo a execução de destruidores, o objeto é considerado inacessível e o objeto se qualifique para coleta.
5. Por fim, em algum momento depois que o objeto se qualifique para coleta, o coletor de lixo libera a memória associada a esse objeto.

O coletor de lixo mantém informações sobre o uso do objeto e usa essas informações para tomar decisões de gerenciamento, de memória como onde na memória para localizar um objeto recém-criado, quando realocar um objeto e, quando um objeto não está mais em uso ou inacessível.

Como outras linguagens que supõem a existência de um coletor de lixo, c# é projetado para que o coletor de lixo pode implementar uma ampla variedade de políticas de gerenciamento de memória. Por exemplo, c# não requer que os destruidores sejam executados ou que os objetos sejam coletados, assim que eles estão qualificados ou que os destruidores ser executado em qualquer ordem específica ou em qualquer thread específico.

O comportamento do coletor de lixo pode ser controlado, até certo ponto, por meio de métodos estáticos na classe `System.GC`. Essa classe pode ser usado para solicitar uma coleta ocorrer, os destruidores para ser executado (ou não executado) e assim por diante.

Uma vez que o coletor de lixo tem latitude ampla ao decidir quando coletar objetos e executar os destruidores, uma implementação em conformidade pode produzir saída que difere daquele mostrado pelo código a seguir. O programa
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
cria uma instância da classe `A` e uma instância da classe `B`. Esses objetos se tornarão elegíveis para coleta de lixo quando a variável `b` é atribuído o valor `null`, pois após esse período é impossível para qualquer código escrito pelo usuário para acessá-los. A saída pode ser
```
Destruct instance of A
Destruct instance of B
```
ou
```
Destruct instance of B
Destruct instance of A
```
porque a linguagem não impõe nenhuma restrição sobre a ordem na qual os objetos são coletadas como lixo.

Em casos sutis, a distinção entre "qualificado para destruição" e "qualificada para a coleta" pode ser importante. Por exemplo,
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

No programa acima, se o coletor de lixo opta por executar o destruidor de `A` antes do destruidor de `B`, em seguida, a saída desse programa pode ser:
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Observe que, embora a instância do `A` não estava em uso e `A`do destruidor foi executado, ele ainda é possível para os métodos de `A` (nesse caso, `F`) a ser chamado do destruidor de outro. Além disso, observe que a execução de um destruidor pode causar um objeto para se tornar utilizável do programa principal novamente. Nesse caso, a execução de `B`do destruidor causou uma instância do `A` que foi anteriormente não em uso para se tornar acessível de referência dinâmica `Test.RefA`. Após a chamada para `WaitForPendingFinalizers`, a instância do `B` está qualificado para coleta, mas a instância do `A` não, é devido a referência `Test.RefA`.

Para evitar confusão e comportamento inesperado, geralmente é uma boa ideia para destruidores deve apenas executar a limpeza nos dados armazenados em seus campos do objeto e não para executar quaisquer ações nos objetos referenciados ou campos estáticos.

Uma alternativa ao uso de destruidores é permitir que uma classe implemente o `System.IDisposable` interface. Isso permite que o cliente do objeto determinar quando liberar os recursos do objeto, normalmente, acessando o objeto como um recurso em um `using` instrução ([a instrução using](statements.md#the-using-statement)).

## <a name="execution-order"></a>Ordem de execução

Execução de um programa c# prossegue, de modo que os efeitos colaterais de cada thread de execução são preservados em pontos críticos de execução. Um ***efeito colateral*** é definido como uma leitura ou gravação de um campo volátil, uma gravação a uma variável não-volátil, uma gravação em um recurso externo e a geração de uma exceção. Os pontos de execução críticos em que a ordem desses efeitos colaterais deve ser preservada são referências a campos voláteis ([campos voláteis](classes.md#volatile-fields)), `lock` instruções ([a instrução lock](statements.md#the-lock-statement)), e criação do thread e encerramento. O ambiente de execução é livre para alterar a ordem de execução de um programa c#, sujeito às seguintes restrições:

*  Dependência de dados é preservada dentro de um thread de execução. Ou seja, o valor de cada variável é computado como se todas as instruções no thread foram executadas na ordem do programa original.
*  As regras são preservadas de ordenação de inicialização ([inicialização do campo](classes.md#field-initialization) e [inicializadores de variável](classes.md#variable-initializers)).
*  A ordenação dos efeitos colaterais é preservada em relação à volátil leituras e gravações ([campos voláteis](classes.md#volatile-fields)). Além disso, o ambiente de execução não precisa avaliar parte de uma expressão, se ele puder deduzir que o valor da expressão que não é usado e que nenhum efeito colateral necessários são produzidas (incluindo qualquer causados por chamar um método ou acessar um campo volátil). Quando a execução do programa é interrompida por um evento assíncrono (por exemplo, uma exceção gerada por outro thread), não há garantia de que os efeitos colaterais observáveis são visíveis na ordem original do programa.
